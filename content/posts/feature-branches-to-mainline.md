---
title: Feature Branches To Mainline
date: 2015-10-10T09:03:54-04:00
draft: false
---
Recently at [my current company](http://intentmedia.com/) my team has been struggling to move from a feature branch workflow to a mainline workflow.  I'm going to define what the feature branches and mainline workflows are. Then I'm going to talk about the costs and benefits of both models. Finally I'm going to talk about why people should use the mainline workflow.


### Feature Branch Workflow 

So what is the feature branch workflow (at least at IntentMedia). For my current team it consisted of someone picking up a story, then creating a new branch, doing the development work, creating a pull request, seeing if the build is green on TeamCity, getting the application checked by a quality analyst, and finally merging in with master.


### Mainline Workflow

So what is the mainline workflow. It consists of someone (preferably a pair) picking up a story, then starting work on master, getting a few commits in, then pushing to master, now this is repeated as many times as is needed till the story is done. When the story is done there whoever owns the story looks at it and approves or asks for some changes. Finally you remove any abstractions so that the next release from master the feature will be turned on. *Notice there isn't any quality analyst in this. I'm going to write another blog post on why there shouldn't people shouldn't have quality analysts at all, but that's for another time.


### Benefits of Feature Branches Workflow

Lets start with the benefits of feature branch workflow. First it allows team members to collaborate on features at their pace. I push to a remote branch, then create a pull request, and then I can ask the entire team to take a look at the pull request and they can do it whenever they want. Another benefit is experimentation. Because I know that nothing I do on a branch is going to make it to production until I merge it in I find that I can be more experimental with my branches. Finally I can run all my tests on remote server instead of locally. Now I personally don't think of this as benefit, but for some reason it has come up over and over again as a good thing.


### Costs of Feature Branches Workflow

So what are the costs of feature branches workflow. I think the number one is you don't have to integrate often. I find this one particularly devastating with pairing. I work with one person on Monday and I create a nice abstraction. Then on Tuesday I'm pairing with someone on a related, but different story (imagine a partner integration). This second story could really use the abstraction from Monday, but the feature branch still hasn't been merged to master. You really don't have many good options in this case. You can try and merge in the other persons branch, but this makes for a weird history and can get to merge conflicts quickly. You can wait, and work on something else, but that is really against the agile get things out as quickly as possible and iterate idea. Another cost is people don't have to run test suits locally. Some people like that they don't have to run test suits locally. It means less dependencies on their machine, they can push to a remote server that is more production like, but I think of it as a cost because this lengthens the amount of time it takes to get feedback. If you don't run your tests locally, then they have a tendency to become longer and have more dependencies. 


### Costs of Mainline Workflow

The number one cost of mainline workflow is developers have to have discipline. Developers have to run tests locally, they have to either maintain standards on their own (I don't suggest this) or have a pair, they must make changes in such a way that they don't break production while working on a story.  Developers have to have ways to turn off half baked features. (Might I suggest just not wiring things in rather than immediately jumping to feature tags/toggles/whatever?) This leaves a lot of things to think about. Small codebases will have a higher number of merge conflicts, but more frequent check ins means that they are usually easier to resolve (I have never had a practical problem with this).
Benefits of Mainline Workflow

The number one benefit is developers have to integrate often. This of course is with the assumption that developers don't have code sitting only on their machine. If you are doing that STOP RIGHT NOW. You have version control so that if your computer is destroyed you don't lose your work. In any case if I create an abstraction for a feature and then someone is working on a related but different story before my feature gets released guess what they can use or modify that abstraction. This facilitates multiple people working on similar yet different features at the same time. I also find that the work flow is simplified. You pull and rebase off of master regularly. You push to master and you don't really have to do much else with git. Now don't get me wrong git is powerful and there are lots of awesome features and in fact I use branches for all kinds of experiments, but not for work I'm pushing to production.  Finally it works with other practices to streamline the process. Pairing is much faster than code review in my experience. Sure it is more convenient to do a code review when you can and branching at least on git facilitates this, but it is also much slower.  

### Why The Mainline Workflow Is Better

The mainline workflow in my experience has been orders of magnitude lower cycle time. Think one day or a couple of hours instead of a week or a couple of days depending on the story. When you multiply this across a team of 5-6 developers this can add up quickly. One thing I have been experimenting with is day branches that are used for code reviews. I haven't figured out exactly what the flow would look like yet, but I'm looking for something that would combine the ability to look back at the change set of a branch without sacrificing the benefits of the mainline workflow.
