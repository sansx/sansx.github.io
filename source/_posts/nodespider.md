---
title: "Node爬虫备忘"  
date: 2018-08-05
comments: true  
tags: 
	- node
	- 爬虫
---

## 前言
最近在爬取了某某电影站的3,000多条数据（下载链接）试手之后，发现爬虫结合[nunjucks](https://mozilla.github.io/nunjucks/cn/templating.html)，感觉可以弄一个不错的网站集合，把我常用的网站整合成一个页面，应该能提高不少信息获取的效率。

现在暂时开了一个小头，者期间关于node的好用的工具或是技巧，慢慢整理在此作为备忘。
<!-- more -->
1. 普通的获取页面信息：[superagent](https://visionmedia.github.io/superagent/)----[中文点我](https://cnodejs.org/topic/5378720ed6e2d16149fa16bd)
<small>(中文更新较慢，推荐看官网)</small>

2. 分析页面信息：[cheerio](https://github.com/cheeriojs/cheerio)  
	[通读cheerio API - CNode技术社区](https://cnodejs.org/topic/5203a71844e76d216a727d2e)
	><small>将获取的html信息进行解析，实现了大部分jQuery选择器语法，包括了jQuery核心的子集。</small>

3. 控制并发：async  
	直接看教程就好[使用async控制并发　 via:alsotang](https://github.com/alsotang/node-lessons/tree/master/lesson5)

4. 虚拟浏览器：phantomjs  　[教程](http://javascript.ruanyifeng.com/tool/phantomjs.html)  
	某种意义上的神器

5. 图片存储  
	获取数据



