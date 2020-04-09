---
title: Euler Problem 001
date: 2017-03-13T09:03:54-04:00
draft: false
---
### The problem

If we list all the natural numbers below 10 that are multiples of 3 or 5, we get 3, 5, 6 and 9. The sum of these multiples is 23.

Find the sum of all the multiples of 3 or 5 below 1000.

### Solution

The first algorithm is super straightforward. We want to simply loop over each number between the start and the end checking if the number is a multiple of 3 or 5 and then adding that to the sum. Now this is going to O(n). It isn't bad at all. 


```javascript
var isMultipleOfThree = function(number) {
  return (number % 3) === 0;
}

var isMultipleOfFive = function(number) {
  return (number % 5) === 0;
}

var eulerProblemOne = function(upperBound){
  var sum = 0;
  
  for(var i = 0; i < upperBound; i+=3){
    if(isMultipleOfFive(i) || isMultipleOfThree(i)){
        sum += i;
    }
  }
  
  return sum;
}
```
But we can do a little better than this. Instead of iterating over every single number between 0 and the upper bound we can simply add up to the upper bound across 3 and 5 making sure to account for double counting. 


```javascript
var isMultipleOfThree = function(number) {
  return (number % 3) === 0;
}

var isMultipleOfFive = function(number) {
  return (number % 5) === 0;
}

var eulerProblemOne = function(upperBound){
  var sum = 0;
  
  for(var i = 3; i < upperBound; i+=3){
    if(!isMultipleOfFive(i)){
      sum += i;
    }
  }
  
  for(var i=5; i < upperBound; i+=5){
    sum +=i;
  }
  return sum;
}
```

If you'd like to see the full code please see my daily [toy problem](https://github.com/charltonaustin/toy-problems/) exercises that I've been working on. It includes tests and a README.
