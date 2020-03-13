---
date: 2019-12-28
title: Font cannot be found when using pandoc to covert to pdf
weight: 10
---

# Error 

When running following command to convert a markdown to pdf:

```
pandoc  xyz.md -o xyz.pdf --pdf-engine=xelatex --template=./xyz_template.tex
```

I get following error.

```
kpathsea:make_tex: Invalid filename `文泉驿微米黑/OT', contains '
kpathsea:make_tex: Invalid filename `文泉驿微米黑/OT', contains '
Error producing PDF.
!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
!
! fontspec error: "font-not-found"
!
! The font "文泉驿微米黑" cannot be found.
!
! See the fontspec documentation for further information.
!
! For immediate help type H <return>.
!...............................................

l.21   \setCJKmainfont{文泉驿微米黑}
```

I am using following command  in xyz_template.tex

```
    \setCJKmainfont{文泉驿微米黑}  
```

# Debugging

Using command fc-list to check if the font does exists

```
~/(master ✗) fc-list | grep 泉
/Users/pandysong/Library/Fonts/WenQuanWeiMiHei-1.ttf: 文泉驿微米黑,文泉驛微米黑,WenQuanYi Micro Hei:style=Regular
```

# Solution

Replace with following english font name resolve the problem:

```
    \setCJKmainfont{WenQuanYi Micro Hei}  
```
