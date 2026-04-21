---
title: Does Unit Testing Make Me Slower
date: 2016-04-12T09:03:54-04:00
draft: false
tags: [
    "tdd",
    "development",
]

---
### Say What?

Recently I asked a coworker if a new application we were working to get into production had unit tests. His response was interesting. He said, "No, but it's too early to have unit tests. The code hasn't really settled and unit tests slow you down."

My initial reaction in my head was "Whoa that's silly". But after some consideration and thinking about how code can be written, I'm beginning to see his point. Not the point that we shouldn't write unit tests, rather that the way many people write unit tests can make writing code more slow.  Unit tests do slow you down some, but the rewards of being able to refactor implementations and know that your code is doing what you think it is doing makes up for any extra time you may sink into them. The sooner you can flush out bugs in the development process, the cheaper it is to fix them.

### Unit testing doesn't have to slow you down (too much)?

That's right. Unit tests don't have to slow you down too much. The more you can decouple your unit tests from your production code the less it will slow you down. So I want to go over some of the common ways that you can decouple your unit tests from your source code and ways that you can tell if you are tightly coupling your code to your interface or to your implementation.

#### Keep your test code simple.

The simpler you keep your test code the easier it will be to spot dependencies. The easier it is to spot dependencies the easier it will be to decouple your test code from your source code. This is true of all code, but is particularly important with test code. Also, when someone comes to read the source code later, they can see pretty quickly what it actually does when you have clean test code. 

#### Does it return a value?

The first thing you can start to look at is; Do your methods return a value? If they do; Does this value rely mostly/only on the method parameters? The more pure functions you have the easier it will be to simply test inputs and outputs. This will minimize the amount of test code that changes when you change the implementation.

#### How are you mocking your collaborators?

When mocking collaborators there are couple of schools of thought. Questions like: 'Should I create test doubles?' or 'Should I use a mocking framework?' must be answered. The trick with mocking is to divorce your implementation as much as possible from your mock. In complex scenarios mocking can quickly become a burden. Many people who start mocking on older code bases find it difficult to keep the complexity of tests to a minimum. They usually find writing tests is a really slow process at the start. The trick is two fold here.  Listen to what your tests are telling you. If they are complex then the underlying implementation is usually too complicated. Secondly, do anything and everything to divorce your tests from changes in the how collaborators work. Create factory methods.  Use deep stubs.  Think about every character you write, then ask yourself if it needs to be in test code. Ask yourself what will have to change in the tests if you change a class.


Remember simpler is better. Not only will it help you with coupling (which will naturally be high with test code), but it will help you with other things that slow you down when writing software.
