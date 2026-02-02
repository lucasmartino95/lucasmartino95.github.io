---
title: R Markdown
author: ~
date: '2026-01-31'
toc: true
toc-depth: 4
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

Lista de: [documentos](https://rmarkdown.rstudio.com/lesson-9.html)


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
se encuentra el archivo, o indincar la ruta absoluta.

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

  
  ``` default
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
Markdown](https://rmarkdown.rstudio.com/authoring_basics.html)

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

Lista de: [motores](https://rmarkdown.rstudio.com/lesson-5.html)


### Explorando un poco los bloques de código

Podemos utilizar **scripts de bash o zsh** que se ejecutarán en nuestra computadora  y
luego mostrarán el resultado en el documento, por ejemplo con el comando `tree`


``` zsh
tree ~/Code/notes/R_Markdown
```

```
## /home/lucas/Code/notes/R_Markdown
## ├── guide.html
## └── guide.rmd
## 
## 1 directory, 2 files
```

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
  de buscar por todo el documento

- Permite a los usuarios sin conocimientos de programación cambiar valores



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
   computadora, por ejemplo: `mkdir ~/Code/lucas_blog`

2. Ingresamos al directorio: `cd ~/code/lucas_blog`

3. Ejecutamos el comando para inicializar el proyecto. Esto nos creará toda la
   estructura del blog, es decir, los directorios y archivos necesarios para
   luego poder publicar el blog en internet: `Rscript -e "blogdown::new_site()"`

4. Observemos el directorio donde se guardan los artículos:

   
   ``` zsh
   cd ~/Code/lucas_blog
   tree content/post
   ```
   
   ```
   ## content/post
   ## └── 2026-01-31-r-markdown
   ##     ├── index.md
   ##     ├── index.rmd
   ##     └── index.rmd.lock~
   ## 
   ## 2 directories, 3 files
   ```

   Hay artículos que utilizan la extensión `.Rmd`. Estos artículos no se mostrarán en
   el sitio web, porque `blogdown` no los convierte automáticamente para la
   vista previa.  
   
   Antes de ejecutar el comando para lanzar el blog, debemos ejecutar el comando
   para convertir los artículos `.Rmd` en `.html`: `Rscript -e
   "blogdown::build_site(build_rmd = 'timestamp')"`. Si omitimos este paso, `blogdown` ignorará el archivo
   `.Rmd`

5. Ahora sí, para lanzar el blog y verlo en nuestra computadora, con un servidor
   local, utilizamos: `Rscript -e "blogdown::serve_site()"`

6. Cada vez que hacemos cambios en algún archivo `.Rmd`, debemos ejecutar el
   comando del paso 4. No hay necesidad de apagar el servidor y volverlo a
   iniciar.

7. Para crear un artículo, utilizamos el comando: `Rscript -e "blogdown::new_post('My_new_post', ext = '.rmd')"`. Lo editamos en nuestro
   editor de texto y volvemos a ejecutar `Rscript -e "blogdown::build_site(build_rmd = 'timestamp')"` 

**NOTA**: `build_rmd = 'timestamp'` lo que hace es actualizar **solo** los
artículos `.rmd` que han sido modificados reciéntemente con respecto a los
archivos convertidos en `.html` o `.md`

**NOTA 2**: El paquete `blogdown` utiliza el generador de sitios estáticos
**Hugo**. Cada vez que inicializamos un blog, automáticamente si hace falta,
`blogdown` instalará **Hugo**.

### Tabla de contenidos

Para incluir una tabla de contenidos en nuestro documento convertido a `.html` debemos colocar
en el encabezado:


``` yaml
---
toc: true
toc-depth: 4
---
```

`toc-depth: 4` establece el nivel de profundidad de encabezados, en este ejemplo
es hasta 4. Además, tener en cuenta que los encabezados nivel uno, no los tiene
en cuenta `blogdown`.

### Completar el blog  

Si queremos cambiar los links de la barra de navegación, tan solo debemos configurar el
archivo `config.yml`. Si queremos cambiar la página "About", tan solo debemos
hacer los cambios en el archivo `content/about.md`

### Publicar en Netlify

Como último paso, nos queda publicar el blog en internet. Podemos
publicarlo gratuitamente en [Netlify](https://www.netlify.com/). Además, podemos
establecer un [dominio](https://es.wikipedia.org/wiki/Dominio_de_internet)
propio. A continuación una [guía para publicar en
Netlify](https://bookdown.org/yihui/blogdown/netlify.html) aunque es un proceso
muy intuitivo. Básicamente hay que registrarse en Netlify, enlazar nuestro
repositorio de GitHub siguiendo los pasos que ofrece la plataforma, y por último
desplegar el blog.

Una vez configurado, para publicar nuestro artículo, en nuestra computadora debemos ejecutar `Rscript
-e "blogdown::build_site(build_rmd = 'timestamp')"` y subir los cambios a
GitHub. Luego, si tenemos configurada la opción en Netlify para desplegar
automáticamente, no tenemos que hacer nada más. Tan solo actualizar la página
una vez que Netlify haya desplegado el último commit.
