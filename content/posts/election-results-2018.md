---
title: Election Results 2018
date: 2018-12-10T09:03:54-04:00
draft: false
---
## Election 2018
[The Tuesday Company](https://www.tuesdaycompany.com/) had the honor to work on many races over the [midterm elections](https://www.tuesdaycompany.com/midterms/).
The team worked incredibly hard and managed to help in over 118 different races.
We had a chance to scale our technology on a national level and managed to have no production issues despite having an large magnitude of increased traffic.
We also learned some valuable lessons about what it means for a team to grow; both in the number of developers and in the number of users.

## Things that worked
###  Get it working first and then you can optimize it later

You hear this all the time, and I've done it in several places, but it is nice to see it from start to finish.
The team worked hard to get something that fit the market quickly and provided immediate value for our customers.
Then as new campaigns joined we would begin to see the cracks in our initial go.
My team's focus and my own focus while building the software was to create something that did just what the business asked.
We never worried if it was the most elegant or efficient solution.
Then as the users on-boarded, we would quickly see the performance problems and work to fix them systematically. 

###  Focus on the highest impact features first

Here again is another phrase that you hear over and over with agile teams.
The strategy is to think big picture, and focus on what would give customers the most bang for their buck.
We would often push back gold plating or pair down features to give the customer something that we thought might work.
Then when we would see that it was working, we could and would continue to upgrade it.
This was a little difficult for our client success team as they preferred to have things in the final format first.
However, after we got into a rhythm and worked with them streamline the process of updating clients on change.
We found that it was an incredibly successful strategy.
It also gave us another way to collect feedback and put it back into the machine.

###  Make sure you do proper capacity testing

We are a small team, but we worked with all of the red to blue congressional races in this midterm.
This meant two things.
First, we had to know the limits of where our software would start to break down.
Second that we had to leverage our technology with the shifting size of the user base.
We managed to do this only through regular capacity testing with production like traffic.
We used a variety of tools and techniques and managed to hit a home run in this area.

## Things that did not work
###  Never put queries into for loops

There were some places where we slipped up a bit.
Occasionally, either through lack of experience or rush to get a feature, we would shoot ourselves in the foot a bit.
One place was putting SQL queries into loops.
We managed to find and solve these problems and they never led to full-on disruption in production.
I believe factoring these out a bit earlier in the process would not have added to much time to the development process.
It also would have saved us a bit of time in coming back and having to optimize certain bits of the code.
That said, this sort of mistake can be expected when you are trying to find the most effective product for your market.

###  Letting deadlines stop communication

One thing that I was surprised at how often it affected our ability to provide quality software was deadlines tripping us up.
There were a few times where because we were busy dealing with the election or some other deadline communications would break down between team members.
This never blew up in our face in a spectacular way, but it created some heartache that could have been avoided.
Making sure that team members do proper hand offs even when a lot is going on is critical.
Also making sure that the culture is such that everyone feels a responsibility to make sure those hand offs also happen.


## Wrap up

I want to say thank you to all the people that helped make this election cycle process.
My co-founders, my employees, all the hard working candidates, and their staffs.
It has been an incredible journey and a nice milestone in the bigger picture.
I am lucky that every day I get to get up and work on something that I care deeply about.
