---
title: Clojure Sieve Of Eratosthenes
date: 2016-04-24T09:03:54-04:00
draft: false
---
I was recently watching a video on Haskell that was showing how you would write a functional form of the [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes). I personally love this algorithm and was interested in what it looked like in Clojure. So I wrote this:

```clojure
(defn sieve [number xs]
  (if (= 0 number)
    xs
    (conj (sieve (dec number)
                 (filter #(not= 0 (mod % (first xs))) (rest xs)))
          (first xs))))
```

This works, but Clojure doesn't have tail call optimization (TCO) by default which means you can get a stack StackOverflowError. So I decided to rewrite it to use the loop and recur construct to remove the StackOverflowError. It looks like this:

```clojure
(defn sieve [number xs]
  (loop [number number
         xs xs
         return-value nil]
    (if (= 0 number)
      return-value
      (recur (dec number)
             (filter #(not= 0 (mod % (first xs))) (rest xs))
             (conj return-value (first xs))))))
```

Which can be called like this:

```clojure
(sieve 1000 (iterate inc 2))
```

I like this implementation, but I wanted to see what would happen when I pushed it towards it's limit. So I went ahead and I ran:

```clojure
(sieve 10000 (iterate inc 2))
```

At which point I promptly got;


StackOverflowError   clojure.lang.Numbers.inc (Numbers.java:112)


A little sad and perplexed, it wasn't immediately obvious to me why I was getting a stack overflow. Well it turns out that recur can't use TCO on lazy lists. In particular;

```clojure
(filter #(not= 0 (mod % (first xs))) (rest xs))
```

Doesn't work, but if we change the implementation to:

```clojure
(defn sieve [number xs]
  (loop [number number
         xs xs
         return-value nil]
    (if (= 0 number)
      return-value
      (recur (dec number)
             (doall (filter #(not= 0 (mod % (first xs))) (rest xs)))
             (conj return-value (first xs))))))

```

and we call it as

```clojure
(sieve 10000 (range 2 1000000))
```

While it takes some time, the above does come back without a StackOverflowError, but at the cost of losing the lazy evaluation.