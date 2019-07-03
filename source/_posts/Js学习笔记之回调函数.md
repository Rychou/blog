---
title: Js学习笔记：回调函数
date: 2018-09-18 14:23:29
tags: [踩坑, Javascript, 回调函数]
---

> A callback is a function that is passed as an argument to another function and is executed after its parent function has completed。

回调函数作为参数传递给另外一个函数，当另外一个函数执行完后再执行这个传递进去的函数（回调函数）。

<!-- more -->

这里我看到一个网上一个很有意思的例子

> 约会结束后你送你女朋友回家，离别时，你肯定会说：“到家了给我发条信息，我很担心你。” 对不，然后你女朋友回家以后还真给你发了条信息。小伙子，你有戏了。其实这就是一个回调的过程。你留了个参数函数（要求女朋友给你发条信息）给你女朋友，然后你女朋友回家，回家的动作是主函数。她必须先回到家以后，主函数执行完了，再执行传进去的函数，然后你就收到一条信息了。

```js
function A(callback) {
  callback()
  console.log('1')
}
function B() {
  setTimeout(console.log('2'), 3000)
}
A(B)
//输出
//1
//2
```

这里回调函数 B 是一个需要等待 3 秒才输出的，然而主函数不必等回调函数 B 执行完毕，可以继续执行自己的代码，然后再去执行回调函数 B。

所以回调函数一般用来做一些耗时的操作，如网络请求等，来实现异步操作。
