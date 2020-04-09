---
title: Top Down Vs Bottom Up Api Design In Clojure Part Zero
date: 2016-04-20T09:03:54-04:00
draft: false
---
### Intro

I saw a lot of presentations at [Clojure/conj](http://clojure-conj.org/) 2015. Most of them were really great, but one of them I was particularly interested in seeing didn't really do it for me. It was called [Bottom Up vs Top Down Design](https://www.youtube.com/watch?v=Tb823aqgX_0&index=13&list=PLZdCLR02grLrl5ie970A24kvti21hGiOf) in Clojure by [Mark Bastian](https://github.com/markbastian).  I thought his presentation was interesting, but it failed to touch on a couple of points that I was interested in. In an effort to learn a bit, I figured I would write post about what I was hoping the talk would actually address.


### Personal Experience

My personal experience has been that working from the top down leads to best API design. I also find that it makes it easier for writing well tested object oriented code. That's because you can pretty easily know when you are going to have dependent behavior and mock that out on the layer below whatever you are writing. This helps to identify and segregate the dependencies early. Something that doesn't always happen when you are coming from the bottom up. What I find often happens when I'm designing from the bottom up is that I don't realize I need to split a class until after I have started using it in the layer above. That said when you are writing properly functional code without side effects collaboration often looks somewhat different then when you are writing collaborating object.


### What I wish I had seen

I've spent most of my programming career professionally programming in an object oriented style. I've only spent a year or so writing truly functional code in Scala, in finance for a trading application. The rest of the time I've spent writing in Java, JavaScript, Python, and Ruby in an object oriented style. Half of the time I've spent staring at the bottom. Writing a small unit test for a method in a class and slowly building up from there. At a few companies I have had the chance to start with a functional test before working my way down to the class level and unit tests. 

All programming I've ever done in Clojure has been of the first kind. I've found it easier to build up more complex functions from simpler functions.  I was hoping that someone with more experience would show what outside in design of software would look like in Clojure. So I think I'm going to have a multi part post about building a tic-tac-toe game in Clojure in two different ways. First from the bottom up and second from the top down. I'm also using a TDD only approach to the project. I'll try and keep the commits relatively clean and done after each test. You can see the code [here](https://github.com/charltonaustin/inside_out_ouside_in).


I think it will be interesting and instructive to compare the two different code bases after I have them written.
