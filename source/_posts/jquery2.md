---
title: 前端学习笔记NO.002
---
### 锋利的jquery

**JS中Ajax的应用***  

1. 实例化对象`var demo = new XMLHttpRequest();`  
2. 通过`open()`初始化对象`demo.send("GET/POST",URL,是否异步);`  
3. 当readyState值被改变时，会触发readystatechange事件，可通过onreadystatechange属性注册该回调函数事件处理器：
`demo.onreadystatechange=callback;//设置回调函数`  
<!-- more -->

4. 使用`send()`发送请求，如果是通过http的GET方法，可以不指定参数或使用null参数调用`send()`：`demo.send(null);`  
 
>处理事件前应先检查[readystate的值](http://www.blogjava.net/hulizhong/archive/2009/05/04/268846.html)和http的状态。一般情况下是在判断请求完成加载（readyState值为4）并且响应已经成功（HTTP状态值为200）时，可调用函数处理响应内容  

#### jQuery中的Ajax
常用方法  
1. `load(url [,data] [,callback])`载入远程HTML代码并插入DOM中，无参数为GET，否则为POST（会产生跨域问题）  
2. `$.get( url [,data] [,callback] [,type])`，回调函数只有两个参数，返回内容和请求状态  
3. `$.post()`操作方法与`$.get()`相同，区别在于post传递的数据量比get大得多，后者通常不能大于2KB，还有post安全性相对较高，之后就是后端获取可选择的方式不同  
4. `$.getScript()`可动态调用JS文件，类似`$("<script type="text/javascript" src='jquery.min.js'>").appendTo("head");`不同之处在于可使用回调函数，在文件加载完之后进行后续操作
5. `$.getJSON( URL , callback(data))`，通过`$.each()`方法遍历数据  
6. `$.ajax`比较复杂，具体查看[官方网站](http://www.jquery123.com/category/ajax/)


----
![](http://wx4.sinaimg.cn/large/a856d76cgy1fhimze6jbug20f00kux6p.gif)




  










