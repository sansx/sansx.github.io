---
title: "修改windows7下的docker源"  
comments: true  
tags: 
	- windows
	- docker
---


### &nbsp;
最近折腾docker时发现，win7并不能直接安装[Docker Desktop for Windows](https://hub.docker.com/editions/community/docker-ce-desktop-windows)，根据官方所说win10之前的版本只能通过[Docker Toolbox](https://docs.docker.com/toolbox/overview/)来使用docker。  
安装完，在拉取镜像时，发现下载速度慢如龟爬，显然问题出在docker源上，由于普通的修改源方法并不适合win7，在经历了各种尝试之后，终于成功修改了docker源。


###  1. 启动docker
<!-- more -->
安装好[Docker Toolbox](https://docs.docker.com/toolbox/overview/)之后，桌面多了三个图标。  

![](https://i.loli.net/2019/05/12/5cd83fb59d10d.png)  

第一个软件Oracle VM VirtualBox——著名的虚拟化软件，可以运行各种虚拟机，显然win7下使用的docker就是运行在VirtualBox创建的虚拟机中的。  

第二个Kitematic (Alpha)，是一个Docker GUI 工具，可以方便的下载管理docker镜像。  

第三个Docker Quickstart Terminal，也就是我们接下来要使用的——docker快速启动终端，可以使用一系列docker指令。

打开Docker Quickstart Terminal，在短暂的黑屏初始化之后，你可以看见这样的shell界面。  

![](https://i.loli.net/2019/05/13/5cd8452ff1284.png)  

  

### 2. 进入虚拟机

在终端中按顺序输入：
```
docker-machine ssh  # 连接虚拟机
sudo vi /var/lib/boot2docker/profile  # 通过编辑器打开配置文件
```
> 不要忘记在终端中使用`TAB`键获取指令提示和自动填充

在profile文件的`--label provider=virtualbox`这一行下添加：
```
--registry-mirror=https://docker.mirrors.ustc.edu.cn
```
![](https://i.loli.net/2019/05/13/5cd97b375ad5995856.png)  
> `https://docker.mirrors.ustc.edu.cn`为中科大镜像源，你可以换成你想要使用的源地址。

### 3. 重启虚拟机

1. 修改完文件之后，使用`:wq`指令，保存并退出vim编辑器。
2. 执行`exit`，退出ssh连接。
3. 最后在终端中使用：
```
docker-machine restart  # 重启VM虚拟机
```

### 其他的方法

由于Docker Quickstart Terminal实际上并没有复制粘贴的功能，尽管有`TAB`辅助，但是在修改文件时就显得很麻烦。  

此前说到，其实win7中的docker实际是运行在VirtualBox的虚拟机中的，所以只要可以连接上这台虚拟机，使用什么终端并没有区别，而大多数终端都是可以方便的粘贴信息。  

查询虚拟机的IP很简单，打开VirtualBox的虚拟机实例的运行窗口，输入`ifconfig`，即可获取IP，那我们既然可以直接操作虚拟机了，为什么还要多此一举，通过其他终端连接，答案很简单，VirtualBox提供的操作窗口在没有配置的情况下，使用起来依旧很麻烦。  

回过头来，获取到IP之后，在终端中输入：
```
ssh docker@<docker-machine-ip>  #  <docker-machine-ip>为你的docker实例IP
```
这时又有一个问题出现了，docker用户的密码呢？  

通过求助万能的[stackoverflow](https://stackoverflow.com/questions/32646952/docker-machine-boot2docker-root-password)，可以得知docker用户的密码为：**tcuser**。  

至此，我们可以成功的使用我们自己的终端，通过正常的ssh登录运行docker的虚拟机，之后只要重复教程的操作就行了，有了终端的复制粘贴，相信修改配置的过程可以轻松愉快不少。





