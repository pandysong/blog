---
date: 2019-01-06
title: Play with tinycorelinux
weight: 10
---

# The problem

My issue is that I got some binary from 3rd party which is a 32bit Linux
executive. However I am using Mac, and I do not want to install a big virtual
machine on my Mac for this tiny little Linux tools.

I found a project in Github called `noah` and tried but not be able to make it
run on my Mac.

At the end of the day, I found `tinycorelinux`.

# run on virtual-box

Download following ISO file.

http://tinycorelinux.net/9.x/x86/release/Core-9.0.iso

A little bit knowledge bout tinycorelinux, it is merely a Linux runs in RAM, so
this ISO is the only file needed to run the Linux.

When create a VM, it needs tens of mega bytes of memory. It gives it to 256M. At
the first, I do not bother to create a VDI at all, as mentioned earlier, it does
not need a disk.


# run the Linux binary in tiny core Linux


Network is setup to access the host

```
wget http://$HOST_IP:8000/thebianry
```

For above command to work, in the host we need to setup a simple http server.

Run following command where you have you `thebinary`
```
python3 -m http.server
```

# remember to chmod

```
chmod +x thebinary
```

## error 'no such file'

Make sure the file exists, otherwise it is possible that you are running a 32bit
binary in a 64bit Linux.

#  persistence of extensions

tiny core Linux support to make the extensions install persistent by put a boot
parameter.

In case of micro-core:
```
mc tce=/dev/sda1
```

Note that this could be done only once. But before doing that, we should format
and make a file system on it.

Do it in three steps:

- From `VirtualBox`, create a Virtual Disk Image (sda1)
- in tiny core Linux, using `cfdisk` or `fdisk` to create partitions, reboot to go
  to next step.

```
cfdisk /dev/sda
```

- in tiny core Linux, using mkfs.ext3 to create a file system on the partition.
```

# create host-only adaptor

In virtual box it is possible to set up second virtual network adaptor to allow
host to connect to VM and vice verse.

Do it in two steps:

- VirtualBox-> File  -> Host Network Manager -> Create , This will create a
  virtual network adaptor
- Now, int the guest  settings, Network -> Adapter2 -> Enable Network Adaptor ->
  Attached to -> Host-only Adaapter -> Name -> vboxnet0


# run ssh server

```
tce-load -wi openssh
sudo /usr/local/etc/init.d/openssh start
```

# connect from host to guest via ssh

```
ssh username@192.168.56.101
```

Chagne the username and ip address accordingly.

# dns issue

After host-only adaptor is enabled the guest nds might not work

checkot following file 
```
/etc/resolv.conf
```

To see if following line exists

```
nameserver 192.168.0.1
```
