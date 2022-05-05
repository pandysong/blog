---
date: 2022-05-01
title: Using Pseudo TTY for testing purpose
weight: 10
---

# Introduction

In my application I have a embedded Linux board, where I have some application
accessing to a UART. The remote device will send command to the UART. The
remote device is not yet ready. Before that, we could use Pseudo tty to do
loopback testing.


# socat

buildroot is used for this embedded system, socat is not default installed. We
could always use following command in buildroot/out/xxxxx/ directory to build
the socat.

```
make socat-rebuild
```

# push socat to target

In the embedded system: (following is the console in adb)

```
# socat -d -d pty,raw,echo=0 pty,raw,echo=0
1970/01/01 02:33:08 socat[2055] N PTY is /dev/pts/1
1970/01/01 02:33:08 socat[2055] N PTY is /dev/pts/2
1970/01/01 02:33:08 socat[2055] N starting data transfer loop with FDs [5,5] and [7,7]
```

Above command creates a loop tunnel between two pts (pseudo tty). So we could
change the application code to talk to /dev/pts/1, and then we could write to
/dev/pts/2 to emulate a remote writing, and vice verse.

Following command emulate the application to accept a command:

```
# cat < /dev/pts/2
hello
```

Following command emulates the remote to send the command:

```
# echo "hello" > /dev/pts/1
```

