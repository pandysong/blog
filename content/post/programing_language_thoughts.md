---
date: 2016-03-13
title: Some thoughts about a programming language 
weight: 10
---

# Programming language 

## Reflect

(On reading golang reflect)
Reflect is not good, I feel like it tries to climb over a wall which was built by golang own. The best is not to build the wall at all.

## Type safety

(On reading some c code enfocing type safety)

Type safety is good to resolve part of the programing problems
(which some people think is critical to have not to introducing a subtle bugs which is difficut to debug,
which I do not think), but it ignores that we have a lot more other problems which could introduce bugs,
"type safe" loses great flexibility to resolve those other problems.

Type safety is not bad as long as it does not become an execuse to ignore other important issues. 

Python is a good justification.

## goroutine (golang) 

Similarly in python there is `coroutine`

# Linux 

## Process, Process Group and Process Session

https://www.usna.edu/Users/cs/aviv/classes/ic221/s16/lec/17/lec.html

## `top` command filter by process name

After running `top -c` , hit o and write a filter on a column, e.g. to show rows where COMMAND column contains the string foo, write COMMAND=foo
