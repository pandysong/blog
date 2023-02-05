---
date: 2019-01-02
title: Rust lang features highlight
weight: 10
---

# Introduction

Highlight the features that Rust has comparing to C/C++, which really reduce
the pain when using C/C++. This could remind me of highlighted features.

## variable shadow and immutable default

These features are for high performance and parallelization.

## expression and statement

Expression make the inner function possible and much easier.

## labeled loop

Much more clear to break the outer loop.

## loop expression with break to return a value


## all pythonic way to loop over

```Rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {}", element);
    }
}
```

## pythonic types: tuple and array

## Ownership: make it stand out from other language

Ownership make it possible to allocate some object in some init() function and
transfer the Ownership to another function and free the memory there. The design
pattern is very useful. If it would be done in C++, it would be very
cumbersome.

## "move" feature, s1 is moved to s2

Rust considers s1 as no longer valid. Therefore, Rust doesn’t need to free
anything when s1 goes out of scope:

```
    let s1 = String::from("hello");
    let s2 = s1;

    println!("{}, world!", s1);
```

## Stack-Only Data: Copy

```
    let x = 5;
    let y = x;

    println!("x = {}, y = {}", x, y);
```

## "copy" trait vs "clone" trait

```

So what types implement the Copy trait? You can check the documentation for the
given type to be sure, but as a general rule, any group of simple scalar values
can implement Copy, and nothing that requires allocation or is some form of
resource can implement Copy. Here are some of the types that implement Copy:

- All the integer types, such as u32.
- The Boolean type, bool, with values true and false.
- All the floating point types, such as f64.
- The character type, char.
- Tuples, if they only contain types that also implement Copy. For example,
  (i32, i32) implements Copy, but (i32, String) does not.

```

## 'only one mutable reference'


```

Mutable references have one big restriction: you can have only one mutable
reference to a particular piece of data at a time.

```

This is For PERFORMANCE?


```
The benefit of having this restriction is that Rust can prevent data races at
compile time. A data race is similar to a race condition and happens when these
three behaviors occur:

- Two or more pointers access the same data at the same time.
- At least one of the pointers is being used to write to the data.
- There’s no mechanism being used to synchronize access to the data.

```

##  NLL (Non-Lexical Lifetimes)


```

The ability of the compiler to tell that a reference is no longer being used at
a point before the end of the scope is called Non-Lexical Lifetimes (NLL for
short), and you can read more about it in The Edition Guide.

```

## Dangling References

In languages with pointers, it’s easy to erroneously create a dangling
pointer--a pointer that references a location in memory that may have been
given to someone else--by freeing some memory while preserving a pointer to
that memory. In Rust, by contrast, the compiler guarantees that references will
never be dangling references: if you have a reference to some data, the
compiler will ensure that the data will not go out of scope before the
reference to the data does.


## The Rules of References

Let’s recap what we’ve discussed about references:

- At any given time, you can have either one mutable reference or any number of
  immutable references.
- References must always be valid.

## slicing is similar to python

## String Slices as Parameters

Knowing that you can take slices of literals and String values leads us to one
more improvement on first_word, and that’s its signature:


```Rust
fn first_word(s: &String) -> &str {
```

A more experienced Rustacean would write the signature shown in Listing 4-9
instead because it allows us to use the same function on both &String values
and &str values.


```
fn first_word(s: &str) -> &str {
```

## Using the Field Init Shorthand


```Rust

fn build_user(email: String, username: String) -> User {
    User {
        email,
        username,
        active: true,
        sign_in_count: 1,
    }
}

```

## tuple structs is like NamedTuple in Python

## Unit-Like Structs Without Any Fields

```

struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}

```

## using 'dbg!' to print values when debugging

very convenient. Rust takes most of the common tasks out of box.

## Associated Functions

Decouple the data structure of an object with's associated functions. This
reflects the fundamental principle of software engineering: "decouple if it
could be".

## null


In his 2009 presentation “Null References: The Billion Dollar Mistake,” Tony
Hoare, the inventor of null, has this to say:

```

I call it my billion-dollar mistake. At that time, I was designing the first
comprehensive type system for references in an object-oriented language. My
goal was to ensure that all use of references should be absolutely safe, with
checking performed automatically by the compiler. But I couldn’t resist the
temptation to put in a null reference, simply because it was so easy to
implement. This has led to innumerable errors, vulnerabilities, and system
crashes, which have probably caused a billion dollars of pain and damage in the
last forty years.

```

## enum option

If the type is Option<t>, it is possible that the value is Null, so you have to
write code to handle the Null case:

```

Eliminating the risk of incorrectly assuming a not-null value helps you to be
more confident in your code. In order to have a value that can possibly be null,
you must explicitly opt in by making the type of that value Option<T>. Then,
when you use that value, you are required to explicitly handle the case when
the value is null. Everywhere that a value has a type that isn’t an Option<T>,
you can safely assume that the value isn’t null. This was a deliberate design
decision for Rust to limit null’s pervasiveness and increase the safety of Rust
code.

```

The way it handles option value provides a lot of robust.

## match vs 'if let'

## use ... as to resolve the name conflicting

## Re-exporting Names with pub use

## Option, Result and '?' operator

Need to review the book section:

https://doc.rust-lang.org/book/ch09-02-recoverable-errors-with-result.html


## unwrap and expect

```

Similarly, the unwrap and expect methods are very handy when prototyping,
before you’re ready to decide how to handle errors. They leave clear markers in
your code for when you’re ready to make your program more robust.

```

## Traits: Defining Shared Behavior

traits are a like duck-typing where it was only possible in python, but it is
implemented in Rust.

### Specifying Multiple Trait Bounds with the + Syntax


```rust

pub fn notify(item: &(impl Summary + Display)) {

```

### Clearer Trait Bounds with where Clauses

```rust

fn some_function<T, U>(t: &T, u: &U) -> i32
    where T: Display + Clone,
          U: Clone + Debug
{

```

## Validating References with Lifetimes

need to re-read again and again:

https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html

## `#[should_panic]` annotion

```rust

#[cfg(test)]
mod tests {
    use super::*;

    #[test]
    #[should_panic]
    fn greater_than_100() {
        Guess::new(200);
    }
}

```

## Deref Coersion


https://doc.rust-lang.org/book/ch15-02-deref.html


## borrow_mut() and borrow() in RefCell<T>

It give a flexibility for compiler to escape to check. Instead, it becomes
run-time checking.
