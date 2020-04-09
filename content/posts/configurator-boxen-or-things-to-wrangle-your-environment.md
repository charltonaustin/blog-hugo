---
title: Configurator Boxen Or Things To Wrangle Your Environment
date: 2015-09-19T09:03:54-04:00
draft: false
---
### The problem 


One thing that I keep seeing consistently regardless of whether I'm working as a full time employee or as consultant, and that I've literally seen from coast to coast is developers and testers trying to simplify their local environments.  Increasingly, companies are using multiple services that allow for more module pieces of software (think micro-services).  This is nice because it can often simplify coding complexity and dependency graphs, but the flip side is that deployments can become more complex.


This increasing deployment complexity comes at a cost. I've had to maintain environments with multiple instance of redis, mongo, mysql, memcachd, plus my microservices all up and running and not conflicting on any ports. This can lead to developers and testers finding it more and more difficult to setup their local environment to run an application. Now on the whole I'm not a fan of manual testing, but sometimes it can be prohibitively expense to automate a test. I'll probably write a post about seams and how people should use contract tests and stubs on either side of an integration, but for now let us just assume you can't always get away with a simple development environment.


### The Three Ways


So what do you do. I've seen three basic approaches to solving this problem. The first is the boxen solution.  Boxen is a layer between puppet and OSX which allows you to automate the set up of a developer/tester machine. The second approach is the Vagrant way or creating virtual environments of your production environment and allowing developers and testers some way of deploying your applications to the virtualized environment. The final way is the docker way, or containerizing you applications and running that inside the development environment. Obviously you can have some overlap of all three methods.


### First what do they all have in common? 


Each of these ultimately makes it easier to stand up an application and see what it does. All of them have their own pluses and minuses.


### Boxen Way


First lets talk about boxen (note I have also done this by convincing people that all developers should have an ubuntu box and then using puppet). Basically boxen uses puppet to set up a development environment.  It should include app dependencies like MySql as well as source things like the source code, and build tools. The nice thing about this approach is if you are interested in pair programming then this really simplifies pair rotation. All development machines are the same and you can keep the development environment management scripts in version control and ultimately only solve development environment issues one time. The drawback is if your development environment isn't exactly like your production environment then guess what you don't deploy the same way and you have to maintain several different environment management scripts. Also developers don't like to give up control over their dev machines. It is definitely a team over the individual sort of situation.


### Vagrant Way

Next lets talk about the Vagrant method. This doesn't have to use Vagrant or VirtualBox.  You can basically use any kind of virtualization.  You simply create a virtualized replica of your production environment and distribute that to developers. This is great because you can use your configuration management scripts like you would in production. Also developers have control over their local machines. Which means they don't really have to install anything other than the virtualization software. The downside is there can be a lot of waiting. Waiting for the base box to download. Waiting for virtualized machines to come up. Waiting for your application to respond. Also there are a thousand glitches that happen with a virtual machines that don't happen with the host. If you have multiple applications that need multiple virtual machines the networking and system requirements to run them all at once can become prohibitively expense.


### Docker Way


This brings us to our final method. What I call the docker method. This is different from the Vagrant method in that you containerize your app rather than the environment. It somewhat suffers from the same problems as above in the sense that it can be complicated to wire together all your docker containers, however some things aren't as bad. For instance a docker container can start a lot faster than a VirtualBox image. It also consumes a lot less resources. There are some other benefits as well. You can often set up the deployment of docker containers to use the same scripts no matter what environment you are deploying too. The biggest drawback of this method however is docker doesn't run natively on OSX so if most of your developers use a mac it can get frustrating quickly.


### Combination Way


Finally I have seen a combination of these three methods. At my current company, [IntentMedia](http://intentmedia.com/) we dockerized many of our applications and their dependencies and then use virtual box so we can deploy the containerized apps to production like images. We also created an app called configurator that simplifies the deployment and maintenance of the different apps and environments. It's nice because the cognitive load when an upstream application changes is a lot less, but I've also sat around waiting while a VirtualBox image was been turned off and re-provisioned by Vagrant, and it can be difficult to keep up with changes in the configurator app. 