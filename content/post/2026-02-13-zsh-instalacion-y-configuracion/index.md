---
title: 'Zsh: instalación y configuración'
author: Lucas Martino
date: '2026-02-13'
toc: true
slug: zsh-instalacion-y-configuracion
categories: []
tags: []
---

## Zsh

**Zsh** es una *shell* o **estación de trabajo** de línea de comandos que provee una
interfaz de usuario tradicional para los **sitemas operativos tipo Unix**. Por
defecto en las distribuciones de Linux, lo más común es encontrarnos con la
*shell* **Bash**.

Con **Zsh** podemos tener un mejor autocompletado de comandos y resaltado de
sintáxis.

## Instalación

En **Arch Linux** es muy sencillo, solo debemos ejecutar `sudo pacman -S zsh`.
Luego, para cambiar la *shell* por defecto de nuestro usuario, debemos ejecutar: `chsh
-s /usr/bin/zsh/`

Si tenemos un administrador de ventanas *tiling* o *tiling window manager*,
debemos mover el código de `~/.bash_profile` a `~/.zprofile` para que se ejecute [Xorg](https://wiki.archlinux.org/title/Xorg) al iniciar sesión.


## Configuración mínima

Primero vamos a cambiar el *prompt*, que es lo que aparece al costado de los
comandos y suele ofrecer información como el nombre del directorio en donde
estamos. Para eso editamos `~/.zshrc`

### Prompt

Por defecto **Zsh** ofrece un *prompt* bastante genérico, para cambiarlo,
añadimos lo siguiente al archivo de configuración:


``` zsh
# Prompt
autoload -Uz promptinit
promptinit
prompt redhat
```

El `prompt redhat` es el mismo prompt que trae por defecto **Bash**.

### Autocompletado

Para tener una mejor experiencia de autocompletado, añadimos lo siguiente:


``` zsh
# Basic auto/tab complete:
autoload -U compinit
zstyle ':completion:*' menu select
zmodload zsh/complist
compinit
_comp_options+=(globdots)		# Include hidden files.
```

### Resaltado de sintáxis

Para el resaltado de sintáxis vamos a utilizar un plugin. Con el siguiente
comando lo instalamos:

`git clone https://github.com/zdharma-continuum/fast-syntax-highlighting 
/usr/share/zsh/plugins/fast-syntax-highlighting`

Luego, al final del `.zshrc` añadimos:

`source
/usr/share/zsh/plugins/fast-syntax-highlighting/fast-syntax-highlighting.plugin.zsh
2>/dev/null`

## Bonus

### fzf

`fzf` es un complemento para nuestra *shell*. Con `fzf` podemos navegar
interactivamente a través de la historia de nuestros comandos con `ctrl + r`

Esto es muy útil ya que podemos dejar de recordar todos los comandos y en su
lugar utilizar `fzf` y escribir alguna palabra que se encuentre en el comando
que queremos ejecutar. También hay otros comandos útiles, revisar 
[fzf](https://wiki.archlinux.org/title/Fzf). Para instalarlo y configurarlo es muy simple:

1. `sudo pacman -S fzf`

2. Editar `.zshrc` agregando lo siguiente: 
  `source <(fzf --zsh)` 

### Mi .zshrc completo

Dejo a continuación mi `.zshrc` completo, el cual está debidamente comentado:


``` zsh
cat -n ~/.zshrc
```

```
     1	autoload -U colors && colors	# Load colors
     2	setopt autocd		# Automatically cd into typed directory.
     3	stty stop undef		# Disable ctrl-s to freeze terminal.
     4	setopt interactive_comments
     5	
     6	# History in cache directory:
     7	HISTSIZE=10000000
     8	SAVEHIST=10000000
     9	HISTFILE="${XDG_CACHE_HOME:-$HOME/.cache}/zsh/history"
    10	setopt inc_append_history
    11	
    12	# Basic auto/tab complete:
    13	autoload -U compinit
    14	zstyle ':completion:*' menu select
    15	zmodload zsh/complist
    16	compinit
    17	_comp_options+=(globdots)		# Include hidden files.
    18	
    19	# Aliases
    20	alias ls='ls --color=auto'
    21	alias grep='grep --color=auto'
    22	alias vim="nvim"
    23	alias lf="lfrun"
    24	
    25	# Vi mode
    26	bindkey -v
    27	export KEYTIMEOUT=1 # controls the delay (in hundredths of a second) Zsh waits
    28	                    # for subsequent keystrokes to complete a multi-character command
    29	
    30	# Prompt
    31	# PROMPT='[%n@%m %~]$ '
    32	autoload -Uz promptinit
    33	promptinit
    34	prompt redhat
    35	
    36	# Decide which editor is launched when a program wants
    37	# to start an editor
    38	export EDITOR=nvim
    39	
    40	# Set up fzf key bindings and fuzzy completion
    41	source <(fzf --zsh)
    42	
    43	# Add ~/.local/bin to PATH
    44	export PATH="$PATH:$HOME/.local/bin"
    45	
    46	# Load syntax highlighting; should be last.
    47	# 2>/dev/null is a redirection that silences error messages by sending them to
    48	# the null device
    49	source /usr/share/zsh/plugins/fast-syntax-highlighting/fast-syntax-highlighting.plugin.zsh 2>/dev/null
```

