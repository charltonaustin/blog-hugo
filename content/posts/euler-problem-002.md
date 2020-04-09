---
title: Euler Problem 002
date: 2017-03-17T09:03:54-04:00
draft: false
---
### The problem

Each new term in the Fibonacci sequence is generated
by adding the previous two terms. By starting with 1 and 2,
the first 10 terms will be:

1, 2, 3, 5, 8, 13, 21, 34, 55, 89, ...

By considering the terms in the Fibonacci sequence
whose values do not exceed four million, find
the sum of the even-valued terms.


###### Solution ######

This is a pretty straight forward problem. We generate the the Fibonacci number and as we go we can check if it isEven keeping track of the sum. Note we don't actually have to check if the number is even. An odd integer plus an odd integer is even. So every third term we have an even integer. Which means we could just keep track of the count and add only the even integers, but the solution that is presented here goes fast enough that I didn't really bother with it.

```javascript
var isEven = function(number){
  return number % 2 === 0;
}

var generateNextFibonacciNumber = function(first, second){
  return first + second;
}

var getSumOfEvens = function(limit){
  var first = 1;
  var second = 2;
  var term = 3;
  total = 2;
  while(term < limit){
    term = generateNextFibonacciNumber(first, second);
    if(isEven(term)){
      total += term;
    }
    first = second;
    second = term;
  }
  return total;
}
```

If you'd like to see the full code please see my daily [toy problem](https://github.com/charltonaustin/toy-problems/) exercises that I've been working on. It includes tests and a README.
