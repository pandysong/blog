---
date: 2020-04-07
title: active-low property in Linux GPIO driver
weight: 10
---

# Introduction

How do you understand this line of code?

```
gpiod_direction_output(p->reset_gpio, 1);
```

If document is not read, it might be understood as driving the reset_gpio to
high. But it is not the case.

# The active-low property

Please refer to document `Documentation/gpio/consumer.txt`, `The active-low
property` section explains that 1 means logic active, while 0 means logic
de-active. What turns our on the physical GPIO depends on the DTS setting:

If the GPIO is set as `GPIO_ACTIVE_LOW`, then `1` will drive to GPIO to low.
If the GPIO is set as `GPIO_ACTIVE_HIGH, then `1` will drive to GPIO to HIGH.

# Suggestions

It is not obvious for the reader to understand the code and it is very
confusing. Why does not the author define an macro?

GPIO_ACTIVE
GPIO_DE_ACTIVE
