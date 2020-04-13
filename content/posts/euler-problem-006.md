---
title: Euler Problem 006
date: 2018-03-23T09:03:54-04:00
draft: false
tags: [
    "javascript",
    "toy problems",
    "development",
    "euler",   
]
---
### The problem
The sum of the squares of the first ten natural numbers is,

1^2 + 2^2 + ... + 10^2 = 385 The square of the sum of the first ten natural numbers is,

(1 + 2 + ... + 10)^2 = 552 = 3025 Hence the difference between the sum of the squares of the first ten natural numbers and the square of the sum is 3025 − 385 = 2640.

Find the difference between the sum of the squares of the first one hundred natural numbers and the square of the sum.

#### Solution ####
The code is not that interesting. You simply walk from 1 to the number keeping track of the differences and return them in the end. Not a bad way to warm up for the day, but definitely isn't going to keep thinking forever.

```javascript
var findDifferenceOfSquares = function(number){
  var differences = [];
  var sum = 0;
  var sumOfSquares = 0;
  for(var i = 1; i < number + 1; i++) {
    sum += i;
    sumOfSquares += i * i;
    differences.push(sum*sum - sumOfSquares);
  }
  return differences;
}
```

If you'd like to see the full code please see my daily [toy problem](https://github.com/charltonaustin/toy-problems/) exercises that I've been working on. It includes tests and a README.
