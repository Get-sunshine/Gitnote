# 第三章 使用MySQL

## 3.1 连接

MySQL与所有客户机-服务器DBMS一样，要求在能执行命令之前登录到DBMS。

## 3.2 选择数据库

在能执行任意数据库操作之前，需要选择一个数据库。使用use关键字。

关键字（key word）：作为MySQL语言组成部分的一个保留字。

使用crashcourse数据库

```mysql
use crashcourse;
```

## 3.3 了解数据库和表

```mysql
查看所有数据库
show databases;

查看选择数据库中的表
show tables;

查看customers表中的列信息
show columns from customers;
等同于
describe customers;

还有其他show语句
show status; 用于显示广泛的服务器状态信息
show create database 和 show create table 分别用来显示创建特定数据库或表的MySQL语句
show grants 用来显示授予用户的安全权限
show errors 和 show warnings 显示服务器错误和警告消息

```

34 页

