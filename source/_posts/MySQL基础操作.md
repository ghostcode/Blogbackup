title: MySQL基础操作
date: 2015-12-29 09:38:23
categories:
tags:
---
__登陆MySQL__

>mysql -u root -p

回车输入数据库密码。

__显示所有的数据库__

>show databases;


```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
```

<!-- more -->

__创建一个数据库__

>create database test;

此时再查看一下数据库，多了一个test：

```
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| test               |
+--------------------+
```

__删除一个数据库__

>drop database test;

__使用一个数据库__

>use test;

数据库存在则显示`Database changed`，反之：`ERROR 1049 (42000): Unknown database 'xxx'`。

__创建一个表__

>create table users;

这样会报错：`RROR 1113 (42000): A table must have at least 1 column`，意思就是表最少得有一列。

>create table users(name varchar(30));

__删除一个表__

>drop table book;

__显示表结构__

>desc users;

__查询表数据__

>select * from users;

__插入表数据__

>insert into users values('username');

>insert into users(name) values('testname');

__删除表数据__

>delect from users where name='username';

__更新表数据__

>update users set name='newname' where name='username';

__清空表数据__

>truncate table users;

>delect from users;

__退出MySQL__

>exit;