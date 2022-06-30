---
title: TypeScript学习
date: 2022-06-30 22:36:29
tags: TypeScript
categories:
- TypeScript
---

### 类型注解
``` ts
// 函数的返回值也可以指定类型
function greeter(person: Person): string {}
let b: "male" | "female" //限制b的值只可以取male或female
let a: boolean | string// 联合类型
let c: any

```

### 接口
``` ts
interface Person {
  firstName: string,
  secondName: string,
}
function greeter(person: Person): string {}
```

### 类
``` ts
class Student {
  fullName: string,
  constructor(public firstName,public middleInitial, public lastName) {
    this.fullName = firstName+ " " + middleInitial + " " + lastName;
  }
}
let user = new Student("Jane","Micky","User")
```