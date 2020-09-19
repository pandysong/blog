---
date: "2019-05-01"
title: A small discussion on `char *` in C and C++
weight: 10
---

## A piece of code


Look at following code:

```
#include <stdio.h>

int main()
{
    char *s = "hello world";
    puts(s);
}
```

Note that puts is basically a `printf` like funtion but without `format`
functinalities.

Now using the following command to compile as a c program.

```
gcc puts_test.c -o puts_test_c
```

There are no errors emitted.


However if we compile as C++ program:

```
g++ -std=c++14 -o puts_test puts_test.cpp
puts_test.cpp:5:15: warning: ISO C++11 does not allow conversion from string literal to 'char *' [-Wwritable-strings]
    char *s = "hello world";
              ^
1 warning generated.
```

It complains. 

This is just an simple example how C++ improves the type safe: `char *s` is
mutable, however a string literal is not: we could not simple assign a literal
to a mutable variable.

## How to fix

Using `const char *` instead of `char *`.

## a bit more

To make the C++ program more with C++ style:

```
#include <cstdio>

int main()
{
    const char *s = "hello world";
    std::puts(s);
}
```
