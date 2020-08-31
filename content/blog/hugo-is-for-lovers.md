+++
title = "Hugo is for lovers"
date = "2020-08-31T13:39:46+02:00"
tags = ["hugo"]
categories = ["pseudo"]
banner = "img/banners/banner-3.jpg"
+++

## Paso 1. Instalar Hugo

Ve a [hugo releases](https://github.com/spf13/hugo/releases) y descarga la
versión apropiada para su sistema operativo y arquitectura.

Guárdelo en algún lugar específico, ya que lo usaremos en el siguiente paso.

Hay instrucciones más completas disponibles en [installing hugo](https://gohugo.io/getting-started/installing/)

## Paso 2. Cree los documentos

Hugo tiene su propio sitio de ejemplo, que también es el sitio de documentación.
estás leyendo ahora mismo.

Siga los siguientes pasos:

 1. Clona el [hugo repository](http://github.com/spf13/hugo)
 2. Ve al repositorio
 3. Ejecute hugo en modo servidor y compile los documentos.
 4. Abra su navegador en http://localhost:1313

Pseudo comandos correspondientes:

    git clone https://github.com/spf13/hugo
    cd hugo
    /path/to/where/you/installed/hugo server --source=./docs
    > 29 pages created
    > 0 tags index created
    > in 27 ms
    > Web Server is available at http://localhost:1313
    > Press ctrl+c to stop

Una vez que haya llegado aquí, siga el resto de esta página en su versión local.

## Paso 3. Cambia el sitio de documentos


Detenga el proceso de Hugo pulsando ctrl + c..

Ahora vamos a ejecutar hugo nuevamente, pero esta vez con hugo en modo reloj.

    /path/to/hugo/from/step/1/hugo server --source=./docs --watch
    > 29 pages created
    > 0 tags index created
    > in 27 ms
    > Web Server is available at http://localhost:1313
    > Watching for changes in /Users/spf13/Code/hugo/docs/content
    > Press ctrl+c to stop


Abra su [editor preferido](http://vim.spf13.com) y cambie una de las fuentes
content pages. How about changing this very file to *fix the typo*. How about changing this very file to *fix the typo*.

Content files are found in `docs/content/`. Unless otherwise specified, files
are located at the same relative location as the url, in our case
`docs/content/overview/quickstart.md`.

Change and save this file.. Notice what happened in your terminal.

    > Change detected, rebuilding site

    > 29 pages created
    > 0 tags index created
    > in 26 ms

Refresh the browser and observe that the typo is now fixed.

Notice how quick that was. Try to refresh the site before it's finished building.. I double dare you.
Having nearly instant feedback enables you to have your creativity flow without waiting for long builds.

## Paso 4. Diviértete

La mejor forma de aprender algo es jugar con él.
