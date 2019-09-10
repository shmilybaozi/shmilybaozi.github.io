---
title: path.join 和 path.resolve区别
tags: JavaScript
categories: JavaScript
description: path.join 和 path.resolve区别 
date: 2018-08-06 10:11:44
---

path.join & path.resolve

<!-- more -->
<!-- markdownlint-disable MD041 MD002-->

## path.join([path1][, path2][, ...])

`path.join()`: 连接任意多个路径字符串。

注：如果连接后的路径字符串是一个长度为零的字符串, 则返回 `'.'`, 表示当前工作目录。

```js
path.join('foo', 'bar', 'baz')
// 返回
'/foo/bar/baz'

// 不合法的字符串将抛出异常
path.join('foo', {}, 'bar')
// 抛出的异常
TypeError: Arguments to path.join must be strings'
```

## path.resolve([from...], to)

`path.resolve()`: 将多个路径解析为一个规范化的绝对路径。

其处理方式类似于对这些路径逐一进行 `cd` 操作, 与 `cd` 操作不同的是, 这引起路径可以是文件, 并且可不必实际存在（`resolve()`方法不会利用底层的文件系统判断路径是否存在, 而只是进行路径字符串操作。）

`path.resolve('foo/bar', '/tmp/file/', '..', 'a/../subFile')` 相当于:

```bash
cd foo/bar
cd /tmp/file/
cd ..
cd a/../subFile
pwd
# Linux pwd命令用于显示工作目录。执行 pwd 指令可立刻得知您目前所在的工作目录的绝对路径名称。
```

```js
path.resolve('/foo/bar', './baz')
// 输出结果为
'/foo/bar/baz'
path.resolve('/foo/bar', '/tmp/file/')
// 输出结果为
'/tmp/file'

path.resolve('wwwroot', 'static_files/png/', '../gif/image.gif')
// 当前的工作路径是 /home/myself/node, 则输出结果为
'/home/myself/node/wwwroot/static_files/gif/image.gif'
```

## 两者的区别

- `join` 是把各个 path 片段连接在一起， `resolve` 把`'/'` 当成根目录

```js
path.join('/a', '/b') // '/a/b'
path.resolve('/a', '/b') // '/b'
```

- `join` 直接拼接字段，`resolve` 解析路径并返回绝对路径

```js
path.join('a', 'b1', '..', 'b2')
// 'a/b2'

path.resolve('a', 'b1', '..', 'b2')
// '/home/myself/node/a/b2'
```