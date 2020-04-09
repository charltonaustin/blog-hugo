---
title: Binary Search Of Sorted Array In Clojure Basic Algorithms Part Three
date: 2016-08-03T09:03:54-04:00
draft: false
---
### The problem

Find an element in a sorted array using  binary search

### The solution

The solution is pretty straight forward. I added in some printlns to get a sense of how we drill down in the recursion to find the element.

```clojure
(defn binary-search [v value]
  (loop [low 0 high (dec (count v))
         depth 0]
    (let [half-way (quot (+ high low)  2)]
      (do (println (str "half-way is " half-way))
          (println (str "low is " low))
          (println (str "high is " high))
          (println (str "depth is " depth))
          (if (<= high (inc low))
            (cond (= value (v low)) low
              (= value (v high)) high
              :else nil)
            (let [middle (quot (+ low high) 2)]
              (if (< (v middle) value)
                (recur (inc middle) high (inc depth))
                (recur low middle (inc depth)))))))))
```

You can call the function like this:

```clojure
(binary-search (vec (range 0 10)) 1000)
```

Dialing up the range you can see how many steps it takes to get to the value.

There's also a java implementation of binary search.
```java
java.util.Collections.binarySearch(List<T> list, T key, Comparator<? super T> c).
```

I decided to see compare the two implementations with a variety of elements in the collection while, looking for a random element. I took the average time over 100,000 runs to see how different the two implementations did.


The two implementations that I ran were.

```clojure
(defn binary-search [v value]
  (loop [low 0 high (dec (count v))
         depth 0]
    (let [half-way (quot (+ high low)  2)]
      (if (<= high (inc low))
        (cond  (= value (v low)) low
          (= value (v high)) high
          :else nil)
        (let [middle (quot (+ low high) 2)]
          (if (< (v middle) value)
            (recur (inc middle) high (inc depth))
            (recur low middle (inc depth))))))))
```

vs

```clojure
(defn java-binary-search [v value]
  (Collections/binarySearch v value compare))
```

For 1000000 elements in the array using java-binary-search I got.

```clojure
{:average 30.04732094221115, :std-deviation 9.367210018023908}
```

For 1000000 element in the array using binary-search I got.

```clojure
{:average 29.451808820724487, :std-deviation 9.199826118870515}
```

Insanely they were basically the same. It took about 5 mins to get the results back for 10,000 runs. So I decided to dial up the number of elements to 10,000,000 and reduce the number of runs to 100.


For java-binary-search I got

```clojure
{:average 870.9942288208008, :std-deviation 705.2356880437546}
```

For binary-search I got

```clojure
{:average 728.1910227966308, :std-deviation 441.7981992776274}
```

There is some difference here. The standard deviations show that java-binary-search has a slightly wider spread, which means it could be faster in some cases with some outliers as compared to binary search. Since I didn't I decided to look at the implementation of binarySearch within Java itself. For the record here it is.

```java
private static <T> int indexedBinarySearch(List<? extends T> l, T key, Comparator<? super T> c) {
    int low = 0;
    int high = l.size()-1;
    while (low <= high) {
        int mid = (low + high) >>> 1;
        T midVal = l.get(mid);
        int cmp = c.compare(midVal, key);
        if (cmp < 0)
            low = mid + 1;
        else if (cmp > 0)
            high = mid - 1;
        else
            return mid; // key found
    }

    return -(low + 1);  // key not found
}
```

This is actually the indexed version of binary search, but since we are using a vector/array I figured this would be the one that was getting used from Clojure (though I didn't confirm this). I'm not sure why the two implementations are so close together. I would love to see the byte code produced by each invocation. Maybe in a future post I will do exactly that. For now this will have to be good.
