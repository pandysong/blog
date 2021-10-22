---
date: 2021-02-10
title: Install Ubuntu 16.04 on old 32bit Intel Atom Machine
weight: 10
---


# Issue 1: Graphic Issue

The same one as "https://askubuntu.com/questions/881608/ubuntu-16-10-with-multiple-graphics-issues-on-atom-intel"

The solution mentioned in the post actually works:

```
sudo apt-get install tasksel
sudo tasksel
```

Select Xubunut-Deskop and then OK, it will install new desktop applications.

Once reboot, it should work.

# When Login Graphic freeze

One could use "Ctrl+Alt+Fn" to switch to console mode, so that changes to the
system could be done.

For example to change the grub configuration:

```
sudo vim /etc/default/grub
```

# show the graphic device and driver

```
lspci -k | grep -EA3 'VGA|3D|Display'
```
