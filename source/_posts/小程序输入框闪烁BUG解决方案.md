---
title: 小程序输入框闪烁BUG解决方案
date: 2018-08-30 14:25:08
tags: [小程序]
---

# 前言

> 本人所说的小程序，都是基于`mpvue`框架而上的，因此`BUG`可能是原生小程序的，也有可能是`mpvue`的。

<!-- more -->

# 问题描述

在小程序`input`组件中，如果使用`v-model`进行双向绑定，在输入时会出现光标闪烁的`BUG`。

# 原因

造成这个 BUG 的原因，是因为在原生小程序`input`组件上，进行了封装。才导致光标闪烁的问题。

# 解决方案

这里提供了两种解决方案。各有各的优势，请选取合适的方案：

### 一、弃用`v-model`,使用`@input`

适用于`input`组件单纯是用户手动输入的，而没有像**授权获取手机号**，然后自动填入`input`组件中。这种需要赋予初始`value`的情况。即适用于**单向绑定，而不是双向绑定**。

```
/**
* 父组件
*/

// html
<childInput @input="onInput"/> <-- 注意这里不能使用:value=value赋予初始值，否则又会出现闪烁的BUG，因为此时就等同于v-model了

// js
import ChildInput from './ChildInput'
export default {
  components:{ ChildInput },
  data(){
    return {
      value:''
    }
  },
  methods(){
    onInput(e){
  	this.value = e
    }
  }
}

/**
* 子组件 ChildInput
*/

// html

<input @input="$emit('input',$event)"/>

```

### 二、不封装`input`组件，直接使用`v-model`

这种情况就是可以直接双向绑定的了。具体原因还不清楚，可能是本身原生小程序的 BUG。

```
/**
* 父组件,不使用封装的子组件，直接使用原生组件。
*/

// html
<input v-model="value"/>

// js
export default {
  data(){
    return {
      value:''
    }
  }
}
```
