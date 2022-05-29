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

