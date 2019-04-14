---
date: 2017-09-28
title: Closure notes
weight: 10
---

# Introduction
The problem in following pages got attention. 
http://www.4clojure.com/problem/60#prob-title

## solution

Here is my solution which passes the unit tests.
```clojure

(fn myreduce
  ([f coll]
   (when-let [s (seq coll)]
     (myreduce f (first s) (rest s))))
  ([f v coll]
   (lazy-seq (cons v (when-let [s (seq coll)]
                       (myreduce f (f v (first s)) (rest s)))))))

```
