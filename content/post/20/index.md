---
title: "Git Fork 仓从主仓拉取更新的方法"
description: "总结起来 Git 的 Fork 仓从主仓拉取代码有两种方法，第一种是配置 upstream remote，从 upstream 拉取更新，第二种是直接使用反向 pull request 完成。"
date: "2019-11-23"
slug: "20"
categories:
    - 技术
tags:
    - Git
    - Tips
keywords:
    - git
    - fork
    - pull
---

# Upstream Remote

这种方法的具体做法是将主仓地址配置到 `Fork` 仓本地的 `upstream` 远端分支中，假设主仓地址为：`https://www.git.com/public/test`，则在本地配置：

```
git remote add upstream https://www.git.com/public/test
```

需要拉取更新代码时，使用：

```
git pull upstream master
```

来指定从 `upstream` 而不是 `origin` 远端分支拉取更新，当拉取完成之后，如果有冲突则解决冲突，没有冲突或者已经解决冲突之后使用：

```
git push origin master
```

将本地提交的代码和从主仓拉取的更新一同 `push` 到自己的 `Fork` 仓中，这样就完成了 `Fork` 仓的代码更新。

# 反向 Pull Request

有一部分 `git` 托管网站支持反向 `pull request`，比如 `github`，这一功能可以十分方便的完成 `Fork` 仓拉取主仓代码更新的操作。

假设主仓地址为 `https://www.git.com/public/test`，`Fork` 仓地址为 `https://www.git.com/xxx/test`，如果对应的 `git` 托管网站支持反向 `pull request`，那么可以直接创建一个从 `public/test` 到 `xxx/test` 的 `pull request`，`Fork` 仓主人同意 `pull request` 即可合入完成更新。

