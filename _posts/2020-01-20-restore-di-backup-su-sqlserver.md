---
layout: post
title: "Restore di un backup su SQL Server"
description: "Come fare il restore di un backup su SQL Server su docker"
tags:
- developer
- database
- sql-server
- docker
- til
---

Starting a sql server 2019 image with docker:

```bash
docker run --name sqlserver-2019 -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Ti6collegato!' -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

A questo punto ho utilizzato [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15) per fare il restore del database.
Il restore del database viene fatto partendo dal presupposto che i file di backup siano all'interno del container in questo folder `/var/opt/mssql/data/`. Non è possibile importare un file .bak presente nel sistema host attraverso SSMS.
Quindi è necessario copiare il file .bak dentro al container utilizzando questo comando:

```bash
docker cp EasySurveyDatabaseDeployment.bak sqlserver-2019:/var/opt/mssql/data/EasySurveyDatabase.bak
```

qui ho trovato l'esempio utile per la copia: [Copying files from host to Docker container](https://stackoverflow.com/questions/22907231/copying-files-from-host-to-docker-container)

Per verificare che il file fosse stato effettivamente copiato ho lanciato una bash nel container con questo comando:

```bash
docker exec -it sqlserver-2019 bash
```

A questo punto è possibile fare il restore del database. Ho creato il database EasySurveyDatabase vuoto per poi richiamare la procedura di restore selezionando il database stesso. Ma questo approccio mi ha dato il seguente errore:

```bash
System.Data.SqlClient.SqlError: The backup set holds a backup of a database other than the existing 'EasySurveyDatabase' database. (Microsoft.SqlServer.SmoExtended)
```

Ho risolto grazie a questo post su StackOverflow: [The backup set holds a backup of a database other than the existing](https://stackoverflow.com/questions/10204480/the-backup-set-holds-a-backup-of-a-database-other-than-the-existing)

> * Don't create an empty database and restore the .bak file on to it.
> * Use 'Restore Database' option accessible by right clicking the "Databases" branch of the SQL Server Management Studio and provide the database name while providing the source to restore.

Dal nodo "Databases" ho selezionato "Restore Databases...". Sulla dialog che appare, seleziono Source -> Device e quindi seleziono il file .bak che mi interessa. A quel punto su Options, seleziono "Overwrite the existing database" (anche se il database non esiste) e premo OK. E il restore va a buon fine.


## SQL Server with Docker

* [Microsoft SQL Server - Docker Hub](https://hub.docker.com/_/microsoft-mssql-server) - Official images for Microsoft SQL Server on Linux for Docker Engine.
* [Quickstart: Run SQL Server container images with Docker](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-ver15&pivots=cs1-bash) (Apr 11, 2019) - In this quickstart, you use Docker to pull and run the SQL Server 2019 container image, [mssql-server](https://hub.docker.com/r/microsoft/mssql-server). Then connect with `sqlcmd` to create your first database and run queries. This image consists of SQL Server running on Linux based on Ubuntu 16.04. It can be used with the Docker Engine 1.8+ on Linux or on Docker for Mac/Windows. This quickstart specifically focuses on using the SQL Server on **linux** image. The Windows image is not covered, but you can learn more about it on the [mssql-server-windows-developer](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/) Docker Hub page.

