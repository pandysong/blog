---
date: 2020-01-12
title: Vim tips quickfix
weight: 10
---

# creat text recognized by quickfix


using following command in vim to copy the text to clipboard:

    let @@=expand("%:p").":".line(".").": ".expand("<cword>") 

something like following will be yanked:

    `/home/pandy/notes.md:24: finit_module`

# load the files to quick fix window

You may want to save a list of lines like that to a file (for recording the key
points of the program logic).

Open the file and then load the current file to quickfix window:
    :cfile %

then open the quickfix window:
    :copen

Now you could browse in the quickfix window and jump to the lines recorded.

# This works also for the compilation output

You may yank the output from any other sources and open the compilation log and
use ":cfile %" or directly use ":cfile <to_your_file_path>" to open it in
quickfix window.

Without quick fix window, one could use "gF" command to jump to the code or
document.
