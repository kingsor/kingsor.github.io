---
date: "2021-07-06"
title: Testare GitHub Pages localmente
summary: Come testare il proprio sito realizzato con Jekyll in locale con docker
categories:
- developer
tags:
- jekyll
- docker
---

Volendo testare il proprio sito pubblicato come [GitHub Pages](https://pages.github.com/) utilizzando [Jekyll](https://jekyllrb.com/), ho cercato su google un modo per poterlo visualizzare in locale prima di fare commit e push su GitHub.
E volendo evitare di dover installare Ruby in locale, ho cercato il modo per farlo utilizzando [docker](https://www.docker.com/).

Questo post su [Dev.to](https://dev.to/) è quello che mi ha aiutato a risolvere il mio problema.

[Testing GitHub Pages Locally](https://dev.to/dillonad/testing-github-pages-locally-agf) (Dec 6, 2019)

Il post propone due modalità di invocazione di una immagine docker di Jekyll dal folder che contiene le proprie pagine.

Windows

```shell
docker run --rm -it -v "%cd%":/srv/jekyll -p 4000:4000 jekyll/jekyll jekyll serve --watch
```

Linux

```shell
docker run --rm -it -v $(pwd):/srv/jekyll -p 4000:4000 jekyll/jekyll jekyll serve --watch
```

Ho quindi selezionato il folder di riferimento e ho aperto un Windows Terminal. Ho lanciato il comando per Windows e ho ricevuto il seguente errore:

```shell
docker: invalid reference format.
See 'docker run --help'.
```

Ho quindi cercato su google quale potesse essere il problema.

Ho trovato questa risposta su Stackoverflow relativamente a questo quesito: [Mount current directory as a volume in Docker on Windows 10](https://stackoverflow.com/a/41489151/2768802).

In pratica lo script va modificato in questo modo dal momento che lo stavo lanciando da PowerShell:

```shell
docker run --rm -it -v ${PWD}:/srv/jekyll -p 4000:4000 jekyll/jekyll jekyll serve --watch
```

il problema di sintassi sembra risolto ma ora è jekyll a segnalarmi un problema, allora modifico il comando docker per entrare nel container invocando la bash:

```shell
docker run --rm -it -v ${PWD}:/srv/jekyll -p 4000:4000 jekyll/jekyll /bin/bash
```

a questo punto riesco a capire che c'è un problema con `Gemfile.lock` e che è necessario rimuoverlo e lanciare `bundle install` per ripristinare tale file.

Una volta risolto il problema con il `Gemfile`, ho potuto richiamare `jekyll serve` e visualizzare il mio sito dal browser.

Avendo risolto il problema su `Gemfile.lock`, ora posso invocare nuovamente docker run come descritto nel post che mi ha fatto da guida.

```shell
docker run --rm -it -v ${PWD}:/srv/jekyll -p 4000:4000 jekyll/jekyll jekyll serve --watch
```
