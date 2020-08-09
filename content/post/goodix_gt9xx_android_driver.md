---
date: 2020-04-07
title: goodix gt9xx android driver.
weight: 10
---

# Introduction

The Android Linux driver was published here:

https://github.com/goodix/gt9xx_driver_android

After reading the code, it is found that when `irq-gpios` is not set, the code
will be working in polling mode. However when I tried to port it to Android
9.0. The code crashes.


```
kernel/kernel/locking/rtmutex.c:1536: (discriminator 9) [<ffffff8008b538a8>] rt_mutex_trylock+0x30/0xb8
kernel/drivers/i2c/i2c-core.c:2343: [<ffffff80086b1a98>] i2c_transfer+0x40/0x94
kernel/drivers/input/touchscreen/gt9xx/gt9xx.c:97: [<ffffff800868da2c>] gtp_i2c_read+0xf4/0x1a8
kernel/drivers/input/touchscreen/gt9xx/gt9xx.c:355: [<ffffff800868e198>] gtp_get_points+0x84/0x284
kernel/drivers/input/touchscreen/gt9xx/gt9xx.c:569: [<ffffff800868e540>] gtp_work_func+0x1a8/0x5ec
kernel/include/linux/hrtimer.h:393: [<ffffff800868e99c>] gtp_timer_handler+0x18/0x40
kernel/kernel/time/hrtimer.c:1261: [<ffffff800810965c>] __hrtimer_run_queues+0x138/0x350
kernel/kernel/time/hrtimer.c:1362: [<ffffff8008109da8>] hrtimer_interrupt+0xa0/0x1b0
kernel/drivers/clocksource/arm_arch_timer.c:150: [<ffffff80087a5610>] arch_timer_handler_virt+0x28/0x3c
kernel/kernel/irq/chip.c:761: [<ffffff80080f99c4>] handle_percpu_devid_irq+0x7c/0x1d0
kernel/kernel/irq/irqdesc.c:352: [<ffffff80080f53dc>] generic_handle_irq+0x18/0x2c
kernel/kernel/irq/irqdesc.c:388: [<ffffff80080f5710>] __handle_domain_irq+0xa8/0xac
kernel/drivers/irqchip/irq-gic.c:352: [<ffffff8008080f24>] gic_handle_irq+0x64/0xa0

```

Above indicates that the gtp_work_func() could not work in interrupt context.
Hence it is believed that the interrupt mode of this driver does not work as
well.

# Fix the problem

To fix the problem, the gtp_work_func has to be moved to non-interrupt context.
The first idea is to use the work queue. The original driver, uses `hrtimer`
(high resolution timer) to trigger timer, which for me, however it does not
make sense, as the I2C reading is not have to be that accurate in terms of
timing. We could simply uses delayed work queue to replace the `hrtimer`
solution.

The fix is put at:

https://github.com/pandysong/gt9xx_driver_android

# Improvement

Further improvement were also done

- Make the interrupt mode working as well (not tested, as I have only polling
  mode test bed)
- Fix some compilation error.
- Add dts setting `goodix,mirror-x` `goodix,mirror-y` to allow mirroring the
  position.

# TBD

Further improvement could be done in the future:

- improve document (allowing polling mode, add mirror-x and mirror-y dts
