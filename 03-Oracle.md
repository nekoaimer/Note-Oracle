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

## SELECT

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
col salary format 9999.9 # 超过四位会全部显示 #

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

# 正则查询
SELECT * FROM 表名 WHERE REGEXP_LIKE(字段名, '正则表达式') ;

SELECT * FROM users WHERE REGEXP_LIKE(name, '^a');
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

## RANK 分析函数

### 建个表

```SQL
create table users(id number, name varchar2(20), salary varchar2(20));

insert into users values(01, 'saber', 2333);
insert into users values(02, 'lain', 2333);
insert into users values(03, 'aimer', 2333);
insert into users values(04, 'jack', 2333);
insert into users values(04, 'anny', 2333);
insert into users values(05, 'bob', 2333);

COL RANKID FORMAT 9999;
```

### RANK

- 值相同 排名相同 序号跳跃

```sql
SELECT RANK() OVER(ORDER BY id) AS RANK, u.* FROM users u;

 RANK    ID NAME       SALARY
----- ----- ---------- --------
    1     1 saber      2333
    2     2 lain       2333
    3     3 aimer      2333
    4     4 jack       2333
    4     4 anny       2333
    6     5 race       2333
```

### DENCE_RANK

- 值相同 排名相同 序号连续

```SQL
 SELECT DENSE_RANK() OVER(ORDER BY id) AS RANKID, u.* FROM users u;
 
RANKID    ID NAME       SALARY
------ ----- ---------- --------
     1     1 saber      2333
     2     2 lain       2333
     3     3 aimer      2333
     4     4 jack       2333
     4     4 anny       2333
     5     5 race       2333
```

### ROW_RANK

- 序号连续 不管值是否相同

```SQL
SELECT ROW_NUMBER() OVER(ORDER BY id) AS RANKID, u.* FROM users u;

RANKID    ID NAME       SALARY
------ ----- ---------- --------
     1     1 saber      2333
     2     2 lain       2333
     3     3 aimer      2333
     4     4 jack       2333
     5     4 anny       2333
     6     5 race       2333
```

### 分页效果

```SQL
SELECT * FROM (SELECT ROW_NUMBER() OVER(ORDER BY ID) AS RANKID, U.* FROM USERS U) WHERE RANKID >= 1 AND RANKID <= 3;

RANKID    ID NAME       SALARY
------ ----- ---------- --------
     1     1 saber      2333
     2     2 lain       2333
     3     3 aimer      2333
```

 ## 索引

### 普通索引

```SQL
CREATE INDEX 索引名称 ON 表名(列名);

# 根据 TEST 表名称搜索业主信息 基于 name 字段建立索引
CREATE INDEX INDEX_TEST_NAME ON TEST(NAME) 
```

### 唯一索引

```SQL
CREATE UNIQUE INDEX 索引名称 ON 表名(列名);

CREATE UNIQUE INDEX INDEX__TEST_ID ON TEST(ID);
```

### 复合索引

```SQL
CREATE INDEX 索引名称 ON 表名(列名, 列名);

CREATE INDEX INDEX_TEST_AH ON TEST (ID, NAME);
```

### 反向键索引

```SQL
# 举个反向键索引例子
10进制   2进制   反向键   10进制
1		0001    1000	8	
2		0010	0100    4
3		0011	1100    12
4		0100	0010	2
5		0101	1010	10
6		0110	0110	6
7		0111	1110	14

CREATE INDEX 索引名称 ON 表名(列名) REVERSE;
```

### 位图索引

- 使用场景：位图索引适合创建在低基数列上
- 位图索引不直接存储`ROWID`，而是存储字节位到`ROWID`的映射
- 优点：减少响应时间，节省空间占用

```SQL
CREATE BITMAP INDEX 索引名称 ON 表名(列名);

# 在 TEST 表的 TYPEID 列上建立位图索引
CREATE BITMAP INDEX INDEX_TEST_TYPEID ON TEST(TYPEID)
```

