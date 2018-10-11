---
title: "学习js数据结构与算法（1）"  
tags: 
	- JavaScript
	- 算法
	- 数据
	- 数组
---

### &nbsp;
最近尝试了一下leetcode，尽管愉快的解决了第一题，但是在查看标准解决方案时，涉及了算法方面的解析，因为对于算法理解不深，看下来是一知半解，正好最近下载了《学习JavaScript数据结构与算法》，暂且先放下leetcode，开始学习算法。

## 数组方法
<!-- more -->
### `Array.from`根据已有的数组创建一个新数组
复制numbers数组
```
let numbers2 = Array.from(numbers)
```
传入函数来过滤值
```
let evens = Array.from(numbers, x=>(x%2==0))
```
> 上述代码会创建一个只包含numbers数组中的偶数的数组

### `Array.of`根据传入的参数创建一个新数组
e.g.  
```
let numbers3 = Array.of(1)
let numbers4 = Array.of(1,2,3,4,5,6)
//相当于
let numbers3 = [1]
let numbers4 = [1,2,3,4,5,6]
```
可以用这个方法复制已有的数组
```
let numbersCopy = Array.of(...number4)
//相当于
let numbersCopy = Array.from(number4)
//或
let numbersCopy = [...number4]
```
### `fill`方法用静态值填充数组，多用于创建数组并初始化
```
let ones = Array(6).fill(1)
```
> `ones`为长度6，所有值为1多数组  

可通过指定起始或结束位置的索引，限制填充的范围

### `copyWithin`方法复制数组中的一系列元素到同一数组指定的起始位置
```
let copyArray = [1,2,3,4,5,6]
copyArray.copyWithin(0,3)
//得到copyArray为[4,5,6,4,5,6]
copyArray = [1,2,3,4,5,6] //重置数组
copyArray.copyWithin(1,3,5)
//得到copyArray为[1,4,5,4,5,6]
```
### `indexOf`方法返回与参数匹配的第一个元素索引，`lastIndexOf`返回与参数匹配的最后一个元素索引，如果不存在返回-1
### &nbsp;

### `find`和`findIndex`接收一个回调函数，搜索 ***一个*** 满足回调函数条件的值
> `find`方法返回第一个满足条件的值，`findIndex`方法则返回这个值在数组里的索引。如果没有满足条件的值，`find`会返回undefined，而`findIndex`返回-1  

### &nbsp;

### 如果数组里存在某个元素，`includes`方法会返回true，否则返回false（可指定起始位置）

## &nbsp;
<p style="text-align:right">待续</p>


