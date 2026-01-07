---
date: "2018-04-30"
summary: This post is a note to self about my first steps in the path for learning
  to use Docker. I started deploying an asp.net core web api in a container.
categories:
- developer
tags:
- side-project
- asp-net-core
- docker
- web-api
title: Deploy ASP.NET Core 2 apps with Docker
---

*Published also at [The DEV Community](https://dev.to/kingsor/deploy-aspnet-core-2-apps-withdocker-598n) on May 3rd, 2018*

This post is a note to self about my first steps in the path for learning to use Docker. I started deploying an asp.net core web api in a container.


## Prerequisites
* Docker ([Windows](https://docs.docker.com/docker-for-windows/install/), [Mac](https://docs.docker.com/docker-for-mac/install/), [Docker Toolbox](https://docs.docker.com/toolbox/overview/))
* ASP.NET Core 2.0 SDK ([Here](https://www.microsoft.com/net/download/windows))

## Building a web api with asp.net 2.0
I created a simple todo list web api following this tutorial: [Create a Web API with ASP.NET Core and Visual Studio for Windows](https://docs.microsoft.com/en-us/aspnet/core/tutorials/first-web-api?view=aspnetcore-2.1).

In order to be able to test the web api without any specific client I added the [Swagger](https://swagger.io/) support to the project using [NSwag](https://github.com/RSuter/NSwag) package.

I accomplished this task following this tutorial: [Get started with NSwag and ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/tutorials/getting-started-with-nswag?view=aspnetcore-2.1).

The NSwag project provides tools to generate Swagger specifications from existing ASP.NET Web API controllers and provide an embedded [Swagger UI](https://swagger.io/swagger-ui/) to interact with the web api. 

The resulting project is [TodoCoreWebApi](https://github.com/kingsor/TodoCoreWebApi) hosted on GitHub.

## Adding a Dockerfile

The following is the Dockerfile for building the docker image.

```dockerfile

FROM microsoft/dotnet:2.0-sdk AS build-env
WORKDIR /app

# copy csproj and restore as distinct layers
COPY ./TodoCoreWebApi/*.csproj ./
RUN dotnet restore

# copy everything else and build
COPY ./TodoCoreWebApi/. ./
RUN dotnet publish -c Release -o out /p:PublishWithAspNetCoreTargetManifest="false"

# build runtime image
FROM microsoft/dotnet:2.0-runtime
WORKDIR /app
COPY --from=build-env /app/out .
# not valid for Heroku
# ENTRYPOINT ["dotnet", "TodoCoreWebApi.dll"]
# this is working
CMD ASPNETCORE_URLS="http://*:$PORT" dotnet TodoCoreWebApi.dll

```

To understand the ins and out of building a Docker image I followed this tutorial: [Building Docker Images for .NET Core Applications](https://docs.microsoft.com/en-us/dotnet/core/docker/building-net-docker-images). I used the [Dockerfile](https://github.com/dotnet/dotnet-docker/blob/master/samples/aspnetapp/Dockerfile) from the [aspnetsample](https://github.com/dotnet/dotnet-docker/tree/master/samples/aspnetapp) project available on the [.NET Core Docker Samples](https://github.com/dotnet/dotnet-docker/tree/master/samples) repository on GitHub.


## Building the Docker image

In the root of the project (where the Dockerfile is located) run this command:

```bash

docker build -t todo-core-webapi .

```

After the build process is completed you can run the image with this command:

```bash

docker run -it --rm -p 8080:80 --name TodoCoreWebApi todo-core-webapi

```

And test the web api at `http://localhost:8080`


## Pushing the image to DockerHub

First, you need to [register for a Docker ID](https://docs.docker.com/docker-id/#register-for-a-docker-id).

After that you can [create a new repository on DockerHub](https://docs.docker.com/docker-hub/repos/#creating-a-new-repository-on-docker-hub).

My Docker ID is [neetpiq](https://hub.docker.com/u/neetpiq/) and the repository for the image I generated is [neetpiq/todo-core-webapi](https://hub.docker.com/r/neetpiq/todo-core-webapi/).

Now, I need to tag my image.

```bash
docker tag todo-core-webapi neetpiq/todo-core-webapi:1.2
```

```bash
docker tag todo-core-webapi neetpiq/todo-core-webapi:latest
```

Finally, I can push the new version of my image.

```bash
docker push neetpiq/todo-core-webapi:1.2
```

```bash
docker push neetpiq/todo-core-webapi:latest
```

Now you may test this web api application image (latest) running this command:

```bash

docker run -d -p 8080:80 --name TodoCoreWebApi neetpiq/todo-core-webapi

```

Or if you want a specific version:

```bash

docker run -d -p 8080:80 --name TodoCoreWebApi neetpiq/todo-core-webapi:1.2

```

## Notes

### Windows Users
If you are using boot2docker on Windows ([Docker Toolbox](https://docs.docker.com/toolbox/overview/)), please note the following:

The Linux VM in the boot2docker VirtualBox maps the c/Users directory in the VM instance to the C:\Users folder in Windows. So be sure your source code for your worker is in a folder under C:\Users, then cd to that folder in the context of the VM (in Boot2Docker terminal) and run it from there.



## Resources
* [Building Docker Images for .NET Core Applications](https://docs.microsoft.com/en-us/dotnet/core/docker/building-net-docker-images)
* [Push Docker Images to Docker Hub](https://github.com/dotnet/dotnet-docker/blob/master/samples/dotnetapp/push-image-to-dockerhub.md)
* [ASP.NET Core Docker Production Sample](https://github.com/dotnet/dotnet-docker/tree/master/samples/aspnetapp)
* [Deploy asp.net core 2.0 apps on Heroku](https://blog.devcenter.co/deploy-asp-net-core-2-0-apps-on-heroku-eea8efd918b6)
* [Pushing a repository image to Docker Hub](https://docs.docker.com/docker-hub/repos/#pushing-a-repository-image-to-docker-hub)
