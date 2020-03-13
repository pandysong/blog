---
date: 2019-10-05
title: ^? shows when pressing "delete" key in Mac
weight: 10
---

Instead of performing "del" operation, it shows the `^?` characters. The
solution is to put following line in .bash_profile.


```
stty erase ^H
```

where to input ^H, you will type ctrl+v and then 'delete'.
