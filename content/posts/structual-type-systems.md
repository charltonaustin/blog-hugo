---
title: Structual Type Systems
date: 2015-11-07T09:03:54-04:00
draft: false
tags: [
]
---
I was talking recently to a friend about programming languages and how they affect what kinds of software we write when we got on the subject of Golang. He's a successful vice president of engineering at a ruby shop and I'm a lowly java software engineer, and yet we both agreed on one thing: what people really want is a flexible type system that allows them to rapidly write software. Him coming from the Ruby world wistfully looked off in the distance wishing he didn't have to worry about runtime type production errors, while basking in the glory of how rapidly and concisely his team could get out code. Me on the other hand tired of slaving away writing so many extra lines of code, while sleeping comfortably with the knowledge that I was unlikely to break any servers in production through type errors.


### What is a structural type system?

A structural type system is a type system based on properties rather than names. In other words if two types have the same properties they are equal. [Here](https://en.wikipedia.org/wiki/Structural_type_system) is a more comprehensive explanation. In the case of Golang a particular type is considered compatible with an interface if it satisfies that interface. [Here](http://jordanorelli.com/post/32665860244/how-to-use-interfaces-in-go) is a quick explanation and example of Golang interfaces. The icing on the cake is this is checked at compile time unlike duck type checking.


### What does this mean for me practically?

Well it means that in some sense you get to have your cake and eat it too. You don't have to annotate your struct with the interface name you can simply create an interface for a given function and then later create structs that conform to this interface. Or you can create structs that make sense in your domain and notice that many structs have similar properties and then refactor out interfaces from the structs. In other words you have the flexibility to design interfaces up front where it makes sense or create interfaces after the fact without having to change source code of the structs. Further if you change an interface things break at compile time rather than at run time. Contrast this with Java or Ruby. In Ruby if a library changes what methods are called on an object you might get a NoMethodError exception at runtime (in production if your testing is bad). In Java if you wanted to pull out an interface after you see the pattern you would have to update all classes that implement it to have the interface annotation. Further if you don't have access to the source code, but you see a good abstraction that makes sense for your methods you can't easily create an interface that accepts an object from the source code you don't have access to. In Golang on the other hand if the struct satisfies your interface no worries. Since you don't have to have access to the source code this applies equally well to library code. Later if the designer of the library or api makes a breaking change you don't have to worry that it will break in production since it won't compile.