---
date: 2019-09-20
title: addr2line
weight: 10
---

# Introduction

When following back trace is seen, how to debug it?

```
[    4.877849] [<c05f563c>] (sensor_probe+0x314/0xcfc) from [<c05fe870>] (i2c_device_probe+0xa8/0xd4)
[    4.886672] [<c05fe870>] (i2c_device_probe+0xa8/0xd4) from [<c0355550>] (really_probe+0xa8/0x20c)
[    4.895403] [<c0355550>] (really_probe+0xa8/0x20c) from [<c03557ac>] (driver_probe_device+0x30/0x48)
[    4.904395] [<c03557ac>] (driver_probe_device+0x30/0x48) from [<c0355824>] (__driver_attach+0x60/0x84)
[    4.913558] [<c0355824>] (__driver_attach+0x60/0x84) from [<c0353fec>] (bus_for_each_dev+0x50/0x90)
[    4.922459] [<c0353fec>] (bus_for_each_dev+0x50/0x90) from [<c0354da0>] (bus_add_driver+0xac/0x20c)
```

The problem here is how to find the line number according to the address


# uses addr2line

Following command will print the line number for the specific address:

```
addr2line -a c05f563c -e vmlinux
```

