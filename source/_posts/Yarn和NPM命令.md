---
title: 'Yarn 和 NPM 命令'
tags:
  - NPM
  - Yarn
categories: 前端工具
description: Yarn 和 NPM 命令
date: 2018-07-01 15:34:24
---

Yarn 和 NPM 命令

<!-- more -->
<!-- markdownlint-disable MD041 MD002-->

## Yarn和 NPM命令

- NPM是随同 NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题
- Yarn 是一个新的包管理器，用于替代现有的 npm 客户端或者其他兼容 npm 仓库的包管理工具。Yarn 保留了现有工作流的特性，优点是更快、更安全、更可靠。

| 命令 | NPM | Yarn |
|---|---|---|
| 安装 | 新版的NodeJS 已经集成了NPM | `npm install -g yarn` |
| 升级 | `npm install npm -g` | 见[Yarn升级](#Yarn升级) |
| 安装所有的依赖包   | `npm install` | `yarn` |
| 安装某个依赖 | `npm install [package] --save/-S` | `yarn add [package]` |
| 安装某个开发时依赖项目 | `npm install [package] --save-dev/-D` | `yarn add [package] --dev/-D` |
| 安装某个全局依赖项目 | `npm install [package] -g` | `yarn global add [package]` |
| 更新某个依赖 | `npm update [package]` | `yarn upgrade [package]` |
| 更新某个依赖到某个版本 | `npm update [package@version]` | `yarn upgrade [package@version]` |
| 更新某个依赖到最新版本 | `npm update [package@latest]` | `yarn upgrade [package] --latest` |
| 删除某个依赖 | `npm uninstall [package]` | `yarn remove [package]` |
| 运行脚本 | `npm run` | `yarn run` |

- 可以通过输入`npm -v`来测试是否成功安装

```bash
npm -v
6.2.0
```

### NPM 常用命令

- NPM提供了很多命令，使用`npm help`可查看所有命令。
- 使用`npm help <command>`可查看某条命令的详细帮助，例如`npm help install`

### 使用淘宝 NPM 镜像

[NPM和 Yarn添加淘宝镜像](/2018/07/01/前端开发环境配置/#NPM和Yarn添加淘宝镜像)

### Yarn升级

#### 通过msi安装升级 Yarn

[下载最新的yarn更新包](https://yarnpkg.com/lang/zh-hans/docs/install/#windows-stable)

- 通过 msi 安装的 yarn 并不会覆盖通过 npm 安装的 yarn，两者同时存在。卸载的话，也是分开卸载的。

#### NPM加版本号安装

虽然，我们不能通过`npm install yarn -g`的方法，获得最新的yarn。但是，我们已经知道了yarn的最新版的版本号，所以，我们可以直接指定版本号进行安装。

```bash
npm install yarn@1.9.2 -g
```

这样的话，就可以不使用msi，而还是采用更高大上的命令行模式安装最新版的yarn了。
我们可以用`npm view yarn version`，查看NPM上的最新版本。

#### NPM 加 latest 安装

```bash
npm install yarn@latest -g
```

### 更新 package.json 的所有依赖

- 安装 `npm-check-updates`

```bash
yarn global add npm-check-updates
# OR
npm i npm-check-updates -g
```

- 手动执行 `ncu` （或 `npm-check-updates`）检查更新

```bash
ncu
```

- 更新 `dependencies` 到新版本

```bash
ncu -u
```

- 更新全部 `dependencies` 到最新版本(包括当前指定版本范围满足最新版本号的,比如^4.2.0 -> ^4.3.0)：

```bash
ncu -a
```
