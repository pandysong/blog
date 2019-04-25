---
date: 2019-04-24
title: Using Cscope to read Linux Souce Code or U-Boot Source Code
weight: 10
---

# create scope data for Linux Kernel or u-boot

Follow the usual instructions on how to build Linux kernel or u-boot until the
step which does the real `make`, at this stage you could create scope data by
typing:

```
make cscope
```

This will create scope.out, which could be used in vim:

```
cs add scope.out
```

Then you could use the command defined in the following section to search the
database and jump around.

# cscope setup

This is generic for all cscope for any projects other than Linux Kernel or
u-boot.

I likes to use `cabbrev` command:

Add following lines into your *.vimrc*

```
cabbrev fs :cs find s <cword>
cabbrev fg :cs find g <cword>
cabbrev fd :cs find d <cword>
cabbrev fc :cs find c <cword>
cabbrev ft :cs find t <cword>
cabbrev fe :cs find e <cword>
cabbrev ff :cs find f <cword>
cabbrev fi :cs find i <cword>
```

If you do not want to do such mapping by default, you may save to an text file
say `~/cscopecmd.txt`, then each time you want to use these command, you may
`so ~/cscopecmd.txt` to set up.

After setting up, when you type `:fs` followed by space, the command will be
expanded to `:cs find s <cword>`, you could then press *<enter>* or edit the
command line as you want.

# other type of database

In u-boot, other type of database like ctags or etags could be generated.

Generate ctags database:

```
make tags
```

Or

```
make ctags
```

Generate etags database:

```
make etags
```

In linux kernel source code, following target are available in Makefile:

```
tags TAGS cscope gtags
```
