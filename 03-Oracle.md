## 连字符

```SQL
# 创建一个表 
create table employess(lastname varchar2(20), firstname varchar2(20));

# 插入数据
insert into employess values('zhang', 'san');

# 进行连字符连接
select lastname || firstname from employess;

# 输出结果
LASTNAME||FIRSTNAME
----------------------------------------
zhangsan
```

## 文字字符串

```SQL
SELECT LASTNAME || ' IS A ' || FIRSTNAME AS NAME FROM EMPLOYESS;

NAME
---------------
zhang IS A san
```

## 去重 distinct

```SQL
select distinct lastname from employess;
```

## 设置别名

```SQL
SELECT  column_name AS new_name, ... FROM table_name
```



## SLECT

- 基本查询

```SQL
SELECT [DISTINCT] column_name1,... FROM [WHERE conditions]
```

### 设置格式

```SQL
# 设置字段显示名字
COLUMN column_name HEADING new_name

col username heading 用户名;

# 设置字符型字段长度
COLUMN column_name FORMAT dataformat

# 设置 username 长度为10
col username format a10;

# 9 代表数字 下面代表工资格式时 四位数和一个小数
col salary format 9999。9 # 超过四位会全部显示 #

# 在数字面前加上 $
col salary format $999.9;

# 清除设置的格式
COLUMN column_name CLEAR

COL SALARY CLEAR;
```

###  模糊查询

```SQL
# 查询 username 含有 a 的
SELECT * FROM USER WHERE USERNAME LIKE '%a%'
```

### 范围查询

```SQL
# 查询工资在 100和1200 之间
select * from users where salary between 100 and 1200;

# 查询工资不在 100和1200 之间　
select * from users where salary not between 100 and 1200;

# 查询 username 为 saber 的数据
select * from users where username in ('saber');
```

## ORDER BY

```SQL
SELECT ... FROM ... [WHERE ...] ORDDER BY column1 DESC/ASC;

# 按 id 从大到小排序
select * from users order by id desc;
```

## case...when

```SQ;
CASE column_name WHERE values1 THEN result1, ... [ELSE result] END


select username, case username when 'saber' then 'aaa' when 'lain' then 'bbb' when 'jack' then 'ccc'end as 部门 from users;
```

## decode

```SQL
decode (column_name, value1, result1, ..., defaultvalue)

select username, decode(username, 'saber', '计算机', 'lain', '市场部门', '其他') as 部门 from users;
```

