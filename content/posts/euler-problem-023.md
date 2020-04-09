---
title: Euler Problem 023
date: 2019-07-31T09:03:54-04:00
draft: false
---
### The problem
A perfect number is a number for which the sum of its proper divisors is exactly equal to the number. For example, the sum of the proper divisors of 28 would be 1 + 2 + 4 + 7 + 14 = 28, which means that 28 is a perfect number.

A number n is called deficient if the sum of its proper divisors is less than n and it is called abundant if this sum exceeds n.

As 12 is the smallest abundant number, 1 + 2 + 3 + 4 + 6 = 16, the smallest number that can be written as the sum of two abundant numbers is 24. By mathematical analysis, it can be shown that all integers greater than 28123 can be written as the sum of two abundant numbers. However, this upper limit cannot be reduced any further by analysis even though it is known that the greatest number that cannot be expressed as the sum of two abundant numbers is less than this limit.

Find the sum of all the positive integers which cannot be written as the sum of two abundant numbers.

#### Solution ####
So my initial thoughts with this guys was generate the abundant numbers. Generate all possible numbers up to a point. Then remove the abundant numbers and finally sum up the rest. Unfortunately I misunderstood the problem to begin with. I needed to remove only the numbers that could be represented as the sum of two abundant numbers. So I made the changes giving me an O(n^2) solution with a pesky splice because of javascript.
The first solution took over two mins some 130s to find the answer. 
```
Finished in 130.656 seconds
```

So I decided to refactor my code a bit. First, instead of removing the numbers, I generate all possible sums and create a poor man's set using a JavaScript object. Then sum up to the number, if the number is not in the set. It works like a charm and only takes .115 seconds. 
```
Finished in 0.115 seconds
```
This is the optimized code. See commit d812cf0c6cc89571d912a1c2194097f9098d5643 to see the code that takes over two mins.

```javascript
var isAbundantNumber = (number) => {
  if(number === 1){
    return false;
  }
  var total = 1;
  for(var i = 2; i <= Math.sqrt(number); i++){
    if(number % i === 0){
      var otherDivisor = number / i
      if(i === otherDivisor){
        total+= otherDivisor;
      }else{
        total+= i + otherDivisor;
      }
      if(total > number){
        return true;
      }
    }
  }
  return false;
}

var findAbundantNumbersLessThan = (limit) => {
  var abundantNumbers = [12];
  for(var i = 13; i < limit; i++){
    if(isAbundantNumber(i)){
      abundantNumbers.push(i);
    }
  }
  return abundantNumbers;
}

var generateAllPossibleSums = (abundantNumbers)=>{
  var sums = {};
  for(var i = 0; i < abundantNumbers.length; i++){
    for(var j = i; j < abundantNumbers.length; j++){
      var sum = abundantNumbers[i]+abundantNumbers[j];
      if(sum > 28123){
        break;
      }
      sums[sum] = true;
    }
  }
  return sums;
}

var sumNumbersExcept = (exceptions, limit) => {
  var total = 0;
  for(var i = 0; i < limit; i++){
    if(!exceptions[i]){
      total+=i;      
    }
  }
  return total;
}

var findSum = ()=>{
  var abundantNumbers = findAbundantNumbersLessThan(28123);
  var allPossibleSums = generateAllPossibleSums(abundantNumbers);
  return sumNumbersExcept(allPossibleSums, 28123);
}
```

If you'd like to see the full code please see my daily [toy problem](https://github.com/charltonaustin/toy-problems/) exercises that I've been working on. It includes tests and a README.
