---
title: 操作DOM节点
date: 2019-3-15 22:05:29
tags: [DOM]
---

# 节点的关系

```html
<html>
  <head></head>
  <body>
    <ul id="test">
      <li>1</li>
      <li>2</li>
      <li>3</li>
      <li>4</li>
    </ul>
  </body>
</html>
```

如上结构，有如下关系：

```js
let nodes = document.getElementsByTagName('li')
console.log(nodes) // li 组成的类数组结构
console.log(nodes[0].parentNode) // ul
console.log(nodes[0].parentNode.firstChild.textContent) // 1
console.log(nodes[0].parentNode.lastChild.textContent) // 4
console.log(nodes[0].parentNode.firstChild.nextSibling.textContent) // 2
console.log(nodes[0].parentNode.firstChild.nextSibling.previousSibling.textContent) // 2
console.log(nodes[0].parentNode.firstChild.nextSibling.nextSibling.textContent) // 3
console.log(nodes[0].parentNode.firstChild.nextSibling.nextSibling.nextSibling.textContent) // 4

```

# 操作节点

## 1. 插入节点

- 方法一： `sourceElement.appendChild(targetElement)` 用于向`sourceElement`的`childNodes`列表结尾添加节点，返回添加的节点。如果添加的`targetElement`已经存在于`childNodes`中，则结果就是把原来的位置移动到新的位置（末尾）

- 方法二：`sourceElement.insertBefore(targetElement, referenceElement)` 用于向`childNodes`中指定元素之前插入元素。若第二个参数为`null`，则结果与方法一相同（插入到末尾）。

## 2. 替换节点

`sourceElement.replaceChild(targetElement, replacedElement)`，用于替换节点。当替换一个元素时，`replacedElement`所有的关系指针会被`targetElement`复制，也就是被取代了位置。此时`replacedElement`依然在文档（`document`）中，只是没有了自己的位置。

## 3. 移除节点

`sourceElement.removeChild(removedChild)`，接受的参数为要从`childNodes`中移除的元素。与`replaceChild`一样，元素移除后，依然在文档中，只是没有了自己的位置。

## 4. 克隆节点

`cloneNode()`，御用克隆一个节点，它接受一个布尔值参数

- 参数为`true`，执行深复制，复制节点和其整个节点树。
- 参数为`false`，执行浅复制，只复制节点本身。复制后的节点归文档所有，但并没有为其指定父节点。可以称为‘孤儿’，除非用上面1、2提到的方法将其添加到文档中。

# DOM扩展——选择符API

## `querySelector()`

其接受一个CSS选择符作为参数，返回匹配的第一个元素，如果没有匹配则返回null。

```js
let body = document.querySelector("body");

let myDic = document.querySelector("#myDiv");
```

## `querySelectorAll()`

与`querySelector()`类似，不同的是该方法返回的是符合匹配原则的`NodeList`实例。
