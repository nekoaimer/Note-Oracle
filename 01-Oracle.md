## 登录/用户	

### 创建用户

```sql
-- 创建用户
create user wateruser identified by 2333 default tablespace waterboss
```

### 登录

```sql
# 登录一
system/密码 

# 登录二
system
密码

# 登陆后 connect 连接
# 使用 connect sys 报错显示 必须使用 SYSDBA 或者 SYSOPER 的权限
# connection as SYS should be as SYSDBA or SYSOPER
connect sys/2333 as sysdba

# 启用 scott 用户
alter user scott account unlock;
# 登录
connect scott/tiger
```

### 赋予权限

```sql
# 创建一个 wateruser 用户时进行登录会发现没有权限 那么执行如下语句
grant dba to wateruser
```



### 查看用户

```sql
# 登录之后 查看用户
show user
```

## TABLESPACE

### 查看表空间

```sql
# 查看表空间结构
desc dba_tablespaces

# 设置默认表空间 # 更改system用户默认表空间
ALTER USER system DEFAULT TABLESPACE xxx;

# 查看所有表空间
select * from v$tablespace;
```

### 创建表空间

#### 永久表空间

```sql
# mytablespace : 表空间名称
# datafile: 指定表空间对应的数据文件（位置）
# size:表空间的初始大小
# autoextend on ：自动增长,
# next： 一次自动增长的大小
create tablespace mytablespace datafile '永久表空间物理文件位置' size 15M autoextend on next 10M permanent online;

# 创建表空间
create tablespace test datafile 'test.dbf'  size 15M autoextend on next 10M permanent online;

# 查看表空间
select file_name from dba_data_files where tablespace_name='TEST';
```

#### 临时表空间

```sql
# 创建临时表空间
create temporary tablespace test tempfile 'test.dbf'  size 10M;

# 查看临时表空间
# select file_name from dba_temp_files
select file_name from dba_temp_files where tablespace_name='TEST2';
```

### 修改表空间状态

#### ONLINE/OFFLINE

```sql
ALTER TABLESPACE TABLESPACE_NAME ONLINE/OFFLINE

# 修改状态为 offline
alter tablespace test offline;

# 查看所有表状态
select tablespace_name, status from dba_tablespaces;
```

#### READ ONLY/READ WRITE

```sql
ALTER TABLESPACE TABLESPACE_NAME READ ONLY/READ WRITE

# 修改状态为 READ ONLY
alter tablespace test READ ONLY;

# 查看所有表状态
select tablespace_name, status from dba_tablespaces;
```

## 增加数据文件

```sql
# 增加数据文件语法
ALTER TABLESPACE tablespace	_name ADD DATAFILE 'xxx.dbf' SIZE xx;

# 增加 a.dbf 数据文件
alter tablespace test add datafile 'a.dbf' size 10m;

# 查看 TEST 表空间中数据文件
select file_name from dba_data_files where tablespace_name='TEST';
```

## 删除数据文件

```sql
# 删除数据文件语法
ALTER TABLESPACE tablespace	_name DROP DATAFILE 'xxx.dbf';

# 删除 a.dbf 数据文件 
alter tablespace test drop datafile 'a.dbf';

# 查看 TEST 表空间数据文件
select tablespace_name, file_name from dba_data_files;
```

## 删除表空间

```sql
drop tablespace test including contents;
```

## TABLE

### CREATE TABLE

```sql
# 创建表
CREATE TABLE T_OWNERS (
  ID NUMBER PRIMARY KEY,
  NAME VARCHAR2(20),
  ADDRESSID NUMBER,
  HOUSENUMBER VARCHAR2(20),
  WATERMETER VARCHAR2(20),
  ADDDATE DATE,
  OWNERTYPEID NUMBER
);
```

### ADD FIELD

```SQL
# 语法
ALTER TABLE TABLE_NAME ADD(列名 类型 [DEFAULE 默认值], 列名 类型 ...)

# 追加字段
ALTER TABLE T_OWNERS ADD(USERNAME VARCHAR2(10) DEFAULT 'Admin', PASSWORD NUMBER(8));
```

### MODIFY FIELD

```SQL
# 语法
ALTER TABLE TABLE_NAME MODIFY(列名 类型 [DEFAULE 默认值], 列名 类型 ...);

# 修改字段
ALTER TABLE T_OWNERS MODIFY(USERNAME VARCHAR2(20), PASSWORD NUMBER(10));
```

### MODIFY FIELD NAME

```SQL
# 语法
ALTER TABLE TABLE_NAME RENAME COLUMN OLDFIELDNAME TO NEWFIELDNAME

# 修改字段名
ALTER TABLE T_OWNERS RENAME COLUMN ID TO U_ID;
```

### DELETE FIELD

```SQL
# 语法
# 删除一个字段
ALTER TABLE TABLE_NAME DROP COLUMN FIELDNAME1;
# 删除多个字段
ALTER TABLE TABLE_NAME DROP (FIELDNAME1, FIELDNAME2)

# 删除字段名
ALTER TABLE T_OWNERS DROP COLUMN ADDDATE;
```

### DELETE TABLE

```SQL
DROP TABLE TABLE_NAME
```

### RENAME TABLE_NAME

```sql
RENAME TABLE_NAME to NEWTABLE_NAME 
s
rename userinfo to users;
```

