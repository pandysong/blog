---
date: 2017-05-22
title: Where to define golang interface
weight: 10
---

# where to define golang interface

https://github.com/golang/go/wiki/CodeReviewComments#interfaces

```Go interfaces generally belong in the package that uses values of the interface type, not the package that implements those values. The implementing package should return concrete (usually pointer or struct) types: that way, new methods can be added to implementations without requiring extensive refactoring.```
