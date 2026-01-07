---
date: "2022-05-15"
title: SQL Server 2019 con docker su Mac M1
summary: Come utilizzare SQL Server su Mac M1 - in realtà si parla di Azure SQL Edge per ARM
categories:
- developer
tags:
- database
- sql-server
- mac-m1
- arm
- docker
- docker-compose
- lessons-learned
series:
- series-docker-compose
---

Si, lo so. Il titolo di questo post è un po' fuorviante.

Sul mio portatile e sul pc di casa ho installato [Docker](https://www.docker.com/products/docker-desktop) sul quale gira un container di SQL Server for Linux come ho spiegato in [questo post](https://kingsor.github.io/2022/01/09/sql-server-2019-docker-compose/).

Quando ho acquistato il mio primo portatile della Apple, il MacBook Air con processore M1 volevo fare la stessa cosa.

Ho installato [Docker per Mac](https://docs.docker.com/desktop/install/mac-install/), la versione per "Mac with Apple silicon" e ho cercato l'immagine di SQL Server 2019 per processori ARM.

E ho scoperto che purtroppo SQL Server 2019 non supporta immagini docker per processori ARM.

Dopo un po' di ricerca su Google, ho trovato questo articolo - [How to Install SQL Server on an M1 Mac (ARM64)](https://database.guide/how-to-install-sql-server-on-an-m1-mac-arm64/) - che spiega come ovviare al problema utilizzando [Azure SQL Edge](https://azure.microsoft.com/en-us/products/azure-sql/edge/) che invece supporta i processori ARM.

Azure SQL Edge è un database relazionale ottimizzato per le distribuzioni IoT e IoT Edge. È basato sulle ultime versioni di SQL Server, quindi si può utilizzare [T-SQL](https://learn.microsoft.com/en-us/sql/t-sql/language-reference?view=sql-server-ver15) come su SQL Server.
Per eseguire query e comandi si possono utilizzare gli stessi strumenti disponibili per SQL Server (come [SSMS](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15), [Azure Data Studio](https://learn.microsoft.com/en-us/sql/azure-data-studio/download-azure-data-studio?view=sql-server-ver15)).

Quindi eseguendo questo comando:

```bash
docker run --cap-add SYS_PTRACE -e 'ACCEPT_EULA=1' -e 'MSSQL_SA_PASSWORD=bigStrongPwd' -p 1433:1433 --name sqledge -d mcr.microsoft.com/azure-sql-edge
```

viene scaricata l'immagine docker e poi lanciato il container con Azure SQL Edge.

Anche in questo caso ho preferito creami un file `docker-compose.yml` che trovo più ordinato e leggibile

```yml
version: "3.7"
services:
    db:
        container_name: azure-sql-edge
        image: mcr.microsoft.com/azure-sql-edge
        cap_add: [ 'SYS_PTRACE' ]
        ports:
            - 1433:1433
        restart: always
        volumes: 
            - azure-sql-edge-data:/var/opt/mssql
        environment:
            MSSQL_SA_PASSWORD: "Ti6collegato!"
            ACCEPT_EULA: "Y"
volumes:
    azure-sql-edge-data:
```

che posso far partire con [Docker Compose](https://docs.docker.com/compose/) eseguendo questo comando dal folder nel quale si trova il file .yml:

```bash
docker-compose up -d
```

Anche in questo caso ho creato un repository su github: [azure-sql-edge-compose](https://github.com/kingsor/azure-sql-edge-compose)

Se si vuole installare anche Azure Data Studio, questo post - [How to Install Azure Data Studio on a Mac](https://database.guide/how-to-install-azure-data-studio-on-a-mac/) - è un'ottima guida in tal senso.

