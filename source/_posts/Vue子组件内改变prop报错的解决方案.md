---
title: Vue子组件内改变prop报错的解决方案
date: 2019-10-15 11:19:15
tags: 报错
categories: Vue
description: Vue子组件内改变prop报错的解决方案
---

Vue子组件内改变prop报错的解决方案

<!-- more -->
<!-- markdownlint-disable MD041 MD002-->

## Vue子组件内改变prop报错的解决方案

[单项数据流](https://cn.vuejs.org/v2/guide/components-props.html#%E5%8D%95%E5%90%91%E6%95%B0%E6%8D%AE%E6%B5%81)

> 所有的 prop 都使得其父子 prop 之间形成了一个**单向下行绑定**：父级 prop 的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外改变父级组件的状态，从而导致你的应用的数据流向难以理解。
>
> 额外的，每次父级组件发生更新时，子组件中所有的 prop 都将会刷新为最新的值。这意味着你**不应该**在一个子组件内部改变 prop。如果你这样做了，Vue 会在浏览器的控制台中发出警告。

Vue警告

```bash
[Vue warn]: Avoid mutating a prop directly since the value will be overwritten whenever the parent component re-renders. Instead, use a data or computed property based on the prop's value. Prop being mutated: "isLoading"
```

### 解决方案

#### 这个 prop 用来传递一个初始值

这个子组件接下来希望将其作为一个本地的 prop 数据来使用。在这种情况下，最好定义一个本地的 data 属性并将这个 prop 用作其初始值：

```js
props: ['initialCounter'],

data: function () {
  return {
    counter: this.initialCounter // 子组件操作 counter
  }
}
```

#### 这个 prop 以一种原始的值传入且需要进行转换

在这种情况下，最好使用这个 prop 的值来定义一个计算属性：

```js
props: ['size'],

computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

#### 利用 `.sync` 修饰符

`.sync` 相当于对一个 prop 进行“双向绑定”

父组件引用代码：

```html
<comp :foo.sync="bar"></comp>
```

会被拓展成：

```html
<comp :foo="bar" @update:foo="val => bar = val"></comp>
<!-- 即 -->
<comp :foo="bar" @update:foo="bar = $event"></comp>
```

当子组件需要更新 foo 的值时，它只需要显式地触发一个更新事件：

```js
this.$emit('update:foo', newValue)
```

注意带有 `.sync` 修饰符的 `v-bind` 不能和表达式一起使用 (例如 `:title.sync=”doc.title + ‘!’”` 是无效的)。取而代之的是，你只能提供你想要绑定的**属性名(即，变量名称)**，类似 `v-model`。

当我们用一个对象同时设置多个 prop 的时候，也可以将这个 `.sync` 修饰符和 `v-bind` 配合使用：

```html
<comp v-bind.sync="doc"></comp>
```

这样会把 `doc` 对象中的每一个属性 (如 `title`) 都作为一个独立的 prop 传进去，然后各自添加用于更新的 `v-on` 监听器。

#### 数组或对象的 prop，传递的是引用

当 prop 是对象的时候，传给子组件的是该对象的引用，类似于指针的一个东西，指向内存中的一个数据空间。

而子组件通过 `objectName.prop` 去操作该 `object` 的时候，更改了内存空间的数据，但是并没有更改内存空间的位置，指针仍然是指向这块内存空间的，所以不会有警告报错。
