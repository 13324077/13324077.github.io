---
layout: post
title: MySQL安装以及相关操作
date: 2018-09-09T12:27:30.000Z
category:
  - 数据库
keywords: 'python, mysql'
tags:
  - MySQL
---

# 什么是数据库

就是按照数据结构组织，存储和管理数据的仓库，目前主要的数据库有：

- sqlserver
- mysql
- Oracle
- SQLite
- Access 等等

以下主要介绍MySQL的安装以及相关操作

# MySQL安装

mySQL是一种开放源代码的关系型数据库，私用结构化查询语言(SQL)对数据库进行管理。

在Ubuntu系统下，通过如下命令进行安装

> sudo apt-get install mysql-server mysql-client

通过如下命令查看是否安装成功

wxer@wxer:~$ sudo netstat -tap | grep mysql

tcp 0 0 localhost:mysql 0.0.0.0:* LISTEN 14889/mysqld

由于MySQL 5.7版本安装时没有设置root密码的选项，之前的版本是有一个引导界面来设置root密码的，但新版本貌似去掉了。 因此，在安装完MySQL之后，需要设置mysql root用户的初始密码，方法如下：

首先，运行如下命令

> sudo mysql_secure_installation

按照提示，一步一步操作

修改完密码后，对于root用户需要通过sudo进行访问。不然会报如下错误

```
wxer@wxer:~$ mysql -uroot -p
Enter password:
ERROR 1698 (28000): Access denied for user 'root'@'localhost'
```

# MySQL相关操作

## 新建用户

创建wxer用户，密码:12345678, 命令如下:

**创建本地登录用户**

> CREATE USER 'wxer'@'localhost' identified by '12345678'

**创建远程登录用户**

> CREATE USER 'wxer'@'%' identified by '12345678'

如下命令测试是否创建成功

首先通过quit，退出root用户

> mysql> quit;

```
> Bye
```

然后登录新增的wxer用户，命令如下

> mysql -u wxer -p

## 授权

授权的命令如下

> GRANT privileges ON databasename.tablename TO 'username'@'host'

其中

**privileges** - 用户的操作权限，如SELECT，INSERT，UPDATE等, **注:** 若要授予所有权限则使用 **ALL**。

**databasename** - 数据库名

**tablename** - 表名

**注:** 若要授权该用户对所有数据库和表的相应操作，

```
GRANT SELECT, INSERT ON test.user TO 'wxer'@'%';

GRANT ALL ON *.* TO 'wxer'@'localhost';
```

## 创建数据库

> CREATE DATABASE [IF NOT EXISTS] db_name

## 删除数据库

> DROP DATABASE [IF EXISTS] db_name

## 选择数据库

> USE db_name

## 创建表

> CREATE TABLE [IF NOT EXISTS] table_name(column_list)engine=table_list

## 删除表

> DROP TABLE table_name

## 插入

> INSERT INTO users VALUES(1, 'Tina'), (2, 'Tom'), (3, 'Tony'), (4, 'Kate')

## 查看表内容

> desc table_name;

# 参考

1. [ubuntu 18.04 为 mysql 设置 root 初始密码](https://www.sunzhongwei.com/set-mysql-root-password-on-ubuntu-1804?from=sidebar_new)

2. [MySql 5.7中添加用户,新建数据库,用户授权,删除用户,修改密码](https://blog.csdn.net/w690333243/article/details/76576952)

3. [MySQL创建用户与授权](https://www.cnblogs.com/zeroone/articles/2298942.html)

4. [Linux下使用Python操作MySQL数据库](https://www.cnblogs.com/lxt287994374/p/3910509.html)

5. [数据库之MySql](http://www.cnblogs.com/xiaobingqianrui/p/8454922.html)
