# courses

https://www.seas.upenn.edu/~cis194/fall16/

## Exercies 1

```Haskell
import CodeWorld
import Data.Fixed
trafficController :: Double -> Picture
trafficController t
    | s < 3 = trafficLight Green
    | s < 4 = trafficLight Amber
    | s < 6 = trafficLight Red
    | s < 7 = trafficLight RedAmber
    where s = mod' t 7

botCircle c = colored c (translated 0 (-2.5) (solidCircle 1))
midCircle c = colored c (translated 0 0 (solidCircle 1))
topCircle c = colored c (translated 0 2.5  (solidCircle 1))

data TrafficState = Green | Amber | Red | RedAmber
trafficLight :: TrafficState -> Picture
trafficLight Green = botCircle green & midCircle black & topCircle black & frame
trafficLight Amber = botCircle black & midCircle yellow & topCircle black & frame
trafficLight Red = botCircle black & midCircle black & topCircle red & frame
trafficLight RedAmber = botCircle black & midCircle yellow & topCircle red & frame

frame = rectangle 2.5 7.5
main :: IO ()
main = animationOf trafficController
```

## Exercise 2


```Haskell
import CodeWorld
import Data.Fixed
tree :: Integer -> Picture -> Picture
tree 0 b = b
tree n b = path [(0,0),(0,1)] & translated 0 1 (
  rotated (pi/10) (tree (n-1) b) & rotated (- pi/10) (tree (n-1) b))

blossom :: Double -> Picture
blossom t = colored yellow (solidCircle ( min s (t / 10.0 * s)))
  where s = 0.2

tree_blossom :: Double -> Picture
tree_blossom t = tree 8 (blossom t)
main = animationOf (tree_blossom)
```

# Which applicaton is applicable of using Haskell?

Haskell is for sure not suitable for embedded application, but where it is most suitable?

Following paper may give some idea:

www.starling-software.com/misc/icfp-2009-cjs.pdf

In short, in finnacial applications, where
- algorithm is complex and the language should be more `expressive`
- better perfomrance than Java
- strong type system and pure functional language makes the software less bug
- much less test code. (In C, usually test code is, sometimes much, more than the production code).


And also this Paper:

https://pdfs.semanticscholar.org/a722/b1ca878a662f132e7fd7a5b6b29298378dd5.pdf

# Exciting Features in Haskell

## Pattern Match

This makes it more expressive:

https://www.haskell.org/tutorial/patterns.html

## List and Tuple are different with ones in Python

In Python, 
- List is mutable sequence of elements, each element could have different type.
- Tuple is immutable sequences of elements, each element could have different type

In Haskell, since all data are immutable:
- List is sequence of elements with all same type with infinite length.
- Tuple is seuqnce of elements with possiblely differnet types with limitted length.

### Recursion

Excellent examples about Recursion and Pattern Matching:

http://learnyouahaskell.com/recursion


The combination of Pattern Matching and Recursion may explain why Haskell needs less test code as the implementation is more or less how the test code is written (consider the edge conditions first).

### Functors, Applicatives, Monads

Every easy-to-understand article about Functor, Applicative and Monad
http://adit.io/posts/2013-04-17-functors,_applicatives,_and_monads_in_pictures.html#functors


### Type Constructor


```For functions, (->) is a type constructor; the types f -> g and (->) f g are the same. Similarly, the types [a] and [] a are the same```


# Real World Project

https://github.com/erkmos/haskell-companies

## Adjoint Uplink

This project is written in Haskell, it is a blockchain project.
https://github.com/adjoint-io/uplink
