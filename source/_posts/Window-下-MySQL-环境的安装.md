---
title: Window 下 MySQL 环境的安装
date: 2018-06-30 11:52:10
tags:
description: MySQL 是最流行的关系型数据库管理系统，在WEB应用方面 MySQL 是最好的RDBMS(Relational Database Management System：关系数据库管理系统)应用软件之一。
---
### 简介：
MySQL 是最流行的关系型数据库管理系统，在WEB应用方面 MySQL 是最好的RDBMS(Relational Database Management System：关系数据库管理系统)应用软件之一。
我选择它的几点原因：
1. 体积小，速度快
2. 免费，源代码开源，跨平台
3. 用户量大，遇到问题容易找到解决方案

### 下载：
可在其官网下载所需版本，注意有的高级版本是收费的，但对于多数用户来说免费版本就够用了，推荐：[MySQL Community Server 5.6](https://dev.mysql.com/downloads/mysql/)，可下载免安装包，解压后免去了安装程序。

### 安装： 
1. 解压 zip 包后，进入此目录 D:\MySQL\mysql-5.6.40-winx64\bin 下的 cmd 命令行工具（根据你安装位置自己调整）
2. 安装 MySQL 服务，执行 mysqld install
3. 登录 MySQL，首次登录按照默认设置是 root 账户，是没有密码的，所以直接回车就可以进入了。
D:\MySQL\mysql-5.6.40-winx64\bin > mysql -u root -p
4. 查询用户密码，mysql> select host,user,authentication_string from mysql.user;
密码是加密过的，看不到明文，这是一串字符，但是能看到用户名
5. 设置或者修改用户密码，mysql> update mysql.user set authentication_string=password("123456") where user="root";
root 是用户名，可根据当前用户更改
6. 修改 root 用户名：
mysql> use mysql;
mysql> update user set user='xly' where user='root';
7. 退出 MySQL, mysql> quit

### 其他:
1. 环境变量
2. 生成 data 文件，mysqld --initialize-insecure --user=mysql  在D:\MySQL\mysql-5.6.40-winx64 目录下生成data目录

### 参考链接：
[Windows下MySQL下载安装、配置与使用](https://www.cnblogs.com/dtting/p/7691202.html)
[WINDOWS下安装MYSQL—图文详解](https://www.cnblogs.com/reyinever/p/8551977.html)