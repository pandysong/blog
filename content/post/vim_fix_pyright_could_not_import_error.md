---
date: 2022-04-22
title: 
weight: 10
---

# Error: "Import xxxx could not be resolved"

"[Pyright: reportMissingImports]"

This might be due to that the Coc is using different python version than the
one you expect to use in your project.

# Fix

Type ":CocConfig" in vim and add following line to the config

"python.pythonPath": "/usr/local/opt/python@3.8/bin/python3"

Where python3 is the one I got from 

"which python3"

Since I am using python3.8 in my project.

Save and exit and restart vim
