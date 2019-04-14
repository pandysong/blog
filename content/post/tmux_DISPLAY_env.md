---
date: 2015-03-28
title: Tmux how to update DISPLAY env vars
weight: 10
---

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

