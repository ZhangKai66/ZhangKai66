---
title: TypeScript学习
date: 2022-06-30 22:36:29
tags: TypeScript
categories:
- TypeScript
---

### 类型注解
``` ts

let b: "male" | "female" //限制b的值只可以取male或female
let c: any
let numbers: (number | string)[] = [1,2,3,'a']// 联合类型

// 类型别名

type NumOrStrArr = (number | string)[];

let number2: NumOrStrArr = [1,'w'];

// 函数类型 分别指定函数参数和返回值的类型 偏向使用这种方式

function add(num1: string,num2: string): string {
  return "a"
}

const add1 = (num1: number,num2: number):number => {
  return 1
}

// 同时指定参数和返回值类型（看着有点别扭）
const add2: (num1:number,num2:number) => number = (num1,num2) => {
  return 1
}

function great(name: string): void {
  console.log('hello')
}

// 可选参数 在参数名后冒号前添加个问号？即可
function mySlice(start?: number,end?: number): void {
  console.log('a')
}

// 对象类型 注意也可以加可选参数
let person: {
  name: string
  age?:number 
  sayHi(name:string):void
} = {
  name: "zk",
  age: "1",
  sayHi(name) {
    console.log('Hi',name)
  }
}

```

### 接口

> 当一个对象类型被多次使用时，一般可以使用接口interface来描述对象的类型，以达到**复用**的目的。也就是上方对象类型（可理解为对象模板）

``` ts
interface Person {
// 每一行后面可以加分号逗号或者不加都行
  firstName: string
  secondName: string
  sayHi(): void
}
function greeter(person: Person): string {}

// 接口和类型别名的对比
// 相同点：都可以给对象指定类型
// 不同点：interface只能给对象指定，类型别名可以为任何类型指定别名

type Person {
  firstName: string
  secondName: string
  sayHi(): void
}
```
#### 接口继承
```ts
interface Point2D {
  x: number
  y: number
}

interface Point3D extends Point2D {
  z: number
}

const p3: Point3D = {} //此时需要你定义x,y,z的值才行
```

### 元组tuple
> 指定确切数量的数组
```ts
let position: [number,number] = [1,2] // 只能有两个数字元素的数组
```

### 类型推论
> 能自动推断某个变量的类型
```ts
// 1. 声明变量并立即初始化之后,未初始化的还是必须要添加类型注解
let age = 10; // :number可以省略掉
// 2. 方法返回值可以推断
// function add(num1: number, num2:number)：number {}
function add(num1: number, num2:number) {}// 这里add括号之后的返回值类型注解可以省略掉
```

### 类型断言
> 使用类型断言指定更具体的类型 a标签link的问题
>使用**as**

```ts
const aLink = document.getElementById('link') as HTMLAnchorElement
// 在浏览器控制台打印 console.dir(),在最后的prototype那边即可看到元素的类型
```

### 字面量类型
```ts
const str = "Hello"// 此时str就是一个字面量类型，就是这个字符串就是一个类型，因为const不可变
function changeDirection(direction: 'up' | 'down' | 'left' | 'right')
// 这样比用string更严谨 更推荐
```

### 枚举
```ts
enum Direction {Up,Down,Left,Right}
enum Direction {Up = 2,Down = 4,Left,Right}
function changeDirection(direction:Direction) {
  console.log(direction)
}

changeDirection(Direction.Up)// 0
```

#### 字符串枚举
//必须要有初始值
```ts
enum Direction {Up="Up",Down="Down",Left="Left",Right="Right"}
```

### 类
``` ts
// constructor不能有返回值类型
class Student {
  fullName: string,
  constructor(public firstName,public middleInitial, public lastName) {
    this.fullName = firstName+ " " + middleInitial + " " + lastName;
  }
}
let user = new Student("Jane","Micky","User")
```
#### 类实例方法
```ts
class Point {
  x = 1
  y = 2

  // 类实例方法
  scale(n: number) {
    this.x *= n
    this.y *= n
  }
}
p.scale(10)
```

#### 类继承 extends & implements(interface)
```ts
// extends
class Animal {
  move() {
    console.log('mvoe')
  }
}
class Dog extends Animal {
  bark() {
    console.log('bark')
  }
}
// implements让class实现接口 约束
// 即Person类必须提供（实现）Singable接口中指定的所有方法和属性
interface Singable {
  sing(): void
}
class Person implements Singable {
  sing() {
    console.log('sing')// 要把interface中的方法实现，重写
  }
}
```

#### 成员可见性
public 是默认的，可以不写
protected 仅在类和子类中可见，**实例对象也不可见**
privated 仅在当前类可见，子类和实例对象不可见

readonly 只读修饰符 防止在构造函数之外对属性赋值
```ts
class Person {
  readonly age: number

  constructor(age:number) {
    this.age = age
  }

  setAge() {
    this.age = 20// 这边会报错 age属性不允许修改
  }
}
```

#### 类型兼容性
对象或者接口的类型兼容性，属性多的可以赋值给属性少的
参数兼容性，参数少的可以赋值给多的
```ts
type F1 = (a: number) => void
type F2 = (a: number, b: number) => void

let f1: F1
let f2: F2 = f1//是可以的
```

### 交叉类型 &

```ts
interface Person { name: string}
interface Contact { phone: string}
type PersonDetail = Person & Contact//就是将两个类型合并
let p1: PersonDetail = {
  name:'',
  phone:''
}
```

#### 交叉类型和接口继承对比
同名属性之间，处理类型冲突的方式不同
```ts
interface A {
  fn: (value: number) => string
}
interface B extends A {
  fn: (value: string) => string//此时代码会报错
}
interface C {
  fn: (value: string) => string
}
type D = A & C// 不会报错 fn的参数变成number和string均可 fn(value: number | string)
```

### 泛型
51课