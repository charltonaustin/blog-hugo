---
title: Counting Frequencies Of Elements In An Array In Clojure Basic Algorithms Part One
date: 2016-07-20T09:03:54-04:00
draft: false
---
So I'm not a CS guy. I went to school for math and I became a programmer pretty late. I also never had the obligatory algorithms class. So I'm going to start a series where I explore some algorithms coding problems.
The problem

So the coding challenge is given an array with N numbers m such that m < N and m is a natural number count the occurrences of m. 
Solution

```clojure
(defn count-freq [array counts]
  (if (empty? array)
    counts
    (let [number (peek array)]
      (count-freq (pop array) (update counts number (fnil inc 0))))))
```

This is a pretty straightforward implementation. It takes roughly O(n) time. It is called like so:

```clojure
(deftest count-freq-test
  (testing    (is (= {2 2 3 2 5 1} (count-freq [2 3 3 2 5] {})))
    (is (= {2 5} (count-freq [2 2 2 2 2] {})))
    (is (= {} (count-freq [] {})))
    ))
```

 I call it using a map, but you could use a properly initialized array if you wanted. This guy still has the StackOverflowError problem since no tail call optimization is employed. That would look like:

```clojure
(defn count-freq [array counts]
  (loop [array array
         counts counts]
    (if (empty? array)
      counts
      (let [number (peek array)]
        (recur (pop array) (update counts number (fnil inc 0)))))))
```

I decided to take a look at the core implementation of frequencies to get a feel for the differences in the implementations. For the record here it is.

```clojure
(defn frequencies
  "Returns a map from distinct items in coll to the number of times  they appear."  
  [coll]
  (persistent!   
    (reduce (fn [counts x] (assoc! counts x (inc (get counts x 0))))
            (transient {}) coll)))
```

The largest differences come in the lack of recursion and the use of transient data structures. Transient data structures are meant for speed while abstracting away the messy details of mutability and keeping with the Clojure model.


>Furthermore, there are no guards against accidentally sharing or aliasing the mutable data structure, especially if you need to call helper functions to do the work. In short, it would be a shame if you had to leave Clojure’s model in order to speed up a piece of code like this. Transient data structures are a solution to this optimization problem that integrates with the Clojure model and provides the same thread safety guarantees you expect of Clojure.
