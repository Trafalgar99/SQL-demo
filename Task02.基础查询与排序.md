# 第二章 基础查询与排序



## 第一部分 知识提要

### 2.1 SELECT语句基础

#### SELECT语句

从表中选出必要的数据，通常与FROM语句同时使用

```mysql
SELECT <列名>, 
  FROM <表名>;
```

#### WHERE语句

选出符合某些条件的数据

```mysql
SELECT <列名>, ……
  FROM <表名>
 WHERE <条件表达式>;
```

#### 相关法则

1. ✳代表全部列的意思
2. SQL中可以随意使用换行符，不影响语句执行（但不可插入空行）
3. 设定汉语别名时需要使用双引号（"）括起来。（**如果英文名称有空格也要用引号**）
4. 在SELECT语句中使用DISTINCT可以删除重复行。
5. 注释是SQL语句中用来标识说明或者注意事项的部分。分为1行注释"-- "和多行注释两种"/* */"。

### 2.2 算术运算符和比较运算符

#### 常见运算符及其含义

| =    | 和~相等   |
| ---- | --------- |
| <>   | 和~不相等 |
| >=   | 大于等于~ |
| >    | 大于~     |
| <=   | 小于等于~ |
| <    | 小于~     |

#### 常用法则

1. SELECT子句中可以使用常数或者表达式。
2. 使用比较运算符时一定要注意不等号和等号的位置。
3. 字符串类型的数据原则上按照字典顺序进行排序，不能与数字的大小顺序混淆。
4. 希望选取NULL记录时，需要在条件表达式中使用**IS NULL**运算符。希望选取不是NULL的记录时，需要在条件表达式中使用**IS NOT NULL**运算符。

### 2.3 逻辑运算符

#### NOT运算符

表示否定，不是...的意思

```mysql
SELECT product_name, product_type, sale_price
  FROM product
 WHERE NOT sale_price >= 1000;
```

#### AND运算符和OR运算符

当希望同时使用多个查询条件时，可以使用AND或者OR运算符。其含义同数学上的交和并

```mysql
SELECT product_name, product_type, regist_date
  FROM product
 WHERE product_type = '办公用品'
   AND ( regist_date = '2009-09-11'
        OR regist_date = '2009-09-20');
```

`AND的优先级高于OR，使用括号可以改变优先级`

### 2.4 对表进行聚合查询

#### 聚合函数

SQL中用于汇总的函数叫做聚合函数。以下五个是最常用的聚合函数：

- COUNT：计算表中的记录数（行数）
- SUM：计算表中数值列中数据的合计值
- AVG：计算表中数值列中数据的平均值
- MAX：求出表中任意列中数据的最大值
- MIN：求出表中任意列中数据的最小值

如果在聚合数据时要略过重复的数据，需要在调用时传入**DISTINCT**关键字

```mysql
-- 计算去除重复数据后的数据行数
SELECT COUNT(DISTINCT product_type)
 FROM product;
-- 是否使用DISTINCT时的动作差异（SUM函数）
SELECT SUM(sale_price), SUM(DISTINCT sale_price)
 FROM product;
```

#### 常用法则

- COUNT函数的结果根据参数的不同而不同。COUNT(*)会得到包含NULL的数据行数，而COUNT(<列名>)会得到NULL之外的数据行数。
- 聚合函数会将NULL排除在外。但COUNT(*)例外，并不会排除NULL。
- MAX/MIN函数几乎适用于所有数据类型的列。SUM/AVG函数只适用于数值类型的列。
- 想要计算值的种类时，可以在COUNT函数的参数中使用DISTINCT。
- 在聚合函数的参数中使用DISTINCT，可以删除重复数据。

### 2.5 对表进行分组

#### GROUP BY 语句

将现有的数据按照某列来汇总统计

```mysql
SELECT <列名1>,<列名2>, <列名3>, ……
  FROM <表名>
 GROUP BY <列名1>, <列名2>, <列名3>, ……;
```

##### GROUP BY书写位置

1 SELECT → 2. FROM → 3. WHERE → 4. GROUP BY

#### 常见错误

1. 在聚合函数的SELECT子句中写了聚合键以外的列使用COUNT等聚合函数时，SELECT子句中如果出现列名，只能是GROUP BY子句中指定的列名（也就是聚合键）。
2. 在GROUP BY子句中使用列的别名SELECT子句中可以通过AS来指定别名，但在GROUP BY中不能使用别名。因为在DBMS中 ,SELECT子句在GROUP BY子句后执行。
3. 在WHERE中使用聚合函数原因是聚合函数的使用前提是结果集已经确定，而WHERE还处于确定结果集的过程中，所以相互矛盾会引发错误。 如果想指定条件，可以在SELECT，HAVING（下面马上会讲）以及ORDER BY子句中使用聚合函数。

### 2.6 为聚合结果指定条件

#### HAVING子句

HAVING字句类似于WHERE语句，但是HAVING专门用于GROUP BY语句之后的条件筛选

```mysql
SELECT product_type, COUNT(*)
  FROM product
 GROUP BY product_type
HAVING COUNT(*) = 2;
```

`HAVING后的列名也必须是聚合键`

### 2.7 对查询结果进行排序

#### ORDER BY语句

当需要按照特定顺序排序时，可以使用 **ORDER BY** 子句。

```mysql
SELECT <列名1>, <列名2>, <列名3>, ……
  FROM <表名>
 ORDER BY <排序基准列1>, <排序基准列2>, ……
```

默认为升序排列，降序排列为DESC

```mysql
SELECT <列名1>, <列名2>, <列名3>, ……
  FROM <表名>
 ORDER BY <排序基准列1> DESC, <排序基准列2> DESC, ……
```

`当用于排序的列名中含有NULL时，NULL会在开头或末尾进行汇总。`

#### ORDER BY中列名可使用别名

SQL在使用 HAVING 子句时 SELECT 语句的顺序为：

FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY。

其中SELECT的执行顺序在 GROUP BY 子句之后，ORDER BY 子句之前。也就是说，当在ORDER BY中使用别名时，已经知道了SELECT设置的别名存在，但是在GROUP BY中使用别名时还不知道别名的存在，所以不能在ORDER BY中可以使用别名，但是在GROUP BY中不能使用别名

#### ORDER BY 排序列中存在 NULL 时，指定其出现在首行或者末行的方式

如果想指定存在 `NULL` 的行出现在首行或者末行，需要特殊处理。

一般有如下两种需求：

一、将 `NULL` 值排在末行，同时将所有 `非NULL` 值按升序排列。

1. 对于数字或者日期类型，可以在排序字段前添加一个负号（minus）来得到反向排序。（`-1、-2、-3....-∞`）

2. 对于字符型或者字符型数字，此方法不一定能得到期望的排序结果，可以使用 `IS NULL` 比较运算符。

```mysql
SELECT *
FROM user
ORDER BY name IS NULL, name ASC
```

3. 使用 `COALESCE` 函数实现需求

```mysql
SELECT *
FROM user
ORDER BY COALESCE(name,'zzzzz') ASC
```

>COALESCE函数在这里的使用方法十分巧妙，COALESCE函数会将第一个参数里的NULL用第二个（如果第二个参数也为NULL就会用第三个参数，以此类推）值填充，第二个参数可以是一个列（会用对应的位置填充），也可以是一个固定值（始终用该固定值填充）。将第二个参数设定为‘zzzzz’，会使name中的NULL被替换为zzzzz，在升序排序时会在最末尾。

二、将 NULL 值排在首行，同时将所有非NULL 值按倒序排列。

1. 对于数字或者日期类型，可以在排序字段前添加一个负号（minus）来实现。（`-∞...-3、-2、-1`）
2. 对于字符型或者字符型数字，此方法不一定能得到期望的排序结果，可以使用 `IS NOT NULL` 比较运算符。
3. 使用 `COALESCE` 函数实现需求

```mysql
SELECT *
FROM user
ORDER BY COALESCE(name,'zzzzz') DESC;
```

------



## 第二部分 习题解答

### 01.

编写一条SQL语句，从 `product`(商品) 表中选取出“登记日期(`regist_date`)在2009年4月28日之后”的商品，查询结果要包含 `product name` 和 `regist_date` 两列。

```mysql
SELECT product_name,regist_date
FROM product
WHERE regist_date > '2009-04-28'
```

<img src=".\images\1660831714148.jpg" style="zoom: 67%;" />

### 02.

请说出对product 表执行如下3条SELECT语句时的返回结果。

1. ```mysql
   SELECT *
     FROM product
    WHERE purchase_price = NULL;
   ```

   > 无结果，希望选取NULL记录时，需要在条件表达式中使用IS NULL运算符。

2. ```mysql
   SELECT *
     FROM product
    WHERE purchase_price <> NULL;
   ```

   > 无结果，希望选取不是NULL的记录时，需要在条件表达式中使用IS NOT NULL运算符。

3. ```mysql
   SELECT *
     FROM product
    WHERE product_name > NULL;
   ```

   > 无结果，NULL不用来比较

### 03.

`2.2.3` 章节中的SELECT语句能够从 `product` 表中取出“销售单价（`sale_price`）比进货单价（`purchase_price`）高出500日元以上”的商品。请写出两条可以得到相同结果的SELECT语句。

```mysql
SELECT product_name, sale_price, purchase_price
  FROM product
 WHERE NOT sale_price - purchase_price < 500;
```

```mysql
SELECT product_name, sale_price, purchase_price
  FROM product
 WHERE   purchase_price-sale_price <= -500;
```

### 04.

请写出一条SELECT语句，从 `product` 表中选取出满足“销售单价打九折之后利润高于 `100` 日元的办公用品和厨房用具”条件的记录。查询结果要包括 `product_name`列、`product_type` 列以及销售单价打九折之后的利润（别名设定为 `profit`）。

```mysql
SELECT
	product_name,
	product_type,
	sale_price*0.9-purchase_price AS profit
FROM product
WHERE sale_price*0.9-purchase_price > 100
```

<img src=".\images\1660832911509.jpg" style="zoom:67%;" />

### 05.

请指出下述SELECT语句中所有的语法错误。

```mysql
SELECT product_id, SUM(product_name)
-- 本SELECT语句中存在错误。
  FROM product 
 GROUP BY product_type 
 WHERE regist_date > '2009-09-01';
```

> 1. sum函数只能对数字累加
> 2. GROUP BY 语句后的条件筛选不能用WHERE，要用HAVING
> 3. HAVING语句中不能出现SELECT中没出现的列

### 06.

请编写一条SELECT语句，求出销售单价（ `sale_price` 列）合计值大于进货单价（ `purchase_price` 列）合计值1.5倍的商品种类。

```mysql
SELECT
	product_type,
	SUM(sale_price) AS sum,
	SUM(purchase_price) AS sum
FROM product
GROUP BY product_type
HAVING SUM(sale_price) > SUM(purchase_price)*1.5
```

<img src=".\images\1660833599361.jpg" style="zoom:67%;" />

### 07.

此前我们曾经使用SELECT语句选取出了product（商品）表中的全部记录。当时我们使用了 `ORDER BY` 子句来指定排列顺序，但现在已经无法记起当时如何指定的了。请根据下列执行结果，思考 `ORDER BY` 子句的内容。

![](https://oss.linklearner.com/wonderful-sql/ch02/ch02.09test27.png)

```mysql
SELECT *
FROM product
ORDER BY 
	COALESCE(regist_date,'2022-12-12')  DESC,
	purchase_price
```

```mysql
-- 方法二
SELECT *
FROM product
ORDER BY -regist_date, sale_price
```

<img src=".\images\1660834030994.jpg" style="zoom: 67%;" />