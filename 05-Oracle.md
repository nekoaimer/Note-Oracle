## 变量

```SQL
DECLARE
  # 定义变量
  v_price number(10, 2); -- 单价
  v_usenum number; -- 水费字数
  v_usenum2 number(10, 2); -- 吨数
  v_money number(10, 2); -- 金额
BEGIN
 # 变量赋值
  v_price:=2.45; -- 单价赋值
  v_usenum:=9213; -- 水费字数
  v_usenum2:= round(v_usenum / 1000, 2);
  v_money:=v_price * v_usenum2; -- 金额
  # 打印结果
  DBMS_OUTPUT.PUT_LINE('金额:' || v_money);
END;
```

## SELECT INTO

```SQL
DECLARE
  v_salary number;
BEGIN
  select salary into v_salary from users where id=1;
  DBMS_OUTPUT.PUT_LINE('工资: ' || v_salary);
END;
```

## 属性类型

### 引用型

```SQL
# 属性类型 (引用型 表名.列名%type)

DECLARE
  v_salary users.salary%type;
BEGIN
  select salary into v_salary from users where id=1;
  DBMS_OUTPUT.PUT_LINE('工资: ' || v_salary);
END;
```

### 记录型

```SQL
# 属性类型 （记录型 表名 %rowtype）

DECLARE
  v_users users%rowtype; -- 行记录类型
BEGIN
  select * into v_users from users where id=1;
  DBMS_OUTPUT.PUT_LINE('姓名: ' || v_users.name || ' 工资: ' || v_users.salary);
END;
```

## 异常

```SQL
# 异常处理

DECLARE
  v_name users.name%type; -- 姓名
  v_salary users.salary%type; -- 工资
  v_users users%rowtype; -- 行记录类型
BEGIN
  select * into v_users from users;
  DBMS_OUTPUT.PUT_LINE('姓名: ' || v_users.name || ' 工资: ' || v_users.salary);
  
EXCEPTION 
   WHEN NO_DATA_FOUND THEN
    DBMS_OUTPUT.PUT_LINE('没有找到该数据');   
   WHEN TOO_MANY_ROWS THEN
    DBMS_OUTPUT.PUT_LINE('返回了多行数据');
END;
```

## 条件判断

### 语法

```SQL
IF expression THEN
  xxx
ELSIF expression THEN
  xxx
ELSE expression
  xxx
END IF;
```

### 案例

```SQL
DECLARE
  v_name users.name%type; -- 姓名
  v_salary users.salary%type; -- 工资
  
  v_price1 users.salary%type; -- 单价
  v_price2 users.salary%type; -- 单价
  v_price3 users.salary%type; -- 单价
  ID users.id%type;
  v_users users%rowtype; -- 行记录类型
BEGIN
  v_price1:=1; -- 倍数
  v_price1:=2; -- 倍数
  v_price1:=3; -- 倍数
  
  select * into v_users from users where id=1;
    
  IF v_users.id < 3 THEN
    v_users.salary:=v_users.salary * v_price1;  
  ELSIF v_users.id < 5 AND v_users.id < 7 THEN
    v_users.salary:=v_users.salary * v_price1;
  ELSE
    v_users.salary:=v_users.salary * v_price3;
  END IF;
  
  DBMS_OUTPUT.PUT_LINE('姓名: ' || v_users.name || ' 工资: ' || v_users.salary);

  
EXCEPTION 
   WHEN NO_DATA_FOUND THEN
    DBMS_OUTPUT.PUT_LINE('没有找到账号数据');   
   WHEN TOO_MANY_ROWS THEN
    DBMS_OUTPUT.PUT_LINE('返回了多行数据');
END;
```

## 循环体

###　无条件循环 1-100

```SQL
DECLARE
  v_num number;
BEGIN
  v_num:=1;
  LOOP
    DBMS_OUTPUT.PUT_LINE(v_num);
    v_num:=v_num+1;
    IF v_num > 100 THEN
      exit;
    END IF;
  END LOOP;
END;

# 等价于下面这种写法

DECLARE
  v_num number;
BEGIN
  v_num:=1;
  LOOP
    DBMS_OUTPUT.PUT_LINE(v_num);
    v_num:=v_num+1;
    EXIT WHEN v_num > 100;
  END LOOP;
END;
```

### WHILE 有条件循环

```SQL
DECLARE
  v_num number;
BEGIN
  v_num:=1;
  WHILE
    v_num < 100
  LOOP
    v_num:=v_num+1;
    DBMS_OUTPUT.PUT_LINE(v_num);
  END LOOP;  
END;
```

### FOR 循环

```SQL
DECLARE # 没有声明区 这句代码可以不写
BEGIN
  FOR v_num in 1 .. 100
  LOOP
    DBMS_OUTPUT.PUT_LINE(v_num);
  END LOOP;
END;
```

##　游标

- 语法

```SQL
# DECLARE
CURSOR 游标名称 IS SQL语句

# USE
OPEN 游标名称

LOOP
  FETCH 游标名称 INTO 变量
  EXIT WHEN  游标名称%notfound
END LOOP;

CLOSE 游标名称
```

### 无参数游标

```SQL
DECLARE 
  CURSOR LessThan4 is SELECT * FROM USERS WHERE ID < 4; 
  v_users users%ROWTYPE;
BEGIN
  OPEN LessThan4; --  打开游标
    LOOP
      FETCH LessThan4 INTO v_users; -- 提取游标
      EXIT WHEN LessThan4%NOTFOUND; -- 退出游标循环
      DBMS_OUTPUT.PUT_LINE('id ' || v_users.id || ' name ' || v_users.name || ' salary ' || v_users.salary);
    END LOOP; 
  CLOSE LessThan4; -- 关闭游标
END;

/* RESULT
id 1 name saber salary 2333
id 2 name lain salary 2333
id 3 name aimer salary 2333
*/
```

### 带参数游标

```SQL

DECLARE 
  CURSOR LessThan4(v_id number) is SELECT * FROM USERS WHERE ID < v_id; 
  v_users users%ROWTYPE;
BEGIN
  OPEN LessThan4(50); --  打开游标
    LOOP
      FETCH LessThan4 INTO v_users; -- 提取游标
      EXIT WHEN LessThan4%NOTFOUND; -- 退出游标循环
      DBMS_OUTPUT.PUT_LINE('id ' || v_users.id || ' name ' || v_users.name || ' salary ' || v_users.salary);
    END LOOP; 
  CLOSE LessThan4; -- 关闭游标
END;
```

### FOR循环游标

```SQL
DECLARE 
  CURSOR LessThan4(v_id number) is SELECT * FROM USERS WHERE ID < v_id; 
BEGIN
  FOR v_users IN LessThan4(3)
  LOOP
    DBMS_OUTPUT.PUT_LINE('id ' || v_users.id || ' name ' || v_users.name || ' salary ' || v_users.salary);
  END LOOP;
END;
```



