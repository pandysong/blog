---
date: 2020-10-10
title: The journey to eventually find picocom
weight: 10
---

# The journey

I am finding that `minicom` and `screen` fail to work with baud rate higher
than 230400.

After have done a lot of search and reading the minicom source code, it is
found that in termios.h the maximum baud rate is 230400.

So the limitation comes from the underlying system level termios. This is
defined by Apple. I am not sure how to change that system configuration. I
assume we have to recompile everything in the system.

# Hacking

Some discussion indicates that there is a hacking around this termios. First it
will feed some fake baud rate to the defined structure and then when setting a
real baud rate to the underlying driver, it will replace with the real baud
rate.

I was wandering if I could hack minicom as well to do that.


# Searching

I am not sure how to do that until I find that discussion and fixes on picom
project on github:

https://github.com/npat-efault/picocom/pull/62/commits/a502f76e5daac08fa2e670084295073e68a5dbc8

So why not use picocom directly?

The custom baudrate on Macos is enabled by default on Intel Macos. 

How to use:

```
./picocom /dev/tty.usbserial-00001014C -b 1500000
```

to quit

```
ctrl-a ctrl-x
```

How to build:

```
git clone https://github.com/npat-efault/picocom.git
cd picocom
make
```

That's it. You could copy around and use it directly.

