---
title: Js学习笔记之行为委托
date: 2018-11-23 14:23:29
tags: [Javascript]
---

# 什么是行为委托

行为委托是一种不同于类的设计模式

<!-- more -->

下面这段是类的设计模式

```js
class Task {
  id;

  /** 构造函数 Task()*/
  Task(ID) { id = ID; }
  outputTask() { output( id ); }
}
class XYZ inherits Task {
  label;

  /** 构造函数 XYZ()*/
  XYZ(ID,Label) { super( ID ); label = Label; }
  outputTask() { super(); output( label ); }
}
class ABC inherits Task {
  // ...
}
```

```js
Task = {
  setID: function(ID) {
    this.id = ID
  },
  outputID: function() {
    console.log(this.id)
  }
}

/** 让 XYZ 委托 Task*/
XYZ = Object.create(Task)
XYZ.prepareTask = function(ID, Label) {
  this.setID(ID)
  this.label = Label
}
XYZ.outputTaskDetails = function() {
  this.outputID()
  console.log(this.label)
}
// ABC = Object.create( Task );
// ABC ... = ...
```

在这段代码中， Task 和 XYZ 并不是类（或者函数），它们是对象。 XYZ 通过 `Object.create(..)` 创建，它的 `[[Prototype]]` 委托了 Task 对象

从代码中可以看出，行为委托的设计模式与类的“父子对象”不同，使用的是一种“兄弟对象”

相比于类的设计模式：

1. 在上面的代码中， `id` 和 `label` 数据成员都是直接存储在 XYZ 上（而不是 Task ）。通常来说，在 `[[Prototype]]` 委托中最好把状态保存在委托者（ XYZ 、 ABC ）而不是委托目标（ Task ）上

2. 在类设计模式中，我们故意让父类（ Task ）和子类（ XYZ ）中都有 `outputTask` 方法，这样就可以利用重写（多态）的优势。在委托行为中则恰好相反：我们会尽量避免在`[[Prototype]]` 链的不同级别中使用相同的命名，否则就需要使用笨拙并且脆弱的语法来消除引用歧义.

   这个设计模式要求尽量少使用容易被重写的通用方法名，提倡使用更有描述性的方法名，尤其是要写清相应对象行为的类型。这样做实际上可以创建出更容易理解和维护的代码，因为方法名（不仅在定义的位置，而是贯穿整个代码）更加清晰

3. `this.setID(ID)` ； XYZ 中的方法首先会寻找 XYZ 自身是否有 `setID(..)` ，但是 XYZ 中并没有这个方法名，因此会通过 `[[Prototype]]` 委托关联到 Task 继续寻找，这时就可以找到`setID(..)` 方法。此外，由于调用位置触发了 this 的隐式绑定规则（参见第 2 章），因此虽然 `setID(..)` 方法在 Task 中，运行时 `this` 仍然会绑定到 XYZ ，这正是我们想要的。在之后的代码中我们还会看到 `this.outputID()` ，原理相同。

   换句话说，我们和 XYZ 进行交互时可以使用 Task 中的通用方法，因为 XYZ 委托了 Task

委托行为意味着某些对象（ XYZ ）在找不到属性或者方法引用时会把这个请求委托给另一个对象（ Task ）。

这是一种极其强大的设计模式，和父类、子类、继承、多态等概念完全不同。在你的脑海中对象并不是按照父类到子类的关系垂直组织的，而是通过任意方向的委托关联并排组织的。

> 本文参考自：
>
> 书籍：P164，[你不知道的 Javascript 之闭包——上卷](https://book.douban.com/subject/26351021/)。
>
> 附：[你不知道的 Javascript 系列电子书网盘链接](https://pan.baidu.com/s/16SXRXo4jXDO_RZHphWv0oA)， 密码：i8jf
