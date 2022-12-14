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

### defer和async的区别

1. 默认模式下，当浏览器遇到 script 标签时，文档的解析将停止，并立即下载并执行脚本，脚本执行完毕后将继续解析文档
2. async模式，当浏览器遇到 script 标签时，文档的解析不会停止，其他线程将下载脚本，脚本下载完成后开始执行脚本，脚本执行的过程中文档将停止解析，直到脚本执行完毕。（**可能阻断html解析**）
3. defer模式，当浏览器遇到 script 标签时，文档的解析不会停止，其他线程将下载脚本，待到文档解析完成，脚本才会执行。（有顺序）脚本依赖于dom文档中元素时（**完全不会阻断** ）

### 从浏览器地址栏输入 url 到请求返回发生了什么
TODO:  补充
1. 输入 URL 后解析出协议、主机、端口、路径等信息，并构造一个 HTTP 请求
2. DNS 域名解析
3. TCP 连接
4. http 请求
5. 服务器处理请求并返回 HTTP 报文
6. 浏览器渲染页面
7. 断开 TCP 连接

### 为什么需要三次握手
这是由 TCP 的自身特点可靠传输决定的。客户端和服务端要进行可靠传输，那么就需要确认双方的接收和发送能力。第一次握手可以确认**客户端**的**发送**能力，第二次握手，确认了**服务端**的**发送**能力和**接收**能力，所以第三次握手才可以确认**客户端**的**接收**能力。不然容易出现丢包的现象。

### 盒模型
区别在于盒子内容宽/高度不同
- 标准盒模型  ：  content
- IE盒模型   ：  content + padding + border
切换方式：box-sizing
content-box: 标准盒
border-box:  IE盒

### 重排 reflow 和 重绘 repaint
重排： 几何信息变化（位置和大小）
重绘： 将渲染树的每个节点都转化为屏幕上的实际像素
> 如何减小重排和重绘
> 最小化重绘和重排，用类名class改样式
> 批量操作dom 减少offsetWidth等计算属性的使用，可用临时变量保存
> 开启GPU加速 tranform或will-change属性，我们使用 translate 会比使用绝对定位改变其 left 、top 等来的高效，因为它不会触发重排或重绘，transform 使浏览器为元素创建⼀个 GPU 图层，这使得动画元素在一个独立的层中进行渲染。当元素的内容没有发生改变，就没有必要进行重绘。

### 主页置灰效果
```style 
html {
    -webkit-filter: grayscale(100%);
    /* Chrome, Safari, Opera */
    filter: grayscale(100%);
  }
```