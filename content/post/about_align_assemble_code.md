---
date: 2019-04-26
title: .align assemble code
weight: 10
---

## Introduction

Often we find following assemble code

```
.align <n>
```

Where n is an integer which could be e.g. 3, 5

In this document, we will do some experiment for understanding this.

## Test code

Create a file `a.S` with following content

```
mov x0,0xf5
.align 3
mov x0,0xf4
```

## Compile

Now using an arm cross compiler to compile above code

```
aarch64-linux-gnu-gcc -c a.S
```

This will generate an object file a.o

```
pandy@ubuntu:~/temp/asm_test$ file a.o
a.o: ELF 64-bit LSB relocatable, ARM aarch64, version 1 (SYSV), not stripped
```

## Disassemble the code

Let's check what is the generated code:

```
pandy@ubuntu:~/temp/asm_test$ aarch64-linux-gnu-objdump -d a.o

a.o:     file format elf64-littleaarch64


Disassembly of section .text:

0000000000000000 <.text>:
   0:   d2801ea0        mov     x0, #0xf5                       // #245
   4:   d503201f        nop
   8:   d2801e80        mov     x0, #0xf4                       // #244
```

It is found that after .align 3, the code is put to the alignment of 8, which
happens to be `2^3`.

# More testing

Let's do some change:

```
mov x0,0xf5
.align 5
mov x0,0xf4
```

```
pandy@ubuntu:~/temp/asm_test$ aarch64-linux-gnu-objdump -d a.o

a.o:     file format elf64-littleaarch64


Disassembly of section .text:

0000000000000000 <.text>:
   0:   d2801ea0        mov     x0, #0xf5                       // #245
   4:   d503201f        nop
   8:   d503201f        nop
   c:   d503201f        nop
  10:   d503201f        nop
  14:   d503201f        nop
  18:   d503201f        nop
  1c:   d503201f        nop
  20:   d2801e80        mov     x0, #0xf4                       // #244
```

0x20 happens to be 2^5.
