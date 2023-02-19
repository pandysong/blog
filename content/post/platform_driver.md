---
date: 2020-11-06
title: Platform Driver
weight: 10
---

# What is a Platform Device?

in Kernel document kernel/Documentation/i2c/instantiating-devices:

>> Unlike PCI or USB devices, I2C devices are not enumerated at the hardware
>> level (at run time). Instead, the software must know (at compile time) which
>> devices are connected on each I2C bus segment. So USB and PCI are not
>> platform devices.


