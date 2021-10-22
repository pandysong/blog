---
date: 2020-09-19
title: Intuitive Explanation of NMS Algorithm (non-maximum suppression)
weight: 10
---

# Introduction

For object detection deep learning algorithm, the deep network output usually
are a list of bounding boxes. Each box contains the coordination's as well as
confidence for a specific type of object.

A lot of blogs exists for the detailed explanation of NMS algorithm. This blog
try to make it more understandable but not that accurate.

# What problem NMS try to resolve?

The problem of the output bounding boxes are:

## There are multiple boxes for an object

How to find the correct bounding box?

The intuitive solution would be to find the object with highest confidence
around the area of target object. For example in the below figure, there 3
bounding boxes around the man:

![NMS for bounding boxes](/img/nms-algorithm.jpg)

The figure is from https://zhuanlan.zhihu.com/p/50126479

The boxes with highest confidence is eventually selected with NMS algorithm.
Note that the confidence is output from deep learning network.

## Multiple Object could overlap

For the same above figure, the bounding boxes of the man and the horse are
overlapped. How to differentiate them? 

The intuitive solution would be to separate boxes with different classes (as
shown in the figure). So this gives a rough detailed steps for NMS algorithms:

# A bit detail steps

1. group the boxes for different classes.
2. for each group, sort the boxes according to the confidence score, get the
   candidate list.
3. pop the box with highest confidence score. Calculated overlapped area with
   all the boxes remaining in the list. Delete the boxes with high overlap
   rate. Because these boxes are considered to be the duplicated ones.
4. Loop with step 3 until the candidate list is empty.

# References

1. https://zhuanlan.zhihu.com/p/50126479
