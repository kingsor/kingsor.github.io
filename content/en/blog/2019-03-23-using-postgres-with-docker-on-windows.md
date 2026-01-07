---
date: "2019-03-23"
title: Using Postgres with Docker on Windows
summary: How I'm getting started learning Postgres, without installing it on my machine but using Docker instead
categories:
- developer
tags:
- database
- postgres
- docker
- docker-compose
- backend
series:
- series-docker-compose
---

I was looking for something short about getting started with Docker Compose and I found this video: [Docker Compose in 12 Minutes](https://www.youtube.com/watch?v=Qw9zlE3t8Ko) (Mar 14, 2017) - *Learn how to use Docker Compose to run multi-container applications easily. This is the second video in this Docker series.*

It is a nice video with a very simple example well explained. There is a project to play with on [GitHub](https://github.com/jakewright/tutorials/tree/master/docker/02-docker-compose).

After this sample I did remember this post: [ASP.Net Core Web API with Docker Compose, PostgreSQL and EF Core.](https://medium.com/front-end-weekly/net-core-web-api-with-docker-compose-postgresql-and-ef-core-21f47351224f) and I like to try it. Here the [source code](https://github.com/rajvirtual/docker-aspnetcore-postgresql).

But before that I wanted to start using PostgreSql with Docker. So I followed this post: [Setup PostgreSQL on Windows with Docker](https://elanderson.net/2018/02/setup-postgresql-on-windows-with-docker/) and this one: [Stop Installing Postgres on Your Laptop : Use Docker Instead](https://blog.dahanne.net/2015/01/19/stop-installing-postgres-on-your-laptop-use-docker-instead/).

So this was my first idea for a docker container:

```bash
docker run -p 5432:5432 --name postgres_db -e POSTGRES_PASSWORD=password -d postgres
```

with all databases data in the container.

Looking on [Postgres Official Docker Image](https://hub.docker.com/_/postgres/) I found the use of mapping a host's folder with postgres data folder inside the container in order to maintain the data if you delete the container or want to use that data with another container for another project.

So this was the evolution:

```bash
docker run -p 5432:5432 --name postgres_db -e POSTGRES_PASSWORD=password -v /c/Users/myuser/DockerProjects/postgres/postgres_db:/var/lib/postgresql/data -d postgres
```

But with Docker Toolbox for Windows 10 Home (my case), my postgres database didn't work.

Looking with `docker logs postgres_db` I found the problem:

```bash
FATAL: data directory "/var/lib/postgresql/data" has wrong ownership
```

[Here](https://github.com/docker-library/postgres/issues/435) one solution:


```yml
postgresql:
  image: postgres:93
  volumes:
  - postgres:/var/lib/postgresql/data
  - ./postgresql/conf:/etc/postgresql/
...
volumes:
  postgres:
```

[Here](https://github.com/cytopia/devilbox/issues/175) something similar:

To summarize: The workaround is to create a (local) volume with:

```bash
$ docker volume create --name data-postgresql --driver local
```

And the `docker-compose.yml` looks something like that:

```yml
services:
  pgsql:
    volumes:
      - data-postgresql:/var/lib/postgresql

volumes:
  data-postgresql:
    external: true
```

I also looked at [Start a container with a volume](https://docs.docker.com/storage/volumes/#start-a-container-with-a-volume) in the [Use Volume - Docker Documentation](https://docs.docker.com/storage/volumes/).


This is my solution for a test container:

```bash
$ docker volume create --name postgres-volume

$ docker run -p 5432:5432 --name postgres_db -e POSTGRES_PASSWORD=password -v postgres-volume:/var/lib/postgresql/data -d postgres
```

At the end I'm able to play with Postgres :-)