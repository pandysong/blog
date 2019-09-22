---
date: 2019-08-25
title: Understanding Android Repo
weight: 10
---

## Introduction

In Android repo tool, after initializing, there exist following directory under
`.repo`:

project-objects
projects

How these are organized?

Basically in projects, the layout is same as the one for the working directory,
the project-objects layout is same as the files in the remote server.

While in the working directory for each project, as usual for git repository,
there is a `.git` directory, which basically has bunch of symbol link to
project-objects and projects directory.

# repo sync

Two steps, first one is git fetch:

When doing fetching, it basically fetch the repository from remote and put
it under directory `.repo/projects` and `.repo/project-objects`

Then doing git rebase to update the current working directory.
