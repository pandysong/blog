---
date: 2014-03-28
title: Python Notes
weight: 10
---

# Key Points

This describes the key points when reading the book `programming in Python 3`.

## get help

use `help (function name)` in Python shell.

## Data Types

In Python, both `str` and the basic numeric types such as int are immutable. That is, once set,
their value cannot be changed.

## Convert Types

To convert a data item from one type to another, we can use the syntax `type (item)`. For example

```bash
>>> int("45")
45
```

## Object Reference

Python does not have variables as such, but instead has object references.

## `tuple` vs list

tuple is immutable while list is mutable.

## tuple

tuple:

>>> "Denmark" , "Finland"
('Denmark', 'Findland')

If we have a one-item tuple and want to use parentheses, we must still use the comma for example (1,)

## len()

Tuples, lists and strings are "sized"

## logical operations

### The identity operator `is`

`is` `is not` is to check if the reference are for same address.

The most common use case for `is` is to compare a data item with the build-in null object, `None`.

>>> a = "Something"
>>> b = None
>>> a is not None, b is None
(True, True)

### Comparison Operators

One particularly nice feature of Python's comparison operator is that they can be chained. For
example:

>>> a=9
>>> 0 <= a <= 10
True

### membership operator

For strings, lists and tuples, we can test for membership using the `in` operator and for
non-membership, using the `not in` operator. For example:

>>> p = (4, "frog", 0,-33,9,2)
>>> 2 in p
True


### Logical Operators

`and` `or` `not`

#### `and` `or` return the operand that determined the result unless it is in a Boolean context
>>> five=5
>>> two=2
>>> five and two
2
>>> two and five
5
>>> zero =0
>>> five and zero and two
0

It means it returns the last operand on which it could determine the result

#### `not` always return a Boolean result

## Control Flow Statements

### `suite`

A sequence of one or more statements is called suite. Keyword `pass` is a statement that does nothing
and can be used where a suite is required.


### `if` `elif` `else`

```
if boolean_expression1:
    suite1
elif boolean_expression2:
    suite2
...
else:
    else_suite
```

colon`:` is part of syntax and be used where a suite is to follow.

### `while`

```
while boolean_expression:
    suite
```

### `for`

```
for variable in iterable:
    suite
```

### basic exception handling

```
try:
    try_suite
except exception1 as variable1:
    exception_suite1
except exception2 as variable2:
    exception_suite2
```

## Arithmetic Operators

### `/` division operator produces a floating point value

### `//` truncating division operator

### `a += 8` is fast than `a = a + 8`

Since int is immutable, what happens is that operation is performed and an object holding the result
is created, and then the target object reference is re-bound to refer to the result object.

### '+=' and '+' are overloaded for both strings and list

Since strings are immutable, '+=' would mean create a new string and re-bound the reference to the
new created object.

list is mutable, hence `+=` does not involve rebinding.

### Truth Value Testing

Any object can be tested for truth value, for use in an `if` or `while` condition or as operand of the
Boolean operations below.

By default, an object is considered true unless its class defines either a __bool__() method that
returns False or a __len__() method that returns zero, when called with the object. [1] Here are
most of the built-in objects considered false:

    constants defined to be false: None and False.
    zero of any numeric type: 0, 0.0, 0j, Decimal(0), Fraction(0, 1)
    empty sequences and collections: '', (), [], {}, set(), range(0)

Operations and built-in functions that have a Boolean result always return 0 or False for false and
1 or True for true, unless otherwise stated. (Important exception: the Boolean operations or and
always return one of their operands.)

### right-hand operator for the list += operator must be an iterable

>>> seeds
['sesame', 'sunflower', 5]
>>> seeds += "this is tets"
['sesame', 'sunflower', 5, 't', 'h', 'i', 's', ' ', 'i', 's', ' ', 't', 'e', 't', 's']

The correct way to append a string as an item

>>> seeds += ["this is tets"]

## functions: def is a statement that works in a similar way to the assignment operator

```
def functionName(arguments):
    suite
```

When def is executed a function object is created and an object reference with the specified name
is created and set to the function object. Since functions are objects, they can be stored
in collection data types and passed as arguments to other functions.

### function return value

the return value is by default `None`.

## Indent

Python statements normally occupy a single line, but they can span multiple lines if they are a
parenthesized expression, a list, set, or dictionary literal, a function call argument list, or a
multi line statement, where every end-of-line character except the last is escaped by preceding it
with a backslash (\). In all these cases any number of lines can be spanned and indentation does not
matter for the second and subsequent lines.

## Exception Seen often

- TypeError
- IndexError
- ValueError


# Data Types

## chinese, greek characters are allowed for identifier

## the size of integer is limited only by the machine's memory.

## `hex` `bin` `oct` to convert number to string

## `int(s, base) to convert string to number

## int.bit_length() returns the number of bits required

## True and False are two buit-in Boolean objects

bool() will return False default value

One could use is False to test the built in objects, but usually we use it in boolean context with or without `not`

## Three Floating Point Types: float (is actually in double), ints (with scale), decimal.Decimal

decimal.Decimal is much slower than the floats, but because of their accuracy, decimal.Decimal numbers are suitable for financial calculations.

decimial.Decimals are of fixed precision they can be used only with the other decimal.Decimals and with ints.
## float.hex(), float.fromhex() convert to and from hex string

```python
>>> 3.25.hex()
'0x1.a000000000000p+1'
```

# str data type `str`

## triple quoted string """some string""" '''some string''' could include new line

escaping the new line using `\`

```python
>>> print("""line one
... line two\
... linet three""")
line one
line twolinet three
```

## raw string

quoted or triple quoted strings whose first quote is preceded by the letter `r`

```python
>>> phone2 = re.comple(r"~((?:[(]\d+[)]?\s*\d+(?:-\d+)?$")
```

## c ctyle long string, must use parentheses

```python
>>> s= ("Line one"
"line one continue")
```

## negative index seq[-1] alwasy gives us the last character
## string slice operation

seq[start]
seq[start:end]
seq[start:end:step]

## What is the convenient way to revert the string?

striding is most often used with sequence types other than strings, but there is one context in which it is used for strings:

seq[::-1]

there is a built-in `reversed()` function

But seq[::-1] is more concise

## `*` and `*=` is overloaded fro `str`, mean replication
## split a string to lines on line terminators, strip the terminators unless f is true

```
s.splitlines(f)
```

## s.partition() and `s.rpartition()` return three strings

## can we assume that a string can be converted to an integer when `isdigit()` returns `Ture`?

No, because as is###() method works on Unicode character.

```python
>>> "\N{circled digit two}".isdigit()
True
```

## strip() function could strip any leading or trailing character

```python
>>> "{[(this is a string needed}}}))".strip("{}[]()")
```
## `str.format()`

Read page 78

### using locals() 

This built-in function to return a dictionary whose keys are local var names and values the local var values

### mapping unpacking operator "**"

**locals() does following:

`locals()` return a dictionary, "**" operator applies to a mapping (such as a dictionary) to produce a
key-value list which is `suitable for passing to a function`

Note that if we want to pass more than one argument to `str.format()`, only the last one can use the mapping unpacking.

### representational form vs string form

```
The replacement field:
{field_name}
{field_name!conversion}
{field_name!format_specification}
{field_name!conversion:format_specification}
```

Conversion: 's' to force string form, 'r' to force representation form, 'a' to force representational form but only using ASCII

### the powerful of str.format is that the format_specifier could use variable

```python
>>> s="the sword of truth"
>>>"{0:.{1}}".format(s,12)
'the sword of'
```

Refer to Page 84 for more details

## UnicodeEncodeError exception

## UTF-8 is default for XML and many web pages. Java uses UTF-16 and Python represents Unicode using either UTF-16 or UTF-32

## .py files uses UTF-8

## The complement of str.encode() is bytes.decode()

## Pythonic way to find the first element on a list using `next`

Find the index of first element with condition on element
next((i for i, x in enumerate(lst) if [condition on x]), [default value])

Help on built-in function next in module builtin:

```
next(iterator[, default])
```

Return the next item from the iterator. If default is given and the iterator
is exhausted, it is returned instead of raising StopIteration.


# Collection Data Types

## Sequence Types Definition

A `sequence` type is one that supports
- member ship operation (in)
- the size function ( len())
- and is iterable


Five built-in sequence types:

- bytearray
- bytes
- list
- str
- tuple

# Tuples

## parentheses 

### not needed on the left hand side of a binary operator

```
a, b = (1,2)

for x,y in ....:
	print(x,y)
```

### not needed on the right handle side of a unary statement

```
del a,b 

def f(x):
	return x, x ** 2
```

## swap value using unpacking 

When we have sequence on the right-hand side of an assignment and we have a
tuple on the left-hand side, we say that the right-hand side has been upacked.

we could use this to swap two values

a, b = (b, a)

on the right hand side parentheses are not needed strictly speaking


## Named Tuples

```python
>>> import collections
>>> Sale = collections.namedtuple("Sale", "Product customerid data quantity price")
>>>
>>>
>>> sales=[]
>>> sales.append(Sale(432, 921, "2008-9-12", 3, 7.99))
>>> sales
[Sale(Product=432, customerid=921, data='2008-9-12', quantity=3, price=7.99)]
```

# Lists

## sequence unpacking for any type of sequence

```python
>>> first, *center, last = [1,2,3,4,5]
>>> first, center, last
(1, [2, 3, 4], 5)
>>> *directory, executable = "/usr/local/bin/gvim".split("/")
>>> directory, executable
(['', 'usr', 'local', 'bin'], 'gvim')


here star (*) is the unpacking operator, the expression * reset is called `starred expressions`
```

## sequence unpacking for function parameters

```python
>>> prd(1,2,3)
6
>>> prd(*(1,2,3))
6
>>> L=2,3,4
>>> prd(*L)
24
```

## range(n) return an object which is iterable and produce 0,1,...,n-1

Note that range() returns an object it is iterable which is not iterator

Refer to 

https://stackoverflow.com/questions/21803830/why-is-the-range-object-not-an-iterator

## iterable vs iterator

An `iterable` is an object which has __iter()__ function and could return an iterator.
While an iterator is a stateful object which knows how to get the next referenes in the object.

range() is an iterable but not an iterator. to have an iterator need to convert to iterable using
iter() 

```python
>>> i=iter(range(5))
>>> next(i)
0
>>> next(i)
1
```

This is the help of list(), it accepts `iterable` (note that iterorator is always iterable)
class list(object)
 |  list() -> new empty list
 |  list(iterable) -> new list initialized from iterable's items

Which means list() accepts both iterable and iterator

```python
>>> i=range(5)
>>> list(i)
[0, 1, 2, 3, 4]
>>> list(iter(i))
[0, 1, 2, 3, 4]
```
 

## insert items using slicing operator

```python
>>> a=[1,2,3,4]
>>> a[1:1]=[5,6]
>>> a
[1, 5, 6, 2, 3, 4]
>>> a[1:3]=[7,8,9]
>>> a
[1, 7, 8, 9, 2, 3, 4]
```

In above case, it delete two items and insert three items

## list.pop(index) to remove and return the value in index

```python
>>> a
[1, 7, 8, 9, 2, 3, 4]
>>> a.pop(2)
8
>>> a
[1, 7, 9, 2, 3, 4]
```
## sorted() is built-in function like reversed()


```python
>>> a=[4,2,1,3]
>>> for n in sorted(a):
...     print(n)
...
1
2
3
4
```

## list comprehensions

[expression for item in iterable if condition]
[expression for item in iterable for item2 in iterable2 ... if condition]

```python
>>> [ i + j for i in range(3) for j in range(4) if i !=j]
[1, 2, 3, 1, 3, 4, 2, 3, 5]
>>> [ i + j for i in range(3) for j in range(4) if i ==j]
[0, 2, 4]
>>> [ i + j for i in range(3) for j in range(4) if i > j]
[1, 2, 3]
```

# Set 

## immutable data types are hash-able since their values cannot be changed so could be added to a set

So float, frozenset, int, str and tuple could be added to a set

## mutable data type cannot be added to a set

## empty set must be created using set(), but not {} which is used to create empty dictionary

## set operator &, |, - (differences), ^ (Symmetric Difference)


```python
>>> set("pecan") | set ("pie")
{'a', 'p', 'i', 'e', 'n', 'c'}
>>> set("pecan") & set ("pie")
{'e', 'p'}
>>> set("pecan") - set ("pie")
{'a', 'n', 'c'}
>>> set("pecan") ^ set ("pie")
{'a', 'n', 'i', 'c'}
```

## common use case for set: fast membership testing

```python
if len(sys.argv) == 1 or sys.argv[1] in {"-h", ""}
```

## Set Comprehensions

```python
>>> a={1,2,4,8}
>>> {x%3 for x in a}
{1, 2}
```

# frozenset created by using frozenset()


# Mapping Types

- dict()
- defaultdict()
- collections.OrderedDict()  (introduced in Python 3.1)

dictionary's key must be hash-able


## construct dict using different method

### using sequence type like tuple and list


```python
>>> dict((("id",1948),))
{'id': 1948}
>>> dict([("id",1948),])
{'id': 1948}
```

### using function keyword parameters

```python
>>> dict(id=1948)
{'id': 1948}
>>> dict(id=1948,name="Washer")
{'id': 1948, 'name': 'Washer'}
```

### using dictionary liberal


```python
>>> {"id":1948,"name":"Washer"}
{'id': 1948, 'name': 'Washer'}
```

### using zip() function


zip() function takes one more more iteratables and returns an interator that returns tuple.

```python
>>> dict(zip(("id","name"),(1982,"Washer")))
{'id': 1982, 'name': 'Washer'}
```

## add items to dictionary

```python
x["X"] =59
```

## delete items from dictionary

```python
del x["X"]
```

Exception raised if no item exists for key "X"

## dict.items() dict.keys() and dict.values() methods all return `dictionary views`

A dictionary view is effectively a read-only `iterable` object that appears to hold the dictionary's
items or keys or values

```python
>>> d=dict(zip(("id","name"),(1982,"Washer")))
>>> { (key+"h", value) for key,value in d.items() if key.startswith("i")  }
{('idh', 1982)}
```

## file object is an iterator

So if we would like to process the text line by line:

```python
for line in open(filename):
    process(line)
```

## Dictionary Comprehensions

{keyexpression: valueexpression for key, value in iterable}
{keyexpression: valueexpression for key, value in iterable if condition}

### get the file sizes in the current directory 

```python
>>> file_sizes={name: os.path.getsize(name) for name in os.listdir(".") if os.path.isfile(name)}
```

## Default Dictionary vs Dictionary

Default Dictionary creates a default value using `factory function` if a nonexistent key is used to
access the dictionary

### factory functions

All of Python's built-in data types can be used as factory functions for example data type `str`


```python
>>> import collections
>>> words = collections.defaultdict(int)
>>> words["abc"]
0
>>> words["abc"] += 1
>>> words["abc"]
1
>>> words["abcd"] += 1
>>> words["abcd"]
1
```


# Iterating and Copying Collections

## iterators


### all(), any(), len(), min(), max()

```python
>>> x=[1,2,3,-1]
>>> all(x), any(x), len(x), min(x), max(x), sum(x)
(True, True, 4, -1, 3, 5)
>>> x += [0]
>>> all(x), any(x), len(x), min(x), max(x), sum(x)
(False, True, 5, -1, 3, 5)
```

### list() tuple() accepts iterators

#### create a list or tuple of successive integers

```python
>>> list(range(5)), list(range(9,14)), tuple(range(10,-11,-5))
([0, 1, 2, 3, 4], [9, 10, 11, 12, 13], (10, 5, 0, -5, -10))
```

### an `iterable` can be unpacked using * operator


```python
>>> def calculate(a,b,c,d):
...     return a*b*c*d
>>> t=(1,2,3,4)
>>> calculate(*t)
24
>>> calculate(*range(1,5))
24
>>> calculate(*range(1,8,2))
105
```

### random.choice() vs random.sample()

random.sample() function will product up to the specified number of items from the iterable it is
given with no repeats

### reversed() returns an iterator VS sorted() returns a list

### sorted(x, key=abs) 

When a key function is passed it is called once for every item in the list to create a decorated
list, then the decorated list is sorted and the sorted list without the decoration is returned as
result

```python
>>> a=[1,2,-1,-2,-3]
>>> sorted(a)
[-3, -2, -1, 1, 2]
>>> sorted(a,key=abs)
[1, -1, 2, -2, -3]
>>> x=["Sloop","Yawl","Cutter", "schooner", "ketch"]
>>> sorted(x, key=str.lower)
['Cutter', 'ketch', 'schooner', 'Sloop', 'Yawl']
```

The key function could also used to convert the type 

```python
>>> sorted(["1.3",-7.5,"5",4])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: '<' not supported between instances of 'float' and 'str'
>>> sorted(["1.3",-7.5,"5",4], key=float)
[-7.5, '1.3', 4, '5']
```
'

## copy (shallow copy) and deepcopy

```python
>>> a=[1,2,3]
>>> b=a
>>> a+=[4]
>>> b
[1, 2, 3, 4]
```
Using slice to do shallow copy
```python
>>> a=[1,2,3]
>>> b=a[:]
>>> a+=[4]
>>> a
[1, 2, 3, 4]
>>> b
[1, 2, 3]
```


using copy.deepcopy() to do deep copy

```python
>>> import copy
>>> x=[53,66,["a","b","c"]]
>>> y=copy.deepcopy(x)
>>> y[1]=40
>>> x[2][0]="q"
>>> x,y
([53, 66, ['q', 'b', 'c']], [53, 40, ['a', 'b', 'c']])
```

## dict shallow copy

using dict.copy()

additionally any copyable object can be copied using functions from the copy module

using copy.copy performing shallow copy and copy.deepcopy() performing deep copy.


# Control Structures and Loops


## expression-1 if boolean_expression else expression-2

```python
>>> count=0;
>>> print("{0} file{1}".format( count if count !=0 else "no", "s" if count !=1 else ""))
no files
>>> count=1
>>> print("{0} file{1}".format( count if count !=0 else "no", "s" if count !=1 else ""))
1 file
>>> count=2
>>> print("{0} file{1}".format( count if count !=0 else "no", "s" if count !=1 else ""))
2 files
```


## while Loops

```
while boolean_expression:
    while_suite
else:
    else_suite
```


`else_suite` is not executed if loop is `break` or exception raised or a `return` is called.


## for Loops

```
for expresss in iterable:
    for_suite
else:
    else_suite
```



# Exception

## Raising Exception

raise exception(args)
raise exception(args) from original_exception
raise

when `raise` is used, it will reraise the currently active exception and if there isn't one it will
raise a TypeError

## Custom Exceptions

### how to define 

```python
class exceptionName(baseException): pass
```

### Useful case for custom exception, break out of deeply nested loops

```python

class FoundException(Exception): pass

try:
    for .......:
        for .......:
            for ......:
                if item == target:
                    raise FoundException
except FoundException:
    print("found")
else
    print("not found")
```
### Another useful case in page 169 to use exception to control the flow

In summary, we have two layer of try/except.

Inner exception handling need to capture the exception and do something and it will re-raise the
exception to outer exception handler (for changing the flow), the outer handler just `pass`. 

In this way, Exception could be utilized to control the flow flexibly.

# Custom Functions

- global functions
    global functions could be accessed from other modules

- local functions
    local functions ( also called nested functions) are functions that are defined inside other functions.

- lambda functions
    lambda functions are expressions so they can be created at their point of use

- methods
    Methods are functions that are associated with a particular data type

## function parameter default value

### define function with default value

```python
def some_function(param1, param2=string.ascii_letters)
    pass
```
### default value are assigned when function is defined in 'def'

## Names

### Naming Scheme

- UPPERCASE for constants 
- TitleCase for classes
- camelCase for GUI
- lowercase or lowercase_with_underscore for everything else

### the name should describe its meaning not the type (amount_due rather than money)

### the function name should say what they do or what they return but never how they do

```
def find(l, s, i=0):                #BAD
def linear_search(l, s, i=0)        #BAD
def first_idnex_of(sorted_name_list, name, start=0)       #GOOD
```

## Docstrings

A string comes immediately after the def line

```python
def shorten(text, length=25, indicator="..."):
    """Returns text or a truncated copy with the indicator added

    text is any string; length is the maximum lenght of the returned
    string (including any indicator); indicator is the string added at
    the end to indicate the text has been shorteded

    >>> shorten("Second Variety")
    `Second Variety`
    >>> shorten("Radio Free Albemuth", 10, "*")
    'Radio Fre*'
    """

    if len(text) > length:
        text = text[:lenght -len(indicator)] + indicator
    return text
```

## using unpacking operator in function parameter list

This is useful when we want to create functions that can take a variable number of positional
arguments.

```python
def product(*args)
    result = 1
    for arg in args:
        result *= arg
    return result
```

## if * is used in parameter, it does not allow the positional parameter after *

```python
def heron(a, b, c, *, units="square meters"):
    s = (a +b + c)/2
    area = math.sqrt( s * (s -a) * (s-b * (s-c)))
    return "{} {}".format(area, units)

heron2(25,24,7 "sq. inches")    #WRONG! raises TypeError
```

## mapping unpacking operator (**)

```python
options = dict(pater="A4", color=Ture)
print_setup(**options)
```

If options contains a key for which there is no corresponding parameter, a TypeError is raised.

## using (**) in function parameter allows many keyword arguments

This is similar like * in function which allows many positional arguments

We can of cause accept both a variable number of positional arguments and a variable number of
keyword arguments

```python
def print_args(*args, **kyargs):
    ...
```

## print()

Help on built-in function print in module builtin:

print(...)
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)

    Prints the values to a stream, or to sys.stdout by default.
    Optional keyword arguments:
    file:  a file-like object (stream); defaults to the current sys.stdout.
    sep:   string inserted between values, default a space.
    end:   string appended after the last value, default a newline.
    flush: whether to forcibly flush the stream.
(END)

so a way to print a list using unpacking
```python
>>> a=[1, 2, 3, 4]
>>> print(*a, sep="*")
1*2*3*4
>>> print(*a, sep=" -- ")
1 -- 2 -- 3 -- 4
```


# Lambda Fucntions Syntax

lambda parameters: expression

## used to sort()
lambda functions are often used as the key function for the built-in sorted() and list.sort() method

```python
>>> elements = [(1,2), (1,11), (2,3)]
>>> elements.sort(key=lambda x: x[0] * x[1])
>>> print(elements)
[(1, 2), (2, 3), (1, 11)]
```
### used on ``

minus_one_dict = collections.defaultdict(lambda: -1)

# Assertions

Syntax:

```
assert boolean_expression, optional_expression

assert are designed from developers, not end-users
```

#Misc

## Get datetime

```python
>>> import datetime
>>> datetime.date.today()
datetime.date(2018, 1, 4)
>>> datetime.date.today().year
2018
```


## using locals() to update dictionary vs to use in str.format()


using in str.format() is generally safe because only the keys named in the format string are used,
with any others harmlessly ignored, but this is not the case for updating the dictionary.

## using input() to prompt a message and get input from console

Help on built-in function input in module builtins:

input(prompt=None, /)
    Read a string from standard input.  The trailing newline is stripped.

    The prompt string, if given, is printed to standard output without a
    trailing newline before reading input.

    If the user hits EOF (*nix: Ctrl-D, Windows: Ctrl-Z+Return), raise EOFError.
    On *nix systems, readline is used if available.


## `global` variable is best to be used for constants


# Modules

## Modules and Packages

### Not all modules have associated .py file

For example, the `sys` module is built into Python, and some other modules are written in other
languages (most commonly, C)

### import syntax

```python
import importable
import importable1, importable2, ...., importableN
import importable as prefered_name
```


other syntax:

```python
from importable import object as prefered_name
from importable import object1, object2, ..., objectN
from importable import (object1, object2, .....,objectN)
from importable import *
```

These may cause name conflicts.

#### third syntax import everything that is not private

means import everything expect for those whose name begin with a leading underscore, or, if the
module has a global __all__ variable that holds a list of names, that all the objects named in the
__all__ variable are imported


### naming convention for custom module: using uppercase for the first letter

As all standard library module filenames are lowercase.

### python has no cycle import problem

## byte-code compiled code

.pyo ( optimized byte-code) -> .pyc (non-optimized byte-code) -> .py (to compiles a byte-code
compiled version)

When python is installed, the standard library modules are usually byte-code compiled as part of
installation process.

## Packages

A package is simply a directory that contains a set of module and a file called __init__.py

## doctest

```python
r"""This module provides several math functions

>>> myadd(1,2)
3
>>> myminus(3,2)
1
"""

def myadd(a,b):
    return a+b

def myminus(a,b):
    return a-b


if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

# Misc
## the way to create platform dependent function

```python
if sys.platform.startswith("win"):
    def clear_screen():
        ....
else:
    def clear_screen():
        ...
clear_screen.__doc__ = """Clears the scrren using the underlying window system's clear screen \
command"""
```
This could dynamically creates the function when the module is imported

# Python's Standard Library

## string handling
### `struct` module provides functions for packing and unpacking numbers, Booleans, strings

To and from bytes objects using their binary representations. This can be useful when handling data
to be sent to or received from low level libraries written in C.

### `string` modules 

### `difflib` could produce output both in standard `diff` formats and in HTML

### Python's most powerful string handling is the `re`

### `io.StringIO`

string-like object that behaves like in-memory text file. Convenient if we want to use the same code
that writes to a file to write to a string.


```python
>>> import io
>>> print("line one")
line one
>>> iostr = io.StringIO()
>>> sys.stdout = iostr
>>> print("line one")
>>> print("line two")
>>> sys.stdout = sys.__stdout__
>>> iostr.getvalue()
'line one\nline two\n'
```

## command-line programming `docopt`

## times and dates

Refer to page 216

## bisect module

It provides functions for searching sorted sequences such as sorted lists, and for iterating items
while preserving the sort order.

```python
>>> a=[1,2,3,4,5,6]  # a is sorted
>>> i = bisect.bisect_left(a,3) #find the place to insert
>>> a[i:i] = [3] # insert. the result is that a is still sorted
>>> a
[1, 2, 3, 3, 4, 5, 6]
```
## heapq module provides functions for turning a sequence such as a list into a heap

a heap is where the first item (at index position 0) is always the smallest item, and for inserting
and removing items while keeping the sequence as heap. Note that a heap does not guarantee that the
data is sorted, a (ascending) sorted data structure ensure that the first item is smallest item but
not vice verse.

It uses the same data struct of list
Heaps are list for which a[k] <= a[2*k+1] and a[k] <= a[2*k+2] for
    all k, counting elements from 0. >>

```python
>>> h=[]
>>> heapq.heappush(a,10)
>>> heapq.heappush(a,9)
>>> heapq.heappush(a,11)
>>> a
[1, 2, 3, 3, 4, 5, 6, 10, 9, 11]
>>> h=[]
>>> heapq.heappush(h,11)
>>> heapq.heappush(h,12)
>>> heapq.heappush(h,10)
>>> h
[10, 12, 11]
>>> heapq.heappop(h)
10
>>> h
[11, 12]
```


```python
>>> a=[1,3,2,4,6,8,0]
>>> heapq.heapify(a)
>>> a
[0, 3, 1, 4, 6, 8, 2]
```
## collections.deque vs list

deque is simiar to list. list is very fast for adding and removing items at the end, a
collection.deque is very fast for adding and removing items both at the beginning and at the end

## collections.Counter

```python
>>> import collections
>>> c= collections.Counter()
>>> c
Counter()
>>> c= collections.Counter("aabddaced")
>>> sorted(c)
['a', 'b', 'c', 'd', 'e']
>>> sorted(c.elements())
['a', 'a', 'a', 'b', 'c', 'd', 'd', 'd', 'e']
>>> c['a']
3
```
## array.array vs list

Array.array is similar to list except that the type of object which could be stored is fixed.

## weakref

If the only reference to an object is week reference, the object can still be scheduled fro garbage
collection.

This is to resolve cycling reference issue. For example, if a is has reference to b, because b is
under management of a, so if a is garbage collected, b will be garbage collected automatically.
But if for sake of convenience, b need to keep a reference to a so it could access some property of
its parent which is a. If both references are strong, both a and b will not be garbage collected.
In this case b should have week reference to a.

## standard library support Base64 by `base64`

Mostly used for handling binary data that is embedded in emails as ASCII text, it can also be used
to store binary inside .py files.

## standard library supports "quoted-printable" by `quopri`

## `bz2`, `gzip', 'zipfile', 'tarfile'

## audio file format: aifc, wave, sndhdr

## configparser

Similar to old sytle windows .ini files is specified in RFC822

## `cvs` for read and write CVS data

## `shutil`

shutil.copy() and shutil.copytree() for copying files and entire dicectory trees.

## `tempfile`

Provide functions to create temporary files and directories.

## `filecmp`

Used to compare files (filecmp.cmp() ) and compare entire directories (filecmp.comfiles() )

## `subprocess` and `multiprocessing`

## os

## os.walk() iterates over an entire directory tree

It walk the directly recessively

## os.path.split() return a 2-tuple with the first element containing path and second the filename

These two pars are also available directly using os.path.basename() and os.path.dirname()

## reduce()
```python
>>> import functools
>>> functools.reduce(lambda x,y: x + y, [1,2,3,4],0)
10
>>> functools.reduce(lambda x,y: x + y, [1,2,3,4])
10
>>> functools.reduce(lambda x,y: x ^ y, [1,2,3,4])
4
>>> functools.reduce(lambda x,y: x ** y, [1,2,3,4])
1
>>> functools.reduce(lambda x,y: x ** y, [2,2,3,4])
16777216
```

# Networking and Internet Programming

## `socket`

Creating sockets, doing DNS, IP address handling

##  `ssl`

Encrypted and authenticated sockets can be set up using ssl module.

## `socketserver`

TCP and UDP server

## `asyncore` `asynchat`

Asynchronous client and server socket handling

# Object - Oriented Programming


## + operator 

If we want to create a class that supports concatenation using the + operator and also the len()
function, we can do so by implementing the __add__() and __len__()

## syntax of creating customer classes

```python

class className:
    suite

class className(base_classes):
    suite
```
## all object attributes must be qualified by `self`

## two steps in create an object

- __new()__ is called to create the object
- __init()__ is called to initialize it

If function needed does not exist in object or in any of object's base classes, AttributeError
exception is raised.

## super().__init()__

This is not required if it is inherited from `object` base class.

## __ne__ should not be implemented since __eq()__ will suffice

## return NotImplemented

```python
class Point:
    def __eq__(self, other):
        if not isinstance(other, point):
            return NotImplemented
```

If we return NotImplemented, Python will then try calling other.__eq__(self) to see whether the
`other` type supports the comparison with the Point type. If there is no such method or if that
methods also returns NotImplemented, Python will give up and raise a `TypeError` exception.

## __repr__(self) vs __str__(self)

__str__() is called by built-in str() function and return the result for human readers.

__repr__() is for built-in repr() function.

## using Properties to control attribute access

```python
@property
def area(self):
    return math.pi * (self.radius ** 2)
```

@property is a `decorator`, a decorator is a function that takes a function or method as its
argument and returns a decorated version, that is, a version of the function or method that is
modified in `some` way.

So an alternative way to create a property likes this (we rarely use this syntax)

```python
def area(self):
    return math.pi * (self.radius ** 2)
area = property(area)
```

Once the area property is created by using @property, the area.getter, area.setter, and
radius.deleter attributes become available. They can be used as decorator to replace themselves with
the method they are used to decorate.

```python
@area.setter(self, area)
    pass

## @staticmethod

@staticmethod
def conjunction(*fuzzies):
    return FuzzyBool(min[float(x) for x in fuzzies])


staticmethod does not get `self` or any other first argument specially passed by Python

But some Python programmers consider the use of static method to be un-Pythonic

instead of staticmethod, we crate a module function, if we create the class by inheriting from float

def conjunction(*fuzzies):
    return FuzzyBool(min(fuzzies))
```


## using `exec` to execute Python code in string

## `pickle` 

Help on module pickle:

NAME
    pickle - Create portable serialized representations of Python objects.

## using assert to ensure object is callable

```python
assert hasattr(someobject, "__call__")
```

## need to re-read Chapter 6 Object-Oriented Programming


## open() function

Python distinguishes between files opened in binary and text modes, even when the underlying
operating system does not. Files opened in binary mode (including 'b' in the mode argument) return
contents as bytes objects without any decoding. In text mode (the default, or when 't' is included
in the mode argument), the contents of the file are returned as Unicode strings, the bytes having
been first decoded using a platform-dependent encoding or using the specified encoding if given.
