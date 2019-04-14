---
date: 2018-04-28
title: Install OpenCV Python in Macos
weight: 10
---

# Install OpenCV Python in Macos

There are several tutorial on the internet on how to insall Python OpenCV support.

Here is the simplifiled guide (for Python 3)

- brew install opencv
- go to the virtual environment directory
- cd lib/python3.6/site-packages/
- ln -s /usr/local/opt/opencv/lib/python3.6/site-packages/cv2.cpython-36m-darwin.so cv2.so

Done, and you could test in python 
```python
>>> import cv2
>>> print(cv2.__version__)
3.4.0
```

