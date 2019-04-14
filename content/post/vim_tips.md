---
date: 2017-04-12
title: Vim tips
weight: 10
---

# using macro and normal mode command

https://medium.com/a-tiny-piece-of-vim/perform-vim-normal-mode-moves-using-the-command-line-730d21d9d19

# view log in vim

One may consider to use plugin "vim-scripts/LogViewer"

# V vs v

V to select whole line

# copy the internal command output to buffer

This is the workaround:
```bash
:redir @+>
:set guifont?
:redir END
```
Now in the reigster "+, it is the output of set guifont?.
we can not using "+p to paste the register content to buffer.


# Excellent Lint Plugin

It works out of box as long as the lint tool is available in the system.
https://github.com/w0rp/ale

config in vim so that we know where the error comes from, as there might be multiple linter in system:
```
let g:ale_echo_msg_format = '[%linter%] %s [%severity%]'
```


# Code Search Plugin using Ag
Inspired by:
https://gorillalogic.com/blog/faster-grepping-vim/

Install Ag first:

Then add following line into .vimrc
```
Plugin 'mileszs/ack.vim'

if executable('ag')
    let g:ackprg = 'ag --vimgrep'
endif

```

Search in all c code:
```
Ack --cc SomeVar
```

Search in all go code:
```
Ack --go SomeVar
```

# slime.vim

The perfact and elegant tool to do repl in tmux (it also support other 

https://github.com/jpalardy/vim-slime

It is also perfect to send the data to another tmux session 
```
"h:i.j" means the tmux session where h is the session identifier
        (either session name or number), the ith window and the jth pane
```

# Query Manual 

put the cursor on a function and press Shift K will show the manual for the function.


