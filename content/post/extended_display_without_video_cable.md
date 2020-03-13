---
date: 2019-04-26
title: Extend Display without Video Cable
weight: 10
---

## Introduction

I need an extended display to show more code. These code exits on remote server
or on my local laptop. Usually if I would need an extended display, I would
need a cable like HDMI to connect to another display device.

I do not like this as first of all these cable is not lightweight, and make a
mess on your neat table, second laptop would become very hot if it needs to
driver additional display.

I was thinking of to extend the display via network, but share the same mouse
and keyboard

# Step 1

Since the display device would need a Linux, which could be fullfilled by a
simple Rasphery Pi board with Ubuntu installed.

using x2x could share the mouse and keyboard for another display.

http://terokarvinen.com/2006/share-mouse-and-keyboard-using-x2x-and-ssh-4

With the fist step enabled, we could move the mouse and keyboard just like it
is extended. I have not tried yet for my mac loptop with another Linux display.

# Step 2

For accessing the source code on my laptop, I could start a tmux background
session. And then the display device could ssh back to my laptop and attach to
the tmux session. In this way, the display device is just a like an extended
display but with no visible cable (all through WiFi).

This is sutible for me as all my work is done on text based environment like
Tmux or Vim, so the traffic is low.

# more discussion


