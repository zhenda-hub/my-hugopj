+++
title = 'Ubuntu使用配置'
subtitle = ""
date = 2024-04-30T13:43:06+08:00
draft = true
toc = true
tags = ["linux"]
+++

# ubuntu 使用配置

[toc]

## app install uninstall

sys_update.sh

```bash
#!/bin/bash
echo "update:"
sudo apt update
echo "upgrade:"
sudo apt upgrade
echo "autoremove:"
sudo apt autoremove
```

```bash
sudo apt install inetutils-traceroute
sudo apt install flameshot
sudo apt-get install gnome-shell-pomodoro
```

## settings

再次点击最小化应用

```bash
gsettings set org.gnome.shell.extensions.dash-to-dock click-action 'minimize'
```

### .\*rc file settings

-   ~/.bashrc

```bash
# auto ls
function cd {
    builtin cd "$@" && ls
}

# alias
alias ls='ls --time-style=long-iso'
alias ll='ls -alFh'
```

-   ~/.vimrc

```bash
syntax on
set autoindent
set tabstop=4
set shiftwidth=4
set cursorline
set backup
set backupdir=~/.vim/undo
```

-   ~/.nanorc

```bash
include "/usr/share/nano/*.nanorc"
set tabsize 4
set backup
```

## zip file

```bash
unzip xxxfile
```

<!--
## Homebrew

https://docs.brew.sh/Homebrew-on-Linux -->

## ssh

<!-- ssh -v ubuntu@134.175.124.152 -->

```bash
ssh -v 用户名@ip地址
scp
```

## open picture

```bash
eog xxx.png
```

## resize mem swap

```bash
# resize swap
sudo swapoff -a
sudo dd if=/dev/zero of=/swapfile bs=1G count=16
sudo chmod 0600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile

# look swap
swapon --show
free -h
```

https://askubuntu.com/questions/178712/how-to-increase-swap-space

## 遗留事项

-   查看所有的 history
-   视频没有预览图
-   ping 很多网站不通 本来就这样把
