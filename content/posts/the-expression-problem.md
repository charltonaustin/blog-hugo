---
title: The Expression Problem
date: 2016-03-03T09:03:54-04:00
draft: false
---
###  What is the expression problem?

The expression problem is the idea that it is easy to add new functions for simple data types without changing the data types, while it is hard to add data types without changing all the functions. At the same time it is easy to add data types with objects (complex data types), while it is hard to add functions without changing all the objects. 


Some great descriptions of this include chapter 6 of [Uncle Bob's](https://sites.google.com/site/unclebobconsultingllc/) book [Clean Code](http://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882). He doesn't actually call it the expression problem, but it is. Or you can see it on [Wikipedia(https://en.wikipedia.org/wiki/Expression_problem). Or my personal favorite because this was the first place I came across it, is in a [paper](http://lampwww.epfl.ch/~odersky/papers/ExpressionProblem.html) from [Martin Odersky](https://en.wikipedia.org/wiki/Martin_Odersky).


### Why do I care?

So why bring this up today? Well because in my everyday programming life I get the feeling that most people have never heard of it. Or maybe a little better most people are rather confused by it. Why do I get that feeling? Because I will often see a class like:

```java
    public class Square implements Shape{
        // Sometimes this is private with getters and setters which is the same.

        public int side;

        public void putSquareIn(Circle hole){
            ... some domain logic ...
        }
    }
```


And what's wrong with that you ask? Well as Uncle Bob so astutely points out:

>This confusion sometimes leads to unfortunate hybrid structures that are half object and half data structure. They have functions that do significant things, and they also have either public variables or public accessors and mutators that... tempting other external functions to use those variables the way a procedural program would use a data structure.

>Such hybrids make it hard to add new functions but also make it hard to add new data structures. They are the worst of both worlds. 

### So what can be done about this?

I've seen a couple of solutions to this problem. For instance [Scala](http://scala-lang.org/) goes to great lengths to make it easy to extend in both dimensions.  [Clojure](https://clojure.org/) has a core set of data types with a uniform interface and many functions that operate on them. Object oriented purists tend to say you should only use objects without procedural programming. Uncle Bob has his own approach.  He suggests you should be careful to not mix the two forms.  Presumably you should try and guess which types of problems will need new functions and which kinds of problems will need new types. In any of these cases I think like so many things in programming the best thing that can be done is careful thought before, during, and after coding. Each method has it's own benefits and costs. Careful thought about what to apply when will probably lead to the best results for your given situation. 


