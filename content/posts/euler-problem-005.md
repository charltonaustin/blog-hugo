---
title: Euler Problem 005
date: 2018-03-22T09:03:54-04:00
draft: false
---
### The problem

2520 is the smallest number that can be divided by each of the numbers from 1 to 10 without any remainder.

What is the smallest positive number that is evenly divisible by all of the numbers from 1 to 20?

### Solution

Now this guy is a higher order, functional dream. I first create a function that returns a function to see if a number is divisible by another number. Then I create a function that uses that creator to create a function that checks that a number is divisible by every number in the range 1 to that number inclusive. Finally I have a function that starts out skipping by the largest number in the range looking for the smallest number is divisible by every number in that range. The solution looks something like below. There are still a few optimizations that we could make. First we could simply check that the first number is an integer. This would mean we could start our range from two. Second we could remove all redundant numbers if we wanted. For instance if the number is divisible by 20 then it is all divisible by 2, 5, and 10. So we could remove those numbers. However the solution runs fast enough in my browser so I stopped where I was.

```javascript
var isDivisibleCreator = function(divisibleBy){
  return  function(number){
    return number % divisibleBy === 0;
  };
}

var isDivisibleByRangeCreator = function(range){
  var checkers = [];
  for(var i = 1; i < range + 1; i++){
    checkers.push(isDivisibleCreator(i));
  }
  return function(number){
    for(var i = 0; i < checkers.length; i++){
      if(!checkers[i](number)){
        return false;
      }
    }
    return true;
  };
}

var smallestDivisbleBy = function(number){
  var isDivisibleByRange = isDivisibleByRangeCreator(number);
  var foundRightNumber = false;
  var maybeRightNumber = number;
  while(!foundRightNumber){
    maybeRightNumber += number;
    if(isDivisibleByRange(maybeRightNumber)){
      foundRightNumber = true;
    }
  }
  return maybeRightNumber;
}
```

If you'd like to see the full code please see my daily [toy problem](https://github.com/charltonaustin/toy-problems/) exercises that I've been working on. It includes tests and a README.
