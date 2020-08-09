---
date: 2019-09-20
title: agent-proxy
weight: 10
---

# agent-proxy

    This is a simple, small proxy which was intended for use with kgdb, or
    gdbserver type connections where you want to share a text console and
    a debug session.
    The idea is that you use the agent-proxy to connect to a serial port
    directly or to a remote terminal server.

# repo

```
https://git.kernel.org/pub/scm/utils/kernel/kgdb/agent-proxy.git
```

```
mkdir -p /temp/kdmx_is_not_hard
cd /temp/kdmx_is_not_hard
git clone git://git.kernel.org/pub/scm/utils/kernel/kgdb/agent-proxy.git
cd kdmx
make
./kdmx -n -p "/dev/ttyxxxx" -s /temp/kdmx_ports &
cu --nostop -l $(cat /temp/kdmx_ports_trm)

# when debugging

$(CROSS_ARCH)-gdb /path/to/vmlinux -ex "target remote $(cat /temp/kdmx_ports_gdb)"
```

# BTW: install gdb on mac

http://tomszilagyi.github.io/2018/03/Remote-gdb-with-stl-pp
