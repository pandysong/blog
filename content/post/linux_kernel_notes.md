---
date: 2013-03-28
title: Linux Notes
weight: 10
---

# basic command 

## uname
```
$ uname -a
Linux syd-svr-pes 4.16.9-1-ARCH #1 SMP PREEMPT Thu May 17 02:10:09 UTC 2018 x86_64 GNU/Linux
```

## driver modules `/lib/modules`

```
cd /lib/modules

```

## lsmod

```
$ lsmod
Module                  Size  Used by
nf_conntrack_ipv4      16384  18
...
...
...
```

## let tar to figure out the compression way

```
tar xf somefile.tar.xz
```
## `more` is old utility, use `less` instead of `more`

Becuase `less` is a bit massive than `more`, some embedded system only have more.

## compile kernel

```
make menuconfig
make all
make modules_install # install modules to /lib/modules/
make install
```


## strace

using strace to see the system call being called

https://www.youtube.com/watch?v=0IQlpFWTFbM


```
strace <your programe>
```

strace could filter as well.

## perf, track L1 cache misses

## ftrace, tracing kernel functions

## /proc

### recover the deleted but still runing exe files

### get the cmdLine

### get fd (file opened)

## https://livegrep.com


## http://eudyptula-challenge.org/

# network 

https://www.slideshare.net/hugolu/the-linux-networking-architecture
