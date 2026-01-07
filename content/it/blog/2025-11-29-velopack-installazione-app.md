---
date: "2025-11-27"
title: Velopack per installazione app
summary: Come utilizzare Velopack per creare il programma di installazione di una applicazione e il sistema di update automatico quando vengono rilasciate nuove versioni
categories:
- developer
tags:
- lessons-learned
---

Sto seguendo il tutorial per arrivare ad installare e fare update di una app WPF.

[Getting Started: WPF App](https://docs.velopack.io/getting-started/csharp?platform=wpf)

Voglio anche provare a pubblicare la prima versione dell'app ed un suo aggiornamento ad una versione successiva.
E lo voglio provare utilizzando un server web.

Così ho pensato di provare un'utility scritta in dotnet e rilasciata come tool dotnet: 

[dotnet-serve](https://github.com/natemcmaster/dotnet-serve)

Si installa da linea di comando:

`dotnet tool install -g dotnet-serve`

Dopo aver installato il tool ho creato il folder `VelopackFolder` nel quale ho creato un folder per ogni app che intendo pubblicare che supporti Velopack per installazione e gestione degli update.

A questo punto posso lanciare il server con questo comando:

`dotnet serve -d .\VelopackFolder\ -p 8088`

E poi si prova il tutto.

