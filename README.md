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
