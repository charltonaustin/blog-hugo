---
title: The Count Down Problem
date: 2016-04-20T09:03:54-04:00
draft: false
tags: [
    "algorithms",
    "haskell",
    "functional programming",
]
---
Recently I've been watching a wonderful [set of videos](https://channel9.msdn.com/Series/C9-Lectures-Erik-Meijer-Functional-Programming-Fundamentals/Lecture-Series-Erik-Meijer-Functional-Programming-Fundamentals-Chapter-1) about Haskell during my lunch break. If you are interested in Haskell and functional programming I would definitely recommend the videos.

I found lecture 11 particularly interesting because of a paper that was written about the content [here](http://www.cs.nott.ac.uk/~pszgmh/countdown.pdf).


The paper is interesting because it shows how you can write a proof for an implementation of a problem in Haskell, then create transformations that speed up that implementation and prove that those transformations are also correct.


I find the question of whether or not an implementation is correct an interesting one. How do we know that the proof is correct? Is that better than a test? If so, why is it better than a test? If not, why not?

### How do we know the if the proof is correct?

This is a difficult one. I'm a math guy. I went to the [University of Chicago](http://www.uchicago.edu/) and studied applied math for my undergraduate degree. When I was presented with a proof I believed it because most of them had been seen by numerous people that were far smarter than myself. When reading the proof about the countdown problem proof for instance I don't know (but I definitely imagine) that there are fewer people that have looked at this proof. And if I were going to write my own proof then I would imagine only the people personally interested in the software would have read it. So the only way to know if a proof is correct would be to prove it to myself with my own (fallible) sense of reason.

### Is this better than a test? If so, why? If not, why not?

This one I'm not sure on. It is definitely not easier than writing some tests, but in some sense it is more complete than writing test cases. Examples of something working are definitely not proofs that an implementation works correctly. In the pathological case you can hand write every case, but one in an implementation. On the other hand, whether or not the proof is correct can be difficult to know for certain as well. The number of "proofs" that I wrote that were incorrect at UChicago was astounding. Both the proof and the tests are simply evidence that the software is correct. There is no way to be certain that either are bullet proof. Perhaps neither is really better. Yet the paper is pretty cool and writing a formal proof is an interesting, if not esoteric, tool in the tool bag.