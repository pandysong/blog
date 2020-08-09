---
date: 2020-03-29
title: simple circuit for reed switch to wake up MCU
weight: 10
---

# Introduction

The reed switch is used in detecting if the door is closed or opened. However
the MCU (and maybe for almost all MCU) I used, only accpets the high to low
transaction external interrupt for waking up the MCU.

Hence a simple circuit is needed to allow triggering high to low interrupt in
both door closing and door opening. This is basically a form or NOT logic
circuit using transistor.


# Circuit

A simple circuit is designed:

~[reed switch](reed_switch_circuit_01.png)


Here the 1Ohm resistor is the reed switch in the state of closing. we could see
that on the collector pin of NPN transistor, the voltage is high.

Now when we open the reed switch:

~[reed switch open](reed_switch_circuit_02.png)

The collector pin voltage is low

# How to use

The base pin is connect to one GPIO (allowing external interrupt) on MCU to
wake up the MCU when reed becomes from open to close.

The collector pin is connect to another GPIO on MCU to wake up the MCU when
reed becomes from close to open.

# Power consumption

In most time, reed is closed (door is closed), the power consumption is 3uA.
While if reed is open, the power consumption is 5.36uA.
