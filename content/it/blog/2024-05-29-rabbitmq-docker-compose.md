---
date: "2024-05-29"
title: RabbitMQ con Docker Compose
summary: Come instanziare RabbitMQ localmente utilizzando Docker Compose
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

## Prerequisiti

Prima di poter instanziare RabbitMQ utilizzando Docker Compose, ci sono i seguenti prerequisiti:

- Docker: dovrai avere [Docker](https://docs.docker.com/get-docker/) installato sul tuo sistema. Se non hai già installato Docker, puoi scaricarlo dal sito Web ufficiale di Docker. Assicurati di scaricare la versione adatta al tuo sistema operativo.
- Docker Compose: dovrai inoltre avere Docker Compose installato sul tuo sistema. Docker Compose viene fornito preinstallato con Docker Desktop per Windows e macOS, ma se utilizzi Linux o se devi installarlo separatamente, puoi seguire le istruzioni presenti in questo post: [Docker Compose documentation page for Linux](https://docs.docker.com/compose/install/linux/).


## Come avviare RabbitMQ

Il file per docker compose è disponibile in questo [repository](https://github.com/kingsor/rabbitmq-compose) su GitHub.

Inizia clonando quel repository sul computer sul quale hai installato Docker.

Apri il terminale nel folder nel quale hai clonato il repository. Il nome di default è `rabbitmq-compose`.

Successivamente puoi far partire RabbitMQ digitando il seguente comando:

```bash

docker compose up -d

```

A questo punto Docker scarica l'immagine di rabbitmq e fa partire il container.


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

La dashboard di gestione di RabbitMQ è accessibile a questo url: [http://localhost:15672](http://localhost:15672) e si può accedere con le credenziali di default (guest/guest).

A questo punto è possibile utilizzare RabbitMQ dalle vostre applicazioni.

