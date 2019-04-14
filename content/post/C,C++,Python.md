---
date: 2018-03-28
title: C, C++ and Python
weight: 10
---

# Introduction

There are lot of articles on the internet about the differences between
different language. However, to do peer to peer comparison sometime does not
make sense as the language (especially new language)is designed has its own
market proposition.

I personally used mostly these three languages, and here to highlight merely
according to my personal experiences.

## C

    ```
    C is a general-purpose, high-level language that was originally developed by
    Dennis M. Ritchie to develop the UNIX operating system at Bell Labs. C was
    originally first implemented on the DEC PDP-11 computer in 1972.
    ```

In some area, like Linux kernel development, C is the only choice, there are
historic reasons. Not because other language like C++ could not operate directly
on hardware, but mainly because community is more or less stick with the C.

In C, essentially, the code needs to manage the ownership (allocation and free)
needs to be managed, needs to manage the pointer. If we will look at the source
code on your project, we will find that most of code is essentially out of the
core business logic and is to stick the core business logic together. For
managing a large scale software project, we will need to apply some pattern from
Object Oriented Design. Patterns like decorator, adaptor, bridge could be
applied in C. Actually in kernel development, these patterns are often used.

OOD or OOP is not specifically for C++ or other object oriented language. OOD is
mostly of thought, the OOD could be applied with any language. However it needs
some cost/tricky skills to apply these patterns.

For example, following code will basically decouple the definition of `struct
circle` from its client, only the place/client which needs to access the `struct
circle` definition needs to know

```c
//foward declaration to hide the implementation
struct circle;
struct circle *create_circle(...);
```

Sometime we could make `struct circle` totally transparent, as only
one implementation needs to care the definition, that is the place to put the
definition of `struct circle`.


Overall C is good for any tasks, but the implementation will be tedious. I would
say avoid using C unless other language could not do the job.


## C++

C++ provides a lot of features to make the OOD task easier. The C++ was tedious
as well as it will need to manage the life cycle by new/delete, however new
standard like C++11/14 make the language more modern,

- It has iterator which make the loop much like Python
- It has `auto`, make the type declaration much easier/less tedious.

Additionally, there is an project which get a lot of attentions:

https://github.com/isocpp/CppCoreGuidelines

The guideline tries to provide information for engineer to follow the most
practice and useful tips/rules.

C++/C should be used where the performance is the key feature, sometimes it
needs to interact with hardware directly, sometimes it needs fast assembly code
optimization. If both C++/C could be used, C++ is preferred for large scale
project. It will be beneficial try to understand the rules and apply these rules
to your project. There are some accomplishing tools for these rule, which could
be found on the internet.

## Python

People say Python is a glue language, which I think is a complement for this
language. As the software engineering mainly tries to decouple/separate logics,
this applies to C/C++ or any other languages. Most of the code is to resolve
engineering issue while the core business logic is more or less small in scope.

Python supports duck-typing which makes Heterogeneous Data processing very easy.
Duck-typing support makes the unit test very easy, as one does not need to really
mock up a type in Python in unit test.

Python has a very concise syntax and is most consistent among these languages,
for example `len(s)` could get the length of the object no matter it is a
dictionary, list or set.  While in other languages sometime we could find more
than one way to achieve the same goal, which is confusing and time consuming for
people to figure out.

Python is fast enough to processing web applications. For some hard real time
system, Python might be suitable because it is relatively slow, and Python
interpreter needs additional ROM/RAM.
