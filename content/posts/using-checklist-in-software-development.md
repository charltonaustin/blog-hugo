---
title: Using Checklist In Software Development
date: 2019-09-09T09:03:54-04:00
draft: false
---
## The dream
For some time I have been wanting to incorporate checklist into the development project.
My initial inspiration came from [The Checklist Manifesto](http://atulgawande.com/book/the-checklist-manifesto/).
The book which was initially published in 2009 sent me frantically searching for checklists in software development.
At the time I couldn't find any.
Today I use the treasure trove of checklists from [Code Complete Two](https://www.amazon.com/Code-Complete-Practical-Handbook-Construction/dp/0735619670) by [Steve McConnell](https://stevemcconnell.com/books/). 
This blog post will describe the use of checklists at my [company](https://www.tuesdaycompany.com/).
Today I will set out to do a few things:

1. Describe the checklists that I have created in the intervening years
1. Talk about The Checklist Manifesto and it's effect on the checklists I have created
1. Talk about the checklists in Code Complete Two and their effect on our checklists
1. Summarize my thinking on the most important things I have learned from executing the ideas in these two books 

## Our initial checklists

The Tuesday Company began with checklists in our QA.
I'm a big believer that almost all QA should be automated, but you have to be *very* smart in how you do that.
The first was just a complete checklist of all the essential features and questions to ask before you releasing something.
The second one was a list of possible kinds of features and the things that commonly go wrong with them.


## The Checklist Manifesto

The number one thing that The Checklist Manifesto changed was how much thought went into the creation of checklist.
Originally, I couldn't find any writing about checklists in software.
If you go to the [code complete wiki](https://en.wikipedia.org/wiki/Code_Complete) today though, you can find a pdf version of their checklists.
So really I just sat down with my QA analysts and worked for a few days coming up with two checklists.
After reading the Checklist Manifesto I began to do more reading about what makes a good checklist.
I also sought information about the various kinds of checklists.
I took the things I learned and incorporated them into our checklists to improve their quality.
The second thing that the Checklist Manifesto really influenced was how to help adoption.
One of the things the book does incredibly well is describe how, where, when, and why checklists did (and did not) work.
You should constantly be experimenting with the checklists to see what is effective and what isn't.
If you find that your engineers are always doing everything on a checklist, you should probably not use that checklist.
Likewise, you should regularly review problems that come up in your software.
I prefer the postmortem method, but you can use anything similar.
The idea is to move effective checks for revealed failures earlier into the development process.


## Code Complete Two
Code Complete Two gave us a set of templates to start with so we could create our own checklists.
One of the most time consuming parts of checklists is creating a base and then really getting them into the culture.
It took us a several days to create our first QA checklist.
It then took a few weeks of me following up with our QA engineer to verify that it was being used.
I also helped to modify it when there were problems or when it was impractical.
The checklists from Code Complete are useful in providing a starting point for having more in-depth conversation.
They are often either too large in scope or too specific to a particular task for me to use them.
I've been searching for, and trying to develop, a pull request checklist that is effective in most situations.
We currently use two lists, our own checklists with ideas taken from Code Complete Two. 


## Most important things I have learned
Checklists don't work out of the gate.
You have to test and refine them.
They are most effective when they are living documents.
They should change as we see new and different problems.
I often create various representations of checklists.
I usually have a word document for ones that I am not going to audit.
I have a form for ones that will have a paper trail or will be generating statistics.
Currently, every time a PR is finished today engineers fill out two surveys.
One for each of the two checklists that we have developed.
Those are then audited weekly for two reasons.
The first is to make sure that we catch any glaring mistakes.
The second is to look for questions that are not useful.
Next after all prod failures we have a postmortem.
That postmortem is then taken and the checklists are reviewed with an eye for adding in new items that could help.
Both of these processes are essential to making the checklists effective.
Also checklists cannot tell someone how to do their job.
If you find when you are starting to create a checklist that it is overly long, or too specific, you might be falling into that trap.
Checklists are meant to help people do the right thing every time.
