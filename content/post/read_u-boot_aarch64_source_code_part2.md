---
date: 2019-04-27
title: Read u-boot aarch64 source code (part 2)
weight: 10
---

## Introduction

`_main` function is defined in a file called `crt0_64.S`, which is `C-runtime
startup Code for AArch64 U-Boot`.

## crt0_64.S

```
/*
 * This file handles the target-independent stages of the U-Boot
 * start-up where a C runtime environment is needed. Its entry point
 * is _main and is branched into from the target's start.S file.
 *
 * _main execution sequence is:
 *
 * 1. Set up initial environment for calling board_init_f().
 *    This environment only provides a stack and a place to store
 *    the GD ('global data') structure, both located in some readily
 *    available RAM (SRAM, locked cache...). In this context, VARIABLE
 *    global data, initialized or not (BSS), are UNAVAILABLE; only
 *    CONSTANT initialized data are available. GD should be zeroed
 *    before board_init_f() is called.
 *
 * 2. Call board_init_f(). This function prepares the hardware for
 *    execution from system RAM (DRAM, DDR...) As system RAM may not
 *    be available yet, , board_init_f() must use the current GD to
 *    store any data which must be passed on to later stages. These
 *    data include the relocation destination, the future stack, and
 *    the future GD location.
 *
 * 3. Set up intermediate environment where the stack and GD are the
 *    ones allocated by board_init_f() in system RAM, but BSS and
 *    initialized non-const data are still not available.
 *
 * 4a.For U-Boot proper (not SPL), call relocate_code(). This function
 *    relocates U-Boot from its current location into the relocation
 *    destination computed by board_init_f().
 *
 * 4b.For SPL, board_init_f() just returns (to crt0). There is no
 *    code relocation in SPL.
 *
 * 5. Set up final environment for calling board_init_r(). This
 *    environment has BSS (initialized to 0), initialized non-const
 *    data (initialized to their intended value), and stack in system
 *    RAM (for SPL moving the stack and GD into RAM is optional - see
 *    CONFIG_SPL_STACK_R). GD has retained values set by board_init_f().
 *
 * TODO: For SPL, implement stack relocation on AArch64.
 *
 * 6. For U-Boot proper (not SPL), some CPUs have some work left to do
 *    at this point regarding memory, so call c_runtime_cpu_setup.
 *
 * 7. Branch to board_init_r().
 *
 * For more information see 'Board Initialisation Flow in README.
 */
```

In the top level README:

```
Board Initialisation Flow:
--------------------------

This is the intended start-up flow for boards. This should apply for both
SPL and U-Boot proper (i.e. they both follow the same rules).

Note: "SPL" stands for "Secondary Program Loader," which is explained in
more detail later in this file.

At present, SPL mostly uses a separate code path, but the function names
and roles of each function are the same. Some boards or architectures
may not conform to this.  At least most ARM boards which use
CONFIG_SPL_FRAMEWORK conform to this.

Execution typically starts with an architecture-specific (and possibly
CPU-specific) start.S file, such as:

	- arch/arm/cpu/armv7/start.S
	- arch/powerpc/cpu/mpc83xx/start.S
	- arch/mips/cpu/start.S

and so on. From there, three functions are called; the purpose and
limitations of each of these functions are described below.

lowlevel_init():
	- purpose: essential init to permit execution to reach board_init_f()
	- no global_data or BSS
	- there is no stack (ARMv7 may have one but it will soon be removed)
	- must not set up SDRAM or use console
	- must only do the bare minimum to allow execution to continue to
		board_init_f()
	- this is almost never needed
	- return normally from this function

board_init_f():
	- purpose: set up the machine ready for running board_init_r():
		i.e. SDRAM and serial UART
	- global_data is available
	- stack is in SRAM
	- BSS is not available, so you cannot use global/static variables,
		only stack variables and global_data

	Non-SPL-specific notes:
	- dram_init() is called to set up DRAM. If already done in SPL this
		can do nothing

	SPL-specific notes:
	- you can override the entire board_init_f() function with your own
		version as needed.
	- preloader_console_init() can be called here in extremis
	- should set up SDRAM, and anything needed to make the UART work
	- these is no need to clear BSS, it will be done by crt0.S
	- must return normally from this function (don't call board_init_r()
		directly)

Here the BSS is cleared. For SPL, if CONFIG_SPL_STACK_R is defined, then at
this point the stack and global_data are relocated to below
CONFIG_SPL_STACK_R_ADDR. For non-SPL, U-Boot is relocated to run at the top of
memory.

board_init_r():
	- purpose: main execution, common code
	- global_data is available
	- SDRAM is available
	- BSS is available, all static/global variables can be used
	- execution eventually continues to main_loop()

	Non-SPL-specific notes:
	- U-Boot is relocated to the top of memory and is now running from
		there.

	SPL-specific notes:
	- stack is optionally in SDRAM, if CONFIG_SPL_STACK_R is defined and
		CONFIG_SPL_STACK_R_ADDR points into SDRAM
	- preloader_console_init() can be called here - typically this is
		done by selecting CONFIG_SPL_BOARD_INIT and then supplying a
		spl_board_init() function containing this call
	- loads U-Boot or (in falcon mode) Linux
```

## setup C runtime for board_init_f(0)

```
/*
 * Set up initial C runtime environment and call board_init_f(0).
 */
#if defined(CONFIG_TPL_BUILD) && defined(CONFIG_TPL_NEEDS_SEPARATE_STACK)
	ldr	x0, =(CONFIG_TPL_STACK)
#elif defined(CONFIG_SPL_BUILD) && defined(CONFIG_SPL_STACK)
	ldr	x0, =(CONFIG_SPL_STACK)
#elif defined(CONFIG_SYS_INIT_SP_BSS_OFFSET)
	adr	x0, __bss_start
	add	x0, x0, #CONFIG_SYS_INIT_SP_BSS_OFFSET
#else
	ldr	x0, =(CONFIG_SYS_INIT_SP_ADDR)
#endif
	bic	sp, x0, #0xf	/* 16-byte alignment for ABI compliance */
	mov	x0, sp
	bl	board_init_f_alloc_reserve
	mov	sp, x0
	/* set up gd here, outside any C code */
	mov	x18, x0
	bl	board_init_f_init_reserve

	mov	x0, #0
	bl	board_init_f
```

Above code first set `x0` with an address where stack pointer (SP) will be
pointed to. And then it will make the `x0` with 16 byte alignment and then set
it to `sp` in the code:

```
	bic	sp, x0, #0xf	/* 16-byte alignment for ABI compliance */
```

Before calling `board_init_f_alloc_reserve`, it puts the value of stack pointer
to `x0` which is the first parameter for the calling function
`board_init_f_alloc_reserve`.

```
	mov	x0, sp
```

`board_init_f_alloc_reserve` reserve memory for `(struct global_data)` with 16
bytes alignment. Note that actual reserving is done by put the return value to
`sp`:

```
	mov	sp, x0
```

Before calling `board_init_f_init_reserve`, it saves the `x0` to `x18` the
reason is that

```
	mov	x18, x0
```

`x0` now point to the `(struct global_data)`, the function
`board_init_f_init_reserve`, zero out this structure.

### `board_init_f_alloc_reserve`

This function gets input of the top address of the memory and return the
address after reserving the memory for `(struct global_data)`.

```
ulong board_init_f_alloc_reserve(ulong top)
{
	/* Reserve early malloc arena */
#if CONFIG_VAL(SYS_MALLOC_F_LEN)
	top -= CONFIG_VAL(SYS_MALLOC_F_LEN);
#endif
	/* LAST : reserve GD (rounded up to a multiple of 16 bytes) */
	top = rounddown(top-sizeof(struct global_data), 16);

	return top;
}
```

### `board_init_f_init_reserve`

The first parameter is always `x0`, it just zeros out the structure.

```
void board_init_f_init_reserve(ulong base)
{
	struct global_data *gd_ptr;

	/*
	 * clear GD entirely and set it up.
	 * Use gd_ptr, as gd may not be properly set yet.
	 */

	gd_ptr = (struct global_data *)base;
	/* zero the area */
	memset(gd_ptr, '\0', sizeof(*gd));
	/* set GD unless architecture did it already */
#if !defined(CONFIG_ARM)
	arch_setup_gd(gd_ptr);
#endif
	/* next alloc will be higher by one GD plus 16-byte alignment */
	base += roundup(sizeof(struct global_data), 16);

	/*
	 * record early malloc arena start.
	 * Use gd as it is now properly set for all architectures.
	 */

#if CONFIG_VAL(SYS_MALLOC_F_LEN)
	/* go down one 'early malloc arena' */
	gd->malloc_base = base;
	/* next alloc will be higher by one 'early malloc arena' size */
	base += CONFIG_VAL(SYS_MALLOC_F_LEN);
#endif
}
```

Now the memory layout is like this:

```
top memory ------------> lower memory
+-------------+--------------------+-------------------------+
| malloc area | struct global_data |                         |
+-------------+--------------------+-------------------------+
              ^                    ^
              |                    |
              gd->malloc_base      Base
                                   sp (stack grow direction ---> )
```

### calling `board_init_f`

```
	mov	x0, #0
	bl	board_init_f
```

In function `board_init_f()`, a list of functions (list in `init_sequence_f[]`)
is run, in which serial_init() is called and dram_init() is called. These two,
I think is the most important ones.


###  relocation

In u-boot, there is a command shell which is called monitor. Following code
relocate the monitor to the address defined in `gd->relocaddr`. The register
`lr` stores the returning address when function `recloate_code` returns.

```
#if !defined(CONFIG_SPL_BUILD)
/*
 * Set up intermediate environment (new sp and gd) and call
 * relocate_code(addr_moni). Trick here is that we'll return
 * 'here' but relocated.
 */
	ldr	x0, [x18, #GD_START_ADDR_SP]	/* x0 <- gd->start_addr_sp */
	bic	sp, x0, #0xf	/* 16-byte alignment for ABI compliance */
	ldr	x18, [x18, #GD_NEW_GD]		/* x18 <- gd->new_gd */

	adr	lr, relocation_return
#if CONFIG_POSITION_INDEPENDENT
	/* Add in link-vs-runtime offset */
	adr	x0, _start		/* x0 <- Runtime value of _start */
	ldr	x9, _TEXT_BASE		/* x9 <- Linked value of _start */
	sub	x9, x9, x0		/* x9 <- Run-vs-link offset */
	add	lr, lr, x9
#endif
	/* Add in link-vs-relocation offset */
	ldr	x9, [x18, #GD_RELOC_OFF]	/* x9 <- gd->reloc_off */
	add	lr, lr, x9	/* new return address after relocation */
	ldr	x0, [x18, #GD_RELOCADDR]	/* x0 <- gd->relocaddr */
	b	relocate_code

relocation_return:
```

`lr` is now the new address, so from `relocation_return` and on, the code is on
new RAM address. The relocation code is in `arch/arm/lib/relocate_64.S`.

We may need a new post on how the relocation is done in details. Will update
this post when that is ready.

### setup full environment

After relocation, the `(struct global_data)` gd and the code are all in DRAM
instead of previously in SRAM (very limited in size). Now we have plenty of
memory, and full c run time environment could be setup.

```
/*
 * Set up final (full) environment
 */
	bl	c_runtime_cpu_setup		/* still call old routine */
```

The code `c_runtime_cpu_setup` is in `arm/cpu/armv8/start.S` which basically
setup the exception vector.

### clear bss

After setting up full C run time environment, bss section is cleared so that
all unutilized values becomes zeros.

```
/*
 * Clear BSS section
 */
	ldr	x0, =__bss_start		/* this is auto-relocated! */
	ldr	x1, =__bss_end			/* this is auto-relocated! */
clear_loop:
	str	xzr, [x0], #8
	cmp	x0, x1
	b.lo	clear_loop
```

### call `board_init_r`

Finally it calls `board_init_r` with two parameters, `x0` is with the new gd_t
which is `struct global_data`, `x1` is the relocation address.

```
	/* call board_init_r(gd_t *id, ulong dest_addr) */
	mov	x0, x18				/* gd_t */
	ldr	x1, [x18, #GD_RELOCADDR]	/* dest_addr */
	b	board_init_r			/* PC relative jump */
```


Function `board_init_r` calls a sequence of functions defined in following

```
static init_fnc_t init_sequence_r[] = {
static init_fnc_t init_sequence_r[] = {
	initr_trace,
	initr_reloc,
	/* TODO: could x86/PPC have this also perhaps? */
#ifdef CONFIG_ARM
	initr_caches,
	/* Note: For Freescale LS2 SoCs, new MMU table is created in DDR.
	 *	 A temporary mapping of IFC high region is since removed,
	 *	 so environmental variables in NOR flash is not available
	 *	 until board_init() is called below to remap IFC to high
	 *	 region.
	 */
#endif
	initr_reloc_global_data,
.
.
.
#if defined(CONFIG_PRAM)
	initr_mem,
#endif
	run_main_loop,
};

```

Note that the last function is `run_main_loop`, hence function `board_init_r`
never returns.

Continue with [part 3](../read_u-boot_aarch64_source_code_part3)
