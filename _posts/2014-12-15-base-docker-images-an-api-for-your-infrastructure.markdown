---
layout: post
title:  "Base Docker Images, an API into your Infrastructure"
date:   2014-12-15 11:42:50
categories: docker
---
Docker provides a very developer-friendly mechanism for standardizing the way applications are deployed to your infrastructure, we all know that. We also all love standardization.

## Base Image

One way to get your containers standardized in some fashion is to release a base image which all containers in your organization are expected to extend from. Treat this base container as API into your infrastructure and standards will be enforced strictly through convention. Here are some concepts to follow in order to treat your base images as an API.

## Versioning

It stands without reason that any published API should carry with it some form of versioning information. For every new release of your base image, make sure you tag that container with a specific version and update the "latest" tag.

{% highlight bash %}
> docker build -t baseimage .
> docker tag baseimage docker.enlightenmint.com/baseimage:1
> docker tag baseimage docker.enlightenmint.com/baseimage:latest
> docker push docker.enlightenmint.com/baseimage:1
> docker push docker.enlightenmint.com/baseimage:latest

{% endhighlight %}

## Releasing

With each new version of the base image, make sure that the development team receives a notification with the release notes. Detail the changes that have occurred since the last release and any expectations for developers to begin to follow clearly stated.

## API

The following is an example to get you thinking about what your own infrastructure API through a base image could look like.

### /application

Standardize the location for where to place all builds by creating a standard application folder (for example /application) in your base image. Within that location, you can very easily create a few volumes.

### /application/logging

If you are running containers in your infrastructure, hopefully you already have a system for consolidating logs from your services.

Setup a volume in your base images application folder where you expect developers to place their logging files. This is easiest on the developer if you create the volume at the root of the application folder you create, for example /application/logging.

### /application/data

Hopefully most of your containers coming from developers are stateless and do not need access to long-term storage. There are cases though for backing services that require persistent data, and hence /application/data is the volume that can be used to place data that must be backed up from the running container.

## Environment Variables

Publish a set of environment variables that the developers can expect to be passed into the container for certain configuration options such as AWS access keys or connection strings to backing services.
