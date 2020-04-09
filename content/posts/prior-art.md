---
title: Prior Art
date: 2019-02-18T09:03:54-04:00
draft: false
---
## The Problem
Sometimes there is the "right way" and other times there is "how it is done in the codebase".
I would argue that a majority of the time the "right way" is the "how it is done in the codebase".
This might seem surprising, but generally I think consistency is incredibly important in a codebase.
So while at first glance a practice might not be the accepted best practice, you shouldn't change the codebase unless you can change everything over to the best practice.
Recently an employee that was working on a part of the codebase argued that there were sections that weren't following best practices.
In particular the infrastructure code was centralized in one place rather than spread throughout the code in modules.
He changed the newest codebase to test out the theory.
Up to this point I was completely on board with his decisions.
You see a practice that doesn't match with best practices, you have a an idea on how to fix it, and you try it out.
It was the next steps that I really didn't agree with.
At this point everyone agreed it was better to go ahead and have the infrastructure code live close to the code.
But due to time constraints the rest of the code was not moved over to the new method.

## Either shift everything, make a plan to shift everything in parts, or roll back and keep consistent practices
At this point you have a few different options.
1. You simply roll back the changes and wait until you have more time to implement the change.
2. You go and update all older relevant sections to the new way.
3. You keep the changes, but you articulate a plan for the whole team to make the changes.

Now each of these has their own set of drawbacks and advantages.
For instance, rolling back the changes and continuing with the old method tends to be the fastest way forward, but you don't have the new practice.
Updating everything all at once tends to be tedious and difficult for the engineer that makes the changes, but it makes sure you have consistency.
Keeping the changes is neither tedious for the engineer nor do you have the best practice everywhere, but at least you are making a start.
The last one is the most tricky though.
You have to articulate a plan that makes sure you make progress on on the change and it tends to work best when you get the whole team on board.

## The one thing you can't do
The one thing you can't do is simply leave it in a half baked state.
This is the where I think most engineers tend to fail.
I would argue that it is worse than simply keeping the less then best practice for the sake of consistency until you can safely make the move.
At least with a consistent, though admittedly not best, practice there is one right way to do things and so refactoring, on-boarding, and learning the codebase all become significantly easier.
This is especially true with operational tasks where time pressure can be high and a need to not make mistakes is crucial.
It also means there is a single right way to do tasks.
Most of the time it isn't that people don't plan on not finishing implementation of a new practice.
Rather the reason it never changes generally stems from two places.
1. Poor communication
2. Poor update plan

The poor communication can come in many forms.
Maybe the whole team doesn't know that updates should occur.
Maybe the team members don't know how to update.
Maybe the team members know they should and how to update, but don't know when updates should occur.

The poor planning can have similar symptoms.
For instance maybe you communicate a plan and it goes something like this;
  "The next time someone touches this infra code we will update to the next practice."
Infra code can go a long time without ever needing to be updated.
Often the longer systems live in a half state the more difficult changes can be to make.
Also it more likely someone will see the old example and perpetuate it in a new part of the code base.
The problem here wasn't that you didn't communicate "when", "where", "how", or "what".
It is that the "when" might not come for a long time, if ever.

An option for modification may be to set a fixed interval for code updates.
We will spend 1 day a week until we get everything moved over to the new practice, unless you change the infra code and that should be updated immediately.
The key here is to consider how long it is going to take, as well as taking into account the risks for your particular project.
