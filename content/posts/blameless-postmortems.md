---
title: Blameless Postmortems
date: 2019-01-14T09:03:54-04:00
draft: false
---
### Subject
Why should we have blameless postmortems?

### Context
I recently read [a blog post](https://codeascraft.com/2012/05/22/blameless-postmortems/) about blameless postmortems from [Etsy](https://www.etsy.com/).
In it there was a simple question about why software engineers shouldn't be punished:
>Why shouldn’t they be punished or reprimanded?
>Because an engineer who thinks they’re going to be reprimanded are disincentivized to give the details necessary to get an understanding of the mechanism, pathology, and operation of the failure.
>This lack of understanding of how the accident occurred all but guarantees that it will repeat.
>If not with the original engineer, another one in the future.

### Thoughts
I'm a big person on why we do things, and in our team, we often have blameless postmortems.
Recently I was talking with a friend about why and this quote came up.
So I wanted to take some time to think about why we have blameless postmortems.
In my mind, the above is a practical reason why people should avoid placing blame, but I have a more theoretical basis as well.
Complex systems are well complex.
They generally have several systems that interact with one another in sometimes unexpected ways.
Wikipedia defines complex systems as systems that, "have distinct properties that arise from these relationships, such as nonlinearity, emergence, spontaneous order, adaptation, and feedback loops, among others."
The ability of someone to hold in their mind all the possible states where something can go wrong is impossible.
These properties which are inherent in the systems guarantee that one person can not hold all places that something can go wrong in their mind.
Let's take an example.
Nonlinear in simple terms means that the change of output from a system is not proportional to the input to that system.
Since a small change in input can cause such a large change in output even small mistakes in the input can cause huge problems.
Further nonlinear systems can seem linear depending on the scale you are looking at.
Just think of exponential growth early in the graph it is almost flat and you can approximate the system with a line very well.
Later in the lifecycle though this model will fail and it can do it rather rapidly and with out much warning.
This means people are likely to miss this kind of behavior and when they do there will be failures.
Should we blame people for not being able to predict something that is essentially unpredictable?
I would say no.
Instead, we should deal with this, by encouraging people to build systems that recover quickly and can cope with failing.
Further, unless there is gross negligence, we should take each failure as learning for the future.
We want people to understand that we acknowledge that complex systems can be complicated and work to make them safe.

### Conclusion
Blame is a tricky beast and not only does it have practical bad effects like those mentioned in the Etsy article, but there are good reasons that we should not blame people from a more theoretical standpoint.
