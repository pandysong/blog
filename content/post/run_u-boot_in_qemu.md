---
date: 2019-04-25
title: Run u-boot in qemu
weight: 10
---

## Introduction

In u-boot source code doc/README.qemu-arm, it could be found that u-boot could
run under qemu. This could help to understand or experiment u-boot without a
real hardware.

## How to

I am running this in a ubuntu virtual machine on my mac book.

This is for running arm:

```
export CROSS_COMPILE=arm-linux-gnueabi-
make qemu_arm_defconfig
make cscope # This is optional
make
```

For AArch64, replace CROSS_COMPILE with correct cross compiler and then:

```
make qemu_arm64_defconfig
```

## Run:

I am running in a virtual machine by ssh to it

```
qemu-system-arm -curses -machine virt -bios u-boot.bin
```

I need `-curses`, otherwise it will complain:

```
Could not initialize SDL(No available video device) - exiting
```

For AArch64:

```
qemu-system-aarch64 -curses -machine virt -cpu cortex-a57 -bios u-boot.bin
```

Press `Esc + 1` to go to the qemu monitor command line interface, you could type
`help` to get all qemu command, e.g. `stop` to stop emulation:

```
compat_monitor0 console
QEMU 2.11.1 monitor - type 'help' for more information
(qemu)

```

Press `Esc + 2` to go to the u-boot interface.


```
serial0 console


U-Boot 2018.09 (Apr 25 2019 - 21:37:28 +1000)

DRAM:  128 MiB
WARNING: Caches not enabled
In:    pl011@9000000
Out:   pl011@9000000
Err:   pl011@9000000
Net:   No ethernet found.
Hit any key to stop autoboot:  0
scanning bus for devices...

Device 0: unknown device
starting USB...
No controllers found
No ethernet found.
No ethernet found.
=>
```

# QEMU version

This is tested on the following version:

```
pandy@ubuntu:~/u-boot-2018.09$ qemu-system-arm --version
QEMU emulator version 2.11.1(Debian 1:2.11+dfsg-1ubuntu7.8)
Copyright (c) 2003-2017 Fabrice Bellard and the QEMU Project developers
```
