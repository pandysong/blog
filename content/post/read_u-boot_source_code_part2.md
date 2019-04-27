---
date: 2019-04-25
title: Read u-boot source code (part 2)
weight: 10
---

### disable MMU and data cache

```
#ifndef CONFIG_SKIP_LOWLEVEL_INIT_ONLY
	/*
	 * Jump to board specific initialization... The Mask ROM will have already initialized
	 * basic memory.  Go here to bump up clock rate and handle wake up conditions.
	 */
	mov	ip, lr		/* persevere link reg across call */
	bl	lowlevel_init	/* go setup pll,mux,memory */
	mov	lr, ip		/* restore link */
#endif
	mov	pc, lr		/* back to my caller */
```

Register `lr` save the return address of the function call, before calling
`lowlevel_init`,  restore it to `lr` after that.

### to be continued

It is found that the using the default `make cscope` there are still a lot of
symbols with same name, and it is difficult to following. I have improved the
method to create the cscope database in
[cscope_u-boot_improved](../cscope_u-boot_improved) and a new post
[read_u-boot_aarch64_source_code_part1](../read_u-boot_aarch64_source_code_part1)
is added by using the new method.

