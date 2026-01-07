---
date: "2019-03-23"
title: Utilizzare Postgres con Docker su Windows
summary: Come ho iniziato a lavorare con Postgres, senza installarlo nella mia macchina ma utilizzando Docker
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


Stavo cercando qualcosa di breve su come iniziare con Docker Compose e ho trovato questo video:

[Docker Compose in 12 Minutes](https://www.youtube.com/watch?v=Qw9zlE3t8Ko) (Mar 14, 2017) - *Learn how to use Docker Compose to run multi-container applications easily. This is the second video in this Docker series.*

È un bel video con un esempio molto semplice e ben spiegato. C'è un progetto con cui fare pratica su [GitHub](https://github.com/jakewright/tutorials/tree/master/docker/02-docker-compose).

Dopo questo esempio mi sono ricordato di questo post:

[ASP.Net Core Web API with Docker Compose, PostgreSQL and EF Core.](https://medium.com/front-end-weekly/net-core-web-api-with-docker-compose-postgresql-and-ef-core-21f47351224f) 

e ho voluto provare quanto descritto nel post. Qui il [codice sorgente](https://github.com/rajvirtual/docker-aspnetcore-postgresql).

Ma prima volevo iniziare a utilizzare PostgreSql con Docker. Così ho seguito le indicazioni di questo post: [Setup PostgreSQL on Windows with Docker](https://elanderson.net/2018/02/setup-postgresql-on-windows-with-docker/) e di questo: [Stop Installing Postgres on Your Laptop : Use Docker Instead](https://blog.dahanne.net/2015/01/19/stop-installing-postgres-on-your-laptop-use-docker-instead/).

Quindi questa è stata la mia prima idea per un docker container:

```bash
docker run -p 5432:5432 --name postgres_db -e POSTGRES_PASSWORD=password -d postgres
```

con tutti i dati dei database del server contenuti nel container.

Guardando nella documentazione di Postgres [Postgres Official Docker Image](https://hub.docker.com/_/postgres/) ho scoperto che è possibile mappare un folder sulla macchina host con il folder dei dati di Postgres all'interno del container in modo da mantenere i database creati anche se il container viene eliminato per fare un update dell'immagine.

Quindi questa è stata l'evoluzione:

```bash
docker run -p 5432:5432 --name postgres_db -e POSTGRES_PASSWORD=password -v /c/Users/myuser/DockerProjects/postgres/postgres_db:/var/lib/postgresql/data -d postgres
```

Ma con Docker Toolbox for Windows 10 Home (il mio caso), il mio container postgres non funzionava.

Guardando ai logs con `docker logs postgres_db` ho scoperto il problema:

```bash
FATAL: data directory "/var/lib/postgresql/data" has wrong ownership
```

[Questa](https://github.com/docker-library/postgres/issues/435) è una soluzione:


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

[Qui](https://github.com/cytopia/devilbox/issues/175) qualcosa di simile:

In conclusione: la soluzione alternativa è creare un volume (locale) con:

```bash
$ docker volume create --name data-postgresql --driver local
```

Di conseguenza il file `docker-compose.yml` diventa qualcosa del genere:

```yml
services:
  pgsql:
    volumes:
      - data-postgresql:/var/lib/postgresql

volumes:
  data-postgresql:
    external: true
```

Mi sono anche letto [Start a container with a volume](https://docs.docker.com/storage/volumes/#start-a-container-with-a-volume) nella sezione [Use Volume - Docker Documentation](https://docs.docker.com/storage/volumes/).


Questa la mia soluzione per un container di test:

```bash
$ docker volume create --name postgres-volume

$ docker run -p 5432:5432 --name postgres_db -e POSTGRES_PASSWORD=password -v postgres-volume:/var/lib/postgresql/data -d postgres
```

E finalmente riesco a sperimentare con Postgres :-)
