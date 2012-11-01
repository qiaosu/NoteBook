MySQL笔记
=======

###安装配置

1. 下载dmg安装包

2. 安装dmg安装包前，先通过命令行
	<pre><code>> mysqladmin shutdown</code></pre>
关闭所有运行中的MySQL服务器实例。

3. 如果想要系统启动时自启动MySQL，还需要安装MySQL Startup Item。

4. 如果不使用Startup Item，输入命令来启动
	<pre><code>> cd /usr/local/mysql
	\> sudo ./bin/mysqld_safe
	(Enter your password, if necessary) 
	(Press Control-Z) 
	\> bg 
	(Press Control-D or enter "exit" to exit the shell)</code></pre> 

###创建用户，分配权限

1. 使用root帐号登录客户端（或命令行登录）

2. 创建用户：
	<pre><code>insert into mysql.user(Host,User,Password) values("localhost","kenshin",password("123456"));</code></pre>

3. 刷新系统权限
	<pre><code>flush privileges; </code></pre>

4. 授权，仍然使用root帐号登录分配权限，先为用户创建一个数据库：
	<pre><code>create database kenshinDB;</code></pre>
	授权给用户：
	<pre><code>grant all privileges on kenshinDB.* to kenshin@localhost identified by '123456';</code></pre>
	刷新系统权限：
	<pre><code>flush privileges; </code></pre>

###删除用户

<pre><code>DELETE FROM user WHERE User="kenshin" and Host="localhost";
flush privileges;
drop database kenshinDB;  </code></pre>

注：flush privileges的意思是强制刷新内存授权表，否则用的还是缓冲中的口令。

###命令

`建库` create database 库名;

`建表` create table 表名 (字段设定列表)；

`删库` drop database 库名;

`删表` drop table 表名；

`清空表记录` delete from 表名;

`查询表记录` select * from 表名;

###数据类型

* 数值型:

	tinyint # -128~127  0～255 1字节

	smallint # -32768 ~32767 0~65535 2字节
	
	mediumint #           0~16777215 3字节

	int # -2147483048 4字节

	bigint # -922337203685477285808  8字节
	
	float # 4字节
	
	double # 8字节
	
	decimal # 12字节

* 字符型
	
	char

	varchar

	text # mediumtext longtext

	blob # 二进制数据 longblob

* 日期型

	date #yyyy-mm-dd

	time #hh:mm:ss

	datetime #:yyyy-mm-dd hh:mm:ss

	timestamp #yyyymmddhhmmss

	year #yyyy

###字段属性

* unsigned #不用负数
* zerofill #前导0自动补齐
* auto-increment #自动递增
* null (not null) #设定
* default 

###索引

* 创建索引

	create index 索引名 on 表名(字段1,字段2)

	用alter的方法创建索引:

	alter table table_name add index index_name (column_list) ;

	alter table table_name add unique (column_list) ;

	alter table table_name add primary key (column_list) ;

	注1：在建表时也可以加入 index 索引名(字段1,字段2)

	注2：索引是增加了读的速度

* 查看索引

	show keys from 表名;

	show index from 表名;

* 删除索引

	drop index index_name on table_name ;

	alter table table_name drop index index_name ;

	alter table table_name drop primary key ;
