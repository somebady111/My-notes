# Oracle语法

## 增

```sql
INSERT INTO customers (customer_id,first_name,last_time,dob,phone) values ( 6,'Fred','Brown','01-JAN-1970','800-555-1215');
```



## 删

```sql
DELETE FROM customers WHERE customer_id = 6;
```



## 改 

```sql
UPDATE customers SET last_time = 'Orange' WHERE customer_id = 6;
```





## 查

```sql
# 查询所有列
SELECT * FROM customers;
# 使用where限定语句
SELECT * FROM customers WHERE customer_id = 6;
# 查询行标识符
SELECT  ROWID，customer_id FROM customer;
# 查询行号
SELECT ROWNUM,customer_id,first_name,last_name FROM customers;
SELECT ROWNUM,customer_id,first_name,last_name FROM customer WHERE customer_id = 6;
# 执行算术运算+ - * /
SELECT 2*6 from dual;
# 执行日期计算,mysql无效
SELECT TO_DATE('25-JUL-2021') + 2 FROM dual;
SELECT TO_DATE('27-JUL-2021') - TO_DATE('25-JUL-2021') FROM dual;
# 列运算
SELECT name,price + 2 FROM products;
SELECT name,price * 3 + 1 FROM products;
# 使用列别名
SELECT price * 2 DOUBLE_PRICE FROM products;
SELECT price * 2 "DOUBLE_PRICE" FROM products;
SELECT proce * 2 as "DOUBLE_PRICE" FROM products;
# 使用连接操作输出结果
SELECT first_name || ' ' || last_name as "customer name" FROM customers;
# 查询空值
SELECT customer_id,first_name,last_name,dob FROM customers WHERE dob IS NULL;
# NVL（）函数替换数据中的空值
SELECT customer_id,first_name,last_name,NVL(dob,'01-JAN-2021') AS DOB FROM customers;
# 重复内容过滤
SELECT DISTINCT customer_id FROM purchases;
# 不等符号
SELECT * FROM customer WHERE customer_id <> 6;
# 大于
SELECT * FROM customer WHERE customer_id > 6;
# 小于等于
SELECT * FROM customer WHERE customer_id <= 6;
# ANY
SELECT * FROM customer WHERE customer_id > ANY(2,3,4);
# ALL
SELECT * FROM customer WHERE customer_id > ALL(2,3,4);
# LIKE 字符,_匹配任意字符，o匹配该字符，%匹配o后的任意字符
SELECT * FROM customer WHERE first_name LIKE '_o%';
SELECT * FROM customer WHERE first_name NOT LIKE '_o%';
# ESCAPE标识通配符和字符的区分
SELECT name FROM customer WHERE first_name LIKE '%\%%' ESCAPE '\';
# IN 操作符
SELECT * FROM customer WHERE customer_id IN(2,3,6);
SELECT * FROM customer WHERE customer_id NOT IN(2,3,6);
SELECT * FROM customer WHERE customer_id NOT IN(2,3,6,NULL);
# BETWEEN操作符
SELECT * FROM customer WHERE customer_id BETWEEN 1 AND 6;
# 逻辑操作符
SELECT * FROM customer WHERE customer_id <= 6 AND customer >=3;
SELECT * FROM customer WHERE customer_id > 4 OR customer <=4;
SELECT * FROM customer WHERE NOT customer >=3;
# ORDER BY 排序
SELECT * FROM custome ORDER BY customer_id;
SELECT * FROM custome ORDER BY customer_id ASC,price DESC;
# 按照列排序
SELECT * FROM custome ORDER BY 1;
```



```sql
# 使用两个表的SELECT查询语句
```





```reStructuredText
备注：
1.dual表
dual表经常与返回值的函数和表达式连用，而不用在查询中运用基础表。
2.NVL()函数
NVL()可以将空值转换成另一个值，NVL（）接收两个参数，第一，需要替换的值；第二，需要替换的内容。对可能出现空值的语句使用NVL（）函数。
```

