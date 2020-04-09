---
title: Leaders In An Array In Clojure Basic Algorithms Part Four
date: 2016-08-17T09:03:54-04:00
draft: false
---
### The problem

Given an array of integers, print the leaders in the array. A leader is an element which is larger than all the elements in the array to its right.

### Solution

Is was my first attempt at coming up with a solution. Naturally I used recursion and simply walked down the array seeing if every element after it was the same.

The time complexity of this is O( n^2 ) which isn't great.

```clojure
(defn find-leader
  [xs]
  (if (empty? xs)
    nil
    (let [head (first xs)
          tail (rest xs)]
      (if (every? #(> head %1) tail)
        (conj (find-leader tail) head)
        (find-leader tail)))))
```

To see if I could get this any better I decided to try and find a non recursive method that does it in O(n) time. To do that we traverse backwards across the array keeping track of the largest element and when we find something smaller then we pop it into an accumulator. Then at the end we simply return the accumulator.

```clojure
(defn find-leader-loop
  [coll]
  (loop [largest (last coll)
         all (butlast coll)
         leaders (conj '() (last coll))]
    (if all
      (if (> (last all) largest)
        (recur (last all) (butlast all) (conj leaders (last all)))
        (recur largest (butlast all) leaders))
      leaders)))
```
One thing to note about this. Since we are using last we should have a vector or else this becomes O( n^2 ) again.
