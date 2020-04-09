---
title: Pairing Vs Code Review
date: 2018-03-27T09:03:54-04:00
draft: false
---
##### Note This was written mostly immediately after I joined CoreOS and then updated this last week.
I recently started working at [CoreOS](https://coreos.com/) and one thing that they do there for every change of code is a code review.
I have never had code reviews before.
I've mostly worked in environments where I have paired directly with people.
So I wanted to do a quick review on the differences between the two methods.

### Communication
I think that programming is a team sport and people need a bunch of communication in order to get anything done.
That said, I want to talk about communication between team members when you are pairing vs when you are doing code reviews.
I think that the communication becomes incredibly slow when you are doing code reviews.
There are couple of reasons for this happening.
When you are pairing, you have to communicate for it to be effective.
This means as disagreements come up, you naturally ask others for their opinions and you work through those differences to come up with a better implementation than would have occurred alone.
It also means you don't have to wait for someone to give you feedback.
You get it almost instantaneously as you are coding.
It also means if you have a question about feedback you get can ask and have it answered immediately.
When you are doing code reviews, this high throughput communication goes away.
When you want to bounce an idea off of someone, it is more difficult because others don't have context.
How could they?
You have been coding without any input from them.
It is also more difficult because often you go through asynchronous communication channels.
For instance you might slack someone, or put a comment on Github.
I think there are two main causes for this.
Sometimes people are just busy.
Sometimes, if people aren't busy it can be cumbersome to have to reach out to people about questions.

### Code quality
I don't think that code quality changes that much with some caveats.
If you are bringing on new people, their code quality will definitely be lower than other experienced people (all other things being equal).
There are a few reasons for this.
Different teams have different coding conventions and It can take time to learn the new habits of one team or unlearn old habits from other teams.
New employees have to read an awful lot and this can be a problem.
Reading code is hard, and it only gets harder when the code is poorly written.
People remember their own code and the code that I've always known best is the code that I've written myself.

### Speed
Speed definitely slows down when you do code reviews as compared to pair programming.
I've often heard people make the argument that in order to have the same output as two people working separately you have to go twice as fast.
I have a couple of reasons why I think that's false.
As you write code, it is easy to get stuck in a thought process, regardless of whether or not it is a good way to go.
No one codes in a vacuum and people get distracted regularly during the course of their day.
They spend a few minutes here on a social media platform.
They spend a few minutes there on a social media platform.
People are much less likely to interrupt a pair than they are to distract a single worker.
Finally you can only hold so much in your brain at one time.
Sharing what code you hold in your head eases the cognitive load which allows you to reuse more code or think of better solutions as you work.
The flip side of this is while you are doing during code reviews there are often lots of time when the code is just sitting.
To combat that stasis, I often see developers starting on their next story.
This wouldn't be bad except the time it takes to context switch back and forth can be large.
This is even more true if you require manual testing that might require database or external integration setup.
However, sometimes when pairing you can't do certain tasks in parallel, and depending on your situation might mean it makes no sense at all.


### Conclusion
I still think that pairing is superior to code review.
However just like almost any plan you make in life your actions should depend on the situation.
It is best to either pair or have a code review than have no review at all even though neither system is foolproof.
If you pair and someone isn't paying attention or if you review code and you aren't paying attention, bugs can creep into your code.
Or maybe no matter what, bugs will creep into your code.
The real question is do you have strategies to mitigate and remedy the negative effects?