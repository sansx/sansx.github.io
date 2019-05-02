---
title: "无法连接到数据库--stackoverflow的解答(二)"  
comments: true  
tags: 
	- mysql
	- stackoverflow
---
## 前言：
Stack Overflow由Jeff Atwood及Joel Spolsky在2008年创立，是一个程序设计领域的问答网站，隶属Stack Exchange Network。其中的问题包涵关于编程几乎的所有领域，解答的质量极高，现将我遇到的问题及解答记录，用作备忘。


#### Q1：[host不被允许连接到此MYSQL服务<br />（Host 'xxx.xx.xxx.xxx' is not allowed to connect to this MySQL server）](https://stackoverflow.com/questions/1559955/host-xxx-xx-xxx-xxx-is-not-allowed-to-connect-to-this-mysql-server)
<!-- more -->
通过  
``` 
mysql -u root -h localhost -p  
```
连接数据库没有问题，但是使用
```
 mysql -u root -h 'any ip address here' -p
```
连接时，却出现以下报错  
`> ERROR 1130 (00000): Host ''xxx.xx.xxx.xxx'' is not allowed to connect to this MySQL server`  

　A1-1:
　或许是因为mysql的安全预防措施。你可以尝试添加新的管理员帐户：

```
mysql> CREATE USER 'monty'@'localhost' IDENTIFIED BY 'some_pass';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'monty'@'localhost' WITH GRANT OPTION;
mysql> CREATE USER 'monty'@'%' IDENTIFIED BY 'some_pass';
mysql> GRANT ALL PRIVILEGES ON *.* TO 'monty'@'%' WITH GRANT OPTION;
mysql> FLUSH PRIVILEGES;
```
　A1-2:
　我的报错信息和你的相似，尽管我使用了root账户，还是出现`Host XXX is not allowed to connect to this MySQL server`。以下是确保root账户如何拥有正确的权限的方法：
　**我的配置**：  
　　-  Ubuntu 14.04 LTS
　　-  MySQL v5.5.37

　 **方法**：
　1. 打开此路径下的文件 'etc/mysql/my.cnf'
　2. 检查：  
　　 - port（默认设置为 'port = 3306'）
　　 - bind-address （默认值为'bind-address = 127.0.0.1';如果你想向所有人开放，注释掉这行即可。 在我的项目中，我将它的值设置为 10.1.1.7）
　3. 现在访问实际服务器上的MySQL数据库（假设连接的远程数据库：IP地址为123.123.123.123，端口为3306，用户为root。记住修改IP地址，端口，用户为你自己的设置）
    ```
    mysql -u root -p
    Enter password: <enter password>
    mysql> GRANT ALL ON *.* to root@'123.123.123.123' IDENTIFIED BY 'put-your-password';
    mysql> FLUSH PRIVILEGES;
    mysql> exit
    ```
　4. 重启你的mysql服务  

##  
#### Q2：[无法连接到远程服务error 61<br />（Can't connect to remote MySQL server with error 61）](https://stackoverflow.com/questions/16161889/cant-connect-to-remote-mysql-server-with-error-61)

当我尝试使用命令`mysql -h <remote-ip> -u any_existing_users -p`或任何其他mysql客户端（如phpmyadmin）连接远程MySQL服务器时，它不起作用，错误提示是`ERROR 2003 (HY000) Can't connect to MySQL server on '<remote-ip>' (61)`。  
但是，当我尝试`ssh <remote-ip>`并通过`mysql -u root -p`在本地连接MySQL时，没有问题。

　A2-1:
　检查你的mysql服务器是否正确运行：  
```
netstat -tulpen
```
　寻找3306字段
　如果没有找到或者只在localhost上，检查`my.cnf`将里面的`bind-address`这行改为：
```
bind-address = 0.0.0.0
```
　之后重启服务，再试一下。

　A2-2:
　在MySql 5.7中，他们更改了文件，因此bind-address现在位于：
```
/etc/mysql/mysql.conf.d/mysqld.cnf
```
　而不是
```
/etc/mysql/my.cnf
```

　A2-3:
　如果您运行MAMP，请不要忘记设置允许访问（在mySQL面板，选中“允许网络访问　MySQL”）

　A2-4:
　在服务器上安装Centos 7后，我遇到了这个问题。我无法通过远程计算机中的　Mysql Workbench访问它。
　问题出在防火墙配置中。最终，解决方案是：
```
sudo firewall-cmd --zone=public --permanent --add-service=mysql
```
　然后重启防火墙：
```
sudo systemctl restart firewalld
```

