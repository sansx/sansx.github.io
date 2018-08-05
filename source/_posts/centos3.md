---
title: "centos7 安装Node.js + npm + pm2"  
tags: 
	- linux
	- centos7
	- Node.js
	- npm
	- pm2
---

## 从EPEL库安装Node.js
安装EPEL库+安装Node.js
```
sudo yum install epel-release
sudo yum install nodejs
```
<!-- more -->
由于node.js较新版本都自带npm，所以一般情况下运行
```
npm -v
```
便可查看npm版本，如果命令无效则执行指令安装npm
```
sudo yum install npm
```
检查安装版本
```
node -v
npm -v
```

如果发现版本过旧或者不是需要的版本
安装npm中的node.js版本管理模块 **'n'** 下载稳定版本
```
npm install -g n
n stable
```
安装最新版本  
```
n latest
```
安装具体版本,n 加 目标版本,如：
```
n v8.9.0
```

升级npm
```
npm -g install npm@next
```
## 安装pm2
在下载更新好node.js 和 npm 后，安装pm2更是简单
全局安装
```
npm install -g pm2
```
一般情况下安装好之后会有警告，这个在npm安装时是正常的可以无视
查看版本
```
pm2 -v
```
如果系统显示pm2指令不存在，则需要将pm2配置成全局
```
ln -s /usr/local/src/node-v8.9.0-linux-x64/bin/pm2  /usr/local/bin/pm2
```
执行之后便可以全局使用pm2指令