---
title: "CoffeeScript 中 switch-when 语句编译后为何是switch (false)"  
comments: true  
date: 2018-07-20
tags: 
	- CoffeeScript

---


### 查阅coffeescript指南时，在[switch条目](http://coffeescript.org/#switch)如此描述
>switch statements can also be used without a control expression, turning them in to a cleaner alternative to if/else chains.  

>switch语句也可以在没有控制表达式的情况下使用,coffeescript会把它们变成一个更<del>简介</del>简洁的if / else链的替代品。
<!-- more -->
```bash
score = 76
grade = switch
  when score < 60 then 'F'
  when score < 70 then 'D'
  when score < 80 then 'C'
  when score < 90 then 'B'
  else 'A'
 # grade == 'C'
```
>经过coffeescript编译后  

```bash
var grade, score;

score = 76;

grade = (function() {
  switch (false) {
    case !(score < 60):
      return 'F';
    case !(score < 70):
      return 'D';
    case !(score < 80):
      return 'C';
    case !(score < 90):
      return 'B';
    default:
      return 'A';
  }
})();

// grade == 'C'
```


　　代码完美运行，但有个问题，为什么编译后的switch要`switch (false)`作为判断条件，而`case`匹配项则全部做了取反操作,我将`switch`部分改为：   
```bash
switch (true) {
    case (score < 60):
      return 'F';
	...
}
```
### 控制台测试结果相同，那么看起来明明是修改之后的代码更加美好，为何coffeesript会选择这样编译  

打开[MDN web技术文档switch](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/switch)语法部分：
>一个 switch 语句首先会计算其 expression 。然后，它将从第一个 case 子句开始直到寻找到一个其表达式值与所输入的 expression 的值所相等的子句（使用 严格运算符，===）并将控制权转给该子句，执行相关语句。

在不知道`case`中值是什么情况下，一旦传入了返回值不是boolean 值的表达式，`switch`将会永远不会匹配上。  

因此，在`switch(true)`时，应该将`case`做处理使其返回boolean值，通过  
显式转换`case Boolean(score < 60):`或  
隐式转换`case !!(score < 60):`  
将匹配项转化为可以正确通过严格运算符判断的值  
那么我们把这个修改之后的`switch(true)`和最开始自动编译的`switch(false)`比较，显然当`switch(true)`时，虽然可读性较高，但是代码量更多，而随着`case`数的增加，两者代码量的差距还会更大。
### 由于这是编译之后的代码，主要目的就是降低代码的体积，虽然第一眼看上去可能会有疑问，但`switch(false)`的简洁使其成为CoffeeScript 编译器的最佳选择。