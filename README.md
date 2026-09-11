# Instrucciones

## Paso 1:

Dale click al link del sitio web que aparece junto al "About" (en la parte superior derecha de este sitio) para que veas en general cuál es objetivo a obtener al finalizar este instructivo.

## Paso 2:

1. Ir al botón de "Use this template" -> "Create a new repository"
2. Llena con el nombre que quieras que tenga tu repositorio

**OJO:** Ahora ya estamos en tu repositorio. Ya no más en la plantilla

## Paso 3:

Ir a Settings -> Pages -> Build and deployment -> Source -> GitHub Actions. Lo único que verás en un cintillo azul que dice que se cambiaron los ajustes.

🚨🚨 **Importante:** 🚨🚨
A partir de ahora el procedimiento es ligeramente diferente al que hicimos las sesiones pasadas.

## Paso 4:

Abre el archivo "_quarto.yml" y observa su contenido:

Ahora ves:
```
website:
  title: "Mi sitio de reportes"
  navbar:
    left:
      - text: "Sobre este sitio"
        href: index.qmd
```

+ Modifica el "title" por el título que quieres que tenga tu sitio
+ Ya dijimos en la sesión pasada que "navbar" indica que agregarás una barra de navegación (los links en la parte superior del sitio)
+ 🚨🚨 **Importante:** 🚨🚨 El archivo "index.qmd" siempre debe existir y con ese nombre. Éste es archivo que construye tu landing page (lo primero que se abre al darle click al link de tu sitio)
+ Puedes cambiar el "text" por otro que te guste

+ La sesión pasada también ya dijimos que "sidebar" agrega una barra lateral

```
  sidebar:
    style: docked
    contents:
      - index.qmd
      - section: "Primer reporte"
        contents:
          - mis_reportes/reporte-01.qmd
      - section: "Segundo reporte"
        contents:
          - mis_reportes/reporte-02.qmd
      - section: "Último reporte"
        contents:
          - mis_reportes/reporte-03.qmd
      - section: "Reporte con dos lenguajes"
        contents:
          - mis_reportes/reporte-04.qmd
      - section: "Reporte pero en slides"
        contents:
          - mis_reportes/slides-01.qmd
      - section: "Reporte en slides embebido en el sitio"
        contents:
          - mis_reportes/ver-slides-01.qmd
```

+ Observa que con `section: "Título de la sección"` y `contents:` seguido de "mis_reportes/reporte-0X.qmd" se le dice a Quarto que quiero mostrar un nuevo contenido que construí mediante un archivo Quarto individual ("reporte-0X.qmd")

+ Esto significa que en la carpeta "mis_reportes" deben vivir todos los reportes individuales que quiero mostrar

+ Si quieres mostrar nuevo contenido, agrega un bloque "section -> contents" como ves los otros

+ 🚨🚨 **Importante:** 🚨🚨 Recuerda que la indentación en el archivo "_quarto.yml" es importante, i.e. los espacios y tabuladores iniciales en cada línea son importantes. Te recomiendo copiar/pegar

+ Los reportes "reporte-01.qmd", "reporte-02.qmd", "reporte-03.qmd" y "reporte-04.qmd" son los mismos que revisamos la sesión pasada

+ Los archivos "slides-01.qmd" y "ver-slides-01.qmd" son nuevos... los revisaremos más adelante

+ Ya vimos como cambiar el tema (colores) general del sitio 

```
format:
  html:
    theme: minty
    toc: true
    lang: es
```

+ Puedes cambiar el `theme`. Ahora está en minty. Puedes seleccionar de entre las opciones

https://quarto.org/docs/output-formats/html-themes.html

+ Cambia el theme por uno que te guste, por ejemplo `theme: superhero`

## Paso 5:

+ Abre el archivo "index.qmd" y observa su contenido

+ Ahora ves:

```
---
title: "Reportes trimestrales"
image: foto_gato.jpg
about:
  template: jolla   # opciones: jolla, trestles, solana, marquee, broadside
  links:
    - icon: github
      text: GitHub
      href: https://github.com
    - icon: linkedin
      text: LinkedIn
      href: https://linkedin.com
    - icon: telegram
      text: Telegram
      href: https://t.me
    - icon: slack
      text: Slack
      href: https://slack.com/intl/es-mx/
---
```

+ Cambia el "title" por uno que te guste más, por ejemplo `title: "Listado de reportes..."`

+ En "image" se le debe indicar lo que estás pensando. La imagen del gatito que aparece en la landing page.

+ El campo "template" lo cambiamos varias veces las sesión pasada. Básicamente nos da un catálogo de opciones ya armadas que no necesitan mucha configuración.

+ A diferencia de la sesión pasada ahora aparecen dos nuevos links en la sección de links (el de Telegram y el de Slack):

```
    - icon: telegram
      text: Telegram
      href: https://t.me
    - icon: slack
      text: Slack
      href: https://slack.com/intl/es-mx/
```
Si quieres ver la lista completa, ve a https://icons.getbootstrap.com/

## Paso 6:

+ Abre el archivo "requirements.txt" y observa su contenido

+ Ahora ves
```
jupyter
pandas
polars
matplotlib
numpy
statistics
```
en este archivo aparece lo que seguro ya te imaginas: las librerías de Python que utilizas en tus archivos `.qmd`. Por ejemplo, si en alguno de tus análisis usaste funciones de las librerías Scikit-Learn (`sklearn`) y `statsmodels` simplemente las agregas en el archivo

```
jupyter
pandas
polars
matplotlib
numpy
statistics
sklearn
statsmodels
```

## Paso 7:

+ Abre el archivo "DESCRIPTION" (nota que no tiene ninguna extensión "txt", "md", "qmd"... NO necesita extensión) y observa su contenido

+ Ahora ves

```
Package: misitio
Type: Package
Title: Mi sitio web con Quarto
Version: 1.0.0
Imports:
    dplyr,
    ggplot2,
    knitr,
    rmarkdown,
    reticulate
```
en este archivo aparece lo que seguro ya te imaginas: las librerías de R que utilizas en tus archivos `.qmd`. Por ejemplo, si en alguno de tus análisis usaste funciones de las librerías (`readr`) y `caret` simplemente las agregas en el archivo. Nota que la indentación y las comas son importantes en este archivo.

```
Package: misitio
Type: Package
Title: Mi sitio web con Quarto
Version: 1.0.0
Imports:
    dplyr,
    ggplot2,
    knitr,
    rmarkdown,
    reticulate,
    readr,
    caret
```

## Paso 8:

+ Ahora vamos a crear la GitHub Page, i.e. el link donde la gente consumirá tu sitio web

+ Dentro del sitio de tu repositorio. Busca la sección de "Actions" en la parte superior

+ En la barra lateral izquierda encontrarás la sección "Actions". Justo abajito aparece la sección "All workflows" y justo abajito aparece la sección "Publicar sitio Quarto en GitHub Pages". Dale click a este último

+ En la sección principal verás un cintillo azul que dice "This workflow has a workflow_dispatch event trigger" y un botón "Run workflow". En el selector de dicho botón dale click a "Run workflow" y la magia empezará a ocurrir.... 

+ Ir a "Actions" de nuevo. Dar click al único workflow y esperar a que termine

+ Una vez que termine el workflow te mostrará el link de tu sitio web

+ Para que lo tengas a la mano todo el tiempo, haz lo siguiente:

  + Ve a la página principal de tu repositorio (aquí en GitHub). Del lado derecho encontrarás la palabra "About" con una tuerquita de lado derecho
  + Marca la casilla de "Use your GitHub Pages website" y "Save changes". Ahora verás la dirección de tu sitio justo debajo de la palabra "About"

+ Si haces cambios en tus archivos para modificar algo. Modifica todo lo que tengas que modificar y después vuelve a hacer: Actions -> All workflows -> Publicar sitio Quarto en GitHub Pages -> Run workflow y espera a que vuelva a renderear tu sitio

+ **Observación:** Si una vez que terminó el workflow tu sitio parece no haber hecho los cambios, dale refresh en tu navegador

# Entendamos los archivos que generan los slides que ya aparecen en nuestros sitio

## Slides como su propio sitio web

+ Abre el archivo "mis_reportes/slides-01.qmd" y observa tu contenido

+ Ahora ves

```
---
title: "Slides con R y Python"
subtitle: "Mis resultados en slides"
author: "Tu nombre"
format:
  revealjs:
    theme: moon
    transition: slide
    slide-number: true
    incremental: false
    code-line-numbers: true
---
```

"title", "subtitle" y "author" indican lo que estás imaginando: El título, subtítulo y nombre de la persona autora de los slides. Cambia estos valores con tus favoritos.

+ También ves:

```
format:
  revealjs:
```

+ **Reveal.js** es un framework de código abierto basado en HTML/CSS/JS que permite crear presentaciones web interactivas y dinámicas directamente en tu navegador web favorito... Es lo que usa Quarto para generar slides

+ Pero tranquilízate, Quarto agregó una capa de sintaxis para que no tengas que escribir código Reveal.js "puro". Utiliza muchas funciones que generan plantillas para que sea sencillo para uno que no es web developer "nativo".

+ También ves:

```
format:
  revealjs:
    theme: simple
    transition: slide
    slide-number: true
    incremental: false
    code-line-numbers: true
```

+ En "theme" se pone el nombre de alguna de las varias plantillas existentes. Ahora está `simple`.

+ Otras opciones disponibles son beige, blood, dark, default, dracula, league, moon, night, serif, simple, sky, solarized

+ Si quieres ver cómo se ven éstas, dale click a https://revealjs.com/themes/

+ Explora el sitio y cambia de plantilla

+ En general se separan las slides de la siguiente manera

```
---

Contenido slide 1

---

Contenido slide 2

---

Contenido slide 3

---
```

+ Dentro de cada slide, se puede usar la sintaxis Markdown/RMarkdown/Quarto que hemos usado siempo

```
# Esto es un título

## Esto es un subtítulo

### Esto es un subsubtítulo
```

También bullets y enumeraciones

```
+ Un bullet
+ Otro bullet
  + Un sub-bullet
  + Otro sub-bullet
+ Otro otro bullet
```

+ Un bullet
+ Otro bullet
  + Un sub-bullet
  + Otro sub-bullet
+ Otro otro bullet

## Slides embebidos (embedded) dentro de un sitio web

+ Abre el archivo "mis_reportes/ver-slides-01.qmd" y observa tu contenido

+ Ahora se ve simplemente

```
<iframe src="slides-01.html" width="100%" height="700" style="border:none;"></iframe>
```

+ Básicamente se mete en un "frame" (iframe) el archivo HTML que Quarto crea con los slides rendereados.

+ **OJO**: Nota que en el source dice `src="slides-01.html"` NO "slides-01.qmd". Tras bambalinas GitHub al crear el sitio genera el archivo "slides-01.html", que es la versión renderizada del archivo "slides-01.qmd"

+ Cambia los valores en `width="100%" height="700"` para modificar el tamaño del frame



