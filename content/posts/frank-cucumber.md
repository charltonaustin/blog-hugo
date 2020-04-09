---
title: Frank Cucumber
date: 2013-02-17T09:03:54-04:00
draft: false
---
Starting my blog again. So my blog has hopelessly become out of date. Since the last time I posted I've started working for ThoughtWorks, gone to India, moved to San Francisco, and finally been put onto my client. Great so now what. Since things are finally starting to settle and since my thoughts have changed on maintaining a blog I've decided it's time to start blogging again. Not that anyone has read anything yet. My goal is to do it once or twice a week depending on time constraints and travel. So lets get started.


For my first post back I'm going to talk about frank-cucumber which is a ruby gem maintained by a co-worker of mine. Recently I got a chance to see a sneak peak to a presentation that Pete Hodgson is going to be giving over the importance of iOS user acceptance testing at [mdevcon](http://www.mdevcon.com/).


This is a short tutorial on how to set up a frank-cucumber project over a working iOS app.

### Things you are going to need (or I expect to be installed):


1. Ruby 1.9.x  (I would suggest using [rvm](https://rvm.io/). [Here](http://stackoverflow.com/questions/3696564/how-to-update-ruby-to-1-9-x-on-mac) is a post to help with the install)
1. xcode 
1. the xcode command line tools
1. svn 

First I would like to say that I've actually done very little iOS programming myself. Mostly I've been doing Java programming for work these days (with a healthy mix of Chef/Cucumber/Gradle/MongoDB/Knockout.js and a couple of other technologies) while learning Scala on the side. That said the presentation was fantastic and I learned a couple of things which I would like to talk about.

If you go to the frank-cucumber [homepage](http://testingwithfrank.com/) you will see that frank is a way to execute cucumber tests against an iOS app. Now what does this mean.

First we are going to need an iOS app to work with.

```bash
$: svn checkout http://countitout.googlecode.com/svn/trunk/ countitout-read-only
```

Now cd into the directory.

```bash
$:cd countitout-read-only/
```

Next you want to install frank and cucumber.

```bash
$:gem install frank-cucumber

$:gem install cucumber
```

Then you need to run frank build to create a frank structure for your iOS app.

```bash
$:frank build
```

This should create the basic frank structure. Next to get the app running with frank simple run frank launch. Now you have an instance with frank running with it, but even better is to use something like frank inspect. This brings up a webpage with symbiote and chance for you to see elements in which you can select on in your cucumber tests.

```bash
$:frank inspect
```

Finally if you would like to see the a basic set of tests run then run cucumber (close down the iOS Simulator first).

```bash
$:cucumber Frank/features
```


Now you are ready to start exploring cucumber testing for your iOS app. :)