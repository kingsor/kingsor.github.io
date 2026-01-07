---
date: "2024-05-29"
title: RabbitMQ with Docker Compose
summary: How to run RabbitMQ locally using Docker Compose
categories:
- developer
tags:
- rabbitmq
- docker
- docker-compose
- lessons-learned
series:
- series-docker-compose

---

Docker compose file for working with rabbit mq

## Prerequisites

Before you can deploy RabbitMQ using Docker Compose, you will need to have a few things in place. Here are the prerequisites:

- **Docker** - You will need to have [Docker](https://docs.docker.com/get-docker/) installed on your system. If you don't have Docker installed already, you can download it from the official Docker website. Be sure to download the version that is appropriate for your operating system.
- **Docker Compose** - You will also need to have Docker Compose installed on your system. Docker Compose comes pre-installed with Docker Desktop for Windows and macOS, but if you are using Linux or if you need to install it separately, you can follow the instructions on the [Docker Compose documentation page for Linux](https://docs.docker.com/compose/install/linux/).

## How to start RabbitMQ

Clone this repo on your machine.

In a terminal window go to the folder where you cloned this repo. Default name is `rabbitmq-compose`.

Then you can start RabbitMQ:

```bash
docker compose up -d
```

Then Docker download rabbitmq image and start the container.


```bash
PS C:\Projects\00-Docker-Projects\rabbitmq-compose> docker compose up -d
[+] Running 11/11
 ✔ rabbitmq Pulled                                                                                                20.1s
   ✔ a8b1c5f80c2d Pull complete                                                                                   11.8s
   ✔ 4dedb6d843e5 Pull complete                                                                                   14.2s
   ✔ 5c1196c9f92f Pull complete                                                                                   14.5s
   ✔ 89aa66202de9 Pull complete                                                                                   14.6s
   ✔ 7482e2b5f1fd Pull complete                                                                                   16.5s
   ✔ cae0f9147f71 Pull complete                                                                                   16.5s
   ✔ 5e8608f82ef5 Pull complete                                                                                   16.6s
   ✔ 76a071de98b9 Pull complete                                                                                   16.6s
   ✔ 140f907150d0 Pull complete                                                                                   16.6s
   ✔ 53c7a9878ba6 Pull complete                                                                                   17.6s
[+] Running 2/2
 ✔ Network rabbitmq-compose_rabbitmq_net  Created                                                                  0.2s
 ✔ Container rabbitmq                     Started                                                                  1.2s
PS C:\Projects\00-Docker-Projects\rabbitmq-compose>

```

The RabbitMQ management interface will be accessible at [http://localhost:15672](http://localhost:15672) and you can log in with the default credentials (guest/guest).

That's all.