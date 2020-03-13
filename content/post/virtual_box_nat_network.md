---
date: 2019-09-23
title: VirtualBox NAT Network
weight: 10
---

# Introduction

I often needs to ssh to the VM which is installed on e.g. a Windows machines
(or a macbook) from another host. In this case NAT network is enough.

NAT basically hide the VM behind the host, but allows to access the server via
NAT. For the world outside the host, it only knows the IP address of host. I
will need to ssh to VM, I just ssh to the host which will do the port forward
to VM

```
another host (30.0.0.2)  <-> host (30.0.0.125) <-> VM (10.0.2.5)
```

In the NAT network, it will configure the port forwarding: 30.0.0.125:22 to
10.0.2.5:22.

I will just need to `ssh pandy@30.0.0.125`.

# How to set 

# NAT *network*

Add a NAT *network*, from File -> Preferences -> Network -> Click the 'add'
icon, just use the default configuration. Please do not change the setting of
`port forwarding`. Click OK.

The default NAT network is 10.0.2.0/24, Virtual Box will create 10.0.2.1 as
gateway. 

# Configure Adaptor for VM

Settings -> Network -> Attach to (Select NAT netowkr), Name (NatNetwork which
is the name configurared in the previous step).

# Configurare IP in VM

Below is for Ubuntu 18.04.

```
sudo vim /etc/netplan/50-cloud-init.yaml
```

Configure the adaptor:
```
# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    ethernets:
        enp0s3:
            dhcp4: no
            addresses: [10.0.2.150/24]
            gateway4: 10.0.2.1
            nameservers:
                addresses: [10.0.2.1]
```


