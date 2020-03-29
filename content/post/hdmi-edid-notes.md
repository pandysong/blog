---
date: 2019-01-02
title: notes when developing HDMI interface
weight: 10
---

#Introduction

HDMI is quite popular interface, the negotiation between the video source and
vide sink is via EDID, which basically are blocks with 128bytes each block.

# Flexibility

The EDID is provided by the sink side via DDC (physically i2c interface) to the
video source (e.g. a computer). The source will then decide which format,
timing, resolution to send over HDMI signal cable.

EDID has been evolving while the industry is moving forward. So the data
structure in these blocks are not simple, hence it is tricky to edit it
manually.

There are
[tools][https://www.analogway.com/emea/products/software-tools/aw-edid-editor/]
available which makes the life much easier.

# Video formatting/timing definition

The most easy way to define a supported resolution is to do it in block 1
where, there is a section "Established Timing", which is the most popular ones
marked in the EDID as bitmaps. And then you may find other timing in standard
resolutions, which are popular as well. Then you could define detailed timing
in "Detailed Timing"

An more friendly and comprehensive document is
[available][https://www.analogway.com/files/uploads/produit/download/en/edid-editor-white-paper.pdf]
