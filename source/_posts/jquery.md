---
title: 前端学习笔记NO.001
date: 2018-04-22
---

### 锋利的jquery

`$().toggle(fn1,fn2,...,fnN)`通过n次点击，按顺序循环执行函数
`$().toggle()`也可用于切换元素显示状态  
  
`$().trigger('click')=$().click()`触发绑定的事件函数，使触发元素获得焦点（浏览器默认操作）。

`$().triggerHandler('click')`仅触发事件函数

---
* jquery中使用`stopPropagation()`阻止事件冒泡;
* `event.preventDefault()`阻止默认行为（表单提交、链接跳转）;
* js中`return false`;
<!-- more -->
`event.target()`获取触发事件的元素

mouseover 和 mouseout 所发生的元素可以通过`event.target`访问，相关元素通过`event.telatedTarget()`访问(触发事件所经过的节点)

`event.pageX()/event.pageY()`获取光标到页面的X,Y坐标，相当于`ev.clientX+document.body.scrollLeft/ev.clientY+document.body.scrollTop`非IE

绑定事件`$().bind('click',fn1).bind('click',fn2)`,类似js中`addEventListener()`;
>可以绑定自定义事件`$().bind('myClick',fn)`,通过`$().trigger('myClick')`触发

移除事件`$().unbind()`;

只执行一次`$().one()`;

### jquery动画
>动画要求在标准模式下，否则会引起画面抖动  

`$('element').hide()`相当于`element.css('display','none')`隐藏元素
`$('element').show()`显示元素  
通过添加参数产生动画效果`$('element').hide(100)`,元素在100ms（毫秒）内隐藏  
`$('element').fadeIn()/$('element').fadeOut()`改变元素透明度
`$('element').slideUP()/$('element').slideDown()`改变元素高度

##### 自定义动画
`$().animate({left:'50px',height:'50px'},1000)`右滑的同时改变高度  
`$().animate({left:'50px'},1000).animate({height:'50px'},1000)`**先**右滑**之后**改变高度  
动画回调函数，在动画执行完后执行`$().animate({},time,fn)`  
######　其他交互方法
1. `toggle(speed,[callback])`
2. `slideToggle(speed,[callbakc])`
3. `fadeTo(speed,opacity,[callback])`  

------------
动画实例：[点击查看](http://xthtx.site/jqdemo1.html)

-----

![流水](http://ws3.sinaimg.cn/large/005uFoOQly1fiib9v3079g30dw07thag.gif)
