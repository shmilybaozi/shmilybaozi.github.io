---
title: hexo&github创建个人博客
date: 2018-11-07 14:27:37
tags: 
- Hexo
- Github
categories: 前端工具
description: hexo&github创建个人博客 
---

利用 hexo 和 github 搭载个人博客

<!-- more -->
<!-- markdownlint-disable MD041 MD002-->

## 准备工作

- 安装了 node.js, npm, 并会使用
- 了解git

## 配置 SSH key

- 用 git 或者 cmder 等命令行工具执行：

  ```bash
  cd ~/. ssh # 检查本机已存在的ssh密钥
  ```

  如果显示 `No such file or directory` 说明还未设置过 SSH key

- 执行：

  ```bash
  ssh-keygen -t rsa -C "邮件地址"
  ```

  然后持续回车, 最后会生成一个文件在 `.ssh\id_rsa.pub` 中, 用记事本打开该文件, 复制里面的内容。

- 打开你的 github 主页, 进入个人设置 -> SSH and GPG keys -> New SSH key
- 将刚复制的内容粘贴到 key 那里, title 取个自己喜欢的, 然后保存。
- 测试是否成功

  ```bash
  ssh -T git@github.com # 注意邮箱地址不用改
  ```

  如果提示 `Are you sure you want to continue connecting (yes/no)?` , 输入yes, 然后就会提示 `...You’ve successfully authenticated, but GitHub does not provide shell access.`, 就是成功了。

- 配置 git 用户名和邮箱

  ```bash
  git config --global user.name "username"
  git config --global user.email  "email"
  ```

## github

- 创建 github 账号
- 创建 github 个人项目 `username.github.io`
- 创建分支 hexo
- 设置 hexo 为默认分支
- 使用 `git clone git@github.com:username/username.github.io.git` 拷贝仓库

## Hexo

[hexo官网](http://hexo.io)

[hexo github](https://github.com/hexojs/hexo)

### 安装

```bash
# npm 安装
npm install -g hexo-cli
```

### 初始化

在刚刚 `git clone` 的这个文件夹中, 执行：

```bash
# 此时当前分支应显示为 hexo
npm install hexo
hexo init
npm install
npm install hexo-deployer-git
```

执行以上命令之后, hexo 就会在文件夹中生成博客相关文件

### 将 hexo 部署到 username.github.io

在 `_config.yml` 中修改：

```yml
deploy:
  type: git
  repository: git@github.com:shmilybaozi/shmilybaozi.github.io.git
  branch: master
```

```bash
hexo g # hexo generate 生成静态文件
hexo g -d # 文件生成后立即部署网站
```

### 开始使用

一些 hexo 命令

```bash
hexo new [layout] <title> # 新建一篇文章, 如果标题包含空格的话, 请使用引号括起来。layout 为 draft 即为草稿
hexo publish [layout] <filename> # 发表草稿, 如果标题包含空格的话, 请使用引号括起来。
hexo g # hexo generate 生成静态文件
hexo g -d # 文件生成后立即部署网站
hexo s # hexo server 启动服务器
hexo d # hexo deploy 部署网站
hexo clean # 清除缓存
hexo s --debug # 调试模式, 更改了配置或文章后随时刷新页面来查看效果
```

### 部署文章的步骤

```bash
hexo clean # 清除缓存
hexo g -d # 文件生成后立即部署网站
```

### 将 hexo 中的代码提交到 git 上

```bash
git add .
git commit -m "..."
git push origin hexo
```

这样一来, 在 github 上的 `username.github.io` 仓库就有两个分支, 一个 `hexo` 分支用来存放网站的原始文件, 一个 `master` 分支用来存放生成的静态网页。

访问 `username.github.io` 就能看到个人博客的样子。