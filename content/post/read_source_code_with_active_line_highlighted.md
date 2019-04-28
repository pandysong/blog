---
date: 2019-04-28
title: Read Source Code with Active Lines Highlighted
weight: 10
---

## cons of cscope database

In [cscope_for_linux_kernel_or_uboot](../cscope_for_linux_kernel_or_uboot) and
[cscope_u-boot_improved](cscope_u-boot_improved), it was discussed how to scope
the files to the ones actually used.

But that is not perfact: in the production code like u-boot there are many
macros, in an active file which is used in compilation some are active but some
are not. When we are reading the code, unless we are debugging in a debugger we
do not immediately know which part of code is active (generatting machine
code), and which part of code is inactive.

This document intends to resolve this issue in a simple way.

## objdump

objdump could dump the line numbers and corresponding assemble code. We could
them only get the filename 

```
aarch64-linux-gnu-objdump -d -l <elf_file> | sed -n "s/\(.*\.[chS]\):\([0-9]*\).*/\1:\2/p" 
```

This will filter out all file names with .c or .S extension suffixed with
':<linenumber>'

## how to highlight in the source code

In the
[post](https://lardcave.net/text/Highlighting%20arbitrary%20lines%20in%20Vim.html),
we know how to highlight multiple lines. We may need to create a database from
which the highlight would be made.

There is another
[hint](https://stackoverflow.com/questions/13675019/vim-highlight-lines-using-line-number-on-external-file)

The basic idea is to define a new color group:

```
:highlight activelines ctermbg=darkred guibg=darkred
```

Then you could add line matching to this group, vim will automatically
highlight for you:

```
:match activelines /\%7l\|\%10l\|\%15l/
```

Or we could lowlight the inactive code

```
:highlight inactivelines ctermfg=lightgray guibg=lightgray
:match activelines /\%11l\|\%12l\|\%13l/
```

## vim plugin `highlight.vim`

For the purpose of highlighting the active lines, a
[plugin](https://github.com/pandysong/highlight.vim) is created after having
done above research.

For using this plugin, a database could be created, as discussed above, we will
create a list from tool chain:

```
aarch64-linux-gnu-objdump -d -l <elf_file> | sed -n "s/\(.*\.[chS]\):\([0-9]*\).*/\1:\2/p"  > lines.txt
```

Then use the tool provided by plugin to convert to the database needed:
```
python3 tools/gen.py /home/pandy/u-boot-2018.09 ../u-boot-2018.09/lines.txt \
> ../u-boot-2018.09/active_lines.txt
```

Now in the vim (with the current path in u-boot directory), you could load the
database:

```
:HighlightAdd active_lines.txt
```

The vim looks like after open an active file:

![screen](/img/vim-activelines-screen.png)

It is found that following code is not compiled:

```
#ifdef COUNTER_FREQUENCY
	ldr	x0, =COUNTER_FREQUENCY
	msr	cntfrq_el0, x0			/* Initialize CNTFRQ */
#endif
```
