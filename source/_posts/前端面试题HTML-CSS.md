---
title: 前端面试题HTML&CSS
date: 2021-12-19 23:22:30
tags: HTML&CSS
categories:
- 面试题
---

1. property和attribute的区别

property是dom元素对象的一个属性，是js里面的对象 值可以是对象、数组在内的任何类型
attribute是html标签上的属性，值只能是字符串

建议使用property，因为attribute的每次修改都会触发页面重绘

2. 正则表达式
`var reg = /^(A|D|W|S){1}[0-9]{1,2}$/;`


3. padStart、padEnd、parseInt的第二参数
padStart的第一参数n 是指将目标字符串用第二参数c扩充到n位（是一共n位 不是n个c）
parseInt第二参数是 将第一个参数以几进制转换2-36