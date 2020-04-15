---
title: Points What Are They Good For
date: 
draft: true
description: "Why story points should be tied to time and what that does for you."
tags: [
    "management",
    "books",
    "entrepreneurship",
]
categories : [
    "management",
]
---
## Should story points be tied to time
I often hear people say story point shouldn't be tied to time.
Instead they should be based on complexity.
This can be for a variety of reason.
[Here](https://rubygarage.org/blog/3-reasons-to-estimate-with-story-points) for instance three reasons are cited.
> Some tasks are difficult to estimate precisely.

> If one developer estimates a project but another completes the task, the estimate becomes invalid. The time needed to complete a task will vary based on a developer’s level of experience.

> People generally underestimate obstacles they might face and consider only the best-case scenario.

Each of which I think has a concrete problem that should be solved.
Like yes some tasks are difficult to estimate, but so what.
Some surgeries are harder than others does that mean you shouldn't do them?
Or some developers are better than other developers.
But that means you should work to figure out how long it will take your team.
Not change estimates from time to complexity based.
By moving from time based estimates to complexity estimates you obscure your problems.
I think the root of most objections come from one simple problem.
When you tie story points to time people see them as binding estimates.
These estimates can box the team in.
I don't disagree with the reasoning.
Time estimates can definitely seem binding.
However, I don't come to the same conclusion either.
That doesnt' mean you should not use time estimates.
Instead story points should be based on ideal days.
They should be an internal tool for the team is making estimates.
Timelines that are created should be based on these, but not directly.
One should take historical estimates and compare them with reality.
The only thing that should be shared outside the team is the model.
This does two things.
First it allows engineers to think in concrete terms of time.
It also allows for simple diagnostic of time vs story points.
Imagine a story estimate is for 1 ideal day and it takes 14 days.
It is easy to see that the estimate is a order of magnitude off.
This is good.
The team knows that the estimate was off and can endeavour to explain why.
Imagine a story that is medium sized though.
It took 14 days as well.
Is this a bad thing?
Without the surrounding context it is pretty difficult asses.
Like what is a medium size story anyways.
Is it based on cyclomatic complexity.
No one goes through the code afterwards and checks how many branches are there.
But by doing that you actually lose a good internal diagnostic.
Second it gives you accurate timelines within some uncertainty that can be shared.
And it doesn't sacrifice points as a diagnostic tool.

## Why is it bad to show internal estimates to external stakeholders
I have one word, [Hawthorne effect](https://en.wikipedia.org/wiki/Hawthorne_effect).
Generally external stakeholders hold the team accountable for that timeline.
They often come back and say, "Hey you said this would take a week and it took a year."
So you end up having pathological behavior.
Like a story that is estimated for a year, but takes an hour.
So you have to separate these things out.
You can't just measure time vs estimate.
I would argue you should be looking at other metrics that improve engineers.
Align the Hawthorne effect with something that will make an engineer better.

## How to create a model?
Well I find that taking historical data is usually the most effective.
This has one glaring problem.
What do you do when there is no historical data.
We'll come back to that.
Assume that you do have historical data.
The first thing I would do is measure how long a stories take.
You can then come up with things like averages, medians, and standard deviations.
Those can help you chart out how long things are going to take.
You can even compare the actual times with estimates and create more sophisticated models.
What about when you don't have any historical data.
Well I would suggest *gasp* not having time based road maps.
Focus on your hiring and trust the people that you have hired.
Assume they are going as fast as they can.
Create a mental model of what you think is important and work on those things.












