---
title: 用gitalk给你的博客添加评论服务
date: 2018-08-18 14:28:03
tags: [Gitalk]
---

# 一、前言

不像 hexo、jekyll 等，为了学习，我的博客是用`vue`和`nodejs`搭建出来的。支持在线编辑和 markdown 渲染等功能。

大家如果感兴趣可以看我的[博客地址](www.rychou.xyz)

最近给自己的博客添加了评论服务，用到的评论服务是[Gitalk](https://github.com/gitalk/gitalk)，它是基于`github issue`搭建出来的评论系统。废话不多说，接下来说说如何使用。

<!-- more -->

# 二、必要的准备

### 在 Github 创建一个用来存放评论的仓库

当然你也可以直接用你博客所在的仓库，因为我的博客部署到了`coding pages`，所以我单独创建了一个仓库用来存放评论。

### 注册一个`Github OAuth application`

[没有的可以点这里申请](https://github.com/settings/applications/new)

![申请OAuth application](https://i.loli.net/2018/08/18/5b77da4eed290.jpg)

注册成功后，记录下你的`clientID`和`clientSecret`。

# 三、安装

有两种方式安装(CDN 和 npm)，大家可自行选择。

### 1. CDN

```
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.css">
  <script src="https://cdn.jsdelivr.net/npm/gitalk@1/dist/gitalk.min.js"></script>

  <!-- or -->

  <link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
  <script src="https://unpkg.com/gitalk/dist/gitalk.min.js"></script>
```

### 2. npm

```
npm i --save gitalk
```

# 四、使用

考虑到`vue`的生命周期等，使用方法上和官方文档给出来的是需要做略微的调整的。

### 引入 gitalk

```
//在index.html页面中CDN引入
<link rel="stylesheet" href="https://unpkg.com/gitalk/dist/gitalk.css">
<script src="https://unpkg.com/gitalk/dist/gitalk.min.js">

//在vue中import
import Gitalk from 'gitalk'
```

### 初始化 Gitalk 实例

```
export default {
  data(){
     return{
	gitalk: new Gitalk({
		clientID: "你的clientID",
        	clientSecret: "你的clientSecret",
        	repo: "刚刚创建的项目名",
        	owner: "github用户名",
        	admin: ["github用户名"],
        	id: window.location.hash, // 默认为pathname,如果你也是像我这样用vue搭建的，我建议使用hash，原因具体后面会讨论到。
        	distractionFreeMode: false // Facebook-like distraction free mode
	})
    }
  }
}
```

### 在恰当的位置添加标签,用来渲染 Gitalk 组件。

```
<div id="gitalk-container"></div>
```

### 渲染

```
mounted(){
  this.gitalk.render("gitalk-container");
}
```

# 五、注意事项

### id 的设置

这个 id 是用来唯一区分页面的，也就是你不同的文章肯定有不同的评论数据，`Gitalk`要通过这个 id 来区分是哪个页面，然后在你的`github`仓库创建相应的`issue`来存放当页评论数据，`id`默认值是`window.location.pathname`

但是默认的只适用于采用`history`模式的路由，但对于`vue-router`这种默认采用`hash`模式的就不行了。

举个例子

```
//在history模式下，路由URL如下格式
www.rychou.xyz/article/69

window.location.pathname;// 值是 /article/69

//在hash模式下，路由变成这样了
www.rychou.xyz/#/article/69

window.location.pathname; // 值是 /
```

此时，在`hash`模式下`id`就不具有唯一性了。

建议使用`window.location.hash`，此时的值是`#/aticle/69`,具有唯一性。

当然你也可以设置`vue-router`为`history`模式，但是这会有一个刷新页面出现 404 的 BUG，解决办法参考[官方文档](https://router.vuejs.org/guide/essentials/history-mode.html#example-server-configurations)

### 不使用类似的评论服务[Gitment](https://github.com/imsun/gitment)的原因

我也试过用`Gitment`，也成功了，但是有一个非常令人不爽的地方，就是每一篇文章都需要手动初始化(创建相应 issue)，因此我还是用了`Gitalk`,它会根据你的配置，自动帮你初始化。

# 六、最后

有问题的同学们，欢迎来[原文](http://www.rychou.xyz/#/article/69)下方提问，同时也相当于帮我测试一下评论服务了。

> 参考链接：
>
> - [Gitalk](https://github.com/gitalk/gitalk)
> - [Gitment](https://github.com/imsun/gitment)
> - [官方文档- vue-router History 模式](https://router.vuejs.org/guide/essentials/history-mode.html#example-server-configurations)
