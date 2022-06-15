---
title: 前端面试题VUE篇
date: 2021-12-19 23:15:04
tags: Vue, 面试
categories:
- 面试题
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
数据劫持 Observer
Dep 收集订阅者

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

### 组件之间是如何传值的

父子组件: props $emit
祖孙组件: provide inject
兄弟组件: eventbus
其他方式: vuex

### data为啥必须是函数

确保vue的各个实例之间的数据不会相互影响 -- TODO:可优化说辞

1.一个组件被复用多次的话，也就会创建多个实例。本质上，这些实例用的都是同一个构造函数。 2.如果data是对象的话，对象属于引用类型，会影响到所有的实例。所以为了保证组件不同的实例之间data不冲突，data必须是一个函数。

### data compute watch之间有什么依赖关系

### 为什么v-for和v-if不建议用在一起

### 插槽相关

### 怎么监听路由参数的变化

- watch $route(to, from)
- beforeRouteUpdate(to, from, next)

### 如何显示组件缓存

keep-alive -- TODO: 详细补充下

### 常用的修饰符

- prevent
- stop
- self
- capture


### 封装过 Vue 的组件吗

☆

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
详细版:


### Vue的两种路由模式 hash和history

1. hash模式，就是连接中有#的，#后面的被称为hash，hash虽然在url中，但是不包括在HTTP请求中，hash变化也不会重新加载页面
2. history模式，采用h5新特性，切提供了两个新方法：pushState和replaceState可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更

### $route 和$router 的区别是什么

- $route 是路由信息对象，包括一些路由信息参数如。path, params, hash, query, fullPath, matched, name
- $router 是VueRouter的实例，相当于一个全局的路由器对象，包含很多属性和子对象，如history对象，this.$router.push（）返回上一个history也是使用$router.go(-1)

### vue router官网记录
router文件中必须要有routes，路由数组：存放多个不同路由(放的是路由信息)，对应上面的$route
```js
const routes = [
   {
      path: "/about",
      component: AboutComponent
   }
]
```
router实例是VueRouter这个插件的实例，routes路由信息是传给router实例的配置参数
```js
const router = new VueRouter({
   routes: routes
})
```
创建并挂在根实例，记得要通过router配置参数注入路由
```js
const app = new Vue({
   router
}).$mount('#app')
```
通过注入路由器，我们可以在任何组件内通过 this.$router 访问路由器，也可以通过 this.$route 访问当前路由

routes的meta字段是路由元信息 
一个路由匹配到的所有路由记录会暴露为 $route 对象 (还有在导航守卫中的路由对象) 的 $route.matched 数组
```js
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {} 
}
```
`https://v3.router.vuejs.org/zh/guide/advanced/data-fetching.html#%E5%AF%BC%E8%88%AA%E5%AE%8C%E6%88%90%E5%90%8E%E8%8E%B7%E5%8F%96%E6%95%B0%E6%8D%AE`
### Vue-Router的动态路由，怎么获取传过来的动态参数 ☆

场景：User 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染
动态路径参数，使用“冒号”开头，一个路径参数，使用冒号标记，当匹配
到一个路由时，参数会被设置到 this.$router.params 中，并且可以在每个组件
中使用。
like this: `{ path: '/user/:id', component: User }`
在匹配到该路由之后，动态参数会被设置到this.$route.params
`template: '<div>User {{ $route.params.id }}</div>'`
也可以是多个参数
like this: `/user/:username/post/:post_id` 与路由一一对应
### Vue-Router 的钩子函数都有哪些
即 路由守卫 记住，在任务导航守卫中都要触发next函数
主要分为三类
1. 全局钩子beforeEach(to,from,next:function) beforeResolve afterEach
2. 路由独享钩子 beforeEnter  // 在routes数组中给某个特定的route单独设置路由狗子，参数一致
3. 组件内钩子
   - beforeRouterEnter // 不！能！获取组件实例 `this`
   - beforeRouterUpdate // 在当前路由改变，但是该组件被复用时调用 对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候
   - beforeRouterLeave // 导航离开该组件的对应路由时调用 可以访问组件实例 `this` （禁止用户在还未保存修改前突然离开）
   - 
用处：一般用的是beforeEach，可以判断用户身份，未登录则导航到Login页面
### 路由传值的方式有哪几种

params
query

### vue组件的定时器怎么销毁

- 如果页面上有很多个定时器，在data选项中添加一个timer对象，给每个定时器去一个名字一一映射在对象timer中，然后在beforeDestory函数中for循环clearInterval掉
- 如果只有一个定时器，可以用`this.$once('hook:beforeDestory',()=>{ clearInterval(timer) })`

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

### mixin用过吗

### 虚拟dom的介绍及其优点

### vue底层是怎么实现对数组的监听的

### 待丰富

Vue-Router的动态路由，怎么获取传过来的动态参数

路由传值的方式有哪几种
