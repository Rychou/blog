---
title: 小程序Tabs懒加载
date: 2018-07-17 14:26:31
tags: [踩坑, 小程序]
---

> 小程序用 mpvue 编写

# v-if 和:hidden 的区别

这两种方式都可以用来控制组件的显示与隐藏，但他们实现原理上有所不同。

<!-- more -->

v-if：这是条件渲染，只有当其为 true 时，才会渲染出来
:hidden：这只是控制组件的显示与隐藏，会将所有组件都在开始的时候全部加载，只是有的组件没有显示而已。

v-if 有更高的切换消耗（切换会有旧组件销毁和新组件挂载），而 hidden 有更高的初始渲染消耗（在初始就挂载所有组件）。所以 hidden 更适合用于频繁切换

# 使用 hidden 实现懒加载

1. 在父组件用 hidden 控制组件隐藏显示

```
index.vue//父组件
----------------------------
...
<template>
<div class="weui-tab__panel">
     <div class="weui-tab__content" :hidden="hiddens[index]" v-for="(tab,index) in tabs" :key="index" >//在tab内容组件外面一层用hidden控制组件隐藏
         <list-delegate  :hidden="hiddens[index]"/>//这里将hidden属性传入tab内容子组件内部，其内部根据hidden进行相关的控制。
     </div>
 </div>
</template>
...
<script>
export default{
	data(){
		return(){
		   tabs: ["推荐", "热门", "我的"],
      		   activeIndex: 0,
		}
	}
	computed: {
   	  hiddens(){
  	    return [this.activeIndex!=0,this.activeIndex!=1,this.activeIndex!=2]//hiddens数组，计算当前hidden的tab，一共有3个tab
   	  }
........
}

</script>
--------------------------------------
```

2.在 tab 内容子组件用传入的 hidden 属性进行相关的控制

```

```
