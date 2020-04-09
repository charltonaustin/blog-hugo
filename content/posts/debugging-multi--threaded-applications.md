---
title: Debugging Multi  Threaded Applications
date: 2015-10-24T09:03:54-04:00
draft: false
---
Recently I had debug a multithreaded application with some surprising results. Seeing as this can be a particularly tricky task for people that haven't done it before I thought  I would write about some lessons I learned from the experience.


### logging Logging *LOGGING* and *MORE LOGGING*

Log to understand the problem. This one is hard to set up retroactively, but it is well worth the effort. Strive for logs that are meaningful, and that give you checkpoints. For instance avoid putting logs in tight loops. Unless there is something you really need to see inside of the tight loop, move the log out. If you really need to log within a tight loop think about either making the log level DEBUGGING, or only logging every nth element, or doing some other logic to really only log things that you will be interested in. In multithreaded application logging can give you clues on whether or not a thread is starved or when events are happening out of order. Most of the time logs in tight loops will be unhelpful. While some people argue that you should log everything and then use other tools to sift through them I think you will save everyone a whole lot of headaches if you try and log only what you really need. True hard disk space is cheap, true you can roll your logs often to make it easier to sift through them, but at the end of the day the simpler your logs are to sift through the more likely you are to solve the problem. 

### Make sure you log all exceptions in children threads

Register your threads with a logger so you don't just swallow un-handled exceptions. This is something that the java docs tends to leave out. With executors you should take a look at http://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ThreadPoolExecutor.html#afterExecute-java.lang.Runnable-java.lang.Throwable- rather than registering directly with the thread (and you should probably be using Executors to create your thread pool). Silently dying threads can be confusing even with lots of good logging unless you log out that something died. If you have a message based system and messages are just falling off the end of the earth this is probably a good place to start.

### Don't start optimizing before you understand what is going on

On my problem clients were timing out because they weren't receiving messages from the server in time. I did a lot of work to speed up the process since only some clients weren't receiving messages and it seemed to only happen under heavy load and after some time. While the optimization was in some sense not wasted effort because it was a time and load sensitive problem it didn't actually have anything to do with clients timing out. It had everything to do with an uncaught exception on a corner case that wasn't being handled properly. Because I started with what I intuitively knew was wrong rather than by systematic understanding through logging it took me some time to figure that out. 