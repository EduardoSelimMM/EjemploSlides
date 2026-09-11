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
          - mis_reportes/ver_slides-01.qmd
```

+ Observa que con `section: "Título de la sección"` y `contents:` seguido de "mis_reportes/reporte-0X.qmd" se le dice a Quarto que quiero mostrar un nuevo contenido que construí mediante un archivo Quarto individual ("reporte-0X.qmd")

+ Esto significa que en la carpeta "mis_reportes" deben vivir todos los reportes individuales que quiero mostrar.

+ 🚨🚨 **Importante:** 🚨🚨 Recuerda que la indentación en el archivo "_quarto.yml" es importante, i.e. los espacios y tabuladores iniciales en cada línea son importantes. Te recomiendo copiar/pegar.

+ Los reportes "reporte-01.qmd", "reporte-02.qmd", "reporte-03.qmd" y "reporte-04.qmd" son los mismos que revisamos la sesión pasada.

+ Los archivos "slides-01.qmd" y "ver-slides-01.qmd" son nuevos... los revisaremos más adelante




En el archivo "_quarto.yml" encontrarás una parte que se ve así

```
format:
  html:
    theme: minty
    toc: true
    lang: es
```

Pues cambiar el `theme`. Ahora está en minty. Puedes seleccionar de entre las opciones

https://quarto.org/docs/output-formats/html-themes.html

Supongamos que me gustó el tema "superhero". Entonces lo cambio

```
format:
  html:
    theme: superhero
    toc: true
    lang: es
```

Crear un documento que se llame
".github/workflows/publish.yml"

Con el siguiente contenido

```
name: Publicar sitio Quarto en GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Clonar repositorio
        uses: actions/checkout@v4

      - name: Instalar Quarto
        uses: quarto-dev/quarto-actions/setup@v2

      - name: Instalar R
        uses: r-lib/actions/setup-r@v2
        with:
          r-version: "release"

      - name: Instalar paquetes de R
        uses: r-lib/actions/setup-r-dependencies@v2
        with:
          packages: |
            any::ggplot2
            any::dplyr
            any::knitr
            any::rmarkdown
            any::readr

      - name: Renderizar sitio
        run: quarto render

      - name: Subir artefacto para Pages
        uses: actions/upload-pages-artifact@v3
        with:
          path: _site

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Desplegar en GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Una vez que exista el documento "publish.yml" se empezará a crear el sitio. Esto tomará unos minutos

Ir a "Actions". Dar click al único workflow y esperar a que termine

Una vez que termine el workflow te mostrará el link de tu sitio web.

Para que lo tengas a la mano todo el tiempo, haz lo siguiente:

Ve a la página principal de tu repositorio (aquí en GitHub). Del lado derecho encontrarás la palabra "About" con una tuerquita de lado derecho.

Marca la casilla de "Use your GitHub Pages website" y "Save changes". Ahora verás la dirección de tu sitio justo debajo de la palabra "About"

## ¿Quieres agregar chunks de R y Python en un mismo reporte?

Ve archivo "publish.yml"

```
name: Publicar sitio Quarto en GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Clonar repositorio
        uses: actions/checkout@v4

      - name: Instalar Quarto
        uses: quarto-dev/quarto-actions/setup@v2

      - name: Instalar R
        uses: r-lib/actions/setup-r@v2
        with:
          r-version: "release"

      - name: Instalar paquetes de R
        uses: r-lib/actions/setup-r-dependencies@v2

      - name: Instalar Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.12"
          cache: 'pip'

      - name: Instalar paquetes de Python
        run: pip install -r requirements.txt

      - name: Renderizar sitio
        run: quarto render
        env:
          RETICULATE_PYTHON: ${{ env.pythonLocation }}/bin/python3

      - name: Subir artefacto para Pages
        uses: actions/upload-pages-artifact@v3
        with:
          path: _site

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - name: Desplegar en GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

