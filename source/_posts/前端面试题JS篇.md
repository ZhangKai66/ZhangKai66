---
title: 前端面试题JS篇
date: 2021-11-30 20:22:56
tags: 面试题
---

## JS基础篇
### 手写js深拷贝
```js
function deepClone(obj = {}) {
  // 这边留意 双等号意味着既不是null也不是undefined
  if(typeof obj !== "object" || obj == null) {
    return obj;
  }
  // 初始化返回值
  let result;
  if(obj instanceof Array) {
    result = [];
  }else {
    result = {};
  }
  // 核心
  for(let item in obj) {
    if(obj.hasOwnProperty(item)) {
      // 递归调用 这边要反复看 自己默写的是result
      result[item] = deepClone(obj[item])
    }
  }
  // 返回
  return result;
}
```
> 递归可以深入研究下 添加到toDo中

### typeof的用法
1. 可以返回所有的基本数据类型 undefined string number boolean
2. 引用数据类型只能返回object，不能具体区分是array还是object
3. typeof function返回值就是function
举例：typeof(class c{}) 也是function

### 原型和原型链
题目：
1. 如何准确判断一个变量是不是数组
2. 手写一个jquery，考虑插件和扩展性
3. class的原型本质，怎么理解

#### class的用法
```js
class Student {
  // 构造函数 就是在初始化新实例/对象的时候执行的方法
  constructor(name,num) {
    this.name = name;
    this.num = num;
  }
  sayHi() {
    console.log(
      `姓名：${this.name}, 学号：${this.num}`
    );
  }
}
const xialuo = new Student("夏洛",120);
xialuo.sayHi();

class Son extends  Student {
  // 继承 调用super函数 处理父亲有的参数
  constructor(name,num,gender) {
    super(name,num);
    // 自己独有的自己绑定
    this.gender = gender;
  }
}
```
#### instanceof进行类型判断
```js
xialuo instanceof Student // true
[] instanceof Array // true
[] instanceof Object // true
```

#### 原型