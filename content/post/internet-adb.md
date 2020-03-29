---
date: 2020-03-14
title: Internet ADB on Android
weight: 10
---

# Introduction
ADB could be connected wirelessly, other than connecting via USB cable. This
could be usefull sometime.

# How to
- Enable developer's menu on the android device
- Enable "Internet Adb" in the developer's menu
- then on your computer: adb connect <ip of your projector>:5555
  here 5555 is the default port
- then you could use adb command as usual for example "adb shell"

