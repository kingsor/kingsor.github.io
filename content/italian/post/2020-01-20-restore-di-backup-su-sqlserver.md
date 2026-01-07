---
date: "2020-01-20"
title: Restore di un backup su SQL Server
summary: Come fare il restore di un backup su un'immagine docker di SQL Server per Linux
categories:
- developer
tags:
- database
- sql-server
- docker
- lessons-learned
---

Per prima cosa ho fatto partire [un'immagine docker di SQL Server 2019 per Linux](https://mcr.microsoft.com/en-us/product/mssql/server/about):

```bash
docker run --name sqlserver-2019 -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=Ti6collegato!' -p 1433:1433 -d mcr.microsoft.com/mssql/server:2019-latest
```

A questo punto ho utilizzato [SQL Server Management Studio](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15) per fare il restore del database.

Il restore del database può essere fatto a condizione che i file di backup siano all'interno del container in questo folder `/var/opt/mssql/data/`. Non è possibile importare un file .bak presente nel sistema host attraverso SSMS.

Per il file di backup ho utilizzato quello relativo al database AdventureWorks2019 disponibile sul [repository GitHub di Microsoft](https://github.com/microsoft) nella sezione [AdventureWorks sample databases](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks).

Quindi è necessario copiare il file .bak dentro al container utilizzando questo comando:

```bash
docker cp AdventureWorks2019.bak sqlserver-2019:/var/opt/mssql/data/AdventureWorks2019.bak
```

qui ho trovato l'esempio utile per la copia: [Copying files from host to Docker container](https://stackoverflow.com/questions/22907231/copying-files-from-host-to-docker-container)

Per verificare che il file fosse stato effettivamente copiato ho lanciato una bash nel container con questo comando:

```bash
docker exec -it sqlserver-2019 bash
```

A questo punto è possibile fare il restore del database. Ho creato il database `AdventureWorks2019` vuoto per poi richiamare la procedura di restore selezionando il database stesso. Ma questo approccio mi ha dato il seguente errore:

```bash
System.Data.SqlClient.SqlError: The backup set holds a backup of a database other than the existing 'AdventureWorks2019' database. (Microsoft.SqlServer.SmoExtended)
```

Ho risolto grazie a questo post su StackOverflow: [The backup set holds a backup of a database other than the existing](https://stackoverflow.com/questions/10204480/the-backup-set-holds-a-backup-of-a-database-other-than-the-existing)

> * Don't create an empty database and restore the .bak file on to it.
> * Use 'Restore Database' option accessible by right clicking the "Databases" branch of the SQL Server Management Studio and provide the database name while providing the source to restore.

In pratica non serve creare un database vuoto. Ho cancellato il database che avevo creato su SSMS e ho seguito questi passi:

- Dal nodo "Databases" ho selezionato "Restore Databases...".
- Sulla dialog che appare, seleziono Source -> Device e quindi seleziono il file .bak che mi interessa.
- A quel punto su Options, seleziono "Overwrite the existing database" (anche se il database non esiste) e premo OK.

E il restore va a buon fine.


## Link utili

* [Microsoft SQL Server - Microsoft Artifact Registry](https://mcr.microsoft.com/en-us/product/mssql/server/about) - Immagini ufficiali per Microsoft SQL Server basate su Ubuntu.
* [Quickstart: Run SQL Server container images with Docker](https://docs.microsoft.com/en-us/sql/linux/quickstart-install-connect-docker?view=sql-server-ver15&pivots=cs1-bash) (Apr 11, 2019) - In this quickstart, you use Docker to pull and run the SQL Server 2019 container image, [mssql-server](https://hub.docker.com/r/microsoft/mssql-server). Then connect with `sqlcmd` to create your first database and run queries. This image consists of SQL Server running on Linux based on Ubuntu 16.04. It can be used with the Docker Engine 1.8+ on Linux or on Docker for Mac/Windows. This quickstart specifically focuses on using the SQL Server on **linux** image. The Windows image is not covered, but you can learn more about it on the [mssql-server-windows-developer](https://hub.docker.com/r/microsoft/mssql-server-windows-developer/) Docker Hub page.

