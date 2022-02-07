---
date: 2021-12-14
title: Reserve the Carriage Return in the Text File
weight: 10
---

## Introduction

I need to process a text from Windows which has lines terminated with Carriage
return and a New line, which is "\r\n", while in Python, it treat all the new
line conditions, for example, for the following code:

```
        with open(fn, 'r') as f:
            for line in f.readlines():
                pass # handle line here
```

The line is always terminated with "\n" (at least in Linux and Mac) no matter
if the line in the file is terminated with "\r\n" or "\r".

This creates some problem for me as I want to search through lines and replace
some characters, but I do not want to change the termination of the line, since
the file handled needs to be put on git, and I do not want to see all lines are
changed and I want to reserve the original format as much as possible.


# Solution

```
        with open(fn, 'r', newline='\n') as f:
            for line in f.readlines():
                pass #handle the line here
```

Now the line in above code contains the '\r' as well, since the '\r' is not
treated as part of the line termination.

The problem if above code is that it is not general enough, e.g. if line is
terminated with '\r', it could not handle, which is not my case.



