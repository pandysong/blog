---
date: 2020-07-12
title: new way to init array in C lang
weight: 10
---

# Introduction

This note introduces a way to initiliaze an item in an array by its index.

# Using index to init specific item in an array

```c
#include <stdio.h>

enum {
    TYPE_1,
    TYPE_2,
    TYPE_3,
};

static int array[] = {
    [TYPE_1] = 1,
    [TYPE_3] = 4,
};

int main()
{
    printf("hello world %d %d\n", array[1], array[2]);
}
```

This is usefuly to insert a new enum type, say if I will change to 

```
enum {
    TYPE_1,
    TYPE_1_1,
    TYPE_2,
    TYPE_3,
};
```

The original code still works.
