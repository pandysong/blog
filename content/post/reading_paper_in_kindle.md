---
date: 2019-07-11
title: Reading Paper in Kindle (converting multi-colums pdf to single column pdf)
weight: 10
---

# The Tool

The tool could be found [here](http://www.willus.com/k2pdfopt)


Download it and put it in a path

# How to

For the text based pdf file

```bash
k2pdfopt <put_pdf_file_path_here> -ui- -as -om 0.1,0.1,0.1,0.1
```

To play around the command line options in k2pdfopt, simple type `k2pdfopt`
