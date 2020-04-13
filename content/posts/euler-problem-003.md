---
title: Euler Problem 003
date: 2017-03-18T09:03:54-04:00
draft: false
tags: [
    "javascript",
    "toy problems",
    "development",
    "euler",   
]
---
### The problem

The prime factors of 13195 are 5, 7, 13 and 29.

What is the largest prime factor of the number 600851475143 ?

###### Solution ######

This one took a bit longer than I expected. My fist attempt was pretty naive. The idea was generate all the primes less than the number and then check if the number is divisible by the prime. At the end we simple return the largest prime number that divides the number evenly. This works up until we get to 600851475143. Then we start getting out of memory errors and it takes too long. The first attempt looks like this.

```javascript
var findPrimesLessThan = function(number){
  var primes = [];
  for (var i = 2; i < number; i++){
    primes.push(i);
  }
  for(var i = 0; i < Math.sqrt(primes.length); i++){
    for(var j = i + 1; j < primes.length; j++){
      if(primes[j] % primes[i] === 0){
        primes.splice(j,1);
      }
    }
  }
  return primes;
}

var isPrimeFactor = function(number, prime){
  return number % prime === 0;
}

var findPrimeFactors = function(number){
  var primes = findPrimesLessThan(number);
  var largestPrimeFactor = primes[0];
  for(var i = 0; i < primes.length; i++){
    if(isPrimeFactor(number, primes[i]) && primes[i] > largestPrimeFactor){
      largestPrimeFactor = primes[i];
    }
  }
  return largestPrimeFactor;
}
```

Now I could have done some optimizations to search backwards or to generate the largest number first, but I remembered Fermat factorization and figured this guy was odd so I would give another method a try. It looks something like this.

```javascript
var findFermatFactorization = function(a, number){
  if(number % 2 === 0){
    return 0;
  }

  var bSquared = a * a - number;
  while(Math.sqrt(bSquared) % 1 !== 0 && a < number){
    a += 1;
    bSquared = a*a-number;
  }
  if(Math.sqrt(bSquared) % 1 !== 0){
    return false;
  }
  return [a, Math.sqrt(bSquared)];
}

var isPrimeNumber = function(number){
  for(var i = 2; i < number; i++){
    if(number % i === 0){
      return false;
    }
  }
  return true;
}

var findPrimeNumber = function(number){
  var factors = findFermatFactorization(Math.ceil(Math.sqrt(number)), number);
  while(factors){
    var addFactor = factors[0] + factors[1];
    var subFactor = factors[0] - factors[1];
    console.log("addFactor: ", addFactor)
    console.log("subFactor: ", subFactor)
    if(isPrimeNumber(addFactor)){
      return addFactor;
    }
    if(isPrimeNumber(subFactor)){
      return subFactor;
    }
    factors = findFermatFactorization(factors[0]+1, number);
  }

  return primes;
}
```
This guy works much faster and doesn't give out of memory errors, but it works on an assumption that admittedly I have not proven. That the first prime we find using Fermat Factorization is indeed the largest prime number that exists. Now I believe it to be true, but I'm not sure how to prove it. I'm just going to have to do a bit more research. 

If you'd like to see the full code please see my daily [toy problem](https://github.com/charltonaustin/toy-problems/) exercises that I've been working on. It includes tests and a README.
