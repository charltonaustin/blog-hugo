---
title: Euler Problem 007
date: 2018-03-28T09:03:54-04:00
draft: false
---
### The problem
By listing the first six prime numbers: 2, 3, 5, 7, 11, and 13, we can see that the 6th prime is 13.

What is the 10001 prime number?

#### Solution ####
Interestingly my first attempt was the good old Sieve of Eratosthenes.
However I kept getting an out of memory error from JavaScript.
I was using NPM 3.10.3 and would get a no free memory error after GC.
My next attempt included going piece by piece and only keeping in memory as many of the composite numbers as necessary and re walking the primes.
To remove the new composite numbers.
This worked much better.
The final solution runs at a very reasonable speed and has no memory issues.

```javascript
var findPrimeN = function(number){
  var primes = [2]
  var numbers = [];
  while(primes.length < number){
    // Grow the numbers array
    var start = primes[primes.length-1] + 1;
    var growth = primes[primes.length-1];
    for(var i = start; i < start + growth; i++){
      numbers.push(i);
    }

    // Remove composite numbers
    for(var i = 0; i < primes.length; i++){
      for(var j = 0; j < numbers.length; j++){
        if(numbers[j] % primes[i] === 0){
          numbers.splice(j,1);
        }
      }
    }


    for(var i = 0; i < numbers.length; i++){
      for(var j = 1; j < numbers.length; j++){
        if(numbers[j] % number[i] === 0){
          number.splice(j,1);
        }
      }
    }

    // move prime numbers to primes
    for(var i = 0; i < numbers.length; i++){
      primes.push(numbers[i]);
    }
    numbers = [];
  }
  return primes[number - 1];
}
```

If you'd like to see the full code please see my daily [toy problem](https://github.com/charltonaustin/toy-problems/) exercises that I've been working on.
It includes tests and a README.
