---
title: webpack官网学习
date: 2022-12-05 09:27:23
tags: webpack
---

## webpack学习与理解

[webpack官网](https://webpack.docschina.org/concepts/)

### 概念（what）
webpack 是一个用于现代 JavaScript 应用程序的 静态模块打包工具。当 webpack 处理应用程序时，它会在内部从一个或多个入口点构建一个 依赖图(dependency graph)，然后将你项目中所需的每一个模块组合成一个或多个 bundles，它们均为静态资源，用于展示你的内容。

### 为啥要用webpack（why）
webpack的作用就是将在node中写好的代码（后端）解析为在浏览器下（前端）可以运行的代码
打包的意思就是，不用再像浏览器那样，先引入a.js再引入b.js，完事儿a.js还要暴露为全局变量。而是直接将两个js打包成为一个bundle.js，然后在网页中直接引入bundle.js就行了。
比如说我们编写一些比较高级的语法，但是部分浏览器是不支持的，此时我们就需要设置一些pollfill去解决该问题。在例如我们编写的TS和Vue文件，这些浏览器都不能识别，而webpack就会使用对应的babel对其进行转化，转化为浏览器可以识别的文件。

### 怎么用（how）
#### 核心概念
1. 入口 entry
2. 输出 output
3. loader
4. 插件 plugin
5. 模式 mode
