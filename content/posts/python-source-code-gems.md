---
title: Python Source Code Gems
date: 2016-04-04T09:03:54-04:00
draft: false
---
So fairly regularly I read about and dip into the python source code. For some reason there seems to be a great deal of beautiful and interesting code in both the c and the standard libraries and tests for python. The other day I was doing some reading about [generators](https://docs.python.org/2/howto/functional.html#passing-values-into-a-generator) (I was thinking about how to marry the ideas of functional and object oriented programming) and I came across something that I found fascinating as well beautiful so I decided I should share it here on this silly little blog. It's a full implementation of a generator built [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes) in test_generators.py.   If you are writing python and you aren't using generators regularly then you are probably missing something.

```python
Build up to a recursive Sieve of Eratosthenes generator.


>>> def firstn(g, n):

...     return [g.next() for i in range(n)]


>>> def intsfrom(i):

...     while 1:

...         yield i

...         i += 1


>>> firstn(intsfrom(5), 7)

[5, 6, 7, 8, 9, 10, 11]


>>> def exclude_multiples(n, ints):

...     for i in ints:

...         if i % n:

...             yield i


>>> firstn(exclude_multiples(3, intsfrom(1)), 6)

[1, 2, 4, 5, 7, 8]


>>> def sieve(ints):

...     prime = ints.next()

...     yield prime

...     not_divisible_by_prime = exclude_multiples(prime, ints)

...     for p in sieve(not_divisible_by_prime):

...         yield p


>>> primes = sieve(intsfrom(2))

>>> firstn(primes, 20)

[2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71]
```