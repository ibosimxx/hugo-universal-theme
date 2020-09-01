+++
banner = "img/banners/banner-5.jpg"
categories = ["Hugo", "Temas", "Go"]
tags = ["jekyll", "Hugo"]
title = "Migrando desde Jekyll"
twitter_author = "jekyllrb"

+++
## Moviendo el contenido a `static`

Jekyll tiene una regla según la cual cualquier directorio que no comience con `_` se copiará como está en la `_site` salida. Hugo mantiene todo el contenido estático en `static`. Por lo tanto, debe moverlo todo allí. 

Con Jekyll, algo que parecía

    ▾ <root>/
        ▾ images/
            logo.png

debe convertirse

    ▾ <root>/
        ▾ static/
            ▾ images/
                logo.png

Además, querrá que cualquier archivo que deba residir en la raíz (como `CNAME`) se mueva a `static`.

## Crea tu archivo de configuración de Hugo

Hugo puede leer su configuración como JSON, YAML o TOML. Hugo también admite la configuración personalizada de parámetros. Consulte la[ Documentación de configuración de Hugo](https://app.forestry.io/overview/configuration/)

## Establezca su carpeta de publicación de configuración en `_site`

El valor predeterminado es que Jekyll publique `_site`y que hugo publique `public`_. Si, como yo, se ha_[_`_site` _asignado a un submodulo de Git en la `gh-pages` branch](http://blog.blindgaenger.net/generate_github_pages_in_a_submodule.html), querrá hacer una de estas dos alternativas:

1. Cambie su submódulo para apuntar al mapa `gh-pages` a público en lugar de  `_site` (recommendado).

        git submodule deinit _site
        git rm _site
        git submodule add -b gh-pages git@github.com:your-username/your-repo.git public
2. O cambie la configuración de Hugo para usar  `_site` en lugar de `public`.

        {
            ..
            "publishdir": "_site",
            ..
        }

## Convierta las plantillas de Jekyll en plantillas de Hugo

Ese es el grueso del trabajo aquí mismo. La documentación es tu amiga. Debes consultar [La documentación de la plantilla de Jekyll](http://jekyllrb.com/docs/templates/) si necesitas refrescar tu memoria sobre cómo construiste tu blog y [la plantilla de Hugo](https://gohugo.io/templates/) para aprender el estilo de Hugo.

Como un única referencia, conviertir mis plantillas para [heyitsalex.net](http://heyitsalex.net/) solo me llevó unas pocas horas.

## Convierta los complementos de Jekyll en Shortcode de Hugo

Jekyll tiene [plugins](http://jekyllrb.com/docs/plugins/); Hugo tiene [shortcodes](/doc/shortcodes/). No es difícil de portar.

### Implementación

Como ejemplo, estaba usando un plugin personalizado [`image_tag`](https://github.com/alexandre-normand/alexandre-normand/blob/74bb12036a71334fdb7dba84e073382fc06908ec/_plugins/image_tag.rb) para generar figuras con subtítulos al ejecutar Jekyll. Mientras leía sobre códigos cortos, descubrí que Hugo tenía un código corto incorporado que hace exactamente lo mismo.

Plugin Jekyll:

    module Jekyll
      class ImageTag < Liquid::Tag
        @url = nil
        @caption = nil
        @class = nil
        @link = nil
        // Patterns
        IMAGE_URL_WITH_CLASS_AND_CAPTION =
        IMAGE_URL_WITH_CLASS_AND_CAPTION_AND_LINK = /(\w+)(\s+)((https?:\/\/|\/)(\S+))(\s+)"(.*?)"(\s+)->((https?:\/\/|\/)(\S+))(\s*)/i
        IMAGE_URL_WITH_CAPTION = /((https?:\/\/|\/)(\S+))(\s+)"(.*?)"/i
        IMAGE_URL_WITH_CLASS = /(\w+)(\s+)((https?:\/\/|\/)(\S+))/i
        IMAGE_URL = /((https?:\/\/|\/)(\S+))/i
        def initialize(tag_name, markup, tokens)
          super
          if markup =~ IMAGE_URL_WITH_CLASS_AND_CAPTION_AND_LINK
            @class   = $1
            @url     = $3
            @caption = $7
            @link = $9
          elsif markup =~ IMAGE_URL_WITH_CLASS_AND_CAPTION
            @class   = $1
            @url     = $3
            @caption = $7
          elsif markup =~ IMAGE_URL_WITH_CAPTION
            @url     = $1
            @caption = $5
          elsif markup =~ IMAGE_URL_WITH_CLASS
            @class = $1
            @url   = $3
          elsif markup =~ IMAGE_URL
            @url = $1
          end
        end
        def render(context)
          if @class
            source = "<figure class='#{@class}'>"
          else
            source = "<figure>"
          end
          if @link
            source += "<a href=\"#{@link}\">"
          end
          source += "<img src=\"#{@url}\">"
          if @link
            source += "</a>"
          end
          source += "<figcaption>#{@caption}</figcaption>" if @caption
          source += "</figure>"
          source
        end
      end
    end
    Liquid::Template.register_tag('image', Jekyll::ImageTag)

está escrito como este código corto de Hugo:

    <!-- image -->
    <figure {{ with .Get "class" }}class="{{.}}"{{ end }}>
        {{ with .Get "link"}}<a href="{{.}}">{{ end }}
            <img src="{{ .Get "src" }}" {{ if or (.Get "alt") (.Get "caption") }}alt="{{ with .Get "alt"}}{{.}}{{else}}{{ .Get "caption" }}{{ end }}"{{ end }} />
        {{ if .Get "link"}}</a>{{ end }}
        {{ if or (or (.Get "title") (.Get "caption")) (.Get "attr")}}
        <figcaption>{{ if isset .Params "title" }}
            {{ .Get "title" }}{{ end }}
            {{ if or (.Get "caption") (.Get "attr")}}<p>
            {{ .Get "caption" }}
            {{ with .Get "attrlink"}}<a href="{{.}}"> {{ end }}
                {{ .Get "attr" }}
            {{ if .Get "attrlink"}}</a> {{ end }}
            </p> {{ end }}
        </figcaption>
        {{ end }}
    </figure>
    <!-- image -->

### Uso

Simplemente cambié:

    {% image full http://farm5.staticflickr.com/4136/4829260124_57712e570a_o_d.jpg "One of my favorite touristy-type photos. I secretly waited for the good light while we were "having fun" and took this. Only regret: a stupid pole in the top-left corner of the frame I had to clumsily get rid of at post-processing." ->http://www.flickr.com/photos/alexnormand/4829260124/in/set-72157624547713078/ %}

a esto (este ejemplo usa una versión ligeramente extendida llamada `fig`, diferente del incorporado `figure`):

    {{%/* fig class="full" src="http://farm5.staticflickr.com/4136/4829260124_57712e570a_o_d.jpg" title="One of my favorite touristy-type photos. I secretly waited for the good light while we were having fun and took this. Only regret: a stupid pole in the top-left corner of the frame I had to clumsily get rid of at post-processing." link="http://www.flickr.com/photos/alexnormand/4829260124/in/set-72157624547713078/" */%}}

Como beneficio adicional, los parámetros con nombre de Shortcode son, posiblemente, más legibles.

## Últimos retoques

### Corregir contenido

Dependiendo de la cantidad de personalización que se hizo con cada publicación con Jekyll, este paso requerirá más o menos esfuerzo. Aquí no hay reglas estrictas y rápidas, excepto que `hugo server --watch` es su amigo. Pruebe sus cambios y corrija los errores según sea necesario.

### Limpiar

Querrá eliminar la configuración de Jekyll en este punto. Si tiene algo más que no se utiliza, elimínelo.

## Un ejemplo práctico en una Diff

[Hey, it's Alex](http://heyitsalex.net/) Fue migrada en menos _que canta un gallo_ from Jekyll to Hugo. Puede ver todos los cambios (y errores) mirando aqui [diff](https://github.com/alexandre-normand/alexandre-normand/compare/869d69435bd2665c3fbf5b5c78d4c22759d7613a...b7f6605b1265e83b4b81495423294208cc74d610).
