---
date: 2020-03-19
title: Experiences of "DO NOT PUT PROBE ON CLOCK Signal"
weight: 10
---

# Introduction

Probe is nice to debug the signals. I have recently use oscilloscope and portable
logic analyzer to debug an issue on port camera driver on Rockchip rk3128.

# Story

It takes me a while to make the signal looks right to be compatible to rk3128
CIF interface which is a YUV422 with VSYNC, HSYNC and clock with additional 8
bit parallel interface. Everything seems right with my skills exhausted.

After one day debugging, I feel that it is possible that probe is distorting
the signal. I do not believe that it will immediately resolve the issue until
the image shows up just after that.

So an important lessons: remove the probe after you have done the measurement,
especially for the high speed clock, where parasitic capacitance on the probe
will definitely distort the signal a lot.
