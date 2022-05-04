---
title: JS数组方法
date: 2022-03-27 18:10:46
tags: js
---

## JS常用数组方法

### 基本属性
1. 特点：能存储 有序节点 可以在已知的两个元素中间插入新的元素

2. 创建数组的两种方法：
var arr = [];
var arr = new Array(); //用的居然是小括号 一般不用这个方法

3. 获取元素：arr[0];获取下标为0的元素 
   替换元素：arr[3] = "fruit" 如果之前没有下标为3的元素 则在3位置处添加新的元素

4. length 数组长度

5. 数组内部可以存储所有类型元素  包括数组和对象
var arr = [{name:"john",age:19},12,"hello"];
console.log(arr[0].name);//显示为john

6. queue（队列）方法：pop/push  shift/unshift
 push在队列末端插入新元素   shift在队列前端取出第一个元素并返回他，后续元素向前移动一格  
 unshift：在队列前端插入新元素 push和unshift都可以一次添加多个元素

7. stack（栈）方法：push/pop 
push pop都是在末端插入或取出元素 只能是一个元素

8. 数组同时支持先进先出(lifo)和后进先出(fifo)   是双端队列

9. 从本质上讲  数组也是一个对象   复制数组也是复制的引用

最后  强调 数组必须是用于  有序数据  的特殊结构 如果不按照有序数组来操作 那么就不会享受到数组的优化

### 数组方法

1. 插入提取方法
   - arr.push 从尾部插入
   - arr.pop 从尾部提取
   - arr.shift 从头部提取
   - arr.unshift  从头部插入

2. splice方法 ：可以实现 添加 删除 插入元素等操作

   - 插入或删除的起始下标：1就是替换第二个元素
   - 删除的个数：0表示添加
   - 添加或者要更换的元素

  >splice(1,1) *删除*：从下标为1的位置删掉1个元素 splice返回的就是被删除掉的元素
  >splice(0,3,"let's","dance") *替换*：从下标为0的位置删掉3个元素 并用let's dance替换在被删掉的区域
  >splice(1,0,"hello","world") *添加*：从下标为1的位置向后添加2个元素 添加在下标为1和2的位置

3. slice方法：切片功能 slice(a,b) 切出从a下标到b下标之前的的元素 左闭右开（左边包括 右边不包括）

    arr.slice()不带参数调用的话 就是创建一份新的arr副本 
    其中b是可选的，不写的话就是从a切到末尾。
    arr.slice()返回的是切出来的值组成的[a,b)
    对 *原数组* 是没有影响的

4. concat方法：
链接方法：arr.concat(a,b)将a和b连接到arr数组的最后。 如果a也是数组 则会提取出数组中的数值 添加到arr后面 ，如果不是数组，其他元素都会以本来的形式插入到数组arr中。

5. arr.foreach()方法：可以为数组中每一个元素运行方法

```js
["Bilbo", "Gandalf", "Nazgul"].forEach((item, index, array) => {
  alert(`${item} is at index ${index} in ${array}`);
});
```

6. arr.indexOf(item,from) / arr.lastIndexOf(item,from) /arr.includes(item,from) 返回item元素在数组中的下标
from指从哪一个下标位置开始查询

7. 转换数组 arr.map() 对数组内所有元素调用某个方法 返回该方法返回值组成的新数组

格式:`let res = arr.map(function(item,index,array){})`
示例：`let length = ["heloo","jack","tom"].map(item => item.length);`

8. arr.sort() 对数组进行原位排序  排序之后的结果返回到这个数组本身 等于是修改了原数组 默认是按字符串排序

9. 利用sort函数 对数字进行排序操作
```js
let arr = [ 1, 2, 15 ];

arr.sort(function(a, b) { return a - b; }); 只需要比较函数返回正数或者负数就可以完成数组的排序

alert(arr);  // 1, 2, 15
  // 还有更简洁的：箭头函数
arr.sort((a,b) => return a-b;);//原答案return都没有写
```

10. arr.reverse() 
用于颠倒数组中的元素顺序

11. 分割字符串 
let names = 'Bilbo, Gandalf, Nazgul';
let arr = names.split(', ');
for (let name of arr) {
  alert( `A message to ${name}.` ); // A message to Bilbo（和其他名字）
}

12. 黏合字符串 与上面分割字符串正好相反：arr.join()
let arr = ['Bilbo', 'Gandalf', 'Nazgul'];
let str = arr.join(';'); // 使用分号 ; 将数组粘合成字符串
alert( str ); // Bilbo;Gandalf;Nazgul

13. arr.reduce()方法：该函数一个接一个地应用于arr数组的所有元素 并将前一次结果作为参数传给下一次调用 其中accumulator是用来储存*上一次计算结果*的 可以用作类似求和的需求
let value = arr.reduce(function(accumulator, item, index, array)
示例：
let arr = [1, 2, 3, 4, 5];
let result = arr.reduce((sum, current) => sum + current, 0);
alert(result); // 15

14. array.isArray() 是数组的话返回true 反之返回false
