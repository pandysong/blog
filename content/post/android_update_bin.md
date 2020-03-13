---
date: 2019-09-23
title: Android update read-only file system
weight: 10
---

# Introduction

During development, from time to time, we have to update part of the system by
using `adb push` command, but sometimes the file is in the read-only part:

```
remote couldn't create file: Read-only file system
```

using the following command to remount as with read and write previlige.
```
adb root
adb shell  mount -o rw,remount -t auto /
```
