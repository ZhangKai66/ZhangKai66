---
title: 后台管理系统中统一管理svg的实现
date: 2022-12-13 22:08:06
tags: svg
---

在我们项目中经常遇到需要展示自定义svg图标的情况，因此我们需要开发一个自定义组件处理**自定义svg图标的形式**。
首先，这个组件需要有以下两种能力
1. 显示外部svg图标
2. 显示项目内的svg图标
### 组件代码
`SvgIcon.vue`
```vue
<template>
  <div
    v-if="isExternal"
    :style="styleExternalIcon"
    class="svg-external-icon svg-icon"
    :class="className"
  />
  <svg v-else class="svg-icon" :class="className" aria-hidden="true">
    <use :xlink:href="iconName" />
  </svg>
</template>

<script setup>
import { isExternal as external } from '@/utils/validate'
import { defineProps, computed } from 'vue'
const props = defineProps({
  // icon 图标
  icon: {
    type: String,
    required: true
  },
  // 图标类名
  className: {
    type: String,
    default: ''
  }
})

/**
 * 判断是否为外部图标
 */
const isExternal = computed(() => external(props.icon))
/**
 * 外部图标样式
 */
const styleExternalIcon = computed(() => ({
  mask: `url(${props.icon}) no-repeat 50% 50%`,
  '-webkit-mask': `url(${props.icon}) no-repeat 50% 50%`
}))
/**
 * 项目内图标
 */
const iconName = computed(() => `#icon-${props.icon}`)
</script>

<style scoped>
.svg-icon {
  width: 1em;
  height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}

.svg-external-icon {
  background-color: currentColor;
  mask-size: cover !important;
  display: inline-block;
}
</style>
```
在父组件中使用方式如下
```vue
<svg-icon icon="https://xxx/user.svg"></svg-icon>
```
这样我们就可以显示**外部图标**了

### 处理内部svg图标的展示
1. 首先导入所有内部图标到icons文件夹下
2. 在icons下创建index.js文件，该文件需完成以下2点
   1. 导入所有svg图标
   2. 完成SvgIcon的全局注册

代码如下，可参考[webpack文档](https://webpack.docschina.org/guides/dependency-management/#requirecontext)
```js
import SvgIcon from '@/components/SvgIcon'

// 通过 require.context() 函数来创建自己的 context
const svgRequire = require.context('./svg', false, /\.svg$/)
// 此时返回一个 require 的函数，可以接受一个 request 的参数，用于 require 的导入。
// 该函数提供了三个属性，可以通过 require.keys() 获取到所有的 svg 图标
// 遍历图标，把图标作为 request 传入到 require 导入函数中，完成本地 svg 图标的导入
svgRequire.keys().forEach(svgIcon => svgRequire(svgIcon))

export default app => {
  app.component('svg-icon', SvgIcon)
}
```
最后，在父组件中使用SvgIcon引入本地svg
```vue
// 用户名   
<svg-icon icon="user" />
// 密码
<svg-icon icon="password" />
// 眼睛
<svg-icon icon="eye" />
```
此时，图标依然无法显示，因为缺少webpack中专门处理svg图标的一个loader
### svg-sprite-loader
[svg-sprite-loader](https://www.npmjs.com/package/svg-sprite-loader) 是 webpack 中专门用来处理 svg 图标的一个 loader,我们需要在项目中加上它
1. 下载该loader,执行`npm i --save-dev svg-sprite-loader`
2. 在vue.config.js文件中（没有则新建一个），新增如下配置
（[webpack文档](https://cli.vuejs.org/zh/guide/webpack.html#%E7%AE%80%E5%8D%95%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%B9%E5%BC%8F)）
```js
const path = require('path')
function resolve(dir) {
  return path.join(__dirname, dir)
}
module.exports = {
  chainWebpack(config) {
    // 设置 svg-sprite-loader
    config.module
      .rule('svg')
      .exclude.add(resolve('src/icons'))
      .end()
    config.module
      .rule('icons')
      .test(/\.svg$/)
      .include.add(resolve('src/icons'))
      .end()
      .use('svg-sprite-loader')
      .loader('svg-sprite-loader')
      .options({
        symbolId: 'icon-[name]'
      })
      .end()
  }
}
```
处理完以上配置之后，重新启动项目，即可展示图标