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
