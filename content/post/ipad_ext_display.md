---
date: 2022-04-10
title: Use iPad as External Display via Tmux
weight: 10
---

# iPad

- Install `shelly`

# Mac

- create a tmux session prefixed with "ext_display"
- then in this tmux session set options to allow the maximum window size
  so resize the tmux windows on mac will not change the size of windows on iPad

```
tmux set-options -g window-size largest
```

change the session name to specific name:

```
tmux rename-session "ext_display"
```

resize the iterm window to smallest.

# convert to Application

# on iPad

## manually

- ssh to my Mac
- then tmux attach to ext_display

```
tmux attach -t ext_display
```

## Automating

- on shelly, configurate to run the specifc script
- in this script, detect if the tmux session name is prefixed with "ext_display": if yes, attach to it

- in shelly, enable "commands" after login, enter the script using absolute
  path:

```
source /Users/pandysong/.enable_ext_display.sh
```

Note that in .enable_ext_display.sh, the file path should be absolute path
instead of relative path
