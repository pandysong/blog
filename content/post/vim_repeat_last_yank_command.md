---
date: 2019-10-05
title: how to append all matches to register
weight: 10
---

# how to

```
set cpoptions+=y
```

So for example if we type "Ayy, to append the current line to the register a,
if we now go to next line which we want to append, you could just type dot
command.

# how to append all matches to register

```
:g/some pattern here/yank A
```

yank to register a, but since it is A, so it means append to register A.
