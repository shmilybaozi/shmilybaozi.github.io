---
title: hexo文章模板设置
tags: Hexo
categories: 前端工具
description: hexo文章模板设置
date: 2018-11-05 22:57:26
---

hexo文章模板设置

<!-- more -->
<!-- markdownlint-disable MD041 MD002-->

## hexo文章模板设置

hexo 项目文件夹中的 scaffold 文件夹中的 `post.md` 和 `draft.md`

对应就是 `hexo new [layout] <title>` 中的 `layout` , 默认为 `post`, 草稿为 `draft`, 如果标题包含空格的话，请使用引号括起来。

```bash
---
title: {{ title }}
tags:
categories:
description:
date: {{ date }}
---

点击阅读前文前, 首页能看到的文章的简短描述

<!-- more -->
<!-- markdownlint-disable MD041 MD002--> # 防止 markdownlint 报错
```

## hexo 草稿

Hexo 的一种特殊布局：`draft`, 这种布局在建立时会被保存到 `source/_drafts` 文件夹, 您可通过 `publish` 命令将草稿移动到 `source/_posts` 文件夹。

该命令的使用方式与 `new` 十分类似, 您也可在命令中指定 `layout` 来指定布局。

```js
hexo publish [layout] <title> # 发表草稿
```

草稿默认不会显示在页面中, 您可在执行时加上 `--draft` 参数, 或是把 `render_drafts` 参数设为 `true` 来预览草稿。

## hexo 分类和标签

**分类**具有顺序性和层次性。Hexo 不支持指定多个同级分类。

**标签**没有顺序和层次。

```yml
categories:
- Diary
- Life
tags:
- PS3
- Games
```

以上会使分类 `Life` 成为 `Diary` 的子分类, 而不是并列分类。

因此，有必要为您的文章选择**尽可能准确的一个分类**