---
title: Js学习笔记：apply与call
date: 2018-08-15 14:32:20
tags: [Javascript]
---

# 一、apply 与 call 的区别

相同点：“可以让一个对象调用另一个对象的方法”

不同点：

- apply 最多只能传入两个参数，第一个为对象，第二个为数组
- call 能传入多个参数，第一个为对象，其后为 n 个参数列表

<!-- more -->

实际上，apply 和 call 实现的功能是一样的，只是传入的参数不同而已。

示例:

```
function add(a,b){
  return a+b;
}
function sub(a,b){
  return a-b;
}
var a1 = add.apply(sub,[4,2]);　　//sub调用add的方法
var a2 = sub.apply(add,[4,2]);
alert(a1);  //6
alert(a2);  //2

/*call的用法*/
var a1 = add.call(sub,4,2);
```

### apply 的一些使用技巧

1. 配合`Math.max()`计算数组最大值

因为`Math.max()`不支持数组的方式，只能`Math.max(a,b,c....)`。
根据 apply 的特点来实现这一功能，`Math.max.apply(null,[1,2,3])`,因为没有新的对象调用 Math 的 max 方法，所以只是传入 null 来利用 apply 的特性帮助进行计算而已。

apply 会将数组中的每个元素一个个传入给 Math.max()。也就相当于`Math.max.call(null,1,2,3)`

同理可以用`Math.min.apply(null,[1,2,3])`计算数组最小值

2. 配合 Array.prototype.push 实现两个数组合并

数组的 push 方法是不能 push 数组的，但是可以同时 push 多个元素，因此可以利用 apply 的特性

```
var a = [1,2,3];
var b = [4,5,6];

Array.prototype.push.apply(a,b);//apply会将为b中每一个元素执行一次push方法。返回值是push后数组a的长度
```

> 参考链接:
> [apply()与 call()的区别](https://www.cnblogs.com/lengyuehuahun/p/5643625.html)

# 二、Object.prototype.toString.call()进行类型检验

首先来看一个问题，用 typeof 来检验类型有什么缺点呢？

答案是 typeof 无法准确地检验对象类型。

```
typeof null //object
typeof [] //object
```

> `null`的类型是`object`，这是由于历史原因造成的。1995 年的 `JavaScript` 语言第一版，只设计了五种数据类型（对象、整数、浮点数、字符串和布尔值），没考虑`null`，只把它当作`object`的一种特殊值。后来`null`独立出来，作为一种单独的数据类型，为了兼容以前的代码，`typeof null`返回`object`就没法改变了。

比较好的方式就是用 `Object.prototype.toString.call()`来进行检验。

```
var a = {};
var b = [];
var c = 1;

Object.prototype.toString.call(a);//[object,Object]
Object.prototype.toString.call(b);//[object,Array]
Object.prototype.toString.call(c);//[object,Number]

//判断a是否是对象类型
Object.prototype.toString.call(a) === "[object,Object]"
```

> 注意：使用 obj.toString()是不能得到类型的。
> 原因：Array,Function 等类型作为 Object 的实例，都重写的了 toString 方法。因此在调用时，是调用了重写后的方法，而不是原型链上的 toString()方法

```
var arr=[1,2,3];
console.log(Array.prototype.hasOwnProperty("toString"));//true
console.log(arr.toString());//1,2,3
delete Array.prototype.toString;//delete操作符可以删除实例属性
console.log(Array.prototype.hasOwnProperty("toString"));//false
console.log(arr.toString());//"[object Array]"
```

删除了重写的方法后，使用 obj.toString()也就相当于调用原型链的方法了，即 Object.prototype.toString.call()

> 参考链接:
>
> - [为什么用 Object.prototype.toString.call(obj)检测对象类型?](https://blog.csdn.net/Bonjourjw/article/details/80419470)
> - [深入理解 Object.prototype.toString.call()](https://www.jianshu.com/p/e4237ebb1cf0)
