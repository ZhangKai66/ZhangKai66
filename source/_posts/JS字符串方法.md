---
title: JS字符串方法
date: 2022-02-23 22:24:34
tags: js
---

## js基础 --- js字符串方法
1. round floor ceil parseInt parseFloat区别
round 四舍五入，返回参数+0.5后，向下取整
floor 返回小于等于参数的最大整数
ceil  返回大于等于参数的最小整数
parseInt 解析一个字符串，并返回一个整数 返回舍去参数的小数部分后的整数
parseFloat  解析一个字符串，并返回一个浮点数

## 整理字符串和数组常用方法

### 字符串方法

#### 查找字符串的方法

- indexOf() 返回字符串中指定文本首次出现的索引index 没找到返回 -1 第二个参数为检索起始位置  不可以设置正则表达式
```js
str.indexOf("china",18)
```
- lastIndexOf() 返回字符串中指定文本最后一次出现的索引 index 没找到返回 -1 同上

- search()  方法返回字符串中指定文本第一次出现的位置 无法设置第二个参数  可以设置正则表达式
```js
str.search("China")   => 17
```

#### 提取字符串的方法

- slice(start, end) 提取字符串的某个部分并在新字符串中返回被提取的部分。
  - 如果参数是负数，则从字符串的结尾开始计数
  - 如果省略第二个参数，则该方法将裁剪字符串的剩余部分
  - 从结尾计数，也是裁剪字符串的剩余部分

- substring(start, end) 类似于slice() 不过不能接受负的索引

- substr(start, length) 类似于slice() 第二个参数规定被提取部分的长度

#### 替换字符串内容

- replace() 
  - replace()方法不会改变调用它的字符串 它返回的是新字符串
  - replace()只替换首个匹配
  - https://www.w3school.com.cn/js/js_string_methods.asp