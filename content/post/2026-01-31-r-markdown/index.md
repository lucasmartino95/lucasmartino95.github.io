---
title: R Markdown
author: Lucas Martino
date: '2026-01-31'
toc: true
slug: r-markdown
categories: []
tags: []
params:
  username: "Lucas"
---

## Introducción

**R Markdown** es un lenguaje de marcado, el cual nos permite escribir texto plano y
**convertirlo en un documento**, por ejemplo:

- PDF
- HTML
- WORD

Lista de: [documentos](https://rmarkdown.rstudio.com/lesson-9.html).


Además, es una **variante del lenguaje Markdown**. Esta variante también nos
permite escribir [bloques de código](#bloques-de-código) y ver el resultado en
el documento final.

Los documentos de **R Markdown**, deben llevar la extensión `.rmd`

## Instalación (Arch Linux)

Para instalar **R Markdown**, debemos seguir los siguientes pasos:

1. Instalar estos paquetes:
    - `r` `pandoc` `base-devel` `texlive`

2. Entrar a la consola interactiva de R con privilegios:
   - `sudo R`

3. Instalar los siguientes paquetes:
   - `install.packages("rmarkdown")`
   - `install.packages("reticulate")`
   - `install.pacakges("languageserver)`

## Encabezado

Todo documento **R Markdown** comienza con un **encabezado YAML** (opcional). En el
encabezado podemos establecer parámetros como:

- Formatos de salida
- Título del documento
- Autor
- Fecha de edición

Lo podemos hacer de la siguiente manera:


``` yaml
---
output:
  html_document:
    toc: true
title: Guía de R Markdown
author: Lucas Martino
date: "`r format(Sys.time(), '%B %d, %Y')`"
---
```

Luego, al ejecutar el comando de conversión, obtendremos un **documento HTML**
debidamente estilizado. El comando sería: `Rscript -e "rmarkdown::render('nombre_del_archivo.rmd')"`

**NOTA**: Para hacer la conversión, debemos estar en el mismo directorio donde
se encuentra el archivo, o indicar la ruta absoluta.

**NOTA 2**: Para que no se ejecute la función dentro de `date`, utilizamos el
[motor
verbatim](https://bookdown.org/yihui/rmarkdown-cookbook/verbatim-code-chunks.html).
Es decir, este motor sirve para fines demostrativos, cuando no se quiere 
ejecutar el [bloque de código](#bloques-de-código) en cuestión.  

También hay otro motor interesante, como `embed`, que permite incrustar 
código de un archivo externo. Se puede ver en este mismo 
[link](https://bookdown.org/yihui/rmarkdown-cookbook/verbatim-code-chunks.html).

### Convertir a varios formatos

Para convertir nuestro documento `.rmd` en varios formatos al mismo tiempo,
debemos colocar en el encabezado los siguientes parámetros:


``` yaml
---
output:
  html_document: default
  pdf_document: default
---
```

Luego, ejecutar el siguiente comando de conversión:
`Rscript -e "rmarkdown::render('nombre_del_archivo.rmd', output_format='all')"`

## Sintáxis

Redactar un texto en **R Markdown** es tan simple como **seguir las reglas de
Markdown**. Por ejemplo:

  
  ``` markdown
  Encabezados:
  # Título
  ## Subtítulo
  ### Subtítulo del subtítulo
  
  Negrita:
  **Texto**
  
  Itálica:
  *Texto*
  
  Links:
  [wikipedia](https://wikipedia.org)
  
  Salto de línea:
  Para hacer un salto de línea debemos colocar dos espacios al final de la
  línea
  ```

### Tablas


``` markdown
| Encabezado 1 | Encabezado 2 | Encabezado 3 |
| :--- | :--- | :--- |
| Fila 1, columna 1 | Fila 1 , columna 2 | Fila 1, columna 3 |
| Fila 2, columna 1 | Fila 2, columna 2 | Fila 2, columna 3 |
```

| Encabezado 1 | Encabezado 2 | Encabezado 3 |
| :--- | :--- | :--- |
| Fila 1, columna 1 | Fila 1 , columna 2 | Fila 1, columna 3 |
| Fila 2, columna 1 | Fila 2, columna 2 | Fila 2, columna 3 |

Lista de: [elementos
Markdown](https://rmarkdown.rstudio.com/authoring_basics.html).

## Bloques de código


```` python
```{python}
print("Hello world")
```
````

Ese código, al convertir el documento `.rmd`, mostrará el bloque y su
consiguiente resultado. El **motor utilizado** es Python: 


``` python
print("Hello world")
```

```
## Hello world
```

Lista de: [motores](https://rmarkdown.rstudio.com/lesson-5.html).


### Explorando un poco los bloques de código

Podemos utilizar **scripts de bash o zsh** que se ejecutarán en nuestra computadora  y
luego mostrarán el resultado en el documento, por ejemplo con el comando `tree`


``` zsh
tree ~/Code/lucasmartino95.github.io/content/post
```

```
/home/lucas/Code/lucasmartino95.github.io/content/post
├── 2026-01-31-r-markdown
│   ├── index.md
│   ├── index.rmd
│   └── index.rmd.lock~
└── 2026-02-03-dotfiles
    ├── index.md
    └── index.rmd

3 directories, 5 files
```

Este [bloque de código](#bloques-de-código) lleva la **opción** `comment = ""` para evitar que incruste
comentarios en la salida. Sería: `{zsh, comment = ""}`

### Parámetros

También podemos utilizar parámetros para ejecutar bloques de código
dinámicos. Por ejemplo, al colocar lo siguiente en el encabezado del documento:


``` yaml
---
params:
  username: "Lucas"
---
```

De este modo, logramos:

- Generar múltiples versiones sin duplicar el documento, con el siguiente
  comando:

  
  ``` zsh
  Rscript -e "rmarkdown::render('nombre_del_archivo.rmd',
  params = list(username = 'Roberto'))"
  ```
      
- Gestionar mejor las variables desde el encabezado YAML, en lugar
  de buscar por todo el documento.

- Permite a los usuarios sin conocimientos de programación cambiar valores.


``` python
def hello(name: str) -> None:
  print(f"Hello, {name}")


name = r.params['username']
hello(name)
```

```
## Hello, Lucas
```

## Blog

A continuación explicaré los pasos a seguir para hacer un blog utilizando **R
Markdown** y el paquete `blogdown`

1. Entrar a la consola interactiva de R: `sudo R`

2. Ejecutar: `install.packages("blogdown")`

Listo! Ahora solo queda **desplegar** el blog.

### Desplegando el blog

1. Creamos el directorio donde queremos guardar nuestro blog, en nuestra
   computadora, por ejemplo: `mkdir ~/Code/lucasmartino95.github.io`

2. Ingresamos al directorio: `cd ~/code/lucasmartino95.github.io`

3. Ejecutamos el comando para inicializar el proyecto. Esto nos creará toda la
   estructura del blog, es decir, los directorios y archivos necesarios para
   luego poder publicar el blog en internet: `Rscript -e "blogdown::new_site()"`

4. Observemos el directorio donde se guardan los artículos:

   
   ``` zsh
   cd ~/Code/lucasmartino95.github.io
   tree content/post
   ```
   
   ```
   content/post
   ├── 2026-01-31-r-markdown
   │   ├── index.md
   │   ├── index.rmd
   │   └── index.rmd.lock~
   └── 2026-02-03-dotfiles
       ├── index.md
       └── index.rmd
   
   3 directories, 5 files
   ```

   Los artículos utilizan la extensión `.rmd`. Estos artículos no se mostrarán en
   el sitio web, porque `blogdown` no los convierte automáticamente para la
   vista previa.  
   
   Antes de ejecutar el comando para lanzar el blog, debemos ejecutar el comando
   para convertir los artículos `.rmd` en `.html`: `Rscript -e
   "blogdown::build_site(build_rmd = 'timestamp')"`. Si omitimos este paso, `blogdown` ignorará el archivo
   `.rmd`

5. Ahora sí, para lanzar el blog y verlo en nuestra computadora, con un servidor
   local, utilizamos: `Rscript -e "blogdown::serve_site()"`

6. Cada vez que hacemos cambios en algún archivo `.rmd`, debemos ejecutar el
   comando del paso cuatro. No hay necesidad de apagar el servidor y volverlo a
   iniciar.

7. Para crear un artículo, utilizamos el comando: `Rscript -e "blogdown::new_post('My_new_post', ext = '.rmd')"`. Lo editamos en nuestro
   editor de texto y volvemos a ejecutar `Rscript -e "blogdown::build_site(build_rmd = 'timestamp')"` 

**NOTA**: `build_rmd = 'timestamp'` lo que hace es actualizar **solo** los
artículos `.rmd` que han sido modificados reciéntemente con respecto a los
archivos convertidos en `.html` o `.md`

**Si sucede que no se actualizan** los
bloques de código, establecer `build_rmd = TRUE` para forzar la conversión.

**NOTA 2**: El paquete `blogdown` utiliza el generador de sitios estáticos
**Hugo**. Cada vez que inicializamos un blog, automáticamente si hace falta,
`blogdown` instalará **Hugo**.

### Tabla de contenidos

Para incluir una tabla de contenidos en nuestros artículos, debemos colocar 
en el encabezado de cada artículo `.rmd`:


``` yaml
---
toc: true
# toc_depth: 4
---
```

`toc-depth: 4` Establece el nivel de profundidad de encabezados, en este ejemplo
es hasta cuatro. Además, tener en cuenta que los encabezados nivel uno, no los tiene
en cuenta `blogdown`. **Por el momento no logro hacer funcionar esta llave. El
nivel de profundidad va hasta tres**.

### Completar el blog  

Si queremos **cambiar los links** de la barra de navegación, tan solo debemos configurar el archivo `config.yml`. Si queremos **cambiar la página "About"**, tan solo debemos
hacer los cambios en el archivo `content/about.md`

### Publicar en GitHub Pages

Para empezar, seguir la [guía para crear un
repositorio](https://docs.github.com/en/pages/quickstart) para nuestro blog.

Luego, en GitHub, desde el repositorio que creamos, ir a: **Settings -> Actions
-> General -> Scroll down to "Workflow permissions" -> Check "Read and write
permissions"**.

#### Agregar archivo hugo.yaml

Luego, seguir los pasos de esta [guía](https://gohugo.io/host-and-deploy/host-on-github-pages/) desde el sitio oficial de Hugo. **Omitir paso dos**.


#### Ignorar directorio public/

Para finalizar, crear un archivo `.gitignore` e incluir el directorio
`public/` ya que GitHub Actions creará esta carpeta por nosotros al subir cambios al
repositorio. Esta carpeta `public/` es la carpeta de producción de nuestro blog.

#### Subir cambios al blog

Si editamos un archivo `.rmd`, aún debemos ejecutar
`Rscript -e "blogdown::build_site('build_rmd = 'timestamp')"` y luego subir los
cambios al repositorio. Luego GitHub Actions se encargará de actualizar la carpeta
`public/` del repositorio.

### Agregar soporte para comentarios

Para el tema por defecto: **hugo-lithium**.

Podemos habilitar una sección de comentarios en el pie de cada artículo. Esto lo
podemos hacer con [Giscus](https://giscus.app/es). Los pasos a seguir son muy
sencillos:

1. Crear un repositorio público en GitHub.

2. [Instalar](https://github.com/apps/giscus) la app de Giscus en el repo
   que creamos.

3. [Habilitar Discussions en el
   repositorio](https://docs.github.com/en/github/administering-a-repository/managing-repository-settings/enabling-or-disabling-github-discussions-for-a-repository).

4. Copiar el **script** de la página de [Giscus](https://giscus.app/es) luego de
   haber completado la configuración en el mismo sitio web.

5. Pegar el **script** en `Code/lucasmartino95.github.io/layouts/partials` en un archivo
   llamado `comments.html` (crear directorio y archivo si no existen). **Hugo** revisará primero esa
   carpeta antes que la del tema **hugo-lithium**. Esto es muy útil ya que si se
   actualiza el tema, no sobrescribirá nuestras modificaciones.

6. Finalmente, habilitar la sección de comentarios para cada artículo. Vamos 
   a copiar el archivo `single.html` de nuestro tema **hugo-lithium** y editarlo: 

    
    ``` zsh
    cd Code/lucasmartino95.github.io
    
    mkdir -p layouts/_default
    
    cp themes/hugo-lithium/layouts/_default/single.html layouts/_default
    ```

    
    ``` zsh
    # Observar línea 25 a 27, es lo que debemos agregar
    cat -n ~/Code/lucasmartino95.github.io/layouts/_default/single.html
    ```
    
    ```
         1	{{ partial "header.html" . }}
         2	
         3	<main class="content" role="main">
         4	
         5	  <article class="article">
         6	    {{ if eq .Section "post" }}
         7	    <span class="article-duration">Lectura de {{ .ReadingTime }} minutos</span>
         8	    {{ end }}
         9	
        10	    <h1 class="article-title">{{ .Title }}</h1>
        11	
        12	    {{ if eq .Section "post" }}
        13	    <span class="article-date">{{ .Date.Format "02-01-2006" }}</span>
        14	    {{ end }}
        15	
        16	    <div class="article-content">
        17	      {{ if .Params.toc }}
        18	      {{ .TableOfContents }}
        19	      {{ end }}
        20	      {{ .Content }}
        21	    </div>
        22	  </article>
        23	
        24	  {{ partial "disqus.html" .}}
        25	  {{ if .Params.comments | default true }}
        26	    {{ partial "comments.hmtl" }}
        27	  {{ end }}
        28	
        29	</main>
        30	
        31	{{ partial "footer.html" . }}
    ```

**Giscus** utiliza la sección **Discussions** de GitHub, donde aloja los comentarios,
por lo que si alguien quiere comentar en nuestro artículo, debe ingresar con su
cuenta de GitHub.

Un acierto de **Giscus** es que tiene traducción al
español. Solo debemos establecer el idioma en el **script**.

Para **deshabilitar los comentarios** en la página "About", editar
`content/about.md` y agregar al encabezado:


``` yaml
comments: false
```

### Editar cuadro de tiempo de lectura


``` zsh
cd ~/Code/lucasmartino95.github.io
cat -n layouts/_default/single.html

# Observar línea 7
```

```
     1	{{ partial "header.html" . }}
     2	
     3	<main class="content" role="main">
     4	
     5	  <article class="article">
     6	    {{ if eq .Section "post" }}
     7	    <span class="article-duration">Lectura de {{ .ReadingTime }} minutos</span>
     8	    {{ end }}
     9	
    10	    <h1 class="article-title">{{ .Title }}</h1>
    11	
    12	    {{ if eq .Section "post" }}
    13	    <span class="article-date">{{ .Date.Format "02-01-2006" }}</span>
    14	    {{ end }}
    15	
    16	    <div class="article-content">
    17	      {{ if .Params.toc }}
    18	      {{ .TableOfContents }}
    19	      {{ end }}
    20	      {{ .Content }}
    21	    </div>
    22	  </article>
    23	
    24	  {{ partial "disqus.html" .}}
    25	  {{ if .Params.comments | default true }}
    26	    {{ partial "comments.hmtl" }}
    27	  {{ end }}
    28	
    29	</main>
    30	
    31	{{ partial "footer.html" . }}
```

### Cambiar formato de fecha al español

Para cambiar la fecha que se muestra en el blog al español, debemos modificar
los archivos `single.html` y `list.html`. En ambos debemos cambiar la línea que
dice:

`{{ .Date.Format "2006-01-02" }}`

Por esto:

`{{ .Date.Format "02-01-2006" }}`

Estos archivos se encuentran en la carpeta del tema **hugo-lithium**. Si ya
tenemos el archivo `single.html` en `layouts/_default` de nuestro directorio
raíz, tan solo debemos copiar el archivo `list.html` de la [misma
manera](#agregar-soporte-para-comentarios) que
hicimos con `single.html` en el paso seis para poder editarlos y conservar las
modificaciones.

Por último, modificar el archivo `config.yaml` de nuestro directorio raíz y
modificar la llave: 

`post: /:year/:month/:day/:slug/`

Por esto:

`post: /:day/:month/:year/:slug`


### Cambiar URL de la página "About"

Editamos `.config.yaml` del directorio raíz:


``` yaml
menu:
  main:
    - name: About
      url: /about/
```

Yo lo cambié por


``` yaml
- name: Sobre mí
  url: /sobre-mi/
```

Entonces, ahora editamos el archivo `content/about.md` agregando lo siguiente al
encabezado:


``` yaml
url: '/sobre-mi/'
```

### Definir lenguaje de nuestro sitio

Lo último que nos queda por hacer si es que deseamos tener nuestro blog en
español, es definir el atributo `lang` en nuestros `.html`

Para eso, lo que tenemos que hacer es solo una cosa, editar `config.yaml`


``` yaml
languageCode: es-AR
```
