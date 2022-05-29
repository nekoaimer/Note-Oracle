## 清空/删除表

```sql
# 清除表中全部数据
TRUNCATE TABLE TABLE_NAME
# 清除users表数据
TRUNCATE TABLE users;

# 删除表
DROP TABLE TABLE_NAME

# 删除 users 表 
DELETE TABLE users;
```

## INSERT TO

```SQL
# 添加指定字段的值
INSERT INTO TABLENAME (field...) VALUES(value...)

# 添加所有字段值
INSERT INTO TABLENAME VALUES(value, value...)

# 添加默认值 name 默认值是 Admin
CREATE TABLE TABLENAME (id number, name varchar2(10) DEFAULT 'Admin')
```

## COPY / INSERT TABLE

```SQL
# 复制表
CREATE TABLE TABLE_NEW AS SELECT column, ... | * FROM TABLE_OLD;

# 复制 users表 到 newusers表
CREATE TABLE NEWUSERS AS SELECT * FROM USERS;

# 添加时复制表
INSERT INTO TABLE_NEW [(column1, ...)] SELECT column1, ...| * FROM TABLE_OLD

# 插入所有字段数据 注意两个表字段都必须一一对应
insert into newusers select * from users;

# 插入指定字段数据
insert into newusers (id) select  id from users;
```

## UPDATE 

```SQL
# 语法
UPDATE TABLE_NAME SET column=value1, ... [WHERE conditions]

# 将所有字段 name 的值 更改为 saber
UPDATE users SET name = 'saber';

# 将满足条件的字段 name 值更改为 lain
UPDATE users SET name = 'lain' WHERE ID = 2
```

## DELETE

```SQL
# 删除所有数据
DELETE FROM TABLE_NAME

# 删除条件数据
DELETE FROM TABLE_NAME [WHERE conditions]
# 删除 id 为 3 的这条数据
DELETE  FROM TEST WHERE ID = 3;
```

## 非空约束

- `CONSTRAINT`指定约束字段名， 后面	跟上 约束类型

### 创建表时添加非空约束

```SQL
# 语法
CREATE TABLE TABLE_NAME (column_name datetype  NOT NULL, ...);

# 创建表 username和userpwd不能为空
CREATE TABLE users (id NUMBER(6),username VARCHAR2(20) NOT NULL, userpwd VARCHAR2(20) NOT NULL);
```

### 修改表时添加非空约束

```SQL
ALTER TABLE TABLE_NAME MODIFY column_name datatype NOT NULL;

# 修改时添加非空约束
alter table users modify username varchar2(20) not null;
```

### 去除非空约束

```SQL
ALTER TABLE TABLE_NAME MODIFY column_name datatype NULL;

# 修改时去除非空约束
alter table users modify username varchar2(20) null;
```

## 主键约束

### 创建表时约束

```SQL
# 语法
CREATE TABLE TABLE_NAME (column_name) datatype PRIMARY KEY, ...)

# 给 id 字段 设置主键约束
create table users (id number(6) primary key);

# 自定义约束名字为 dept_pk
create table dept(id number constraint dept_pk primary key);
```

### 联合约束

```SQL
# 语法
CONSTRAINT constraint_name PRIMARY KEY (column_name1);

# 将 id 和 username 进行联合约束 
create table users (id number(6), username varchar2(20), userpwd varchar2(20), constraint pk_id_username primary key(id, username));

# 将 id 和 name 进行联合约束 
create table dept (id number, name varchar2(20), constraint dept_pk primary key(id, name));

# 忘记了约束名字可以在数据字典中查询
DESC USER_CONSTRAINTS;

# 查询	
select constraint_name from user_constraints where table_name='USERS';
```

### 修改时添加主键约束

```SQL
# 语法
ADD CONSREAINT constraint_name PRIMARY KEY(column_name. ...);

# 添加id为主键
alter table users add constraint pk_id primary key(id);

# 修改id为主键
create table dept(id number);
alter table dept modify id number constraint dept_pk primary key;

# 查询数据字典
desc user_constraints;

# 查询约束名字
select constraint_name from user_constraints where table_name='USERS';
```

### 更改约束名字

```SQL
# 语法	
RENAME CONSTRAINT old_name TO new_name;

# 更改约束名
alter table users rename constraint pk_id_username to new_pk_id;
```

### 禁用/删除主键约束

```SQL
# 禁用主键约束
DISABLE|ENABLE CONSTRAINT constraint_name

# 禁用 new_pk_id 主键约束
alter table users disable constraint new_pk_id

# 删除主键约束
DROP CONSTRAINT constraint_name

# 删除 new_pk_id 主键约束
alter table users drop constraint new_pk_id;

# 查看 users 表约束
select constraint_name, status from user_constraints where table_name = 'USERS';


DROP PRIMARY KEY [CASCADE]
```

## 外键约束

### 创建外键约束

```SQL
CREATE TABLE table1 (column_name datatype) REFERENCES table2(column_name), ...);

# 主表
create table dept1(id number primary key);

# 从表
create table dept2(id number, d_id number, constraint dept_fk foreign key(d_id) references dept1(id))

# 主表插入数据
insert into dept1 values(1);

# 从表插入数据
insert into dept2 values(1, 1);

# ORA-02291: 违反完整约束条件 (SYSTEM.DEPT_FK) - 未找到父项关键字
insert into dept2 values(1, 2); 
```

### 添加约束

```SQL
# 添加约束
alter table dept3 add constraint dept3_fk foreign key(d_id) references dept1(id);
```

### 禁用/删除约束

```SQL
# 禁用
DISABLE|ENABLE CONSTRAINT constraint_name;

alter table table_name disable constraint constraint_name

# 删除
DROP CONSTRAINT constraint_name;
alter table table_name drop constraint constraint_name

alter table dept2 drop constraint dept_fk;
```

## 唯一性约束

### 创建唯一约束

```SQL
# 创建方式一
CREATE TABLE DEPT(id number CONSTRAINT dept_uk UNIQUE, name VARCHAR2(20));

# 创建方式二
CREATE TABLE DEPT(id NUMBER, name VARCHAR2(20), CONSTRAINT dept_uk UNIQUE(id));
```

### 唯一约束联合起名

```SQL
# 创建表级唯一约束
CREATE TABLE DEPT(id NUMBER, name VARCHAR2(20), CONSTRAINT dept_uk UNIQUE(id));
```

### 修改添加

```SQL
# 修改添加 unique 约束 如下两种写法效果一样
ALTER TABLE DEPT MODIFY(NAME UNIQUE);

ALTER TABLE DEPT MODIFY NAME UNIQUE;
```

###　禁用／删除唯一约束

```SQL
# 禁用
DISABLE|ENABLE CONSTRAINT constraint_name;

alter table users disable constraint constraint_name

# 删除
DROP CONSTRAINT constraint_name

alter table users drop constraint constraint_name
```

## 检查约束

### 创建表时检查约束

```SQL
create table dept (id number, salary number(8, 2) constraint dept_ck check (salary > 1000));
```

### 修改表时检查约束

```SQL
SQL> CREATE TABLE DEPT(ID NUMBER, SALARY NUMBER);

表已创建。

SQL> ALTER TABLE DEPT MODIFY SALARY NUMBER CONSTRAINT DEPT_CK CHECK(SALARY > 999);

表已更改。

SQL> INSERT INTO DEPT VALUES(1, 888);
INSERT INTO DEPT VALUES(1, 888)
*
第 1 行出现错误:
ORA-02290: 违反检查约束条件 (SYSTEM.DEPT_CK)


SQL> INSERT INTO DEPT VALUES(1, 1000);

已创建 1 行。
```

## 启用/禁用约束

