---
title: Euler Problem 009
date: 2019-03-06T09:03:54-04:00
draft: false
---
### The problem
A Pythagorean triplet is a set of three natural numbers, a < b < c, for which,
a^2 + b^2 = c^2

For example, 32 + 42 = 9 + 16 = 25 = 52.

There exists exactly one Pythagorean triplet for which a + b + c = 1000.
Find the product abc.

#### Solution ####
This solution is straight forward. We satisfy the constraint that a < b < c by incrementing up by one in each variable and then we search for the solution. At first glance this might seem like an O(n^3) problem, but it's constant, at least in the sense that we are only going to 1000 and we have no variable inputs. Mine runs fast enough. So I guess brute force it is today.

```javascript
var findPythagoreanTriplet = function(){
  for(var a = 1; a < 1000; a++){
    for(var b = a+1; b < 1000; b++){
      for(var c = b+1; c < 1000; c++){
        if(a + b + c === 1000 && a * a + b * b === c * c){
          return a*b*c;
        }
      }
    }
  }
  return 0;
}
```

If you'd like to see the full code please see my daily [toy problem](https://github.com/charltonaustin/toy-problems/) exercises that I've been working on. It includes tests and a README.
