---
title: 'src目录结构分析'
tags: Vue
categories: Vue
description: src目录结构分析
date: 2018-07-08 23:15:22
---

src目录结构及命名原则分析

<!-- more -->
<!-- markdownlint-disable MD002 MD041-->

## src目录结构分析

```bash
.
├── adapter -----> 适配器,处理拿到的数据,整理成对象或者别的能直接使用的数据
|   ├── report.js -----> (驼峰命名：与api命名一致)
|   └── selectPhoneNumber.js -----> (驼峰命名: 与api命名一致)
|
├── api -----> 通过后台接口拿到数据
|   ├── report.js -----> (驼峰命名)
|   └── selectPhoneNumber.js -----> (驼峰命名)
|
├── assets
|
├── components -----> 组件,通过在 views 中的 vue 文件的调用并传入数据,然后渲染在页面中,复用性强
|   ├── AreaCard.vue ----->  (Pascal命名)
|   ├── Chart.vue
|   └── Entry.vue
|
├── router -----> 路由
|   └── index.js
|
├── store -----> vuex
|   ├── modules -----> vuex模块
|   |   ├── clothesSize.js -----> (驼峰命名)
|   |   ├── login.js
|   |   └── user.js
|   └── index.js
|
├── util -----> 自己封装的js
|   ├── fetch.js
|   └── index.js
|
└── views -----> 页面展示
    ├── Report.vue -----> (Pascal命名: 与api命名一致)
    ├── Dormitory
    |   ├── DormitoryStudent.vue -----> (Pascal命名)
    |   ├── Edit.vue
    |   └── index.vue
    └── Home.vue
```

api -----> adapter -----> views -----> 同一个页面：这三个文件夹的命名对应一致 或者 api、adapter 与 views 中的文件夹命名一致
