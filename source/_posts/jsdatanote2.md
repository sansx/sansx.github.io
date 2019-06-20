---
title: "学习js数据结构与算法（2）"  
date: 2018-10-26
tags: 
	- JavaScript
	- 算法
	- 数据
---

### &nbsp;
数组是常用的数据结构，尽管可以在数组的任意位置上删除或添加元素。然而，有时我们还需要一种在添加或删除元素时有更多控制的数据结构。有两种类似于数组但是更为可控的数据类型，称之为栈和队列。

## 栈
>栈是一种遵从后进先出（LIFO）原则的有序集合。新添加的或待删除的元素都保存在栈的同一端，称作栈顶，另一端为栈底。在栈里，新元素都靠近栈顶，旧元素都接近栈底。  

<!-- more -->

#### 创建栈
一个栈类需要有如下几个方法：
+ `push(element(s))`：添加一个（或几个）新元素到栈顶。
+ `pop()`：移除栈顶的元素，同时返回被移除的元素。
+ `peek()`：返回栈顶的元素，不对栈做任何修改。
+ `isEmpty()`：如果栈里没有任何元素就返回`true`，否则返回`false`。
+ `clear()`：移除栈里的所有元素。
+ `size()`：返回栈里的元素个数。与数组的`length`属性类似。

>尽管ES6引入了类的语法，由于js本身并没有私有变量这个概念，所以创建的栈相当容易被外部修改。
>
#### 用栈解决问题
##### 1. 进制转换


![图片](http://images3.10qianwan.com/10qianwan/20180919/b_0_201809191439244359.jpg)
>十进制转二进制的方法：使用十进制的数据不断除以2，直到商为0为止，从下往上取余就是对应的二进制。

我们可以将每一次的余数传入栈中，当计算结束时，再将栈内的所有余数按顺序取出，连接为字符串，即为传入参数的二进制形式。

```javascript
/**
 * 
 * @param {Number} decnum 
 * 二进制转化函数
 */
function divideBy2(decnum){
    let remStack = new Stack(),			//之前构造的栈类，含有提到的所有方法。
    rem,
    binaryString = ""
    while(decnum>0){					//当参数大于0时，循环
        rem = Math.floor(decnum % 2)	//求余
        remStack.push(rem)				//将求得的数传入栈中
        decnum = Math.floor(decnum/2)	//将除2后的数向下取整，储存为变量
    }
    while(!remStack.isEmpty()){			//当栈不为空时循环
        binaryString += remStack.pop().toString()  //输出栈顶余数，拼接为二进制
    }
    return binaryString
}
```

> 经过测试后，可以发现这个函数可以正确转化十进制为二进制。
> 而在这个函数的基础之上，我们还可以将这个函数改造为高阶函数，使其可以返回十进制以下的任意进制。

```javascript

function divideByN(num){
    return function(decnum){ //返回一个函数
        let remStack = new Stack(),
        rem,
        binaryString = ""
        while(decnum>0){
            rem = Math.floor(decnum % num) 	//
            remStack.push(rem)
            decnum = Math.floor(decnum/num) //
        }
        while(!remStack.isEmpty()){
            binaryString += remStack.pop().toString()
        }
        return binaryString
    }
}

let newfn = divideByN(2) //返回一个转化为二进制的函数
console.log(newfn(10))
```

> 将上述代码在控制台输出，可以发现结果正确为`10-->1010`，之所以只能转化为十以下的进制是因为，一共只有0～9这10个数字，超过十进制之后需要用大写字母表示10及以上的数字。
>
> 10—A，11—B，12—C，13—D...

##### 2. 平衡括号
在编程过程中，括号必须是平衡的。平衡括号的意思是，每个左括号一定对应着一个右括号，括号内又套着括号。看下面这些个括号组成的平衡表达式：

```
(()()()())

(((())))

(()((())()))
```
对比下面这些不平衡的括号：
```
((((((())

()))

(()()(()
```
正确地区分平衡和不平衡括号，对很多编程语言来说，都是重要的内容。

在js中一共会使用到3种括号：`[]、{}、()`

那么现在有个问题，如何判断一段**括号字符串**的括号是否平衡。

仔细观察一下平衡式的结构特点。当从左到右读入一串括号的时候，最早读到的一个右括号总是与他前面的紧邻的左括号匹配，同样，最后一个右括号要与最先读到的左括号相匹配。即右括号与左括号是反序的，它们从内到外一一匹配，这就给我们启示，可以用栈来解决问题。

![](http://interactivepython.org/courselib/static/pythonds/_images/simpleparcheck.png)
```javascript
function parenthesesChecker(symbols) {
  const stack = new Stack();
  const opens = '([{';
  const closers = ')]}';
  let balanced = true;
  let index = 0;
  let symbol;
  let top;

  while (index < symbols.length && balanced) {
    symbol = symbols[index];
    if (opens.indexOf(symbol) >= 0) {
      stack.push(symbol);
    } else if (stack.isEmpty()) {
      balanced = false;
    } else {
      top = stack.pop();
      if (!(opens.indexOf(top) === closers.indexOf(symbol))) {
        balanced = false;
      }
    }
    index++;
  }
  return balanced && stack.isEmpty();
}
```

这个函数首先创建了`opens`和`closers`这两个变量，将括号拆分为左右两个集合，且位置一一对应，用作之后的对照判断，`balanced`做为是否平衡的判断，以此来作为控制`while`循环条件的一部分。

+ 进入循环后，将传入的括号字符串第`index`个字符取出，做为变量`symbol`的值。

  >index初始值为0，在while循环中累加

+ 首先判断，`symbol`是否存在于`opens`中，如果存在，将`symbol`存入栈中。

+ |-- 如果不存在，表示`symbol`为右半边括号，当栈为空，即栈中没有左半边与之对应，则括号必然不平衡，将`balanced`设为`false`，中止循环。

+ `symbol`为右半边括号，当栈中存有左半部分括号时，将栈顶的括号取出，与`symbol`比较，判断是否对应：

+ |— `opens.indexOf(top) === closers.indexOf(symbol)`为`true`时，继续循环。`index++`

+ |—否则，将`balanced`设为`false`，中止循环。
.
.
.
+ 循环结束后返回结果