---
date: 2019-04-28
title: vim's `sign` feature
weight: 10
---

## An example

Following define a sign with name `piet`:

```
sign define piet text=. linehl=Search texthl=Search
```

Note that `linehl=<groupname>` is used to highlight the whole line. We may
define a new group using following command

```
:highlight <groupname> ctermbg=darkred guibg=darkred
```

Then using the command `:sign place` to put a sign 

```
:exe ":sign place 2 line=104 name=piet file=" . expand("%:p")
```

![linehl](/img/sign_linehl.png)

## Conclusion

It is possible to use this feature in the
[plugin](https://github.com/pandysong/highlight.vim). However I am not sure
how I could use the feature without the sign character in the beginning of a
line, which is a bit annoying.
