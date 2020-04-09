---
title: Permutations Of A String In Clojure Basic Algorithms Part Two
date: 2016-07-27T09:03:54-04:00
draft: false
---
### The problem

Find all permutations of a string

### The solution

My first solution is comes from Java and is a recursive solution.
```clojure
(defn- perms [pre v]
  (let [n (count v)]
    (if (zero? n)
      nil      
      (loop [i 0]
        (if (< i n)
          (do (perms (conj pre (nth v i))
              (into [] (concat(first (split-at i v))
                              (second (split-at (inc i) v)))))
            (recur (inc i)))
          nil)))))
```
It has some problems. It is recursive so vulnerable to stack overflow errors, but more than that it is pretty slow and not very idiomatic Clojure. So I went in search of a better more idiomatic implementation of this problem and I found this.
```clojure
(defn- iter-perm [v]                                            
  (let [len (count v)
        j (loop [i (- len 2)]
            (cond (= i -1) nil (< (v i) (v (inc i))) i
                  :else (recur (dec i))))]
    (when j
      (let [vj (v j)
            l (loop [i (dec len)]
                (if (< vj (v i)) i (recur (dec i))))]
        (loop [v (assoc v j (v l) l vj) k (inc j) l (dec len)]

          (if (< k l)
            (recur (assoc v k (v l) l (v k)) (inc k) (dec l))
            v))))))

(defn- vec-lex-permutations [v]
  (when v (cons v (lazy-seq (vec-lex-permutations (iter-perm v))))))

(defn lex-permutations
  "Fast lexicographic permutation generator for a sequence of numbers"  [c]
  (lazy-seq    (let [vec-sorted (vec (sort c))]
      (if (zero? (count vec-sorted))
        (list [])
        (vec-lex-permutations vec-sorted)))))

(defn permutations
  "All the permutations of items, lexicographic by index"  [items]
  (let [v (vec items)]
    (map #(map v %) (lex-permutations (range (count v) )))))
```

This comes from the combinatorics library and I think it is instructive to walk through each function to see what is going on.

Permutations simply creates a new vector the size of the original vector. Then passes that off to lex-permutations which returns all the permutations of that new vector [1..N]. It then takes the original elements and maps those over the permutations to return the elements.

Lex-permutations in our case doesn't do much. We already have a sorted vector so it just passes the vector to vec-lex-permutations.

Vec-lex-permutations starts with [1..N] and then builds a new vector [[1..N]..[N..1]] by repeatedly calling iter-perm. Iter-perm will return a permutations of [1..N] until it gets to [N..1] and then it will return a nil which will end the iteration.

Finally we come to the iter-perm function. This guys is where most of the work happens. I think it is instructive to walk through and example with [1 2] as the input to vec-lex-permutations and see what happens. Before we do that though on a high level this function takes in sequence [1..N] goes through permutations until it gets to [N..1] and then outputs nil after that. This nil is what signals the stop of vec-lex-permutations gives you the sequence of permutations [[1..N]..[N..1]].

So what happens with [1 2] well in iter-perm we have.

```clojure
(defn- iter-perm [v]                                        ; 1.  v = [1 2]
  (let [len (count v)                                       ; 2.  len = 2
        j (loop [i (- len 2)]                               ; 3.  j = 0 ; i = 0
            (cond (= i -1) nil                              ; 4.  i != -1
                  (< (v i) (v (inc i))) i                   ; 5.  return i (0)
                  :else (recur (dec i))))]                  ; 6.  skip
    (when j                                                 ; 7.  j  = 0
      (let [vj (v j)                                        ; 8.  vj = 1
            l (loop [i (dec len)]                           ; 8.  l  = 1 ; i = 1
                (if (< vj (v i)) i (recur (dec i))))]       ; 10. 1 < 2 => i = 1
     (loop [v (assoc v j (v l) l vj) k (inc j) l (dec len)] ; 11. v = [2 1] ; k = 1 ; l = 1
          (if (< k l)                                       ; 12. 1 < 1 != True
          (recur (assoc v k (v l) l (v k)) (inc k) (dec l)) ; 13. skip
            v))))))                                         ; 14. return v ([2 1])
```

The right hand steps show what happens, but basically on the first invocation we find the first permutation [1..N] is [1 2] and the second is [N..1] which is [2 1] and the next call will give us nil which stops the sequence and passes back up the stack [[1 2][2 1]] which is all the permutations of our two element vector.

This was definitely an interesting problem and I'm glad to see the clojure implementation.
