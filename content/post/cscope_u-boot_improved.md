---
date: 2019-04-26
title: Using Cscope to U-Boot Source Code (Improved)
weight: 10
---

## Disadvantage of `make cscope`

In the [post](../cscope_for_linux_kernel_or_uboot), we uses the infrastructure
by u-boot or Linux makefile to build the cscope database. But the disadvantage
of that approach is that there are duplicated files for the same functions, so
it is difficult the find a single path for the specific build.

E.g. in the current build

```
pandy@ubuntu:~/u-boot-2018.09$ cat cscope.files | wc -l
5342
```

The database includes 5K files.

## Improvement

The approach is to limit the files to the ones used in the real build. We use
the utility `readelf` to list all the files.

```
readelf --string-dump=.debug_str u-boot | sed -n '/\/\|\.c/{s/.*\]  //p}' > cscope.files
```

This will create a list of files which is used in the compilation and included
in the elf file.

Then use following command to create cscope database:

```
cscope -bqk
```

The advantage of this method is that this method includes only files which are
used in actual compilation.

## Header files

The problem of this method is that it does not include header files. In order
to overcome this issue, I need to find the all header files used in the
compilation.

gcc has an option `-H`, which will print all used header files used in
compilation.

In u-boot, environment var `KCFLAGS` could be used to specify customized
option. Add following environment var:

```
export KCFLAGS=-H
```

Prepare for a clean build:

```
make clean
```

Capture all printing:

```
make > headers.txt 2>&1
```

Filter out all header files:
```
cat headers.txt  | sed -n 's/.* \(.*\.h\)/\1/p' | sort | uniq > headers_filted_uniq.txt
```

Create cscope.files as usualy:

```
readelf --string-dump=.debug_str u-boot | sed -n '/\/\|\.c/{s/.*\]  //p}' > cscope.files
```

Then add all header files:

```
cat headers_filted_uniq.txt >> cscope.files
```

Finally create cscope database:

```
cscope -bqk
```
