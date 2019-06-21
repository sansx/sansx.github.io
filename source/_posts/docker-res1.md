---
title: "近期关于docker的问题"  
comments: true  
date: 2019-06-10
tags: 
	- docker
---


### &nbsp;

由于docker对win7的支持不是那么友好，有问题的地方不少，有时甚至不得不使用"特殊"的方法使用和操作docker容器。以下是几篇对我有帮助的文章，稍有修改。点击标题或者原文地址可查看原文。

###   启动docker
<!-- more -->
## [Docker容器使用问题：Failed to get D-Bus connection: Operation not permitted](https://forum.huawei.com/enterprise/zh/thread-427747.html)
>原文地址：[https://forum.huawei.com/enterprise/zh/thread-427747.html](https://forum.huawei.com/enterprise/zh/thread-427747.html)

Docker的设计理念是在容器里面不运行后台服务，容器本身就是宿主机上的一个独立的主进程，也可以间接的理解为就是容器里运行服务的应用进程。一个容器的生命周期是围绕这个主进程存在的，所以正确的使用容器方法是将里面的服务运行在前台。

再说到systemd，这个套件已经成为主流Linux发行版（比如CentOS7、Ubuntu14+）默认的服务管理，取代了传统的SystemV风格服务管理。systemd维护系统服务程序，它需要特权去会访问Linux内核。而容器并不是一个完整的操作系统，只有一个文件系统，而且默认启动只是普通用户这样的权限访问Linux内核，也就是没有特权，所以自然就用不了！

因此，请遵守容器设计原则，一个容器里运行一个前台服务！

我就想这样运行，难道解决不了吗？

答：可以，以特权模式运行容器。

创建容器：

```
docker run -d -name centos7 --privileged=true centos:7 /usr/sbin/init
```

进入容器：

```
docker exec -it centos7 /bin/bash
```

这样可以使用systemctl启动服务了。

## [使用ssh连接docker容器](http://blog.csdn.net/qq_34021712/article/details/73379851)

>原文地址：[http://blog.csdn.net/qq_34021712/article/details/73379851](http://blog.csdn.net/qq_34021712/article/details/73379851)

#### 第一步：进入一个已经运行的docker容器中

#### 第二步：设置root密码

#### 第三步：安装Openssh

```
yum -y install openssh-server openssh-clients
```

#### 第四步：修改SSH配置文件以下选项，去掉#注释，将四个选项启用：

`$ vi /etc/ssh/sshd_config`
进入文件后修改以下几项：
```
port=22 #开启22端口
RSAAuthentication yes #启用 RSA 认证
PubkeyAuthentication yes #启用公钥私钥配对认证方式
AuthorizedKeysFile .ssh/authorized_keys #公钥文件路径（和上面生成的文件同）
PermitRootLogin yes #root能使用ssh登录
```
重启ssh服务，并设置开机启动：
```
$ service sshd restart
$ chkconfig sshd on
```

重启sh服务时 报错,找不到命令`bash: service: command not found`
解决方法：`yum install initscripts`

然后再次启用 报错`Failed to get D-Bus connection: Operation not permitted`
网上查了一下说是centos7的坑
解决：在启用docker容器时 添加/usr/sbin/init
```
docker run -i -d --net=none --name javadocker3 -v /usr/local/software/:/mnt/software/ 3bee3060bfc8 /usr/sbin/init
```

#### 第六步：测试是否可以通过ssh连接

#### 第七步：保存为一个新的镜像,以后基于该镜像启动,不用再进行此操作了
```
docker commit -m 'base' -a 'wang sai chao' a142e478cbeb  java-base:base
```
其中：

          -m 来指定提交的说明信息，跟我们使用的版本控制工具一样；
    
          -a 可以指定更新的用户信息；之后是用来创建镜像的容器的ID；
    
          最后指定目标镜像的仓库名和 tag 信息。创建成功后会返回这个镜像的 ID 信息。

-----

## [停止、删除所有的docker容器和镜像](https://colobu.com/2018/05/15/Stop-and-remove-all-docker-containers-and-images/)

>原文地址：[https://colobu.com/2018/05/15/Stop-and-remove-all-docker-containers-and-images/](https://colobu.com/2018/05/15/Stop-and-remove-all-docker-containers-and-images/)

#### 列出所有的容器 ID

```
docker ps -aq
```

#### 停止所有的容器

```
docker stop $(docker ps -aq)
```

#### 删除所有的容器

```
docker rm $(docker ps -aq)
```

#### 删除所有的镜像

```
docker rmi $(docker images -q)
```

#### 复制文件

```
docker cp mycontainer:/opt/file.txt /opt/local/
docker cp /opt/local/file.txt mycontainer:/opt/
```
### docker 1.13新增：
清理镜像|容器|网络
```
docker image prune
docker container prune
docker system prune
```

- `docker image prune --force --all`或者`docker image prune -f -a` : 删除所有不使用的镜像
- `docker container prune -f`: 删除所有停止的容器