---
title: "Linux 备忘"
description: "个人的 Linux 备忘，不定期更新，有需要的朋友可以稍微看一下。"
date: "2019-03-21"
slug: "16"
categories:
    - 技术
tags:
    - Linux
keywords:
    - linux
---

# 👦 用户类
新建用户：

```
sudo useradd -m -s /bin/bash $username
```

移入 `sudo` 分组：

```
sudo usermod -a -G sudo $username
```

为用户设置密码：

```
sudo passwd $username
```

# 📦 包管理类
`apt` 更新源：

```
sudo apt-get install update
```

`apt` 更新软件包：

```
sudo apt-get install upgrade
```

# 🥤 软件安装
`Oh My ZSH` 安装：

```
sudo apt-get install zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
vim ~/.zshrc
# 修改 ZSH_THEME 即主题，我用的是
# 记得修改字体为 powerline 系列
```

`git` 初始配置：

```
# 配置用户名邮箱
sudo git config --global user.name "$username"
sudo git config --global user.email "$email"

# 默认编辑器
sudo git config --global core.editor vim

# 自动储存用户名和密码
sudo git config --global credential.helper store
```

`Node.js` 安装，可以参考 [Install Node.js via package manager](https://nodejs.org/en/download/package-manager/)：

```
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs

# 更改 ~/.confg 拥有者，否则试用 npm 可能会提示 Permisson Denied
sudo chown -R $user ~/.npm
sudo chown -R $user ~/.config
```

`MySQL(Debian/Ubuntu)` 安装：[Debian9安装MySQL](https://www.jianshu.com/p/40b770d86a7b)


# ☕ 杂项
更改主机名：

```
sudo vim /etc/hosts
sudo vim /etc/hostname

# 然后修改所有原有主机名为你的主机名

sudo hostname $hostname
```

`systemctl` 目录：

```
/lib/systemd/system
```

