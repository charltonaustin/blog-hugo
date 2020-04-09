---
title: Writing Distributed Test Reduction
date: 2016-04-07T09:03:54-04:00
draft: false
---
### The Problem

I recently joined [HedgeServ](https://www.hedgeserv.com/restricted/get/index.html) a fintech company and one of my first tasks at the new job has been dealing with how long it takes tests to run. The company has some great end to end test coverage, but the testing time can be rather long (think hours).

### The solution

Paul Hammant, my mentor of sorts wrote a [blog post](http://paulhammant.com/2015/01/11/reducing-test-times-by-only-running-impacted-tests/) about only running tests that actually need to be run when you change a source code set. So my job was to implement a solution that took a set of source files that were touched and only run required tests. This would mean that developers can run only tests that are affected by their source code changes.


### The devil's in the details

#### Design Considerations

When I was tasked to build this application the first thing I did was start to break the problem down. There is the mapping over the test code to the source code (for each test we need a list of touched source code). Then we must map the source code to the test code (i.e. each source code has a list of test classes that touched it). Finally there must be some way of consuming that data. Paul's post does a pretty good job of describing a proof of concept for how we create the mappings and then consume them. The tricky (and I think most interesting) bit that is not described by Paul is how to do this in a parallel fashion. In Paul's example he run's each test sequentially updating the mapping at each step, but for the real world example at HedgeServ this isn't possible.

#### Real world problems

If you try and run all the tests sequentially the test time explodes. Right now the tests run in a couple of hours because we can run many of them in parallel. However transforming the test code to source code mapping into a source code to test code mapping isn't really something you can do in parallel. What you can do in parallel is run the tests and create just the test code to source code mapping.

#### Theory to the rescue

The solution is to think of this problem in terms of mapping and then reducing over that mapped data. In other words we run multiple tests in parallel where each tests creates a list of source code that it touches. Then after all tests have created their mapping have a second job that comes along and transforms all that data to the second mapping. There are a couple of benefits to thinking in terms of two tasks. The tests take much longer than the simple data transforms, so you can reduce the time (think mins) to run the tests by scaling up the number of machines that run the tests. Since transforming the test code to source code map into the source code to test code map is much faster than actually running the tests, you can fairly trivially recreate the second map on demand. 


### Some considerations

Some things to keep in mind about creating this kind of tool. First your tests must be independent. This means shared resources should be kept to a minimum. In my case that meant writing environment scripts that automated the (previously) hand driven creation of environments for each set of tests that I ran in parallel. This had some great second order effects as well. We can now scale the number of tests that are run concurrently according to the number of servers we have to run them. Second it can be tricky to deal with data setup and tear down. In our case the source code is used to create the data and to remove the data from the database before and after each test. To deal with this we have to note source code that is touched by every test. This can be accomplished by running the data creation/deletion by itself, then looking over the map of source code to test code and removing the mappings that are shared by everyone. This isn't perfect since a test can traverse the same source code multiple times and you would incorrectly remove it from a particular test, but it should be fairly accurate and the times it isn't should be protected by failures in your data creation and deletion. The cleanest way would be to not have source code involved in the creation and deletion of data, but this can sometimes be impractical. 