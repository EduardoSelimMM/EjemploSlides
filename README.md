# Instrucciones

## Paso 1:
Ir al botón de "Use this template" -> "Create a new repository"

Llenar los campos

Ir a Settings -> Pages -> Build and deployment -> Source -> GitHub Actions

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

