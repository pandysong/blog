---
date: 2023-02-18
title: using repo to manage official sdk from vendor and local development repo
weight: 10
---

# Introduction

You just get a new SDK from your vendor via repo command from vendor's server.
You have started a new project based on this SDK. However since it is not
possible to push your code changes to your supplier, you have to have a local
mirror (That could be done by changing the manifests.xml). But you also want to
track the new updates from your vendor, how to get update from your vendor as
well as maintain a local copies?

This post explains how.

TLDR: it is possible to accomplish this by using multiple git remote and
"repo"'s command "repo init -b <branch name>".

# Get the SDK from your supplier

For demo purpose, I have created a manifests on github.

```
repo init -u git@github.com:pandysong/repo_test_manifests.git -b master -m door_phone.xml
```

For whom has difficulties to access the google server add following before repo
command:

```
REPO_URL='https://mirrors.tuna.tsinghua.edu.cn/git/git-repo' repo init -u git@github.com:pandysong/repo_test_manifests.git -b master -m door_phone.xml
```

sync the projects:
```
repo sync
```

# Start your development

Now you have the SDK ready for your own product development. 

You want to change the code in "repo_test_sdk_project_a". You check the remote
of this repo and find that the remote is "SDK".

```
repo_test_sdk_project_a$ git remote -v
sdk     ssh://git@github.com/pandysong/repo_test_sdk_project_a.git (fetch)
sdk     ssh://git@github.com/pandysong/repo_test_sdk_project_a.git (push)
```

Of case you could not push to remote "sdk" because it is on your supplier's server.

So you need to create a repository. For demo purpose: I create a repo on my
local disk (instead of creating on a server):

```
~/temp/repo_test/repo_test_sdk_project_a$ git init --bare
```

Now add an additional remote to this project:

```
repo_test_sdk_project_a$ git remote add idh /home/pandy/temp/repo_test/repo_test_sdk_project_a/
repo_test_sdk_project_a$ git remote -v
idh     /home/pandy/temp/repo_test_sdk_project_a.git (fetch)
idh     /home/pandy/temp/repo_test_sdk_project_a.git (push)
sdk     ssh://git@github.com/pandysong/repo_test_sdk_project_a.git (fetch)
sdk     ssh://git@github.com/pandysong/repo_test_sdk_project_a.git (push)
```

Now push the all the code to your own repo

```
repo_test_sdk_project_a$ git checkout master
repo_test_sdk_project_a$ git push idh master
```


Now you start the actual development and add some file to this repository.

```
touch file_added_by_idh.txt
git checkout -b door_phone_dev
git add file_added_by_idh.txt
git commit -m "file added for idh door phone project"
git remote -v
git push idh door_phone_dev
```

You could now get a new manifests.xml from the current working directies:


# create a local xml

```
repo manifest -r -o door_phone.xml
```

Using this new door_phone.xml to overwrite the original one.

```
cp door_phone.xml .repo/manifests/door_phone.xml
```

We need to manually edit the door_phone.xml to have multiple remote

```
diff --git a/door_phone.xml b/door_phone.xml
index 0c40d9e..fc1aed7 100644
--- a/door_phone.xml
+++ b/door_phone.xml
@@ -1,9 +1,10 @@
 <?xml version="1.0" encoding="UTF-8"?>
 <manifest>
   <remote fetch="ssh://git@github.com" name="sdk"/>
+  <remote fetch="/home/pandy/temp/" name="idh"/>

   <default remote="sdk" sync-j="4"/>

-  <project name="pandysong/repo_test_sdk_project_a.git" path="repo_test_sdk_project_a" revision="cee3730e491d3f81f865660deb39205e6622723f" upstream="master"/>
+  <project name="repo_test_sdk_project_a.git" path="repo_test_sdk_project_a" revision="19ed9158622d83a4952f3d7298526f7f44e3e6a4" upstream="door_phone_dev" remote="idh"/>
   <project name="pandysong/repo_test_sdk_project_b.git" path="repo_test_sdk_project_b" revision="132a25ac3e64a25dbfb66ee7bb472fb97fff2a02" upstream="master"/>
 </manifest>
```


Since this manifests is for our local usage, we need to create a local
repository for the local development.

For demo purpose we create a bare git repo in the local disk

```
mkdir manifests_idh.git
cd manifests_idh.git/
git init --bare
```


Add this remote to manifests:
```
git remote add idh /home/pandy/temp/manifests_idh.git
git remote -v
```


```
idh     /home/pandy/temp/manifests_idh.git (fetch)
idh     /home/pandy/temp/manifests_idh.git  (push)
origin  git@github.com:pandysong/repo_test_manifests.git (fetch)
origin  git@github.com:pandysong/repo_test_manifests.git (push)
```

Edit the door_phone.xml file to crate 

Create a development branch:

```
git checkout -b door_phone_dev
git add -u
git commit -m "door phone dev: add some code on repo_test_sdk_project_a"
git push idh door_phone_dev
git push idh master
```

# use the local manifests_idh to manage the development


```
repo init -b door_phone_dev -u /home/pandy/temp/manifests_idh.git/ -m door_phone.xml
```

Now you could switch to SDK release by 

```
repo init -b master -m door_phone.xml
```

Or you could use the switch to development branch

```
repo init -b door_phone_dev -m door_phone.xml
```

After switching, remember to sync:

```
repo sync
```

In the modified project, you could find two remotes:

```
repo_test_sdk_project_a$ git remote -v
idh     /home/pandy/temp/repo_test_sdk_project_a.git (fetch)
idh     /home/pandy/temp/repo_test_sdk_project_a.git (push)
sdk     ssh://git@github.com/pandysong/repo_test_sdk_project_a.git (fetch)
sdk     ssh://git@github.com/pandysong/repo_test_sdk_project_a.git (push)
```

# make a release

Once a phase of development is done, you may want to make a release.  Use the
following command to make a snapshot of all git repository.

```
repo manifest -r -o door_phone.xml
```

If you have several tags on the door_phone_dev branch, and want to go back to
some specific hash. Just checkout the specific manifests

```
cd .repo/manifests
git checkout 42004ea1b4392ebcde9f485f0bc3aa08f2cb6185
```

And then do 

```
repo sync
```

It will go back to the specific version (like a time machine)

You may want to make a tag for this release.

# How to upgrade the upstream

What if the upstream SDK has upgraded.

The SDK manifests is always upgraded on master, while your own project is
branched out from the SDK release.

The steps to upgrade to upstream new SDK would be:

- in the .repo/manifests, you should see two remotes one is "idh", another
   should be the vendor's. If not, use the git command to add the remote and
fetch the latest changes and then checkout the manifests xml latest version.

- then `repo sync` to get the latest version.

Now go to each project, you would see the master is upgraded to latest SDK
version.

For the new development on new sdk, you would need to create branch and rebase
the changes on the old sdk to this new branch and then using "git manifests -r
-o xxx.xml" to create a new xml file and manually fix the "remote" and
"upstream" elements and put this new xml on a branch from new SDK tag.
