---
date: 2017-04-12
title: Understanding `yield` in Python
weight: 10
---

# understanding yield in python

```python
def gen():
    s = "init string"
    while True:
        s = yield s
        s += " processed"

c= gen()
print(next(c))
print(format(c.send("hello")))
```

The above has following output

```bash
init string
hello processed
```

when next(c) is called, the gen function runs until line :

```python
s = yield s
```

However since it does not send anything (using next()), 
the gen() function only runs the right part of the statement:

```python
yield s
```

Now when c.send("hello") is called, it send "hello" to gen(), 
and gen() starts to run and s get assiged so at the same line 

```
s = yield s
```

s is now "hello", but c.send("hello") will need gen() to run until the next yield.
In this case, gen() continue runs:

```python
s += "processed"
```

s now becomes "hello processed"

it continues to run until get the next statement which has yield:

```python
s = yield s
```

Now the right part of statement is executed, and and c.send("hello") returns the "hello processed".

# <a name="setup_requirement"></a> setup.py vs requirements.txt
Very good and clear document:
https://caremad.io/posts/2013/07/setup-vs-requirement/
