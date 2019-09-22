---
date: 2019-09-20
title: Android aidl code generator
weight: 10
---

# Introduction

In Android, in between Java space and normal Linux User space (written in C++),
the popular communication interface is called Binder. Two processes in Linux
user space could also use `Binder` to communicate. `aidl_generator` is the tool
to generate the client and server stubs for `Binder` interface.

# Detail Explanations.

The code and document is in the Android source code tree:

```
system/tools/aidl/docs/aidl-cpp.md
```
