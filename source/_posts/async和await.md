---
title: async和await
date: 2021-11-08 21:23:37
tags: javascript, async, await
categories:
- javascript
---

# async await要点
1. await 后面可以追加 promise 对象，获取 resolve 的值
2. await 必须包裹在 async 函数里面
3. async 函数执行返回的也是一个 promise 对象
4. try-catch 截获 promise 中 reject 的值