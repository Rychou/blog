---
title: ES6数组去重和重复元素统计
date: 2018-09-27 14:23:29
tags: [Javascript]
---

<!-- more -->

# 一、去重

这里利用到了`ES6`的`Set`结构不允许数据重复的特性

```js
let arr1 = [1, 1, 2, 3, 1, 2, 4, 2]
//先将数组转化为Set数据类型，然后再转回数组类型
let dedupeArr = Array.from(new Set(arr1))
```

# 二、统计

```js
let count = 0
let obj = {} //最终返回的数据
dedupeArr.forEach(i => {
  count = 0
  arr1.forEach(j => {
    if (i === j) count++
  })
  obj[i] = count //键名为i(数组元素)，值为count(出现次数)
})
```

# 三、遍历对象

```js
for(let i = 0 in obj){
	console.log(i+':'+obj[i]);
}
// 数组元素:出现次数
// 1:3
// 2:3
// 3:1
// 4:1
```
