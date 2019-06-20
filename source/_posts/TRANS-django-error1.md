---
title: "[译]本地项目（django）连接数据库（postgres）出现问题"  
date: 2019-05-18
comments: true  
tags: 
	- 翻译
	- databases 
	- docker
---
&nbsp;  
---

> * 原文地址：[Local project has problems connecting to its database](http://support.divio.com/local-development/troubleshooting/local-project-has-problems-connecting-to-its-database)
> *  原文作者：Daniele Procida
> * 译者：[sansx](https://github.com/sansx)

> 此文章来自[Divio](https://www.divio.com/)，为保证文章的通用性会，删除一些相关部分。
> 此文章的项目都是运行在docker下，容器基于docker-compose创建。

<!-- more -->

有时，本地项目会无法启动。你可能会见到：
```
This page isn’t working 127.0.0.1 didn’t send any data.
ERR_EMPTY_RESPONSE
```
在你的浏览器，或者：
```
django.db.utils.OperationalError: could not connect to server: Connection refused
Is the server running on host "postgres" (172.20.0.2) and accepting TCP/IP connections on port 5432?
```
在控制台或日志中。  

这基本就是表明 Django的程序已经运行，但是无法连接数据库。通常来说，这是因为尽管web容器（web container）和数据库容器（database container）是同时启动的，数据库开启会更慢一些，所以当Django尝试连接数据库时，他什么也找不到（因为数据库还在开启中）。  

解决办法是在保持数据库运行的情况下，重启Django。  

试试下面几步：
1. 停止web服务`docker-compose stop`。
2. 在你的终端中使用`docker-compose up`开启web服务（这将会使你看到他运行时的控制台输出）。
3. 尝试在浏览器中加载你的站点——你可能会得到数据库连接失败的报错；如果是这样，继续往下走：
4. 找到项目中的`settings.py`文件。
5. 保存文件——你**不**需要做出任何修改，项目中*任何*Python文件的储存都将会使Django程序重启。
6. 在终端中，你将看到程序已经重启。
7. 如果重启后不再有报错信息，试着刷新一下你在浏览器中的站点。

---

翻译后记：

这个问题出现的原因其实就是Django在数据库初始化之前启动了，尽管在docker-compose文件中的`depends_on:`写下了web服务是基于数据库的，但是实际数据库的容器启动后，数据库还需要经过一段时间的初始化才能使用，然而docker并不能准确得知数据库加载好的时间，因此才会产生这种问题。  

顺带一提，我的解决办法：
在`docker-compose stop`之后，其实容器已经创建好了，只是变成了stop状态。  
通过`docker container ls -a`找到你的服务器容器，通过`docker container start <数据库容器ID>`，提前启动数据库，然后再`docker-compose up`即可。

回头看一下这篇文章的 技术部分也不是很多，主要还是练手，学习一下翻译文章的技巧之类的，以后可能会选择某一系列的文章进行翻译。





