---
title: promise
date: 2022-04-14 23:30:10
tags: javascript, promise, async, await
---

# Promise

## promise代码

```js
const p1 = new Promise((resolve,reject) => {
  resolve();
  // reject();
});
p1.then(() => {}).catch(() => {})
```

## promise的三个状态

### 状态

 - pending
 - fulfilled(resolved)
 - rejected

### 特征

- 不可逆
- 只能从 pending->fulfilled 或者 pending->rejected

### 变化过程

执行resolve()  该promise的状态则 pending->fulfilled
执行reject()  该promise的状态则 pending->rejected

### 触发回调

resolved状态的promise可以触发then回调
rejected状态的promise可以触发catch回调

## then\catch 怎么影响状态的变化

then正常返回resolved,如果有报错则 返回rejected
catch正常返回resolved,如果有报错则 返回rejected

```js
const p2 = Promise.resolve().then(() => {
    throw new Error('then error')
}) // rejected状态的promise 后续p2的then则不会触发
```

## 面试题
```js
Promise.resolve().then(() => {
  console.log(1);//1
  throw new Error("bug")
}).catch(() => {
  console.log(2);//2
}).then(()=> {
  console.log(3);//3 注意这边catch正常的返回也是resolved状态的promise，是可以触发后续的then回调的
})
```