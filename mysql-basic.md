---
layout: post
title: mysql basic
date: 2022-05-17 08:41:02
tags:
---

## 开启服务

```mysql
# 开启 MySQL 服务
net start mysql80

# 停止 Mysql 服务
net stop mysql80 

# login
mysql -h主机名 -P端口号 -u用户名 -p密码

mysql -uroot(user) -p(password) -P(端口)

# 密码在下一行输入
mysql -uroot(user) -p 接着输出密码

# 登录指定端口数据库
mysql -uroot(user) -p(password) -P(port)

# 指定协议
mysql -u root -P3306 -h(host) 127.0.0.1(localhost) -p 

# 登录成功可以查看版本号
select version();
```

## 服务正在启动或停止中

```mysql
1. 首先以管理员身份打开命令行窗口，注意是管理员身份，不然无权限访问。

2. 输入命令“tasklist| findstr "mysql"”，用于查找mysql的残留进程。果不其然，确实存在mysql的残留进程，难怪一直提示MySQL服务处于正在启动或者停止的状态中，此时要做的就是杀死MySQL进程。

3. 输入命令“taskkill/f /t /im mysqld.exe”，就可以将mysql残留进程全部杀死了，

4. 输入命令“tasklist| findstr "mysql"”，查看是否还留有有其他的mysql残留进程，如果还有，则继续杀死，直到完全杀死进程为止。

5. 当mysql残留进程全部结束之后，我们就可以正常启动MySQL服务了，如下图所示：

需要注意的是此时还是要以管理员的身份进入命令行窗口。
```



## 数据库创建

```mysql
# 查看数据库
show databases; 

# 创建数据库
create database	xxx

# 使用数据库 
use xxx

# 创建table id name
create table employees(id int,name varchar(15));

# 创建内容
insert into employees values(001,'saber');
/* select * from employees; 展示
+------+-------+
| id   | name  |
+------+-------+
|    1 | saber |
+------+-------+
*/

# 以SQL语句形式展示表结构
show create database database_name;
show create table table_name;
```

## 查看字符集设置

```mysql
# 查看字符集
show variables like 'character_%';
 
# 对应字符集规则
show variables like 'collation_%';
```

## SELECT

```mysql
# 通过 * 把 users 表中所有数据查询出来alter
select * from users

# 从 users 表中把 username 和 password 对应数据查询出来
select username, password from users

# 可以给查询的字段起别名
select username as uname, password as pwd from users
```

## INSERT INTO

```mysql
# 插入新的数据行 列的值通过values指定
# 注意 列和值要对应 多个列和多个值直接 使用英文逗号分隔
INSERT INTO table_name (列1, 列2, ...) VALUES (值1, 值2, ...)

insert into users (id, username, password) values (5, 'black', '2333')
```

##　UPDATE

```mysql
# UPDATE 指定更新哪个表数据
# SET 指定列对应的新值
# WHERE 指定更新的条件
UPDATE table_name set 列名称 = 新值 WHERE 列名称 = 某值

# 更新users表 将id为4的用户密码设置为888888
update users set password='888888' where id=4

# 更新users表 将id为2的用户 用户名、密码、状态码更新 注意用英文逗号分开
update users set username='admin', password='666666', status=1 where id = 2
```

## DELETE

```mysql
# 从指定表中 根据WHERE条件 删除对应的数据行
DELETE FROM table_name WHERE 列名称 = 值

# 删除users表中id为5的这行数据
delete from users where id =5
```

## WHERE

- WHERE 字句用于现打选择标准。在SELECT、UPDATE、DELETE, 皆可使用WHERE子句来限定选择标准

```mysql
# 查询语句中的WHERE条件
SELECT 列名称 FROM 表名称 WHERE 列 运算符 值

# 查询users表中id不为1的所有数据
select * from users where id<>1	(id!=1)

# 各被罚下语句中的WHERE条件
UPDATE 表名称 SET 列=新值 WHERE 列 运算符 值

# 删除语句中WHERE条件
DELETE FROM 表名称 WHERE 列 运算符 值
```

## 运算符

```mysql
<> 不等于 也可以写成 !=

其余与JS无异

BETWEEN 在某个范围
LIKE 搜索某种模式
```

## AND & OR

```mysql
# 查询users表 状态为0并且id不为2的所有数据
select * from users where status=0 and id <> 2

# 查询users表 状态为1或者id不为1的所有数据
select * from users where status=1 or id<>1
```

## OEDER BY 排序

```mysql
# ORDER BY 默认进行升序 ASC代表生效
SELECT * FROM table_name ORDER BY 字段名 ASC|DESC

# 查询users表 将id字段名默认进行升序排序
select * from users order by id  

# 查询users表 将id字段名进行降序排序
select * from users order by id desc
```

## OEDER BY 多重排序

```mysql
// 查询users表 将status降序排序基础上再对username进行升序排序
select * from users order by status desc, username
```

## COUNT(*)

- [“select count (1)”是什么意思？_AntPorter的博客-CSDN博客_count(1)](https://blog.csdn.net/qianzhitu/article/details/107713362#:~:text=select count,(*)返回所有满足条件的记录数，此时同select sum (1))

```mysql
// 查询users表中 状态为0的总数
select count(*) from users where status=0

// 给返回的字段起别名
select count(*) as total from users where id <> 1
```

## DRAP

```mysql
drop database xxx
```

## 命令行操作sql乱码问题

```mysql 
mysql> insert into users values(1, '樱岛麻衣');
ERROR xxx (HY000): Incorrect string value: '\xC4\xD0' for column 'name' at row 1

# 
[mysql]
default-character-set=utf8

[mysqld]
character-set-server=utf8
collation-server=utf8_general_ci
```

