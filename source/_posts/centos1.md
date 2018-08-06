---
title: "centos7 FTP服务安装配置"  
tags: 
	- linux
	- centos7
---

最近因为要测试爬虫所以要将node页面上传，在几经周折之后终于可以成功将本地文件上传到VPS上。
文章参考：[云库网](http://yunkus.com/centos7-ftp-service-install-config/)
<!-- more -->
1. 首先安装vsftp  
  ```
  $ yum install vsftpd
  ```
2. 进行配置  
  ```
  $ vi /etc/vsftpd/vsftpd.conf  
  ```
  >有的VPS要先安装vim编辑器  
  >`$ yum install vim`  

    把文件中的`anonymous_enable=YES`改为`anonymous_enable=NO`之后保存退出  
  此举表示不允许匿名登录FTP  

3.  启动 vsftp 服务  
  ```
  $ systemctl start vsftpd.service
  ```

  设置服务器重启自动启动服务  
  ```
  $ systemctl enable vsftpd.service  

  ```
4.   创建FTP用户
  ```
  $ useradd -d 用户目录 -s /sbin/nologin 用户名
  ```
  > -s：禁止此用户登录SSH的权限  
  > /sbin/nologin：不允许此用户登录系统，但可以登录FTP	

5.   设置用户密码  
  ```
  $ passwd 用户名
  ```

6.  检查21端口是否开启  
  ```  
  $ firewall-cmd --query-port=21/tcp
  ```
  如果返回NO，执行下方指令  
  ```  
  $ firewall-cmd --add-port=21/tcp
  ```

设置永久开启  
```  
firewall-cmd --get-active-zones
firewall-cmd --zone=public --add-port=21/tcp --permanent  
firewall-cmd --reload
```

 >检查新的防火墙规则  
 >`$ firewall-cmd --list-all`


### 期间问题


##### 无法删除用户
在`userdel -r USERNAME`时，提示此用户在XX端口运行，`kill -9`无法彻底关闭时 
分别输入`vipw`和`vipw -s`,找到之前创建的用户，dd删除保存即可 
