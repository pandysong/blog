---
date: 2019-04-26
title: .quad assemble code
weight: 10
---

## Introduction

The `.quad` directive is used to define 64 bit numeric value(s). In similar way
how .byte directive works. In this document, we will do some experiment for
understanding `.quad`.

## Test code

Create a file `a.S` with following content

```
.globl _start
_start:
mov x0,0xf5
.quad 0x123456789ABCDEF0,2,3
.align 5
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

0000000000000000 <_start>:
   0:   d2801ea0        mov     x0, #0xf5                       // #245
   4:   9abcdef0        .word   0x9abcdef0
   8:   12345678        .word   0x12345678
   c:   00000002        .word   0x00000002
  10:   00000000        .word   0x00000000
  14:   00000003        .word   0x00000003
  18:   00000000        .word   0x00000000
  1c:   d503201f        nop
  20:   d2801e80        mov     x0, #0xf4                       // #244
```
