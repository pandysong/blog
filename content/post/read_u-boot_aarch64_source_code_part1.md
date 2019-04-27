---
date: 2019-04-27
title: Read u-boot aarch64 source code (part 1)
weight: 10
---

## Introduction

In previous [code walking through](../read_u-boot_source_code_part1), we uses
`make cscope` which is not quite convenient. In this tutorial, we used the new
method to create cscope database mentioned in
[cscope_u-boot_improved](../cscope_u-boot_improved).

## start.S

After loading the cscope.out, I could find search the starting point by the
following command

```
cs find f start.S
```

This leads me to the file `/arm/cpu/armv8/start.S`.


## content values

The first instruction is to jump to function `reset`, just after the branch
instruction it defines some global variable, _TEXT_BASE is 64bit address which
is defined during linking.



```
#else
	b	reset
#endif

	.align 3

.globl	_TEXT_BASE
_TEXT_BASE:
	.quad	CONFIG_SYS_TEXT_BASE

```

Then it defines some variable according to the linker script (refer to
u-boot.lds). `_end_ofs` is the offset of `_end`,  `_bss_start_ofs` is the
offset of start address of bss section. and `_bss_end_ofs` is the offset of
end address of bss section.

```
/*
 * These are defined in the linker script.
 */
.globl	_end_ofs
_end_ofs:
	.quad	_end - _start

.globl	_bss_start_ofs
_bss_start_ofs:
	.quad	__bss_start - _start

.globl	_bss_end_ofs
_bss_end_ofs:
	.quad	__bss_end - _start
```

## reset function

The reset function ump to a function `save_boot_params`.

```
reset:
	/* Allow the board to save important registers */
	b	save_boot_params
.globl	save_boot_params_ret
save_boot_params_ret:
```

From `save_boot_params`, it jumps back to `save_boot_params_ret`.

## CONFIG_POSITION_INDEPENDENT

This part of code relocate the address `_TEXT_BASE` which is defined when
linking to the real run time address `_start`.

```
#if CONFIG_POSITION_INDEPENDENT
	/*
	 * Fix .rela.dyn relocations. This allows U-Boot to be loaded to and
	 * executed at a different address than it was linked at.
	 */
pie_fixup:
	adr	x0, _start		/* x0 <- Runtime value of _start */
	ldr	x1, _TEXT_BASE		/* x1 <- Linked value of _start */
	sub	x9, x0, x1		/* x9 <- Run-vs-link offset */
	adr	x2, __rel_dyn_start	/* x2 <- Runtime &__rel_dyn_start */
	adr	x3, __rel_dyn_end	/* x3 <- Runtime &__rel_dyn_end */
pie_fix_loop:
	ldp	x0, x1, [x2], #16	/* (x0, x1) <- (Link location, fixup) */
	ldr	x4, [x2], #8		/* x4 <- addend */
	cmp	w1, #1027		/* relative fixup? */
	bne	pie_skip_reloc
	/* relative fix: store addend plus offset at dest location */
	add	x0, x0, x9
	add	x4, x4, x9
	str	x4, [x0]
pie_skip_reloc:
	cmp	x2, x3
	b.lo	pie_fix_loop
pie_fixup_done:
#endif
```
For the details of implementation, we may need another post to discuss.


## other functionalities


The rest of files are:

- disable MMU, data cache, instruction cache,
- call `apply_core_errata`
- call `lowlevel_init`

## end of start.S

```
master_cpu:
    bl  _main
```


Continue with [part 2](../read_u-boot_aarch64_source_code_part2)
