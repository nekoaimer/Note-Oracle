---
layout: post
title: mysql basic
date: 2022-05-17 08:41:02
tags:
---

## SOURCE

```mysql
# 通过 cmd 命令 导入资源
source E:\资料\atguigudb.sql 
```

```mysql
# 去重
SELECT DISTINCT field FROM table
```

## IFNULL

```mysql
# 如果 commission_pct 为null, 则按0来算
SELECT employee_id, salary "月工资", salary * (1 + IFNULL(commission_pct, 0)) * 12 "年工资" , commission_pct FROM employees;
```

## 着重号

```mysql
# 如果名字和关键字或保留字重复了, 可以用着重号表示
SELECT * FROM　`ORDER`
```

## 查询常数

```mysql
SELECT '常数', employee_id FROM employees
```

## 显示表结构

```mysql
// 下面两种都可以查看表结构
DESCRIBE employees

DESC employees
```

## 运算发

```mysql
# + - * / 正常

# 除法 / 也可以使用 div 代表 (division)
# 区别在于 div 是整除 没有小数点的结果
SELECT 100 / 3.222; # 31.0366
SELECT 100 div 3.222; # 31

# 取模 % 也可以使用 mod 代表 (modulo)
```

## 匹配为值null的数据

```mysql
SELECT * FROM employees WHERE IFNULL(commission_pct,0) = 0;
SELECT * FROM employees WHERE commission_pct <=> NULL;
SELECT * FROM employees WHERE commission_pct IS NULL;
```

## ESCAPE

```mysql
# 定义转义字符 $ 那么 '_$_a%' 与 '_\_a%' 结果一样

SELECT last_name FROM employees WHERE last_name LIKE '_$_a%'  ESCAPE '$'
```

