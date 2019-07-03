---
title: 用Promise实现小程序接口链式调用
date: 2018-10-06 14:23:29
tags: [小程序, Promise, mpvue]
---

<!-- more -->

# 一、前言

> 作者平时使用`mpvue`开发小程序，所以下面讲到的方法都是基于`mpvue`而言的，当然本质上原生小程序语法同样适用

大家都知道，小程序的接口都是采用回调的方式，这样如果代码逻辑复杂了，将会导致代码难以阅读。今天就通过`ES6`的`Promise`函数，来稍微改造一下小程序接口，让我们的代码实现链式调用，便于阅读。

# 二、核心代码

在`utils`目录下新建文件`WXP.js`（WX Promise）,代码内容如下：

```js
// 代码核心，利用到了ES6的Promise函数
function f(func, obj) {
  return new Promise((resolve, reject) => {
    func({
      ...obj,
      success: resolve,
      fail: reject
    })
  })
}

// 改写所有wx接口
var WX = {}

for (let key in wx) {
  WX[key] = obj => f(wx[key], obj)
}

//将改写后的导出
export default WX
```

代码原理很简单，主要就是在代码回调的地方，分别调用`resolve`和`reject`函数，分别对应着`success`和`fail`回调

# 三、在顶层`main.js`下全局配置`WXP.js`

```js
// main.js
import Vue from 'vue'
import App from './App'
import WXP from './utils/wxp'

Vue.config.productionTip = false
App.mpType = 'app'

Vue.prototype.WXP = WXP

const app = new Vue(App)
app.$mount()
```

这么做主要是全局配置之后，就不需要每个文件里面都导入`WXP.js`文件，便于使用。

# 四、使用

改造前的回调方式：

```js
wx.showLoading({
  title: '测试',
  success: function() {
    wx.login({
      success: function(res) {
        console.log(res.code)
      },
      fail: function(err) {
        console.error(err)
      }
    })
  },
  fail: function(err) {
    console.error(err)
  }
})
```

改造后的链式调用方式

```js
this.WXP.showLoading({ title: '测试' })
  .then(res => {
    return this.WXP.login()
  })
  .then(res => {
    console.log(res.code)
  })
  .catch(err => console.error(err))
```

可以看出来，当回调的方式层次不深的时候还是便于阅读的，但是异步操作多了，层次就会多，这样代码就会变得难以阅读。而改造后的链式调用方式，不管你层次有多少，都是一条链一样，一步一步的，思路清晰，易于阅读。不得不说，`Promise`大法好哈哈哈哈哈哈哈哈哈

> 参考连接：
> [小程序 API](https://developers.weixin.qq.com/miniprogram/dev/api/)
 > [大白话讲解 Promise](http://www.cnblogs.com/lvdabao/p/es6-promise-1.html)
 > [阮大佬的 Promise](http://es6.ruanyifeng.com/#docs/promise)
