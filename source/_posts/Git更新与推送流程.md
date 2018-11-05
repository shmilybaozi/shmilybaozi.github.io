---
title: Git更新与推送流程
tags: Git
categories: Git
description: Git更新与推送流程
date: 2018-07-29 15:48:01
---

Git更新与推送流程

<!-- more -->
<!-- markdownlint-disable MD041 MD002-->

## Git更新与推送流程

- 设置主仓库地址

```bash
git remote add upstream https://repo-address
```

- 获取主仓库最新提交

```bash
git pull upstream
```

- 将主仓库的 `master` 分支合并到本地的 `master` 分支

```bash
git merge upstream/master master
```

- 解决冲突并提交合并

```bash
git commit -a -m 'merge upstream master'
```

- 推送至远程仓库

```bash
git push
```

- 提交Pull Request

## 自定义Git

### 让Git显示颜色

```bash
git config --global color.ui true
```

### 配置别名

- `git st`表示`git status`
- `git co`表示`git checkout`
- `git ci`表示`git commit`
- `git br`表示`git branch`

```bash
git config --global alias.st status
git config --global alias.co checkout
git config --global alias.ci commit
git config --global alias.br branch
```

- 命令`git reset HEAD file`可以把暂存区的修改撤销掉（unstage），重新放回工作区。既然是一个unstage操作，就可以配置一个unstage别名：

```bash
git config --global alias.unstage 'reset HEAD'
```

- 配置一个`git last`，让其显示最后一次提交信息：

```bash
git config --global alias.last 'log -1'
```

- 还有人丧心病狂地把`lg`配置成了：

```bash
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

### 配置文件

配置Git的时候，加上`--global`是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。
配置文件放哪了?每个仓库的Git配置文件都放在`.git/config`文件中
而当前用户的Git配置文件放在用户主目录下的一个隐藏文件`.gitconfig`中
别名就在`[alias]`后面，要删除别名，直接把对应的行删掉即可。
配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置。

```bash
[alias]
  st = status
  co = checkout
  ci = commit
  br = branch
  unstage = reset HEAD
  last = log -1
  lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
  up = upstream
  og = origin
  ms = master
  mg = merge
  plp = pull upstream
  mgums = merge upstream/master master
  rap = remote add upstream
  rag = remote add origin
  psg = push -u origin
```