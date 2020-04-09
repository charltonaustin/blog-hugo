---
title: Moving From Blogger To Clojure
date: 2016-08-09T09:03:54-04:00
draft: false
---
Recently I had a problem with [my old blog](http://charltonaustin.blogspot.com/) where writing new blog posts would be buggy. Things like code formatting wouldn't be consistent and so I moved to images of code. Which is so silly considering that it is just text. I've always had a bit of a problem with blogspot. I disliked the lack of control over how things are displayed and served up content. I knew that I wanted to move to HTTPS and that I was tired of working within the confines of WYSIWYG editors. So I took the plunge and decided to create a new clojure app and bought some hosting on [Digital Ocean](https://www.digitalocean.com/).

## Markdown for the win
I decided that I wanted to keep two repositories for my blog. One that was just the [content](https://github.com/charltonaustin/blog-entries) and a second one a [webpage](https://github.com/charltonaustin/blog) to display that content. I briefly toyed with the idea of using github pages, but in the end I decided against it because I was interested in keeping track of the number of visitors to my website and having a place for comments. It took some time to find the time to actually get everything moved over, but now that I've moved it I'm really happy with the solution. As it stands if I want to update the blog I simply update the content and within a few mins I see the updates on my webpage. 

## Clojure and Luminus 
If you haven't had a chance to use [Luminus](http://www.luminusweb.net/) before you should give it a spin. I've written servers in everything Struts to Spring to Dropwizard and I like Luminus a lot. The docs are great and it has been a joy to write Clojure. Everything from actually writing the app to deploying was pretty smooth. The one thing that still drives me crazy in Clojure is debugging. Writing a Clojure app can be challenging depending on what editor or IDE you are using. The learning curve of emacs can be steep, but once you've got it figured out debugging comes pretty easily and honestly writing code is a joy.

## NGINX and Let's Encrypt
The webserver I'm using is NGINX. Setting it up is pretty straightforward. There are some great docs both at [Digital Ocean](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-14-04-lts) and at [Luminus](http://www.luminusweb.net/docs/deployment.md#fronting_with_nginx). I also used certbot from the [EFF](https://certbot.eff.org/). I love that I can enforce HTTPS using [HSTS](https://en.wikipedia.org/wiki/HTTP_Strict_Transport_Security). While I do want to keep track of how many visitors I have I don't really care about their personal information and I think at this point every one pretty much agrees we should be using SSL for everything. One thing that I will probably blog about in the future is how to set up auto cert updates, but with certbot that might not be necessary as the instructions look pretty straight forward. 

## Finally it's all updated
Now that it is all updated I should get some of the back logged posts that I have out and start updating this thing more regularly. Also I really want to make some more blog posts about what I'm teaching my students and JavaScript.
