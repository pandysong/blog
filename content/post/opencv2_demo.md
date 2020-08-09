---
date: 2019-04-24
title: OpenCV on OSX
weight: 10
---

# Introduction

This document describes how to build a simple opencv project in macos. It is
done all in command lines.

# install the minimum set of xcode

```
xcode-select --install
```

# install python3

This is needed to run opencv4.

```
brew install python3
brew switch python <the version you have installed>
```

test if python3 works

```
python3
```

# install opencv

```
brew install opencv@2
```

# pkg-config

check if $PKG_CONFIG is configured:

```
echo $PKG_CONFIG
```

If not set it

```
export PKG_CONFIG=/usr/local/lib/pkgconfig/
```

This is a tool to setup the path parameters when using command line to build.

# setup pkg-config for opencv@2

```
ln -s /usr/local/opt/opencv@2/lib/pkgconfig/opencv.pc $PKG_CONFIG
```


Test to see if pkg-config return parameters for opencv:

```
pkg-config --cflags --libs opencv
```


# compile the project

```
g++ $(pkg-config --cflags --libs opencv)  demo.cpp
```

# `ld: symbol(s) not found for architecture x86_64`

``` 
There are two implementations of the standard C++ library available on OS X:
libstdc++ and libc++. They are not binary compatible and libMLi3 requires
libstdc++.  On 10.8 and earlier libstdc++ is chosen by default, on 10.9 libc++
is chosen by default. To ensure compatibility with libMLi3, we need to choose
libstdc++ manually.  To do this, add -stdlib=libstdc++ to the linking command.
```
