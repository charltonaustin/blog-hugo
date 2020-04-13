---
title: Bugs In Software
date: 2020-01-14T09:03:54-04:00
draft: false
tags: [
    "testing",
    "management",
    "books",
]
categories : [
    "management",
]
---
## The Architecture of Open Source Applications
If you haven't seen or read [The Architecture of Open Source Applications](http://aosabook.org/en/intro1.html) then you are missing out.
Sure, it is a bit long.
Also, each section is written by a different writer making it slightly inconsistent.
But, there are gems throughout the book that make it worth the read.

## One of the gems
Here's an example of one of the gems from the [Berkeley DB](http://aosabook.org/en/bdb.html) section.

> Design Lesson 13: "There is rarely such thing as an unimportant bug. Sure, there's a typo now and then, but usually a bug implies somebody didn't fully understand what they were doing and implemented the wrong thing. When you fix a bug, don't look for the symptom: look for the underlying cause, the misunderstanding, if you will, because that leads to a better understanding of the program's architecture as well as revealing fundamental underlying flaws in the design itself."

This quote is incredible because of how true it is.
The one thing I would argue against is it's assumption that all engineers are good at what they do.
Most of the time bugs come from a misunderstanding of requirements.
Occasionally, they come from lack of foresight.
On the latter occasions the code is wrong not because of requirements.
The code is wrong because of a lack of analysis (of the code).
Assuming you have hired the right people, this last error should be rare.
We can assume then that a bug is from either a typo or a misunderstanding.

## What to do with this little gem
Once convinced that a bug comes from a typo or a misunderstanding, bug management becomes easier.
If it is a typo then your job is pretty easy.
Find a static analysis tool to help with the problem.
If it is a misunderstanding, things become more interesting.
Most of software is built as a giant game of telephone.
(Usually) there is an end user who talks to customer service.
Customer service relays the information to a product person.
The product person talks with some kind of engineer.
Now there might be some extra people, or occasionally the names change.
For instance; someone from business development might be talking to an end user.
Or instead of a product person talking to an engineer, it might be an engineering manager.
Rarely do you see end users talking directly to engineers though.
When you have a giant game of telephone, communication tends to be the failure.
I have the following three part system to be effective.
First, and key, is to think critically about where information is lost.
Create procedures to help eliminate loss of information.
Finally, verify that the procedure works.
If things do not work, repeat from step one.
Occasionally, upon analysis of the situation, you find the problem isn't communication.
When this happens a thorough understanding of 'why' is important.
Here applying the [five whys technique](https://en.wikipedia.org/wiki/5_Whys) is good.
For instance you might find that it has to do with language.
This is where other techniques become vitally important.
If you find that language is often a barrier then you should review [domain driven design](https://en.wikipedia.org/wiki/Domain-driven_design) practices.


