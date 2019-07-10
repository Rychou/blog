---

title: 从一道题目窥探事件循环机制

date: 2019-7-08 12:00

tags: Javascript

---

# 从一道题目窥探事件循环机制

> 本文中提到的事件循环机制只针对于浏览器。浏览器和 Node 的事件循环机制是不同的，Node的事件循环是基于libuv实现的，感兴趣的可自行了解。

开篇先来道题目吧：这位同学，请把下面代码在浏览器控制台的输出顺序手写一下。

```js
console.log('1');

setTimeout(()=>{
    console.log('2');
}, 0);

new Promise((resolve)=>{
    console.log('3');
    resolve();
}).then(()=>{
    console.log('4');
})

setTimeout(()=>{
	console.log('5')
    new Promise((resolve)=>{
        console.log('6')
        resolve();
    }).then(()=>{
        console.log('7')
    })
    console.log('8')
},0)

console.log('9')
```

## 几个重要的概念

### 1. 函数调用栈(Call stack)

![](https://cdn-images-1.medium.com/max/800/1*-WOpjPy_2Idl4jIAzPokRQ.jpeg)

函数调用栈的作用：用于记录函数调用。

在执行的一个函数时，会将该函数 push 到调用栈顶，调用完成后将栈顶函数 pop 弹出。同理，如果一个函数调用期间调用了子函数，则该子函数会被push到调用栈顶，然后执行。



### 2. Web APIs

浏览器提供了多种异步的Web API，如DOM，times（计时器），AJAX等。

当我们调用一个 Web API 时，如 setTimeout，setTimeout函数会被 push 调用栈顶然后执行，但是 setTimeout 的回调函数不会立即被 push 到调用栈顶，而是起一个计时器任务。当这个计时器结束时，该回调函数会被塞到任务队列（CallBack Queue）中。这个队列中的回调函数的调用就是由事件循环机制来控制的。



### 3. 任务队列（CallBack Queue）

任务队列可分为两类：

- 宏任务(macro-task)：script（整体代码）,setTimeout/setInterval
- 微任务(micro-task)：Promise, Object.observe（已废弃）

任务源：setTimeout/setInterval, Promise这些属于任务源，不同的任务源会进入不同的任务队列

## 什么是事件循环机制

事件循环机制决定了`Javascript`代码执行的顺序

众所周知，Javascript是单线程的。在整个代码执行期间，会执行各种同步和异步的代码，保证其代码的执行顺序就很重要了。而事件循环机制就是干这个事的。

事件循环机制的流程如下：

![](D:\blog\source\_posts\event-loop\event-loop-flow.png)

## 题目分析

### 第一轮事件循环

首先，执行全局代码(script):

1. 输出 **`1`**
2. 遇到第一个 `setTimeout`, ` setTimeout` 函数是立即执行的，而 Web API 的计时器会给其回调函数起个计时器，0 ms后将该回调函数插入 `macro-task `队列。
3. 遇到第一个`Promise`, `Promise`的回调函数是立即执行的，输出**`3`**，并且 resolve。此时 `then`回调进入`micro-task`队列
4. 遇到第二个 `setTimeout`，经过 Web API 计时器后(0 ms)，其回调函数插入`macro-task`队列
5. 输出 **`9`**
6. 全局代码执行完毕

然后，检查`micro-task`是否有待执行的微任务。**步骤3** 的 `then` 回调在`micro-task`中，取出执行。

1. 输出 **`4`**

然后，检查`micro-task`是否有待执行的微任务。此时发现没有了，第一轮事件循环结束

### 第二轮事件循环

首先，取出`macro-task`队头的 **第一个`setTimeout`回调函数 **执行：

1. 输出 **`2`**

然后，检查`micro-task`是否有待执行的微任务。此时发现没有了，第二轮事件循环结束

### 第三轮事件循环

首先，取出`macro-task`队头的 **第二个`setTimeout`回调函数 **执行：

1. 输出 **`5`**
2. 遇到第二个`Promise`，输出 **`6`**, 并且 resolve，此时`then`回调函数进入`micro-task`队列
3. 输出 **`8`**

然后，检查`micro-task`是否有待执行的微任务。发现有个 `then` 回调在`micro-task`中，取出执行。

​	1. 输出 **`7`**

## 把问题升级一下

让我们来加入 async，看看输出结果如何：

```js
async function async1(){
	console.log('async1')
}

console.log('1');

setTimeout(()=>{
    console.log('2');
}, 0);

new Promise((resolve)=>{
    console.log('3');
    resolve();
}).then(()=>{
    console.log('4');
})

setTimeout(()=>{
	console.log('5')
    new Promise((resolve)=>{
        console.log('6')
        resolve();
    }).then(()=>{
        console.log('7')
    })
    console.log('8')
},0)

await async1();

console.log('9')
```

其实`async`只是一个语法糖，其返回值是一个`Promise`，上面代码中 `await` 语句后面的代码其实相当于 `Promise`的`then`回调，与下面代码等同：

```js
async function async1(){
	console.log('async1')
}

console.log('1');

setTimeout(()=>{
    console.log('2');
}, 0);

new Promise((resolve)=>{
    console.log('3');
    resolve();
}).then(()=>{
    console.log('4');
})

setTimeout(()=>{
	console.log('5')
    new Promise((resolve)=>{
        console.log('6')
        resolve();
    }).then(()=>{
        console.log('7')
    })
    console.log('8')
},0)

// await 部分的代码与下面代码等同
new Promise((resolve)=>{
    async1();
    resolve();
}).then(()=>{
    console.log('9')
})
```



