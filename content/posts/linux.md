---
title: "Mac Command Line Tools"
date: 2018-01-03T11:48:54-05:00
tags: [linux, tool, utilities]
---

---
## oh my zsh

```
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

<!-- export JAVA_HOME=$(/usr/libexec/java_home) -->
export ANSIBLE_SSH_CONTROL_PATH='/tmp/asb-%%h-%%p-%%r'

```

```bash
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

## tar, gunzip and bzip2

```bash

# x => extract
tar -xvf example.tar  [target] # optional can extract specific target file or subfolder
tar -xzvf example.tar.gz
tar -xjvf example.tar.bz2

# c => create
tar -czvf example.tar.gz   example

```

![compression time](https://www.rootusers.com/wp-content/uploads/2015/08/compression-time.png)
![ratio](https://www.rootusers.com/wp-content/uploads/2015/08/compression-ratio.png)



## homebrew 

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## record terminal

https://asciinema.org/

```
brew install asciinema
asciinema rec

```




## mkdir -p


## Clear python cache files

`find . | grep -E "(__pycache__|\.pyc|\.pyo$)" | xargs rm -rf`

## List binding ports

### lsof - list open files

    $ lsof -i -P -n | grep LISTEN
    SpotifyWe   465 byronzhu    5u  IPv4 0xbb818c7dfcdfcc01      0t0  TCP 127.0.0.1:4380 (LISTEN)
    gotour     1773 byronzhu    3u  IPv4 0xbb818c7e15913961      0t0  TCP 127.0.0.1:3999 (LISTEN)
    epmd       2888 byronzhu    3u  IPv4 0xbb818c7dff51dc01      0t0  TCP *:4369 (LISTEN)
    epmd       2888 byronzhu    4u  IPv6 0xbb818c7df2ef6031      0t0  TCP *:4369 (LISTEN)
    beam.smp   2895 byronzhu   79u  IPv4 0xbb818c7dfce003e1      0t0  TCP *:25672 (LISTEN)
    beam.smp   2895 byronzhu   89u  IPv4 0xbb818c7e00f35c01      0t0  TCP 127.0.0.1:5672 (LISTEN)
    beam.smp   2895 byronzhu   90u  IPv6 0xbb818c7df2ef65f1      0t0  TCP *:1883 (LISTEN)
    beam.smp   2895 byronzhu   91u  IPv6 0xbb818c7df2ef4ef1      0t0  TCP *:61613 (LISTEN)
    beam.smp   2895 byronzhu   92u  IPv4 0xbb818c7dfce01681      0t0  TCP *:15672 (LISTEN)
    hugo      71055 byronzhu   87u  IPv4 0xbb818c7e15913011      0t0  TCP 127.0.0.1:1313 (LISTEN)

### netstat - show network status

    $ netstat -an | grep LISTEN
    tcp4       0      0  *.15672                *.*                    LISTEN
    tcp46      0      0  *.61613                *.*                    LISTEN
    tcp46      0      0  *.1883                 *.*                    LISTEN
    tcp4       0      0  127.0.0.1.5672         *.*                    LISTEN
    tcp4       0      0  *.25672                *.*                    LISTEN
    tcp6       0      0  *.4369                 *.*                    LISTEN
    tcp4       0      0  *.4369                 *.*                    LISTEN
    tcp4       0      0  127.0.0.1.1313         *.*                    LISTEN
    tcp4       0      0  127.0.0.1.3999         *.*                    LISTEN
    tcp4       0      0  127.0.0.1.4380         *.*                    LISTEN



## References

- https://explainshell.com/

