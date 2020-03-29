---
date: 2020-03-14
title: Build developer tools on Android
weight: 10
---

# Introduction

In Android development environment, source code is organized by modules. It is
same for the tool binaries. Some tools are not build or installed by default,
especially those for debug purpose.

# AndroidDriverTools

The developer's tools such as this one mentioned in below command are not with
the usual SDK.

Here is the step to download and build on your build server:

```bash
cd ~/<your working directory>/
git clone https://github.com/MeekJeen/AndroidDriverTools.git
cd AndroidDriverTools/i2c-tools
source ../../../../build/envsetup.sh
mm
```

Then on your PC, install it to android device:
```bash

scp -r fuyong:rk3128_working/out/target/product/rk3128_box/system/xbin ./
adb root
adb remount
adb push xbin/i2c* /system/xbin/
```
