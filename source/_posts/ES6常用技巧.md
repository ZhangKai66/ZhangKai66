---
title: ES6常用技巧
date: 2021-11-25 23:06:49
tags: ES6
---

## ES6
### 对象取值方式
> 用解构赋值的方式
```js
const {a,b,c} = obj;
const res = a + b; 
```
如果想创建的变量名和对象的属性名不一致，可用取别名的方式
```js
const {a:myName} = obj;
```
> 注意：解构的对象不能为undefined和null，会报错
可以给一个默认值：空对象
```js
const {a,b,c} = obj || {};
```

### 合并数据方式
一般方法
```js
// array
const a = [1,2];
const b = [4,3];
const c = a.concat(b);
// object
const d = {
  a: 1
}
const e = {
  f: 1
}
const g = Object.assign({},d,e)//a:1,f:1 注意Object.assign是浅拷贝
```
ES6方法
```js
const c = [...new Set([...a,...b])];
const g = {...d,...e} //解构赋值也是浅拷贝
```

### if中判断条件
```js
if(type == 1 || type == 2 || type == 3 ) {...}
// 改进之后 用es6新增的includes方法
const condition = [1,2,3];
if(condition.includes(type))
```

### 获取对象属性值的方法
```js
const name = obj && obj.name
// 改进之后 es6的可选链操作符
const name = obj?.name
```

### 输入框非空的判断
```js
if(value !== null && value !== undefined && value !== '')
// 改进后 空值合并运算符
if(value ?? '' !== '')
```

### 添加动态变化属性名的对象属性
```js
let obj = {}
let index = 1;
let key = `topic${index}`;
obj[key] = '话题内容'
// 改进之后 对象属性名用表达式
obj[`topic${index}`] = '话题内容'
```