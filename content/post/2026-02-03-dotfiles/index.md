---
title: Dotfiles
author: Lucas Martino 
date: '2026-02-03'
toc: true
slug: dotfiles
categories: []
tags: []
---

## Prerrequisito para seguir la guía

[Enlace simbólico](https://es.wikipedia.org/wiki/Enlace_simbólico)

## ¿Qué son los dotfiles?

Se le llama **dotfiles** a los archivos de configuración de un sistema operativo
tipo [Unix](https://es.wikipedia.org/wiki/Unix). Tal como su nombre lo indica, suelen ser archivos que tienen un `.` al
principio de su nombre. Esto causa que sean archivos ocultos. Pero también
incluye archivos que no sean ocultos, es decir sin un `.` al principio.

## La manera más simple de manejar los dotfiles

**GNU Stow** es la herramienta que utilizo para manejar los dotfiles de mi
computadora. Eso combinado con **Git** y **GitHub** para tener acceso a los archivos desde
cualquier otra computadora. El proceso es muy sencillo con **GNU Stow**.

## Instalación (Arch Linux)

Simplemente ejecutar `sudo pacman -S stow`

## ¿Cómo usar GNU Stow?

Para empezar, vamos a mover los archivos que están en nuestro `$HOME` a una
carpeta llamada `dotfiles`, entonces, por ejemplo:


``` zsh
cd ~

mkdir -p ~/Code/dotfiles

mv ~/.bashrc Code/dotfiles
mv ~/.bash_profile Code/dotfiles
mv ~/.gitconfig Code/dotfiles

mv ~/.config/alacritty # Movemos un directorio
```

Listo, una vez que movimos los archivos y/o directorios a nuestra carpeta `dotfiles`,
básicamente vamos a generar enlaces simbólicos a donde corresponda para cada
archivo o directorio:

`stow -d ~/Code/dotfiles -t ~/ .`

Lo que hace este comando es: 

- `-d` Establece el directorio donde se ejecutará `stow`, en este caso es:
  `~/Code/dotfiles`

- `-t` Establece el directorio donde generará los enlaces simbólicos, en este
  caso es: `~/`

- `.` Por último, el `.` indica que se quiere enlazar todos los archivos y/o
  directorios donde se ejecutará `stow`

## Hacer que GNU Stow ignore ciertos archivos

Podría ser que dentro de tu directorio `dotfiles` tengas archivos que quisieras
ignorar, es decir, que **GNU Stow** no genere los enlaces simbólicos, por ejemplo un
archivo `README.md`. Para eso podemos crear un archivo `.stow-local-ignore` y
dentro colocamos el archivo o directorio que queremos ignorar. Por ejemplo:


``` zsh
cat ~/Code/dotfiles/.stow-local-ignore
```

```
## README.md
## .gitignore
## .git
## dotfiles_docs
## .mypy_cache
```

## Git y GitHub

Una vez que ya generamos los enlaces simbólicos y opcionalmente hayamos
configurado nuestro archivo `.stow-local-ignore`, ya podemos iniciar un 
repositorio Git en nuestra carpeta `~/Code/dotfiles` y subirla a GitHub. 

## Deshacer enlaces simbólicos

Si por algún motivo, necesitas deshacer los enlaces simbólicos que generó **GNU
Stow**, la forma más sencilla de hacerlo es con el comando: `stow -D .` dentro
del directorio raíz de los `dotfiles`. Por ejemplo, si quisieras cambiar la
ubicación de tu carpeta `dotfiles` luego de haber generado los enlaces simbólicos.
