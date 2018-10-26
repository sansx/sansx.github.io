---
title: "table样式问题--stackoverflow的解答(一)"  
comments: true  
tags: 
	- css
	- stackoverflow
---
## 前言：
Stack Overflow由Jeff Atwood及Joel Spolsky在2008年创立，是一个程序设计领域的问答网站，隶属Stack Exchange Network。其中的问题包涵关于编程几乎的所有领域，解答的质量极高，现将我遇到的问题及解答记录，用作备忘。


#### Q1：[如何添加margin到table中的&lt;tr&gt;？](https://stackoverflow.com/questions/10690299/how-to-add-a-margin-to-a-table-row-tr)
<!-- more -->
因为table中的row无法拥有margin值，因此有以下替代方法。  
　A1:在需要添加margin的&lt;tr&gt;前后加入&lt;tr&gt;；<96票> 
 
　A2:为里面的&lt;td&gt;增加padding值；<42票>  

　A3:设置&lt;table&gt;的相邻单元格的边框间的距离（仅用于“边框分离”模式）。<26票>

	table {
  		border-collapse:separate; 
  		border-spacing: 0 1em;
	}

　A4:为&lt;tr&gt;增加行高;<17票>

	tr{
    	line-height:30px;
	}

　A5:为&lt;tr&gt;添加透明的上下边框(border);<16票>

	tr {
    	border-top: 10px solid;
    	border-bottom: 10px solid;
    	border-color: transparent;
	}  
>tip:向table添加`border-collapse: collapse;`才可以渲染边框；  
  
  

#### 近期小问题：
如何消去inline-block元素间间隙：  
1. 去除换行  
2. 设置`font-size:0;`


----
![](https://cl.ly/2k300h2Z0g0J/j1.gif);