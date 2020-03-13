---
date: 2019-04-26
title: SSH Agent How to
weight: 10
---

## Step 1

Change config in ~/.ssh/config


```
host xxxx
    user your_prefered_user_name
    ForwardX11 yes
    ForwardX11Trusted yes
    ForwardAgent yes
```

## Step 2

Add the key you would like to forward:


```
ssh-add ~/.ssh/id_rsa
```

Using commands to verify the key is added:

```
ssh-add -l
```
