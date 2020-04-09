---
title: Applications Integrating On The Database
date: 2020-03-25T09:03:54-04:00
draft: false
description: 
---
## The problem
You shouldn't do it.
You've read it before.
Don't integrate applications on the database.
The advice is relatively straightforward, but it can often be difficult to understand.
My first piece of advice is that when you have a new process it should have it's own database.
While the overhead is often real, it is minimal compared to the pain caused when you want to pull the two processes apart later.
However, I find that people often fight against having two databases from the start.
They see the immediate pain, and either don't know (or can't see) the pain of the future.
If they know and then discount the immediate pain, they may be overly discounting future pain. 
There is a name for this sort of bias, it is called [hyperbolic discounting](https://en.wikipedia.org/wiki/Hyperbolic_discounting).
Regardless, sometimes you decide that you are going to have a single database for multiple processes.
Maybe it is because recreating many data models isn't your idea of fun.
Perhaps it is because figuring out how to broker the communication between two apps is no fun.
So, you copy the models, or you pull them into a shared location.
You allow the database to decide what consistency looks like.
This runs smoothly for months (or even years).
Then one day you realize you can't change one application without changing the other.
You realize they are tightly coupled, practically speaking, even if they aren't conceptually speaking.

## Software language, column purpose, and process ownership
If more than one system process uses a particular table/column then automatically the deployment of changes to that column/table must take that process into account.
How it must take those changes into account depends on the how the services use the table/column.
For instance; there are readable backwards compatible changes and writeable backwards compatible changes.
These changes also depend on the language.
One example may be as follows; *service A* reads and writes to column 1 and *service B* only reads from column 1.
Both services are in python.
Currently column 1 is a string enum.
If you then were to expand that to a varchar or text field, you could only change *service A* provided that *service B* doesn't do anything based on the enum type.
If it is just a reporting service then you are done.
Let's look at the same scenario in Java. 
If your type reflects the enum type that comes from the database you are likely to have to change both services.
This coupling likely started out because when you built *service A* and *service B* it was more straightforward for them to directly access the same database.
To make these independent only one process should be allowed to read or write from a column.


## So how to decide when to stop integrating on the DB
Let's assume that we have two modules using a database column.
We can look for a few indications that it is time to separate out a process and a database.
If *module B* is simple and doesn't need to be changed unless we are making changes to *module A*,
then this is a good indication that it is time to create a separate column for a new *service B* based on *module B*.
This change requires we weigh the complexity of deploying two services and coordinating data updates and communication vs the ability to work on each module independently.
Once we start to break it down in these terms we can start to make strategic decisions.
For instance, if *module A* and *module B* are developed by a single team and you don't expect many changes in the data model then the answer becomes clear - 
You should probably leave well enough alone.
Likewise, if *module A* and *module B* are owned by two different teams or need drastically different deployment cycles then it becomes clear that you should probably separate out *service A* and *service B*.
This means you have to deal with the overhead of communication between the services.


## How does the database that you choose change the problem
Interestingly Google has been doing a lot of work around distributed databases.
Most applications need to deal with distributed systems in order to manage scale.
People end up sharding their MySQL database because a single large vertical instance can't meet or is too expensive for traffic levels.
So in some sense the choice of database gives you another lever to pull in terms of where the complexity lives.
In particular you can choose to model many small services with their own database if your data shards well.
On the other hand you could decide to deal with eventual consistency on the application level so that you can have databases that span the globe.

## Isn't it easier to do it later
People like monoliths.
Why?
They are easy to work with.
You have one place for everything.
However, they aren't fun forever.
Anyone that has worked at scale learns that there are limits to what you can do in a monolith.
Eventually team structure or process needs require that you split up your monolith.
So why would you want to start by splitting services on the database level?
Why not decide that you don't know as much as you will tomorrow so you should wait until you have pain?
There are two reasons I argue that you should split early.
The first reason is that combining data models is generally easier than splitting them.
The second reason is that large scale conceptual differences are fairly consistent markers that identify a need for a different process.
This can work in reverse.
If you are missing the large scale conceptual difference, then perhaps it makes sense to wait until you feel the pain.
Though I would recommend that you "up your pain meter", since it is generally very difficult to split data once it is used by two modules or processes.
