---
title: Euler Problem 026
date: 2019-08-19T09:03:54-04:00
draft: false
tags: [
    "javascript",
    "toy problems",
    "development",
    "euler",   
]
---
### The problem
A unit fraction contains 1 in the numerator. The decimal representation of the unit fractions with denominators 2 to 10 are given:

    1/2	= 	0.5
    1/3	= 	0.(3)
    1/4	= 	0.25
    1/5	= 	0.2
    1/6	= 	0.1(6)
    1/7	= 	0.(142857)
    1/8	= 	0.125
    1/9	= 	0.(1)
    1/10	= 	0.1 

Where 0.1(6) means 0.166666..., and has a 1-digit recurring cycle. It can be seen that 1/7 has a 6-digit recurring cycle.

Find the value of d < 1000 for which 1/d contains the longest recurring cycle in its decimal fraction part.

#### Solution ####
The problem is an interesting one. My initial thoughts lead me down the path of seeing if I could find cycles using JavaScript division, but I realized that it wasn't necessary. I'm not particularly proud of my solution to this problem. There are a number of things that I'm still a little shaky on. First I only find the longest non terminating decimal sequence. That is I don't remove any leading digits that might not be a part of the cycle. For instance in my algorithm 1/6 = 0.1(6) should have a cycle of 1 but I give it a length of two. This doesn't actually matter because 1/983 has the longest cycle regardless, but this might not work for other number ranges. In any case here is my solution. I could do some more optimizations, but it runs rather quickly (`Finished in 0.046 seconds`). 

```javascript
var findNum = (num, den) =>{
  while(den > num){
    num = num * 10;
  }
  return num;
}
 
var findRepeatingNumerator = (den) => {
  var count = 0;
  var num = 1;
  var nums = {};
  while(true){
    count++;
    num = findNum(num, den);
    if(nums[num]){
      return count;
    }
    nums[num] = true;
    var remainder = num % den;
    if(remainder === 0){
      return 0;
    }
    num = remainder;
  }
}

var findLargest = () => {
  var largest = 0;
  var largestI = 0;
  for(var i = 2; i < 1001; i++){
    var current = findRepeatingNumerator(i);
    if(current > largest){
      largest = current;
      largestI = i;
    }
  }
  return largestI;
}
```

If you'd like to see the full code please see my daily [toy problem](https://github.com/charltonaustin/toy-problems/) exercises that I've been working on. It includes tests and a README.
