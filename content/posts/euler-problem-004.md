---
title: Euler Problem 004
date: 2017-03-27T09:03:54-04:00
draft: false
---
### The problem

A palindromic number reads the same both ways. The largest palindrome made from the product of two 2-digit numbers is 9009 = 91 × 99.

Find the largest palindrome made from the product of two 3-digit numbers.

###### Solution ######

Now this guy is your daily warm up kind of easy. I created two functions. The first one simply checks a number to see if it is a palindrome. The second one accepts a number finds the largest multiplication combination that is a palindrome. Now I didn't have to go all the way to zero, but since we are looking for the largest it should work for any number. 

```javascript
var isPallidrome = function(number){
  var stringNumber = "" + number;
  for(var i = 0; i < stringNumber.length / 2; i++){
    var lastIndex = stringNumber.length - 1 - i;
    if(stringNumber[i] !== stringNumber[lastIndex]){
      return false;
    }
  }
  return true;
}

var findPallidromes = function(number){
  var largestPallidrome = -Infinity;
  for(i = number; i > 0; i--){
    for(j = number; j > 0; j--){
      if(isPallidrome(i*j) && i*j > largestPallidrome){
        largestPallidrome = i*j;
      }
    }
  }
    return largestPallidrome;
}
```

If you'd like to see the full code please see my daily [toy problem](https://github.com/charltonaustin/toy-problems/) exercises that I've been working on. It includes tests and a README.
