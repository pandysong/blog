---
date: 2019-08-19
title: reading device tree source (dts) file using folded feature in vim
weight: 10
---

## Introduction

I am the heavy user of vim and like the `folded` feature very much. I wanted to
use the feature in device tree source file (dts) but it looks it does not
support by default. Vim support `folded` feature for popular languages like C
or Python or JavaScript, however it looks not working for `dts`. I search
around on internet and could not find the plugin for dts folder.

I was wandering if I could write such plugin by myself, however I could not
find time. 

One day, I was wondering around and find this plugin:

```
https://github.com/pseewald/vim-anyfold
```

## What is vim-anyfold

In a simple word, it detects the structural information via the indent in the
source file. DTS is a very well organized file. I use this plugin against dts
file and it works great.
