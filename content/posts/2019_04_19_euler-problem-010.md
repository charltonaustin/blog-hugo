---
title: Euler Problem 010
date: 2019-04-19T09:03:54-04:00
draft: false
tags: [
    "javascript",
    "toy problems",
    "development",
    "euler",   
]

---
### The problem
The four adjacent digits in the 1000-digit number that have the greatest product are 9 × 9 × 8 × 9 = 5832.


73167176531330624919225119674426574742355349194934
96983520312774506326239578318016984801869478851843
85861560789112949495459501737958331952853208805511
12540698747158523863050715693290963295227443043557
66896648950445244523161731856403098711121722383113
62229893423380308135336276614282806444486645238749
30358907296290491560440772390713810515859307960866
70172427121883998797908792274921901699720888093776
65727333001053367881220235421809751254540594752243
52584907711670556013604839586446706324415722155397
53697817977846174064955149290862569321978468622482
83972241375657056057490261407972968652414535100474
82166370484403199890008895243450658541227588666881
16427171479924442928230863465674813919123162824586
17866458359124566529476545682848912883142607690042
24219022671055626321111109370544217506941658960408
07198403850962455444362981230987879927244284909188
84580156166097919133875499200524063689912560717606
05886116467109405077541002256983155200055935729725
71636269561882670428252483600823257530420752963450

Find the thirteen adjacent digits in the 1000-digit number that have the greatest product. What is the value of this product?

#### Solution ####
This guy is a straightforward fun warm up for the day. A simple loop to walk over the numbers looking in all the possible directions (there are four). Going down and up are the same, going left and right are the same, forward-up is the same as going backward-down, and going forward-down is the same as going backward-up. Then as we traverse we keep track of the largest value.

```javascript
var findLargestMultiple = function(string, length){
  var largestMultiple = 0;
  for(var i = 0; i < string.length-length; i++){
    var currentMultiple = 1;
    for(var j = i; j < i+length; j++){
      console.log(Number(string[j]));
      currentMultiple *= Number(string[j]);
    }
    if(currentMultiple > largestMultiple){
      largestMultiple = currentMultiple;
    }
  }
  return largestMultiple;
}
```

If you'd like to see the full code please see my daily [toy problem](https://github.com/charltonaustin/toy-problems/) exercises that I've been working on. It includes tests and a README.
