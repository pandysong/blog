---
date: 2019-01-02
title: Array of pointer to array in C
weight: 10
---

# Introduction

Following code demonstrates how to initialize an array of pointer to an int
array.

```
#include <stdio.h>
int main()
{
    int *cmd[] = {
        (int []){0x1, 0x2, 0x3},
        (int []){0x4, 0x5, 0x6, 0x7},
    };

    printf("%d %d\n", cmd[0][1], cmd[1][2]);
}
```

See the differences with the following code?

```
#include <stdio.h>
int main()
{
    int cmd[2][4] = {
        {0x1, 0x2, 0x3},
        {0x4, 0x5, 0x6, 0x7},
    };

    printf("%d %d\n", cmd[0][1], cmd[1][2]);
}
```

It is two dimensions array initialization.
