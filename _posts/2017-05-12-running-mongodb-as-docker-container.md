---
layout: post
title: "Running MongoDB as a Docker container"
description: "Installing and creating a MongoDB database in a Docker container."
category: Developer
draft: false
tags:
- development
- side-project
- mondodb
- docker
- github
---

I've tried to run a mongo docker image on docker toolbox on my machine. But there was a problem with the http/https proxy settings.

So I exported the oracle docker image (that I use for work), removed the docker-machine, created it again with no proxy configuration, imported the previous exported image and everything worked fine as before.

Then after reading this post [Running MongoDB as a Docker container](https://www.thachmai.info/2015/04/30/running-mongodb-container/), I was able to start a MongoDB instance running the following command:

```bash
docker run --name mongodb -d -p 27017:27017 mongo
```

This way, I'm able to connect to the mongodb instance via mongo cli, Robo 3T and my code.

From the host machine I started `mongo 192.168.99.100:27017` and I tested some mongo commands.
Then I run [ShortUrl](https://github.com/kingsor/ShortURL) project against that mongodb instance. And everything went fine.