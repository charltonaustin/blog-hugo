---
title: Load And Performance Testing With Flask
date: 2018-11-13T09:03:54-04:00
draft: false
---

## The problem
At my current gig, we've spent some time building out our initial product.
Things have been going well, and we have been growing.
These days we started to notice that some over our API endpoints weren't as responsive as before.
We've also started to increase our number of users significantly.
To that end, it was time for us to start measuring our requests and fixing them if necessary.

## Types of performance tests
A quick google search will give you as many types of performance testing as you could want.
Figuring out what you need to test is always the first step in improving your system.
If you expect that you are going to have very spiky traffic, then you should probably do tests that will mimic this.
This might mean that you measure how fast your system can scale up if you get massive amounts of traffics over a few minutes.
At the Tuesday Company, since we are expecting more users and we are starting to see lag times in loading, we decided to test increased user loads and speed of endpoints.
We also decided to stress test our system to see where our servers started to break down.

## Load vs. Speed vs. Stress
Load in our case is the measure of response time while multiple clients connect to our API server.
Speed, on the other hand, is the measure of how long a single endpoint takes to respond on average.
Stress is how many concurrent users it takes for the server to start having error conditions.
Those are our working definitions.

## Our current tech stack
We are using python at the moment along with flask for our API server.
After a little searching and thought we decided to use [Locust](https://github.com/locustio/locust) to run our tests.
Since we needed some way to measure where our bottlenecks were, we also decided to use the [Werkzeug profiler](http://werkzeug.pocoo.org/docs/0.14/contrib/profiler/)
Locust has some pretty good documentation, and it was straightforward creating a few scenarios that covered our most common use cases.
Werkzeug was a bit more involved, but I followed [this guide](http://www.alexandrejoseph.com/blog/2015-12-17-profiling-werkzeug-flask-app.html) and the profiler documentation and managed to muddle through.
At the end of the day, I had to install [qcachegrind](http://brewformulas.org/Qcachegrind) instead of [kcachegrind](https://kcachegrind.github.io/html/Home.html) as recommended by the guide.
Also when setting up the profiler, I had to use this configuration to get the output I needed.

```
f = open('profiler.log', 'w')
stream = MergeStream(sys.stdout, f)
profiler = ProfilerMiddleware(app, stream, profile_dir='/where/to/save')
```

## Our results
At the end of the day, we verified about three significant changes to our infrastructure and code.
We managed to reduce the call time for two of our endpoints by half with code and database changes.
We managed to increase the number of users we could handle by roughly double, while keeping performance at the previous level with an infrastructure change.
We also managed to improve by an order of magnitude.
