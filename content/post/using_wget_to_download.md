---
date: 2019-07-26
title: using wget to download all files
weight: 10
---

# how to

```
 wget -r -l1 -A.pdf -A.DSN  target_web.com/target_url
```

`-r` means `recursive`
`-l1` means recursive depth is 1, '-l0' means indefinitely

