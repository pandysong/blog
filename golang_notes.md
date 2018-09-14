Language Spec: https://golang.org/ref/spec

Package: https://golang.org/pkg/

# Must watch 1
Go Concurrency Patterns (slides) : https://www.youtube.com/watch?v=f6kdp27TYZs

## goroutines
important: goroutines are not threads. They are cheap, it's practical to have thousands, even
hundreds of thousands of goroutines

## channels
`channel sending and receving are synchronized operations`

Channels are first-class types

## shared memory
`do not communicate via shared memory` using channels instead

## using another goroutine to get the value to other two channels and feed to one channels

So that the two channels are not necessarily synchronized


using `select` to have only one go routine to combine two input to one channel

## we can send a channel via a channel

Two way communications to make two goroutine synchronized

## timeout using select

select {
    case s := <-c:
        do_sth()
    case <- time.After( time.Second):
        fmt.Println("You are too slow")
        return
}


# Must watch 2

https://vimeo.com/53221558

# Section 1
# A name with capital letter is exported

# function arguments: type comes after the variable name

## shorten the function parameters:

In this example, we shortened

```go
x int, y int
```

to

```go
x, y int
```

## a function could return any number of results

This is similar to Python. This is different with JavaScript.  In JavaScript, multiple result should
be shuffled into a object before returning.

```go
func swap(x, y string) (string, string) {
    return y, x
}
```

## naked return  and named return values


```go

package main

import "fmt"

func split(sum int) (x, y int) {
    x = sum * 4 / 9
    y = sum - x
    return
}

func main() {
    fmt.Println(split(17))
}

```

## 'var' statements

# basic types

```
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias for uint8

rune // alias for int32
     // represents a Unicode code point

float32 float64

complex64 complex128
```


# variable declarations factored into blocks

# Zero values

declared without an explicit initial value are given their zero value. 


The zero value is:

   0 for numeric types,
   false for the boolean type, and
   "" (the empty string) for strings.

# fmt.Printf

%T for type
%v for value
%q for value with quoted

# type conversions

Unlike in C, in Go assignment between items of different type requires an explicit conversion. Try
removing the float64 or uint conversions in the example and see what happens. 

# type inference

When declaring a variable without specifying an explicit type (either by using the := syntax or var
= expression syntax), the variable's type is inferred from the value on the right hand side. 

Notes:
This is like Python? Maybe not, inference means the type is deterministic when compiling


# Constants


Constants

Constants are declared like variables, but with the `const` keyword.

Constants can be character, string, boolean, or numeric values.

Constants cannot be declared using the := syntax.

This is different with Python, in Python constant is by convention using capital letters

# Numeric Constants

Numeric constants are high-precision values.

An untyped constant takes the type needed by its context. 

## Pass by values

Variable is a map or slice

Maps and slices are reference types in Go and should be passed by values.

# Section 2

## For

No parentheses surrounding the three components of the for statement and braces {} are always
required.

### For is Go's "while"

```golang
func main() {
    sum := 1
    for sum < 1000 {
        sum += sum
    }
    fmt.Println(sum)
}
```

### Forever

```
func main() {
    for {
    }
}
```

## if statement

### if with a short statement

Like `for`, the `if` statement can start with a short statement to execute before the condition. 

```go
func pow(x, n, lim float64) float64 {
    if v := math.Pow(x, n); v < lim {
        return v
    }
    return lim
}
```


## switch 

A `switch` statement is a shorter way to write a sequence of if - else statements. It runs the first
case whose value is equal to the condition expression.

Go's switch is like the one in C, C++, Java, JavaScript, and PHP, except that Go only runs the
selected case, not all the cases that follow. In effect, the break statement that is needed at the
end of each case in those languages is provided automatically in Go. Another important difference is
that Go's switch cases need not be constants, and the values involved need not be integers.

## Switch with no condition

Same as `switch true`

## Defer

A defer statement defers the execution of a function until the surrounding function returns.


Useful example are mentioned in https://kylewbanks.com/blog/when-to-use-defer-in-go

```go
func BillCustomer(c *Customer) error {
    c.mutex.Lock()
    defer c.mutex.Unlock()
    
    if err := c.Bill(); err != nil {
        return err
    }
    
    if err := c.Notify(); err != nil {
        return err
    }
    
    // ... do more stuff ...
    
    return nil
}
```

# Section 3

## Pointers

pointer's zero value is nil

Unlike C, Go has no pointer arithmetic

## Structs

```go
type Vertex struct {
    X int
    Y int
}
```

## pointers to structs

## arrays

```
var a [10]int
```

Declares a variable a as an array of ten integers

## Slices

An array has a fixed size. A list on the other hand, is a dynamically-sized

Much like in python, but slice in go does not support stride, and negative index
But it could ignore the start index (default as 0) and end index (default as len())

```go
package main

import "fmt"

func main() {
    primes := [6]int{2, 3, 5, 7, 11, 13}

    var s []int = primes[1:4]
    fmt.Println(s)
}

```

### Like python, Slices are like reference to arrays

### slice literal
A slice literal is like an array literal without the length.

This is an array literal:

[3]bool{true, true, false}

And this creates the same array as above, then builds a slice that references it:

[]bool{true, true, false}

### slice length vs capacity

length: the number of elements the slice contains
capacity: the number of elements in the underlying arrays

## creating a slice with make

a := make([]int, 5)  // len(a)=5

b := make([]int, 0, 5) // len(b)=0, cap(b)=5

    // Extend its length.
    s = s[:4]
    printSlice(s)

    // Drop its first two values. This is interesting as it will reduce the capacity by 2
    s = s[2:]
    printSlice(s)

### some article about array and slices

http://openmymind.net/The-Minimum-You-Need-To-Know-About-Arrays-And-Slices-In-Go/

## Appending to a slice

```
func append(s []T, vs ...T) []T
```

## range is like enumerate() in python (syntax is a bit different using `:=` instead of `in` )

```go

package main

import "fmt"

var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}

func main() {
    for i, v := range pow {
        fmt.Printf("2**%d = %d\n", i, v)
    }
}

```

if `i` is not needed, it could be replaced with `_`

if `v` is not needed, it could be ',v' could be removed entirely


## maps

```go
var m map[string]Vertex

func main() {
    m = make(map[string]Vertex)
    m["Bell Labs"] = Vertex{
        40.68433, -74.39967,
    }
    fmt.Println(m["Bell Labs"])
}
```

### mutating maps

deleting:

delete(m, key) is similar like in python del d[key]

### testing if key is in map

elem,ok = m[key]

if key is in m, ok is true. if not, ok is false.

if key is not in map then elem is the zero value for the map's element type


### strings.Field is like Pyton's split()


## Function values like in Python

Functions are values too. They can be passed around just like other values.

Function values may be used as function arguments and return values. 


## Function closures

https://www.calhoun.io/5-useful-ways-to-use-closures-in-go/


### use cases: isolating data:

it has similar use cases like python's generator function, but not flexible as generator function.
But channel in golang is like generator in Python. channel is communication between threads it is a
bit tedious to use comparing to generator which is implemented in a single thread.

The reason why golang has no generator is maybe because golang needs to compiled while Python is
script language which could save state in a single thread in a flexible way.

Note that following example is a good example how to write consice code for return the first values
in iteration.



```go
package main

import "fmt"

// fibonacci is a function that returns
// a function that returns an int.
func fibonacci() func() int {
   z,a,b := 0,0,1
   return func() int {
         z,a, b =a, b,a+b
         return z
   }
}

func main() {
    f := fibonacci()
    for i := 0; i < 10; i++ {
        fmt.Println(f())
    }
}

```

to read 

https://bbengfort.github.io/snippets/2016/12/22/yielding-functions-for-iteration-golang.html

### using function as function decorator

It is used to verify that a user is authenticated, log web requests, write default headers, and much
more.

### using closure in sort.Search()

In following example the closure access numbers although it is never passed in
```go

package main

import (  
  "fmt"
  "sort"
)

func main() {  
  numbers := []int{1, 11, -5, 8, 2, 0, 12}
  sort.Ints(numbers)
  fmt.Println("Sorted:", numbers)

  index := sort.Search(len(numbers), func(i int) bool {
    return numbers[i] >= 7
  })
  fmt.Println("The first number >= 7 is at index:", index)
  fmt.Println("The first number >= 7 is:", numbers[index])
}

```

### one liner to setup and tear down 

```go
package main

import "fmt"

func main() {  
  f = setup()
  defer f()
}

func setup() func() {  
  fmt.Println("pretend to set things up")

  return func() {
    fmt.Println("pretend to tear things down")
  }
}

```


# Section 4

## methods

A method is a function with a special receiver argument.

The receiver appears in its own argument list between the func keyword and the method name. 

```go
func (v Vertex) Abs() float64 {
    return math.Sqrt(v.X*v.X + v.Y*v.Y)
}
```

### Pointer receiver

Note that this Scale function will receive a value as pointer. so if v is a variable of Vertex.
v.Scale(f) will compile and work.

```go
func (v *Vertex) Scale(f float64) {
    v.X = v.X * f
    v.Y = v.Y * f
}
```

In general, all methods on a given type should have either value or pointer receivers, but not a
mixture of both. (We'll see why over the next few pages.) 


## interface


### Interfaces are implemented implicitly

A type implements an interface by implementing its methods. There is no explicit declaration of
intent, no "implements" keyword. 


```go
package main

import "fmt"

type I interface {
    M()
}

type T struct {
    S string
}

// This method means type T implements the interface I,
// but we don't need to explicitly declare that it does so.
func (t T) M() {
    fmt.Println(t.S)
}

func main() {
    var i I = T{"hello"}
    i.M()
}

```

Under the covers, interface values can be thought of as a tuple of a value and a concrete type:

(value, type)

An interface value holds a value of a specific underlying concrete type.

Calling a method on an interface value executes the method of the same name on its underlying type. 


### Interface values with nil underlying values

If the concrete value inside the interface itself is nil, the method will be called with a nil
receiver.

In some languages this would trigger a null pointer exception, but in Go it is common to write
methods that gracefully handle being called with a nil receiver (as with the method M in this
example.)

Note that an interface value that holds a nil concrete value is itself non-nil.


### Nil interface values

A nil interface value holds neither value nor concrete type.

Calling a method on a nil interface is a run-time error because there is no type inside the
interface tuple to indicate which concrete method to call.

### The empty interface

The interface type that specifies zero methods is known as the empty interface:

interface{}

An empty interface may hold values of any type. (Every type implements at least zero methods.)

Empty interfaces are used by code that handles values of unknown type. For example, fmt.Print takes
any number of arguments of type interface{}.

```go

package main

import "fmt"

func main() {
    var i interface{}
    describe(i)

    i = 42
    describe(i)

    i = "hello"
    describe(i)
}

func describe(i interface{}) {
    fmt.Printf("(%v, %T)\n", i, i)
}

```


# to read

https://www.calhoun.io/gotchas-and-common-mistakes-with-closures-in-go/


# Type assertions

A type assertion provides access to an interface value's underlying concrete value.

t := i.(T)

This statement asserts that the interface value i holds the concrete type T and assigns the
underlying T value to the variable t.

If i does not hold a T, the statement will trigger a panic.

To test whether an interface value holds a specific type, a type assertion can return two values:
the underlying value and a boolean value that reports whether the assertion succeeded.

t, ok := i.(T)

If i holds a T, then t will be the underlying value and ok will be true.

If not, ok will be false and t will be the zero value of type T, and no panic occurs.

Note the similarity between this syntax and that of reading from a map. 

# type switches

```go

switch v := i.(type) {
case T:
    // here v has type T
case S:
    // here v has type S
default:
    // no match; here v has the same type as i
}

```

# Errors


```go
func (e ErrNegativeSqrt) Error() string
```

# Readers

The io.Reader interface has a Read method:

func (T) Read(b []byte) (n int, err error)



# Section 5

https://tour.golang.org/concurrency/1

## goroutines

Goroutines

A goroutine is a lightweight thread managed by the Go runtime.

go f(x, y, z)
starts a new goroutine running

f(x, y, z)

The evaluation of f, x, y, and z happens in the current goroutine and the execution of f happens in
the new goroutine. 


## Channels

Channels are a typed conduit through which you can send and receive values with the channel
operator, <-.

ch <- v    // Send v to channel ch.
v := <-ch  // Receive from ch, and
           // assign value to v.

(The data flows in the direction of the arrow.)

Like maps and slices, channels must be created before use:

ch := make(chan int)

By default, sends and receives block until the other side is ready. This allows goroutines to
synchronize without explicit locks or condition variables.

The example code sums the numbers in a slice, distributing the work between two goroutines. Once
both goroutines have completed their computation, it calculates the final result. 

### Buffered Channels

Channels can be buffered. Provide the buffer length as the second argument to make to initialize a
buffered channel:

ch := make(chan int, 100)

### Range and close

A sender can close a channel to indicate that no more values will be sent. Receivers can test
whether a channel has been closed by assigning a second parameter to the receive expression: after

v, ok := <-ch

ok is false if there are no more values to receive and the channel is closed.

The loop for i := range c receives values from the channel repeatedly until it is closed. 

Note: Only the sender should close a channel, never the receiver. Sending on a closed channel will
cause a panic.

Another note: Channels aren't like files; you don't usually need to close them. Closing is only
necessary when the receiver must be told there are no more values coming, such as to terminate a
range loop. 

## Select

Following example using two channels to communicate between two threads.

```go
package main

import "fmt"

func fibonacci(c, quit chan int) {
    x, y := 0, 1
    for {
        select {
        case c <- x:
            x, y = y, x+y
        case <-quit:
            fmt.Println("quit")
            return
        }
    }
}

func main() {
    c := make(chan int)
    quit := make(chan int)
    go func() {
        for i := 0; i < 10; i++ {
            fmt.Println(<-c)
        }
        quit <- 0
    }()
    fibonacci(c, quit)
}

```

In this example, the reason why the receiver is running in a thread is because the sender may need
some time to tear down before main exits

### default selection

```go
)ckage main

import (
    "fmt"
    "time"
)

func main() {
    tick := time.Tick(100 * time.Millisecond)
    boom := time.After(500 * time.Millisecond)
    for {
        select {
        case <-tick:
            fmt.Println("tick.")
        case <-boom:
            fmt.Println("BOOM!")
            return
        default:
            fmt.Println("    .")
            time.Sleep(50 * time.Millisecond)
        }
    }
}

```

## sync.Mutex

type Mutex

A Mutex is a mutual exclusion lock. The zero value for a Mutex is an unlocked mutex. 

So user could directly using the Lock() and Unlock()


