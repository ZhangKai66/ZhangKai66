---
title: 前端面试题VUE篇
date: 2021-12-19 23:15:04
tags: Vue, 面试题
---

## Vue相关

### Vue 的最大的优势是什么

1. 数据可以双向绑定  mvvm
2. 组件化开发
3. 单页面应用，减少后台请求
4. 第三方库比较好用

### mvvm 和 mvc 区别是什么

- mvvm(model view viewmodel) 

viewModel是核心 
可以将model->view  by数据绑定 ；
可以将view->model  bydom事件监听  即 双向绑定

- mvc(model view controller)
c->vm vm是从c中抽离业务逻辑 相当于是在c基础上封装了一层

### Vue 数据双向绑定的原理是什么

数据劫持结合发布者-订阅者模式
Object.defineProperty()
待补充

### Object.defineProperty 和 Proxy 的区别

- proxy



- Object.defineProperty

### Vue的生命周期  （现在有11个）

1. beforeCreate
2. created
3. beforeMount
4. mounted
5. beforeUpdate
6. updated
7. activated        keep-alive激活时调用
8. deactivated      keep-alive停用时调用
9. beforeDestory
10. destoryed
11. errorCaptured   捕获到子孙组件的错误时调用

了解各部分是干啥的

### 封装过 Vue 的组件吗

☆

### 组件之间是如何传值的


### data为啥必须是函数



### 怎么监听路由参数的变化

- watch $route(to, from)
- beforeRouteUpdate(to, from, next)

### 如何显示组件缓存



### 常用的修饰符

- prevent
- stop
- self
- capture

### Vue踩过的坑

- 给data中的对象直接添加属性并赋值 无效果
解决方法：Vue.set(object, attribute, value)新增的属性也是响应式的
- 在created中操作dom报错，无法获取到dom
解决方法：Vue.nextTick()

### Vue 渲染模板时怎么保留模板中的 HTML 注释呢

```js
<template comments> ... </template>
// 将组件中comments选项设置为true即可
```

### Vue的template编译的理解

简单说： 先转化成AST树，再得到render函数，并返回VNode

### Vue的两种路由模式 hash和history

1. hash模式，就是连接中有#的，#后面的被称为hash，hash虽然在url中，但是不包括在HTTP请求中，hash变化也不会重新加载页面
2. history模式，采用h5新特性，切提供了两个新方法：pushState和replaceState可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更

### $route 和$router 的区别是什么

- $route 是路由信息对象，包括一些路由信息参数如。path, params, hash, query, fullPath, matched, name
- $router 是VueRouter的实例，相当于一个全局的路由器对象，包含很多属性和子对象，如history对象，this.$router.push（）返回上一个history也是使用$router.go(-1)

### Vue-Router的动态路由，怎么获取传过来的动态参数 ☆

场景：User 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染
动态路径参数，使用“冒号”开头，一个路径参数，使用冒号标记，当匹配
到一个路由时，参数会被设置到 this.$router.params 中，并且可以在每个组件
中使用。

### Vue-Router 的钩子函数都有哪些

主要分为三类
1. 全局钩子beforeEach(to,from,next:function)
2. 路由独享钩子 beforeEnter
3. 组件内钩子
   - beforeRouterEnter
   - beforeRouterUpdate
   - beforeRouterLeave

### 路由传值的方式有哪几种

155page

### Vue 中操作 data 中数组的方法中哪些可以触发视图更新，哪些不可以，不可以的话有什么解决办法

> 可以触发更新

- push()
- pop()
- unshift()
- shift()
- splice()
- sort()
- reverse()

- filter()
- concat()
- slice()

> 不可以触发更新

- 直接利用索引设置新数组值  this.arr[index] = newV;
- 直接修改数组的长度  this.arr.length = newL;

> 解决第一种

this.$set(this.aarr,index,newValue)
this.arr.splice(index,1,newValue)

>解决第二种

this.arr.splce(newLength)

### 怎么重置Vue中的data

Object.assign(target, source) 方法用于将所有可枚举属性的值从一个或多个源对
象复制到目标对象。它将返回目标对象。
```js
Object.assign(this.$data, this.$options.data(this))
```

### Vue首屏加载优化

- 不常改变的库放到index.html中，用cdn引入
- 路由懒加载
- 不生成map文件
- vue组件尽量不全局引入
- 开启gzip压缩
- 首页单独做服务端渲染

### Vue的nextTick原理
如果需要必须对数据更改并刷新后的DOM做处理时

>在Vue生命周期的created()钩子函数进行的DOM操作一定要放在Vue.nextTick()的回调函数中

原理：利用异步队列

在每个 macro-task 运行完以后，UI 都会重渲染，那么在 micro-task （异步事件回调） 中就完成数据更新，当前 次事件循环 结束就可以得到最新的 UI 了。反之如果新建一个 macro-task 来做数据更新，那么渲染就会进行两次。

总结

1. vue用异步队列的方式来控制DOM更新和nextTick回调先后执行
2. micro-task因为其高优先级特性，能确保队列中的微任务在一次事件循环前被执行完毕
3. 因为兼容性问题，vue不得不做了microtask向macrotask的降级方案

### 待丰富

Vue-Router的动态路由，怎么获取传过来的动态参数

路由传值的方式有哪几种

Vue-Router 的钩子函数都有哪些