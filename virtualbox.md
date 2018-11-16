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
