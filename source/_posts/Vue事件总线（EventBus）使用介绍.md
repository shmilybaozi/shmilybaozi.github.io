---
title: Vue事件总线（EventBus）使用介绍
date: 2019-10-18 16:16:18
tags:
- EventBus
- 子组件向父组件通信
- 兄弟组件通信
categories: Vue
description: Vue事件总线（EventBus）使用介绍
---

Vue事件总线（EventBus）使用介绍

<!-- more -->
<!-- markdownlint-disable MD041 MD002-->

## 子组件向父组件通信

可以使用 `v-on` 监听子组件上 `$emit` 的变化

父组件引用代码：

```html
<!-- 监听子组件上 title-changed 变化 -->
<comp :title="title" @title-changed="updateTitle($event)"></comp>
```

```js
updateTitle (title) {
  this.title = title;
}
```

子组件上定义`$emit`：

```js
changeTitle() {
  // 子组件向父组件传值 $emit("传值名字", "内容")
  this.$emit("title-changed", "子组件向父组件传值")
},
```

关于[事件名](https://cn.vuejs.org/v2/guide/components-custom-events.html#%E4%BA%8B%E4%BB%B6%E5%90%8D)的命名方式，官方推荐采用 kebab-case 命名

> 不同于组件和 prop，事件名不会被用作一个 JavaScript 变量名或属性名，所以就没有理由使用 camelCase 或 PascalCase 了。
>
> 推荐**始终使用 kebab-case 的事件名**

## 事件总线（EventBus）

跨多层父子组件通信，可以通过单独的事件中心管理组件间的通信，在组件中，可以使用 `$emit`, `$on`, `$off` 分别来分发、监听、取消监听事件

### 创建事件中心

- 在 `src` 中创建 `eventBus.ts` 文件

```ts
import Vue from 'vue';
export default new Vue();

// 在要用的地方
// 引入 eventBus
import eventBus from '../eventBus';
// 使用
eventBus.XXX
```

- 另外一种方式，在项目中的 `@/index.ts` 或 `main.ts` 初始化 EventBus

```ts
Vue.prototype.$bus = new Vue();

// 在要用的地方
this.$bus.XXX
```

>注意，这种方式初始化的 EventBus 是一个全局的事件总线。

### 发送事件

```ts
// 发送消息
EventBus.$emit(channel: string, callback(payload1,…))
```

A.vue

```js
<template>
    <button @click="sendMsg">点击发送事件</button>
</template>

<script>
import { EventBus } from "../eventBus";
export default {
  methods: {
    sendMsg() {
      EventBus.$emit("a-msg", '来自A页面的消息');
    }
  }
};
</script>
```

### 接收事件

```ts
// 监听接收消息
EventBus.$on(channel: string, callback(payload1,…))
```

B.vue

```js
<template>
  <p>{{msg}}</p>
</template>

<script>
import { EventBus } from "../eventBus";
export default {
  data(){
    return {
      msg: ''
    }
  },
  mounted() {
    EventBus.$on("a-msg", (msg) => {
      // A 发送来的消息
      this.msg = msg;
    });
  }
};
</script>
```

### 移除事件监听者

通常会在 Vue 页面销毁时，同时移除 EventBus 事件监听

B.vue

```js
<script>
export default {
  ...
  beforeDestroy() {
    // 这是一个全局的事件总线，不需要引入
    this.$bus.$off('a-msg');
  }
};
</script>
```

### 使用 vue-happy-bus 移除事件监听者

它主要解决在非父子组件间通信时，解决重复绑定事件、无法自动销毁的而导致回调函数被执行多次的问题

```js
// npm
npm install vue-happy-bus --save

//main.js中引用
//bus状态管理
import BusFactory from 'vue-happy-bus'
//使用全局变量引用
Vue.prototype.$BusFactory=BusFactory;

// 这里注意一点每个使用 Bus 的组件中都要在 data 中注册这个事件 Bus:this.$BusFactory(this) 这样才能调用其中这个 this 指向 vue 原型

// a.vue中引用
...
data: function() {
  Bus: this.$BusFactory(this)
},
methods: {
  //在点击事件上执行这个方法就能传递参数
  click_me: function() {
    this.Bus.$emit("clear_me", "clear_me")
  }
}

// b.vue
...
created(){
  this.Bus.$on("clear_me", (em) => {
    console.log(em)
  }
}
...

```

## 参考资料

- [Vue事件总线（EventBus）使用详细介绍](https://blog.csdn.net/i168wintop/article/details/95107935)
- [Vue EventBus 使用中的重复触发解决方案及存在的Bug](https://juejin.im/post/5b3db678e51d45199060d478)
- [vue-happy-bus](https://github.com/tangdaohai/vue-happy-bus)
