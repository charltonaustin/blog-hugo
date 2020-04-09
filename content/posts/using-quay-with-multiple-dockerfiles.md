---
title: Using Quay With Multiple Dockerfiles
date: 2017-04-08T09:03:54-04:00
draft: false
---
# The problem

Let's say you have the following folder layout:
```
app1/
app2/
shared-infra-resources/
continuous-integration/
                      app1/
                          app1.Dockerfile
                      app2/
                          app2.Dockerfile
```
And you would like to build app1.Dockerfile and app2.Dockerfile in your Docker registry.

# The Solution

If you are using [DockerHub](https://hub.docker.com).
Then you already can't.
DockerHub doesn't allow you to specify a context separate from your Dockerfile location.
At least they did not as of April 8, 2017.
So you can't specify use the continuous-integration/app2/app2.Dockerfile as the Dockerfile location and the root as the context.
However, if you are using [Quay](https://quay.io) then you are off to a good start.
Quay has a built in build system that you can use to build your repository.
In the UI you can either upload a dockerfile or an archive and it will build it.
Via the [API](https://docs.quay.io/api/) you can do something very similar, you can request a build on behalf of an organization or a user by sending a post request to https://quay.io/v1/repository/<organization or user name>/<repository name>/build/ .
If you look at the [swagger docs](http://docs.quay.io/api/swagger/) you will see you can pass in an archive url, a context, a dockerfile path, and some docker tags.
The archive url is simply a url where you can download your build files.
The context is the folder in your buildfile that the docker build will be executed from.
The dockerfile path is the location and name of the dockerfile.
Finally docker tags are arbitrary tag names that can be used to identify your build, version, or whatever other metadata you might want associated with your docker image.
The request might look something like this.
```bash
export HEADERS=--header "Content-Type:application/json" --header "Authorization:Bearer <my oauth token or my robot token>"
echo "{\"archive_url\":\"https://path/to/hosted/gzip\",\"docker_tags\":[\"<my tag here>\"],\"context\":\"/my/context\",\"dockerfile_path\":\"/my/context/<dockerfile name>\"}" > post.json
curl -X POST -d @post.json https://quay.io/api/v1/repository/<organization or user name>/<repository name>/build/ $HEADERS
```


# Wrap up

So why might you do this rather than just running a docker build in your CI?
Well there are a couple of answers to this.
You might have your continuous integration, CI, centered on docker images already and you don't want to run [docker in docker](https://jpetazzo.github.io/2015/09/03/do-not-use-docker-in-docker-for-ci/) during the build process.
This means you can have your CI server watch your version control repository and simply submit an archive of your repository when you want to build a new docker image.
Another reason to prefer this method over simply setting up a git webhook in Quay is the use of tags.
In the current UI there is no place to specify custom tags when creating the webhook.
This can make it difficult for your CI server to come along later and pick up the correct container image for deployment.
I think in a future post I will talk about how to setup a full CD pipeline using a mono repository and multiple Dockerfile to clarify this point.

