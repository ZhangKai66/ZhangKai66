---
title: 前端面试题HTML&CSS
date: 2021-12-19 23:22:30
tags: HTML&CSS
categories:
- 面试题
---

### 介绍下 BFC 及其应用
BFC(Block Formarting Context) 块级格式化上下文 TODO:
### property和attribute的区别

property是dom元素对象的一个属性，是js里面的对象 值可以是对象、数组在内的任何类型
attribute是html标签上的属性，值只能是字符串

建议使用property，因为attribute的每次修改都会触发页面重绘

### 怎么让一个 div 水平垂直居中

1. margin+定位
``` css
position:absolute;
top:50%;
left:50%;
margin-left: -50%*width;
margin-top: -50%*height;
```
2. 定位+transform
``` css
position:absolute;
top:50%;
left:50%;
transform:translate(-50%,-50%);
```
3. margin:auto
``` css
position: absolute;
left: 0;
top: 0;
right: 0;
bottom: 0;
margin: auto;
```
4. flex
``` css
display: flex;
justify-content: center;
align-items: center;
```
5. grid
``` css
/* father */
display:grid;
/* child */
justify-self: center;
align-self: center;
```

### 