## 测试表

```SQL
create table users(id number, name varchar2(20), salary varchar2(20));

insert into users values(01, 'saber', 2333);
insert into users values(02, 'lain', 2333);
insert into users values(03, 'aimer', 2333);
insert into users values(04, 'jack', 2333);
insert into users values(04, 'anny', 2333);
insert into users values(05, 'bob', 2333);

COL RANKID FORMAT 9999;


CREATE TABLE USERTYPE(U_ID NUMBER, TYPENAME VARCHAR(20));
INSERT INTO USERTYPE VALUES(1, 'A');
INSERT INTO USERTYPE VALUES(2, 'B');
INSERT INTO USERTYPE VALUES(3, 'C');
INSERT INTO USERTYPE VALUES(4, 'D');
INSERT INTO USERTYPE VALUES(5, 'E');
```

## 视图

```SQL
# 语法
CREATE [OR REPLACE] [FORCE] VIEW VIEW_NAME AS SUBQUERY(别名) [WITH CHECK OPTION] [WITH READ ONLY];

# 创建
CREATE VIEW VIEW_USERS  AS SELECT * FROM users WHERE id=4;
# 更新
UPDATE VIEW_USERS SET NAME='AOB' WHERE NAME='anny';
# 查询
SELECT * FROM VIEW_USERS;
SELECT * FROM USERS;
```

### WITH CHECK OPTION]

```SQL
# 创建带检查约束的视图
CREATE VIEW view OR REPLACE view_users AS SELECT * FROM users WHERE id=4 WITH CHECK OPTION	;
 
# ORA-01402: 视图 WITH CHECK OPTION where 子句违规
UPDATE view_users2 SET id=3 WHERE ID=4
```

###　WITH READ ONLY

```SQL
CREATE OR REPLACE VIEW VIEW_USERS AS SELECT * FROM USERS WHERE ID=4 WITH READ ONLY;

# ORA-42399: 无法对只读视图执行 DML 操作
UPDATE VIEW_USERS SET NAME='ABC';
```

### FORCE

```SQL
# ORA-00942: 表或视图不存在
CREATE VIEW VIEW_TEST AS SELECT * FROM TEST;

# 警告: 创建的视图带有编译错误。 
# 虽然警告了 但视图依然创建出来了 在 PLSQL 中VIEW 可以看到带红叉的视图
CREATE FORCE VIEW VIEW_TEST AS SELECT * FROM TEST;
```

### 多表管理视图

```SQL
CREATE OR REPLACE VIEW VIEW_USERS AS SELECT USERS.ID, USERS.NAME, USERTYPE.TYPENAME FROM USERS, USERTYPE WHERE USERS.ID=USERTYPE.U_ID;

# ORA-01779: 无法修改与非键值保存表对应的列
UPDATE view_users SET id=9 WHERE id=3

# 下面就🆗 因为 TYPENAME 不在id主键所在表的字段中
UPDATE view_users SET TYPENAME='SSR' WHERE ID=3; 
```

### 聚合统计视图

```SQL
 create view view_users  as select count(id) cid, name from users  group by name,id order by name;

# ORA-01732: 此视图的数据操纵操作非法
UPDATE VIEW_USERS SET CID=2 WHERE NAME='ABC';
```

### 删除视图

```SQL
DROP VIEW VIEW_NAME
```

## 物化视图

### 创建语法

```SQL
CREATE MATERIALIZED VIEW VIEW_NAME [BUILD IMMEDIATE | BULLD DEFERRED] REFRESH [FAST | COMPLETE | FORCE] 
[ON [COMMIT | DEMAND ] | START WITH (START_TIME) NEXT(NEXT_TIME)] AS SUBQUERY
```

### DEFERRED

```SQL
# 创建物化视图
CREATE MATERIALIZED VIEW MV_USERS AS SELECT * FROM USERS;

# 向基表插入数据
UPDATE USERS VALUES(7, 'AAA', 234);

# 查询发现只有 USERS 表有新增数据 因为MV_USERS 表需要手动刷新
SELECT * FROM USERS;
SELECT * FROM MV_USERS;

# 执行下面代码手动刷新 C 指代 COMPLETE
BEGIN 
   DBMS_MVIEW.REFRESH('MV_USERS', 'C');
END;

# 再次查询刷新结果 
SELECT * FROM MV_USERS;
```

### ON COMMIT

```SQL
# 向基表插入数据
CREATE MATERIALIZED VIEW MV_USERS REFRESH ON COMMIT AS SELECT * FROM USERS;

# 插入数据
INSERT INTO USERS VALUES(0, '0', 0);
COMMIT;

# 查询数据 已经自动刷新了
SELECT * FROM USERS;
SELECT * FROM MV_USERS;
```

### FAST

```SQL
# 实体化视图日志已创建。
CREATE MATERIALIZED VIEW LOG ON USERS WITH ROWID;
```

## 序列

### 创建序列

```SQL
CREATE SEQUENCE sequence # 创建序列名称
 [INCREMENT BY n] # 递增的序列值是 n 如果 n 是证书就递增 如多是负数就递减 默认是1
 [START WITH] # 开始的值，递增默认时 minvalue 递减是 maxvalue
 [{MAXVALUE n | NOMAXVALUE}] # 最大值
 [{MINVALUE n | NOMINVALUE}] # 最小值
 [{CYCLE | NOCYCLE}] # 循环/不循环
 [{CACHE n | NOCACHE}]  # 分配并存入到内存中


# 从10开始， 每次增长1 最小值10 最大值100 循环
CREATE SEQUENCE SEQ_TEST INCREMENT BY 1 START WITH 10 MINVALUE 10 MAXVALUE 201 CYCLE;
```

### 查询序列

```SQL
# 查询序列下一个值
SELECT SEQ_TEST.PREVVAL FROM DUAL

# 查询序列当前值
SELECT SEQ_TEST.CURRVAL FROM DUAL

# 有最大值的非循环序列
CREATE SEQUENCE SEQ_TEST MAXVALUE 20;
```

## 同义词

```SQL
# 私有 同义词已创建
CREATE SYNONYM OUSERS FOR USERS;

# 公有 同义词已创建
CREATE PUBLIC SYNONYM OUSERS FOR USERS;
```

