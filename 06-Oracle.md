## 存储函数

### 创建语法

```SQL
CREATE [OR REPLACE] FUNCTION 函数名称

(参数名称 参数类型, 参数名称 参数类型, ...)

RETURN 结果变量数据类型

IS 
  变量声明部分;

BEGIN 
  逻辑部分;
  RETURN 结果变量;

[EXCEPTION 异常处理部分]

END;
```

### 存储函数测试

```SQL
CREATE OR REPLACE FUNCTION fn(v_id NUMBER)
RETURN VARCHAR2
IS
  v_name varchar2(20);
BEGIN
  SELECT NAME INTO v_name FROM users WHERE id=v_id;
  
  RETURN v_name;
END;
```

### 存储函数应用和

```SQL
SELECT fn(2) FROM DUAL;

SELECT fn(id) FROM users;
```

## 存储过程

### 创建语法

```SQL
CREATE [OR REPLACE] PROCEDURE 函数名称

(参数名称 参数类型, 参数名称 参数类型, ...)

IS|AS
  变量声明部分;

BEGIN 
  逻辑部分;
  RETURN 结果变量;

[EXCEPTION 异常处理部分]

END;
```

### 创建不带传出参数存储过程

```SQL
# 创建一个序列
CREATE SEQUENCE seq_users START WITH 11; -- 序列

# 创建存储过程
CREATE OR REPLACE PROCEDURE pro_users (v_name VARCHAR2, v_salary NUMBER)

IS 

BEGIN
  INSERT INTO USERS VALUES(seq_users.nextval, v_name, v_salary);
  COMMIT;
END;

# 第一种调用方法
CALL pro_users('ABC', 123);

# 第二章调用方法
BEGIN
  pro_users('ABD', 2);
END;

# 查询结果
select * from users;
```

### 创建带传出参数存储过程

```SQL
CREATE SEQUENCE seq_users START WITH 11; 

CREATE OR REPLACE PROCEDURE pro_users
(
  v_name VARCHAR2,
  v_salary NUMBER,
  v_id OUT NUMBER
)

IS

BEGIN
  SELECT seq_users.nextval INTO v_id FROM DUAL;
  INSERT INTO USERS VALUES(v_id, v_name, v_salary);
  COMMIT;
END;

# 调用
DECLARE
  v_id NUMBER;
BEGIN
  pro_users('MASTER', 000, v_id);
  DBMS_OUTPUT.PUT_LINE(v_id);
END;

CALL pro_users('ABC', 123);

select * from users;
```

### 存储过程搭配循环

```SQL
CREATE OR REPLACE FUNCTION f(num NUMBER)

RETURN NUMBER

IS
  v_num number;
BEGIN
  DECLARE
  BEGIN 
    v_num:=1;
    LOOP
      v_num:=v_num+num;
      EXIT WHEN v_num > 100;   
    END LOOP;
  END;     
   DBMS_OUTPUT.PUT_LINE(v_num);
  RETURN v_num;
 
END;

select  * from users;
select  f(1) from users;
```

## 触发器

### 语法

```SQL
CREATE [OR REPLACE] TRIGGER 触发器名
  BEFORE | AFTER
  [DELETE ] [[OR] INSERT [[OR] UPDATE [OF 列名]]
  ON 表名
  [FOR EACH ROW ][WHERE(条件)]
DECLARE 
  ...
BEGIN
   PLSQL 块            
END;     
            
FOR EACH ROW  # 作用是标注此触发器是行级触发器 语句级触发器

# 在触发器中触发语句与伪记录变量的值
触发语句     :OLD                    :NEW
INSERT    所有字段都是空(NULL)    将要插入的数据
UPDATE    更新以前该执行的值       更新后的值
DELETE    删除以前该行的值         所有字段都是空(NULL)
```

### 前置触发器

```SQL
CREATE OR REPLACE TRIGGER tri_users_salary
BEFORE
UPDATE OF salary
ON users 
FOR EACH ROW

DECLARE

BEGIN
  :new.salary:=:new.salary + 10;
END;

# 更改 salary 的值就会自动触发 + 10
```

###　后置触发器

```SQL

-- 创建日志表 记录修改前与修改后的名称
CREATE TABLE USERLOG(
updatetime DATE,
ownerid NUMBER,
oldname VARCHAR2(20),
newname VARCHAR2(20)
)

-- 创建后置触发器
CREATE OR REPLACE TRIGGER tri_users_log
AFTER
UPDATE OF name
ON users
FOR EACH ROW
  
DECLARE

BEGIN 
  INSERT INTO USERLOG VALUES(SYSDATE, :new.id, :old.name, :new.name);
END;

# 更新name 触发后置触发器 记录到 USERLOG 表中
UPDATE USERS SET name='aaaa' WHERE id=12;
```

