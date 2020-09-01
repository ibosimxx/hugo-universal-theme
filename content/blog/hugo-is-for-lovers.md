+++
author = "Ibosim"
banner = "img/banners/banner-3.jpg"
categories = []
date = 2020-08-31T11:50:46Z
tags = ["hugo"]
title = "Hugo es para fans"

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


Abra su [editor preferido](http://vim.spf13.com) y cambie una de las paginas 
de contenido.  ¿Qué tal cambiar este mismo archivo para * corregir el error tipográfico * ?

Los archivos de contenido se encuentran en `docs/content/`.  A menos que se especifique lo contrario, 
los archivos se encuentran en la misma ubicación relativa que la URL, en nuestro caso
`docs/content/overview/quickstart.md`.

Cambie y guarde este archivo. Observe lo que sucedió en su terminal.

    > Change detected, rebuilding site

    > 29 pages created
    > 0 tags index created
    > in 26 ms

Actualice el navegador y observe que el error tipográfico ahora está arreglado.

Note lo rápido que fue. Intenta actualizar el sitio antes de que termine de construirse. Te reto dos veces.
Tener comentarios casi instantáneos le permite hacer que su creatividad fluya sin tener que esperar largas construcciones.

## Paso 4. Diviértete

La mejor forma de aprender algo es jugar con él.