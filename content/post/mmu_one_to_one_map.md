---
date: "2019-04-14T15:00:46+08:00"
title: MMU One to One mapping
weight: 10
---

# Introduction

What is the purpse of one to one mapping when using MMU? If we have the same
physical address with virtual address, why bother to do memory mapping?

# MMU Functionalities

Refer to following paragraph in [post][1]. In short, the MMU not only provide
the memory address mapping, but also provide memory protection for example, set
the memory page to read-only or write-only or allowing execution.

```
The MMU does much more than mapping a virtual address space to a physical
address space of different size. The most important point of an MMU is memory
protection, which is relevant even if both address spaces have the same size:
The MMU handles pages (e.g. 4 kB of size) of virtual memory that are mapped to
pages of physical memory.

In most systems, there is not only a single virtual address space, but one for
every process. Under MMU control, every process can only access pages as
allowed by the operating system (which programs the MMU). Most pages of
different processes are isolated from each other, so that e.g. one process
cannot crash another process by writing into its memory.

Mapping of virtual to physical pages under OS control allows address space
randomization, so that reading across a virtual page boundary results in
reading random data instead of certain data (a protection against e.g. buffer
overflow attacks).

Moreover, even if there is a single process, pages can be treated as
read-write, read-only, execute only, and access forbidden. This allows to
restrict a process to access its own pages in a permitted way, e.g. it can make
it impossible to execute "data" stored.
```

[1]: https://stackoverflow.com/questions/21797441/do-we-require-mmu-when-virtual-address-space-is-equal-to-physical-address-space
[2]: https://en.wikipedia.org/wiki/Memory_management_%28operating_systems%29
