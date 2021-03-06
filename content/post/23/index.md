---
title: "MacOS Homebrew 安装、更新慢解决方法"
description: "分为两部分：Homebrew 安装与 Homebrew 换源"
date: "2020-02-18"
slug: "23"
categories:
    - 技术
tags:
    - Tips
    - MacOS
    - Homebrew
keywords:
    - macos
    - homebrew
    - 安装
    - 更新
---

# 🍖 安装

如果使用 `Homebrew` 官方的安装脚本进行安装，会发现安装十分缓慢，我们可以更换安装脚本中设置的仓库路径来加速安装过程。

首先先将官方的脚本下载下来，命名为 `install`：

```shell
curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install >> install
```

使用 `vim` 或者其他文本编辑器打开 `install` 脚本，修改：

```shell
BREW_REPO
```

一行为：

```shell
BREW_REPO = "https://mirrors.ustc.edu.cn/brew.git".freeze
```

保存后重新使用 `ruby` 运行脚本：

```shell
ruby ./install
```

脚本会飞速安装，然后停顿在 `homebrew-core` 的下载过程，此时使用 `^C` 快捷键强制结束进程，将 `homebrew-core` 手动下载到 `homebrew` 安装目录：

```shell
git clone git://mirrors.ustc.edu.cn/homebrew-core.git/ /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core --depth=1
```

# 🧀 换源

完成上面的步骤之后，使用如下命令完成换源：

```shell
# change brew source
cd $(brew --repo)
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

# change brew-core source
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```

完成换源之后，再执行更新指令一次：

```shell
brew update
```

看看是不是比以前快了许多呢？
