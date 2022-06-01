---
title: 用js实现常见的数据结构
date: 2022-05-11 22:54:46
tags: 数据结构
categories:
- 算法
---
### stack栈
```js
class Stack {
  constructor() {
    this.items = [];
  }
  get length() {
    return this.items.length;
  }
  get peek() {
    this.items[this.items.length - 1];
  }
  push(ele) {
    this.items.push(ele);
  }
  pop() {
    return this.items.pop();
  }
}
```

### Queue队列
```js
class Queue {
  constructor() {
    this.items = [];
  }
  get isEmpty() {
    return this.items.length === 0;
  }
  get length() {
    return this.items.length;
  }
  enqueue(ele) {
    this.items.push(ele);
  }
  dequeue() {
    return this.items.shift();
  }
}
```

### 循环队列
```js
class LoopQueue extends Queue {
  constructor(maxSize) {
    super();
    this.maxSize = maxSize;
    this.head = -1;
    this.tail = -1;
  }
}
```

https://blog.csdn.net/qq_45534061/article/details/113821910