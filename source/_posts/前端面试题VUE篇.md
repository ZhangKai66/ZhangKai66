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

### 桌面文件 24题  148页