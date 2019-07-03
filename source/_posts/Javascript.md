---
title: Js中splice()于slice()的区别
date: 2017-11-06 23:48:33
tags: Javascript
---



> splice() 方法与 slice() 方法的作用是不同的，splice() 方法会直接对数组进行修改,可以删除元素，也可以插入元素。而slice()是返回已有数组中的选定元素，可以截取字符串。
<!-- more -->

## splice()语法 ##

	array.splice(start)
	array.splice(start, deleteCount) 
	array.splice(start, deleteCount, item1, item2, ...)

**示例**

	var myFish = ["angel", "clown", "mandarin", "surgeon"];

	//从第 2 位开始删除 0 个元素，插入 "drum"
	var removed = myFish.splice(2, 0, "drum");
	//运算后的 myFish:["angel", "clown", "drum", "mandarin", "surgeon"]
	//被删除元素数组：[]，没有元素被删除
	
	//从第 3 位开始删除 1 个元素
	removed = myFish.splice(3, 1);
	//运算后的myFish：["angel", "clown", "drum", "surgeon"]
	//被删除元素数组：["mandarin"]
	
	//从第 2 位开始删除 1 个元素，然后插入 "trumpet"
	removed = myFish.splice(2, 1, "trumpet");
	//运算后的myFish: ["angel", "clown", "trumpet", "surgeon"]
	//被删除元素数组：["drum"]
	
	//从第 0 位开始删除 2 个元素，然后插入 "parrot", "anemone" 和 "blue"
	removed = myFish.splice(0, 2, "parrot", "anemone", "blue");
	//运算后的myFish：["parrot", "anemone", "blue", "trumpet", "surgeon"]
	//被删除元素的数组：["angel", "clown"]
	
	//从第 3 位开始删除 2 个元素
	removed = myFish.splice(3, Number.MAX_VALUE);
	//运算后的myFish: ["parrot", "anemone", "blue"]
	//被删除元素的数组：["trumpet", "surgeon"]

## slice()语法 ##

	array.slice(start, end)

**示例**

>**在数组中读取元素：**

	var fruits = ["Banana", "Orange", "Lemon", "Apple", "Mango"];
	var citrus = fruits.slice(1,3);

>输出：

	Orange,Lemon

>截取字符串

	var str="www.runoob.com!";
	document.write(str.slice(4)+"<br>"); // 从第 5 个字符开始截取到末尾
	document.write(str.slice(4,10)); // 从第 5 个字符开始截取到第10个字符


