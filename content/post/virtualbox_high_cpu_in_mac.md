---
date: 2019-04-23
title: Virtualbox High CPU in Mac
weight: 10
---

# How to resolve

My problem resolved after following one of the solutions in:

https://www.virtualbox.org/ticket/18089?cversion=1&cnum_hist=13

Which is 

```
I just updated to mac OS 10.14.3 and to VirtualBox 5.2.24, and the problem
recurred on a VM where it had been fine before. I had previously set this VM to
use the null audio driver. The settings were:

Audio Enabled
Null Audio Driver
ICH AC97
Audio Output Enabled
Audio Input Enabled

I shut it down, unchecked the first box to disable audio entirely, restarted
the VM, and CPU load is normal again.

```
