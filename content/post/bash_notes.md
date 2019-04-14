---
date: 2014-09-28
title: Bash notes
weight: 10
---


- Commands

    - set -o vi to set the vi editing mode

    - clear screen
        ESC to enter vi editing mode
        CTRL-L
    - put # at the begning of line to put that line to history file

    - CTRL-U to erase whole line

    - CTRL-S to stop sending the typing characters and buffer them, CTRL-Q to resume and send the
      buffered keys

    - CTRL-D to terminate the inputs

    - !! to repeat the last command

    - source scriptname
      this cause the commands in the script to be read and run as if you typed them in

- script
    - $*  parameters in one string
    - $@  parameters in seperate double quoted string
    - $#  number of parameters
    - $? the exit status of last command that ran

    - must use ${10} for the tenth of of positional parameter

- string operators

    - ${varname:-word}
        if varname exists and isn't null, return its value; othewise return word.

    - ${varname:=word}
        if varname exists and isn't null, return its value; othewise return set it to word and
        return word.

    - ${varname:+word}
        if varname exists and isn't null, return word; othewise return null
        return word.

    - ${varname:?message}
        if varname exists and isn't null, return word; othewise print message and abort current
        command or script

    -${variable#pattern}
        if the pattern matches the begining of the variable's value, detel the shortest part that
        matches and returne the reset     

    -${variable##pattern}
        if the pattern matches the begining of the variable's value, detel the longest part that
        matches and returne the reset     
