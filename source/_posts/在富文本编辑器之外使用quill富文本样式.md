---
title: 在富文本编辑器之外使用quill富文本样式
date: 2018-07-17 12:37:17
tags: [踩坑, vue]
layout: page
---

在将使用富文本编辑器时遇到了一个这样的问题：如何在富文本编辑器 VueEditor 之外依然能够有原来的样式

一开始的思路是全局引入 quill 的样式文件，因为 VueEditor 是基于 quill 的一个富文本编辑器

<!-- more -->

```js
npm install quill@1.3.6 //先安装quill

//在main.js引入样式文件
import 'quill/dist/quill.core.css'
import 'quill/dist/quill.snow.css'
import 'quill/dist/quill.bubble.css'

```

结果发现依然行不通，通过查看 quill 的样式文件发现

```css
//quill/dist/quill.snow.css

.ql-snow .ql-editor pre.ql-syntax {
  //这是富文本编辑器中代码段（code）的样式
  background-color: #23241f;
  color: #f8f8f2;
  overflow: visible;
}
```

样式文件是使用了父元素控制的，而通过富文本编辑器生成的 html 字符串中，是没有包含父元素的，因此在要渲染 html 的元素中给他嵌套父元素以及赋予相应的 class 就可以了！

```html
<div class="ql-container ql-snow" style="border:none">
  <div class="ql-editor">
    <div v-html="article.content"></div>
    //这是渲染html字符串的元素，其余是给他嵌套的父元素
  </div>
</div>
```

参考链接：https://segmentfault.com/q/1010000012313485
