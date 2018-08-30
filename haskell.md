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
