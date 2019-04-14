---
date: 2017-04-12
title: Https Proxy Server in Python
weight: 10
---

# https proxy server

Sometime you may need a simple proxy server to workaround the network
limitation.

## https server

using https://github.com/qwj/python-proxy

On windows:
>>> pproxy.exe -i http+socks://:8123

On Client 

$ pip install --proxy https://serveraddress:8123 --user flake8
