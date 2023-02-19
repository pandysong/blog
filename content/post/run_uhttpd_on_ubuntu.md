---
date: 2023-02-04
title: openwrt uhttpd - run it on ubuntu
weight: 10
---

		
# run uhttpd on ubuntu

For development purpose or for debuging purpose, it might be interesting to run
it on ubuntu or any other PC linux environments.


# prerequisite

# install lua5.1

```
sudo apt install lua5.1
sudo apt install lua5.1-0-dev
```
# libubox

For generic information, refer to https://openwrt.org/docs/techref/libubox

It provides basic infrastructures for higher level application: 

Clone the following code and build it:
https://git.openwrt.org/project/libubox.git

```
cd libubox
mkdir build
cd build
cmake ../
make
sudo make install
```

# ubus

https://git.openwrt.org/project/ubus.git

similar to libubox:

```
mkdir build
cd build
cmake ../
make
sudo make install
```

# openssl-1.1.1o

```
./config
make
sudo make intall
```

# ustream-ssl


https://git.openwrt.org/project/ustream-ssl.git


```
mkdir build
cd build
cmake ../
make
sudo make install
```


# build uhttpd

Turn off ucode for simplicity:

```
mkdir build
cd build
cmake ../ -DUCODE_SUPPORT=off
```

# run the uhttpd

```
./uhttpd -c ../../httpd.conf  -p 0.0.0.0:8090 -h ../../www/
```

Now in the same mach, a http request could be issue

```
wget 127.0.0.1:8090
```

This will return the index.html in ../../www/ folder.

# lua support in details

each server has one lua_prefix, with this url prefix, it will be handled by the
corresponding lua_handler:

```
uhttpd/main.c:500: add_lua_prefix
```

Note that in main.c it allows only one handler, but in the supporting function,
it allows multiple handlers with multiple url as a list of lua prefix are built
in the linked-list conf.lua_prefix.

During uhttpd bootup, for each prefix, `uh_lua_state_init` is called, to
initialize the running environments:

In each environment, the C function is added to the running-time of lua:

```
 lua_pushcfunction(L, uh_lua_send);
 lua_setfield(L, -2, "send");
```

above function add the lh_lua_send c code to lua environment and named it as
"send" which will be called in lua script. Similar to "uh_lua_recv",
"uh_lua_urldecode", "urlencode", and global string "docroot"
