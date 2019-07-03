---
title: vue和小程序踩坑指南
date: 2018-07-19 14:25:08
tags: [踩坑, 小程序, Vue]
---

# 一、参考文档

- [小程序官方 Api 文档](https://developers.weixin.qq.com/miniprogram/dev/api/)
- [mpvue 官方文档](http://mpvue.com/mpvue/)
- [vue 官方文档](https://cn.vuejs.org/v2/guide/)
  <!-- more -->

# 二、工具篇

### 1. VSCode 插件

- Eslint 格式化代码插件，在写代码时提示格式以及保存代码时自动格式化代码。[配置教程](https://juejin.im/entry/5a4516eb5188252b145b5af8)
- mpvue snippets 小程序和 mpvue 代码提示
- Vetur Vue 开发工具

### 2. [normalize 前端解析 Json API](https://github.com/yury-dymov/json-api-normalizer)

```
a:{
  1:{
    type:'',
    id:''
  }
}

对于这种对象的key为数字的，然而并不知道数字是啥，可以使用a[Object.keys(a)[0]]来访问第一个元素
```

### 3. [mpvue-router-patch](https://github.com/F-loat/mpvue-router-patch)在 mpvue 中使用 vue-router 的写法

### 4. [flyio 发起网络请求](https://www.npmjs.com/package/flyio)

### 5. 封装小程序 API，实现异步调用

```
// WXP.js

// 核心
function f (func, obj) {
  return new Promise((resolve, reject) => {
    func({
      ...obj,
      success: resolve,
      fail: reject
    })
  })
}

export default {

  // 获取认证 code
  login: obj => f(wx.login, obj),
}

//使用

import WXP from 'WXP'

WXP.login({})
    .then(res=>{})
    .catch(err=>{})
```

### 6. Promise 实现 finally.

```
// 安装
npm i --save promise.prototype.finally

// main.js引入
require('promise.prototype.finally').shim();

// 使用
promise()//某个异步操作
.then()
.catch()
.finally()
```

# 三、踩坑

### 1.修改 v-html 渲染的标签样式不生效的问题

产生原因：在<style scoped></style>中加了 scoped 属性，这个属性的作用是让样式私有化（只在本组件生效）。而使用了 v-html 之后，其实际是被当成子组件了，因此导致样式不生效

解决办法：去掉<style scoped></style>中的 scoped

```
<template>
  <div v-html="htmlString"></div>
</template>

<script>
export default{
  data(){
    return(){
  	htmlString:''//要渲染的html字符串
    }
  }
}
</script>

<style lang="scss">//去掉scoped
</style>
```

### 2. 如何在 vue 中使用 hightlight.js 高亮你的代码

#### 安装 highlight.js

```
npm install highlight.js --save
```

#### 在顶层 main.js 中加入以下代码段

```
import hljs from 'highlight.js'
import 'highlight.js/styles/monokai.css'//样式文件

//vue自定义指令，检索所有的code标签进行高亮
Vue.directive('highlight',function (el) {
  let blocks = el.querySelectorAll('code');
  blocks.forEach((block)=>{
    hljs.highlightBlock(block)
  })
})
```

#### 在你文章的顶层标签加入自定义指令 v-highlight

```
<div class="article-body" v-html="compileMarkdown" v-highlight></div>
```

### 3. 不要在 computed 内使用 split()方法

### 4.wx.showToast({title:'text',image:'/state/xx.png'})需要用绝对路径

### 5. `<scroll-view></scroll-view>`组件

#### 5.1. 如若需要竖向滚动，则需要设置固定高度，并且单位不能为 rpx（设置成 rpx 会有显示的问题，显示不全）

```
<scroll-view
          :scroll-y="true"
          :style="{'height': scrollHeight+'px;','padding-top':'100rpx'}" //切不要设置成rpx，否则会出现显示不全的bug
          :enable-back-to-top="true"
          @scrolltolower="loadData"
        >
...
</scroll-view>

//获取屏幕高度
wx.getSystemInfo({
      success: res => {
        this.scrollHeight = parseInt(res.windowHeight);
      }
    });
```

#### 5.2.使用`<scroll-view></scroll-view>`组件与`onPullDownRefresh`不能同时使用

参考：[微信小程序—下拉刷新上拉加载](https://blog.csdn.net/zhenqiangzhang/article/details/54347602)

# 6. 录音

#### 6.1. 格式问题导致录音无法播放。

小程序推荐使用的录音管理上下文`wx.getRecorderManager()`支持的录音格式有两种:`aac/mp3`。如果不指定格式，在安卓端录音的文件格式是 m4a 的，会导致无法播放。

所以需要在录音时手动设置格式为`mp3`格式，`aac`会有播放不全的 BUG

```
this.recordManager.start({
    format: 'mp3' //
})
```
