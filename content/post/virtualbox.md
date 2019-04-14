---
date: 2018-04-12
title: Notes about Virtualbox
weight: 10
---

# Virtual Box DHCP Server not working 

I installed a mini Ubuntu Linux guest, however the host-only 
network interface could not get appropriate ip address from dhcp server

```
$ VBoxManage  list dhcpservers
NetworkName:    HostInterfaceNetworking-vboxnet0
IP:             192.168.56.100
NetworkMask:    255.255.255.0
lowerIPAddress: 192.168.56.101
upperIPAddress: 192.168.56.254
Enabled:        No
```

It looks the dhcp server is not enabled, alought it looks enabled from GUI

```
$ VBoxManage dhcpserver modify --netname HostInterfaceNetworking-vboxnet0 --enable
$ VBoxManage  list dhcpservers
NetworkName:    HostInterfaceNetworking-vboxnet0
IP:             192.168.56.100
NetworkMask:    255.255.255.0
lowerIPAddress: 192.168.56.101
upperIPAddress: 192.168.56.254
Enabled:        Yes
```
Remember to reboot VirtualBox, otherwise it does not work (at least in my setup)

# ubuntu netplan

new ubuntu release uses netplan to configure interface. 

Add the last two lines to enable the interface `enp0s8`

```
pandy@ubuntu:~$ cat /etc/netplan/01-netcfg.yaml
# This file describes the network interfaces available on your system
# For more information, see netplan(5).
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: yes
    enp0s8:
      dhcp4: yes
```
