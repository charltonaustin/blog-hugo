---
title: Points What Are They Good For
date: 2023-04-11
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
This does two things.
# Should Story Points Be Tied to Time

I often hear people say that story points shouldn't be tied to time. Instead, they should be based on complexity. This can be for a variety of reasons. For instance, three reasons are cited in [this article](https://rubygarage.org/blog/3-reasons-to-estimate-with-story-points):

- Some tasks are difficult to estimate precisely.
- If one developer estimates a project but another completes the task, the estimate becomes invalid. The time needed to complete a task will vary based on a developer's level of experience.
- People generally underestimate obstacles they might face and consider only the best-case scenario.

Each of these reasons presents a concrete problem that should be solved. For example, some tasks are difficult to estimate, but that doesn't mean you should avoid estimating them. Similarly, different developers may complete tasks at different rates, but that means you should work to figure out how long it will take your team, not change estimates from time to complexity-based.

By moving from time-based estimates to complexity estimates, you obscure your problems. I think the root of most objections comes from one simple problem: when you tie story points to time, people see them as binding estimates. These estimates can box the team in.

I don't disagree with the reasoning that time estimates can seem binding. However, I don't come to the same conclusion. That doesn't mean you should not use time estimates. Instead, story points should be based on ideal days. They should be an internal tool for the team to make estimates. Timelines that are created should be based on these, but not directly. One should take historical estimates and compare them with reality. The only thing that should be shared outside the team is the model.

This does two things. First, it allows engineers to think in concrete terms of time. It also allows for a simple diagnostic of time vs. story points. For example, if a story estimate is for one ideal day and it takes 14 days, it is easy to see that the estimate is an order of magnitude off. This is good. The team knows that the estimate was off and can endeavor to explain why. Imagine a story that is medium-sized, though. It took 14 days as well. Without the surrounding context, it is pretty difficult to assess. Like, what is a medium-size story anyway? Is it based on cyclomatic complexity? No one goes through the code afterwards and checks how many branches are there. But by doing that, you actually lose a good internal diagnostic.

Second, it gives you accurate timelines within some uncertainty that can be shared. And it doesn't sacrifice points as a diagnostic tool.

## Why Is It Bad to Show Internal Estimates to External Stakeholders?

I have one word: the Hawthorne effect. Generally, external stakeholders hold the team accountable for that timeline. They often come back and say, "Hey, you said this would take a week and it took a year." So you end up having pathological behavior. Like a story that is estimated for a year but takes an hour. So you have to separate these things out. You can't just measure time vs. estimate. I would argue you should be looking at other metrics that improve engineers. Align the Hawthorne effect with something that will make an engineer better.

## How to Create a Model?

Well, I find that taking historical data is usually the most effective. This has one glaring problem. What do you do when there is no historical data? We'll come back to that.

Assume that you do have historical data. The first thing I would do is measure how long stories take. You can then come up with things like averages, medians, and standard
