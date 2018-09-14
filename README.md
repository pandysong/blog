# What is this

This is not a project, but a list to record the interesting blogs/article/projects I have browsed

# X11 forwarding
## How X11 forwarding works (in High Level)

This blog explains clearly in high level how it works.

http://alexteichman.com/octo/blog/2014/01/01/x11-forwarding-and-terminal-multiplexers/

## tmux how to update DISPLAY env vars

if tmux attached to an existing session by using 

```bash
tmux attach-session -t your-session-name
```

The DISPLAY env var may still be the old value kept from when it was detached. 

Following blog provides a solution

https://raimue.blog/2013/01/30/tmux-update-environment/

It introduces a shell function:

```bash
function tmux() {
    local tmux=$(type -fp tmux)
    case "$1" in
        update-environment|update-env|env-update)
            local v
            while read v; do
                if [[ $v == -* ]]; then
                    unset ${v/#-/}
                else
                    # Add quotes around the argument
                    v=${v/=/=\"}
                    v=${v/%/\"}
                    eval export $v
                fi
            done < <(tmux show-environment)
            ;;
        *)
            $tmux "$@"
            ;;
    esac
}
```

Save it to a file ~/.whatever_name_you_want.sh, and source it from ~/.bashrc.

Whenever you need to use X11 forard and found it does not work, try the following command to update the environment vars

```bash
tmux update-env
```
# Python Project List

https://github.com/checkcheckzz/python-github-projects

https://awesome-python.com/

# Vim Plugin for Python Code Folding

https://github.com/tmhedberg/SimpylFold


# cross-entropy

When reading tensorflow document, it reminds that this post could explain cross-entropy well.

http://colah.github.io/posts/2015-09-Visual-Information/

> So, why should you care about cross-entropy? Well, cross-entropy gives us a way to express how different two probability distributions are.

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

# golang goroutine

goroutine has advantages on correntcy which could make the application flow streamlized. But people may forget another important factor that goroutine is stateful. So if you are make a statemachine or something similar, it might be code of smell, probably it could be optimized off.

# where to define golang interface

https://github.com/golang/go/wiki/CodeReviewComments#interfaces

```Go interfaces generally belong in the package that uses values of the interface type, not the package that implements those values. The implementing package should return concrete (usually pointer or struct) types: that way, new methods can be added to implementations without requiring extensive refactoring.```


                    
# Vim: copy the internal command output to buffer

This is the workaround:
```bash
:redir @+>
:set guifont?
:redir END
```
Now in the reigster "+, it is the output of set guifont?.
we can not using "+p to paste the register content to buffer.

## Excellent Lint Plugin

It works out of box as long as the lint tool is available in the system.
https://github.com/w0rp/ale

## Code Search Plugin using Ag
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

## slime.vim

The perfact and elegant tool to do repl in tmux (it also support other 

https://github.com/jpalardy/vim-slime

# https proxy server

#https server

using https://github.com/qwj/python-proxy

On windows:
>>> pproxy.exe -i http+socks://:8123

On Client 

$ pip install --proxy https://serveraddress:8123 --user flake8


It is also perfect to send the data to another tmux session 

```
"h:i.j" means the tmux session where h is the session identifier
        (either session name or number), the ith window and the jth pane
```

## Query Manual 

put the cursor on a function and press Shift K will show the manual for the function.
