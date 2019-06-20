----
title: MYSQL数据库基础操作
date: 2018-06-20
----
## &nbsp;
>数据库虽然时常有接触，但是细节部分容易忘记，因此将基础部分重新写一遍，一是复习，二是以后可以直接查看，一切从简。(指令为windows环境下的dos)


#### *服务器操作
开启mysql服务器:  
`net start mysql`  
停止mysql:  
`net stop mysql`   
连接服务器:`\>mysql -u(用户名) -h(服务器地址) -p(密码)`
>tip:开发环境下服务器地址可以跳过，默认为'127.0.0.1'  
>**-p后面直接接上密码，不能出现空格**
<!-- more -->
断开服务器：`mysql>quit;`

#### 数据库操作
创建数据库：  `CREATE DATABASE 数据库名`  
查看数据库：  `SHOW DATABASES`  
选择数据库：  `USE 数据库名`  
删除数据库：  `DROP DATABASE 数据库名`

#### 数据表操作
创建数据表：   
	CREATE 　
.*---*-[TEMPORARY] 　TABLE 　[IF NOT EXISTS] 　数据表名
	[(create_definition,...)][table_options][select_statement];

>[TEMPORARY]关键字表示创建一个临时数据表  
>数据类型介绍－>[点我查看](http://blog.csdn.net/bzhxuexi/article/details/43700435)；

查看数据表结构  
`SHOW [FULL]COLUMNS FROM 数据表名 [FROM 数据库名]`;  
或`SHOW [FULL]COLUMNS FROM 数据表名.数据库名`；  
或`DESCRIBE 数据表名`（可简写为`DESC 数据表名`）

修改表结构：  
`ALTER[IGNORE] TABLE 数据表名 alter_spec[,alter_spec]...`；  
[详情&教程](http://www.runoob.com/mysql/mysql-alter.html)；


重命名表：  
1.  `ALTER TABLE 旧表名 REMANE TO 新表名`；  
2.  `RENAME TABLE 旧表名 TO 新表名`；

*删除表：  
`DROP TABLE [IF EXISTS] 数据表名;`

#### MYSQL语句操作  
插入记录：  
`INSERT INTO 数据表名(column_name,...) VALUES (values1,...);`

修改记录：  
`UPDATE  数据表名 SET column_name1 = new_value1,column_name2 = new_value2,...;`

删除记录：  
`DELETE FROM 数据表名 WHERE condition`

查询记录：  
最简单版本`SELECT * FROM 数据表名；`  
具体查看->[这里](http://www.runoob.com/mysql/mysql-select-query.html);

------
![](https://cl.ly/3k0g3d253o2F/j4.gif);