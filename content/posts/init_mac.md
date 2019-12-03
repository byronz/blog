---
title: "Init_mac"
date: 2018-05-26T00:55:53-04:00
tags: [mac, install]
---

**REMIDNER** 
    copy your previous terminal history

## Set up byronz.github.com

1. `ssh-keygen -t rsa -b 4096 -C "byron.zhu.xq@gmail.com"`
2. `cat ~/.ssh/id_rsa.pub | pbcopy`
3. add ssh key in github
4. `git clone git@github.com:byronz/blog.git`
5. if you have public, need to readd as submodule `git rm -f public`
6. `git submodule add -b master git@github.com:byronz/byronz.github.io.git public`
7. cd themes
8. `git submodule add -b master git@github.com:mgjohansen/hucore.git hucore`
9. install hugo `brew install hugo`

https://gohugo.io/hosting-and-deployment/hosting-on-github/

## Python

- brew install python3
- cd /tmp && curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
- python3 get-pip.py



## Setup Docker

- https://store.docker.com/editions/community/docker-ce-desktop-mac


## Setup VS Code

### Python

### Golang


## Setup iterm2

1. install **zsh** `brew install zsh` and **oh my zsh** `sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
`
2. [install powerline fonts](https://github.com/powerline/fonts) and set the font as iterm text font
3. .zshrc 

```
# If you come from bash you might have to change your $PATH.
# export PATH=$HOME/bin:/usr/local/bin:$PATH

# Path to your oh-my-zsh installation.
export ZSH=/Users/byronz/.oh-my-zsh

# Set name of the theme to load. Optionally, if you set this to "random"
# it'll load a random theme each time that oh-my-zsh is loaded.
# See https://github.com/robbyrussell/oh-my-zsh/wiki/Themes
ZSH_THEME="agnoster"

# Set list of themes to load
# Setting this variable when ZSH_THEME=random
# cause zsh load theme from this variable instead of
# looking in ~/.oh-my-zsh/themes/
# An empty array have no effect
# ZSH_THEME_RANDOM_CANDIDATES=( "robbyrussell" "agnoster" )

# Uncomment the following line to use case-sensitive completion.
# CASE_SENSITIVE="true"

# Uncomment the following line to use hyphen-insensitive completion. Case
# sensitive completion must be off. _ and - will be interchangeable.
# HYPHEN_INSENSITIVE="true"

# Uncomment the following line to disable bi-weekly auto-update checks.
# DISABLE_AUTO_UPDATE="true"

# Uncomment the following line to change how often to auto-update (in days).
# export UPDATE_ZSH_DAYS=13

# Uncomment the following line to disable colors in ls.
# DISABLE_LS_COLORS="true"

# Uncomment the following line to disable auto-setting terminal title.
# DISABLE_AUTO_TITLE="true"

# Uncomment the following line to enable command auto-correction.
# ENABLE_CORRECTION="true"

# Uncomment the following line to display red dots whilst waiting for completion.
# COMPLETION_WAITING_DOTS="true"

# Uncomment the following line if you want to disable marking untracked files
# under VCS as dirty. This makes repository status check for large repositories
# much, much faster.
# DISABLE_UNTRACKED_FILES_DIRTY="true"

# Uncomment the following line if you want to change the command execution time
# stamp shown in the history command output.
# The optional three formats: "mm/dd/yyyy"|"dd.mm.yyyy"|"yyyy-mm-dd"
# HIST_STAMPS="mm/dd/yyyy"

# Would you like to use another custom folder than $ZSH/custom?
# ZSH_CUSTOM=/path/to/new-custom-folder

# Which plugins would you like to load? (plugins can be found in ~/.oh-my-zsh/plugins/*)
# Custom plugins may be added to ~/.oh-my-zsh/custom/plugins/
# Example format: plugins=(rails git textmate ruby lighthouse)
# Add wisely, as too many plugins slow down shell startup.
plugins=(
  git
)

source $ZSH/oh-my-zsh.sh

# User configuration

# export MANPATH="/usr/local/man:$MANPATH"

# You may need to manually set your language environment
# export LANG=en_US.UTF-8

# Preferred editor for local and remote sessions
# if [[ -n $SSH_CONNECTION ]]; then
#   export EDITOR='vim'
# else
#   export EDITOR='mvim'
# fi

# Compilation flags
# export ARCHFLAGS="-arch x86_64"

# ssh
# export SSH_KEY_PATH="~/.ssh/rsa_id"

# Set personal aliases, overriding those provided by oh-my-zsh libs,
# plugins, and themes. Aliases can be placed here, though oh-my-zsh
# users are encouraged to define aliases within the ZSH_CUSTOM folder.
# For a full list of active aliases, run `alias`.
#
# Example aliases
# alias zshconfig="mate ~/.zshrc"
# alias ohmyzsh="mate ~/.oh-my-zsh"
alias gr='grep -Rnif /dev/stdin . <<<'
alias tri='tree -I "*.pyc|__pycache__"'
alias youtube-dl='/usr/local/Cellar/youtube-dl/2018.01.14/bin/youtube-dl'
alias y2mp3='youtube-dl --extract-audio --audio-format mp3  --audio-quality 0'

# alias for git
alias gs="git status"
alias gsv="git status -v"
alias gaa="git add -A"
alias gcm="git commit -m"
alias gk="git checkout"
alias gd='git diff'
alias gdc='git diff --cached'
```

set ./oh-my-zsh.sh

```

# Check for updates on initial load...
if [ "$DISABLE_AUTO_UPDATE" != "true" ]; then
  env ZSH=$ZSH DISABLE_UPDATE_PROMPT=$DISABLE_UPDATE_PROMPT zsh -f $ZSH/tools/check_for_upgrade.sh
fi

# Initializes Oh My Zsh

# add a function path
fpath=($ZSH/functions $ZSH/completions $fpath)

# Load all stock functions (from $fpath files) called below.
autoload -U compaudit compinit

# Set ZSH_CUSTOM to the path where your custom config files
# and plugins exists, or else we will use the default custom/
if [[ -z "$ZSH_CUSTOM" ]]; then
    ZSH_CUSTOM="$ZSH/custom"
fi

# Set ZSH_CACHE_DIR to the path where cache files should be created
# or else we will use the default cache/
if [[ -z "$ZSH_CACHE_DIR" ]]; then
  ZSH_CACHE_DIR="$ZSH/cache"
fi


# Load all of the config files in ~/oh-my-zsh that end in .zsh
# TIP: Add files you don't want in git to .gitignore
for config_file ($ZSH/lib/*.zsh); do
  custom_config_file="${ZSH_CUSTOM}/lib/${config_file:t}"
  [ -f "${custom_config_file}" ] && config_file=${custom_config_file}
  source $config_file
done


is_plugin() {
  local base_dir=$1
  local name=$2
  test -f $base_dir/plugins/$name/$name.plugin.zsh \
    || test -f $base_dir/plugins/$name/_$name
}
# Add all defined plugins to fpath. This must be done
# before running compinit.
for plugin ($plugins); do
  if is_plugin $ZSH_CUSTOM $plugin; then
    fpath=($ZSH_CUSTOM/plugins/$plugin $fpath)
  elif is_plugin $ZSH $plugin; then
    fpath=($ZSH/plugins/$plugin $fpath)
  fi
done

# Figure out the SHORT hostname
if [[ "$OSTYPE" = darwin* ]]; then
  # macOS's $HOST changes with dhcp, etc. Use ComputerName if possible.
  SHORT_HOST=$(scutil --get ComputerName 2>/dev/null) || SHORT_HOST=${HOST/.*/}
else
  SHORT_HOST=${HOST/.*/}
fi

# Save the location of the current completion dump file.
if [ -z "$ZSH_COMPDUMP" ]; then
  ZSH_COMPDUMP="${ZDOTDIR:-${HOME}}/.zcompdump-${SHORT_HOST}-${ZSH_VERSION}"
fi

if [[ $ZSH_DISABLE_COMPFIX != true ]]; then
  # If completion insecurities exist, warn the user without enabling completions.
  if ! compaudit &>/dev/null; then
    # This function resides in the "lib/compfix.zsh" script sourced above.
    handle_completion_insecurities
  # Else, enable and cache completions to the desired file.
  else
    compinit -d "${ZSH_COMPDUMP}"
  fi
else
  compinit -u -d "${ZSH_COMPDUMP}"
fi

# Load all of the plugins that were defined in ~/.zshrc
for plugin ($plugins); do
  if [ -f $ZSH_CUSTOM/plugins/$plugin/$plugin.plugin.zsh ]; then
    source $ZSH_CUSTOM/plugins/$plugin/$plugin.plugin.zsh
  elif [ -f $ZSH/plugins/$plugin/$plugin.plugin.zsh ]; then
    source $ZSH/plugins/$plugin/$plugin.plugin.zsh
  fi
done

# Load all of your custom configurations from custom/
for config_file ($ZSH_CUSTOM/*.zsh(N)); do
  source $config_file
done
unset config_file

# Load the theme
if [[ "$ZSH_THEME" == "random" ]]; then
  if [[ "${(t)ZSH_THEME_RANDOM_CANDIDATES}" = "array" ]] && [[ "${#ZSH_THEME_RANDOM_CANDIDATES[@]}" -gt 0 ]]; then
    themes=($ZSH/themes/${^ZSH_THEME_RANDOM_CANDIDATES}.zsh-theme)
  else
    themes=($ZSH/themes/*zsh-theme)
  fi
  N=${#themes[@]}
  ((N=(RANDOM%N)+1))
  RANDOM_THEME=${themes[$N]}
  source "$RANDOM_THEME"
  echo "[oh-my-zsh] Random theme '$RANDOM_THEME' loaded..."
else
  if [ ! "$ZSH_THEME" = ""  ]; then
    if [ -f "$ZSH_CUSTOM/$ZSH_THEME.zsh-theme" ]; then
      source "$ZSH_CUSTOM/$ZSH_THEME.zsh-theme"
    elif [ -f "$ZSH_CUSTOM/themes/$ZSH_THEME.zsh-theme" ]; then
      source "$ZSH_CUSTOM/themes/$ZSH_THEME.zsh-theme"
    else
      source "$ZSH/themes/$ZSH_THEME.zsh-theme"
    fi
  fi
fi

# agnoster prompt modified
ZSH_THEME_GIT_PROMPT_DIRTY='±'

function _git_prompt_info() {
  ref=$(git symbolic-ref HEAD 2> /dev/null) || ref="➦ $(git show-ref --head -s --abbrev |head -n1 2> /dev/null)"
  echo "${ref/refs\/heads\// }$(parse_git_dirty)"
}

function _git_info() {
  if $(git rev-parse --is-inside-work-tree >/dev/null 2>&1); then
    local BG_COLOR=green
    if [[ -n $(parse_git_dirty) ]]; then
      BG_COLOR=yellow
      FG_COLOR=black
    fi

    if [[ ! -z $(git ls-files --other --exclude-standard 2> /dev/null) ]]; then
        BG_COLOR=red
        FG_COLOR=white
    fi
    echo "%{%K{$BG_COLOR}%}%{%F{$FG_COLOR}%} $(_git_prompt_info) %{%F{$BG_COLOR}%K{blue}%}"
  else
    echo "%{%K{blue}%}"
  fi
}

function virtualenv_info {
    [ $VIRTUAL_ENV ] && echo '('`basename $VIRTUAL_ENV`') '
}

PROMPT_HOST='%{%b%F{gray}%K{black}%} %(?.%{%F{green}%}✔.%{%F{red}%}✘)%{%F{yellow}%} %n %{%F{black}%}'
PROMPT_DIR='%{%F{white}%} %~%  '
PROMPT_SU='%(!.%{%k%F{blue}%K{black}%}%{%F{yellow}%} ⚡ %{%k%F{black}%}.%{%k%F{blue}%})%{%f%k%b%}'

PROMPT='%{%f%b%k%}$PROMPT_HOST$(_git_info)$PROMPT_DIR$PROMPT_SU
$(virtualenv_info)❯ '
RPROMPT='%{$fg[green]%}[%*]%{$reset_color%}'

```


