---
date: "2022-01-09"
title: SQL Server 2019 con docker compose
summary: Come utilizzare docker compose per instanziare SQL Server 2019
categories:
- developer
tags:
- database
- sql-server
- docker
- docker-compose
- lessons-learned
series:
- series-docker-compose

---

Come ho [scritto tempo fa](/2020/01/20/restore-di-backup-su-sqlserver/), utilizzo [SQL Server 2019 per Linux con docker](https://mcr.microsoft.com/en-us/product/mssql/server/about) lanciando questo comando:

```bash
docker run --name sqlserver-2019 -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Ti6collegato!' -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

per poi connettermi al server con [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15) per interagire con database e tabelle.

Ma mi sono reso conto che non utilizzo un [volume](https://docs.docker.com/storage/volumes/) e quindi i dati sono salvati all'interno del [container](https://www.docker.com/resources/what-container/) docker. Se volessi passare ad una nuova versione dell'immagine per generare un nuovo container perderei ogni dato.

Per avere una visualizzazione più chiara dei vari parametri relativi all'immagine docker, ho pensato di utilizzare [Docker Compose](https://docs.docker.com/compose/).

Dopo una ricerca su google, ho trovato questo post scritto da [Jason Robert](https://espressocoder.com/about/): [Exploring SQL Server 2019 with Docker](https://espressocoder.com/2020/07/07/exploring-sql-server-2019-with-docker/).

L'autore descrive come installare SQL Server 2019 per Linux utilizzando [Docker](https://www.docker.com/products/docker-desktop). Nel suo caso la necessità è quella di poter utilizzare SQL Server su MacBook. E alla fine descrive come utilizzare Docker Compose perchè anche lui preferisce il file YAML per la possibilità di visualizzare i parametri in maniera chiara rispetto alla linea di comando. Sebbene Docker Compose sia pensato per lanciare diversi containers contemporaneamente, è possibile anche utilizzarlo per lanciare un singolo container.

Ecco il file `docker-compose.yml`

```yml
version: "3.7"
services:
    db:
        container_name: mssql-2019
        image: mcr.microsoft.com/mssql/server:2019-latest
        ports:
            - 1433:1433
        restart: always
        volumes: 
            - mssql-2019-data:/var/opt/mssql
        environment:
            SA_PASSWORD: "Ti6collegato!"
            ACCEPT_EULA: "Y"
volumes:
    mssql-2019-data:
```

A questo punto, dal folder nel quale si trova il file .yml si può eseguire questo comando:

```bash
docker-compose up -d
```

Questo [comando](https://docs.docker.com/engine/reference/commandline/compose_up/) crea il container con SQL Server 2019 e il volume (`mssql-2019-data`) che contiene i dati se non esistono già e poi fa partire il container.

Per poter utilizzare il file .yml anche in altre occasioni, ho creato un repository su github: [mssql-compose](https://github.com/kingsor/mssql-compose).

