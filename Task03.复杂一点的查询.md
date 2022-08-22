

# 第三章 复杂一点的查询

## 第一部分 知识提要

### 3.1 视图

#### 什么是视图

视图是一个虚拟的表，操作视图时会根据创建视图的SELECT语句生成一张虚拟表，然后在这张虚拟表上做SQL操作。

#### 视图与表的区别

**视图并没有保存数据**视图并不是数据库真实存储的数据表，它可以看作是一个窗口，通过这个窗口我们可以看到数据库表中真实存在的数据。所以我们要区别视图和数据表的本质，即视图是基于真实表的一张虚拟的表，其数据来源均建立在真实表的基础上。

#### 视图优点

1. 通过定义视图可以将频繁使用的SELECT语句保存以提高效率。
2. 通过定义视图可以使用户看到的数据更加清晰。
3. 通过定义视图可以不对外公开数据表全部字段，增强数据的保密性。
4. 通过定义视图可以降低数据的冗余。

#### 创建视图

```mysql
CREATE VIEW <视图名称>(<列名1>,<列名2>,...) AS <SELECT语句>
```

**注意事项：**在一般的DBMS中定义视图时不能使用ORDER BY语句。因为视图和表一样，**数据行都是没有顺序的**。

#### 修改视图结构

```mysql
ALTER VIEW <视图名> AS <SELECT语句>
```

#### 更新视图内容

对于一个视图来说，如果包含以下结构的任意一种都是不可以被更新的：

- 聚合函数 SUM()、MIN()、MAX()、COUNT() 等。
- DISTINCT 关键字。
- GROUP BY 子句。
- HAVING 子句。
- UNION 或 UNION ALL 运算符。
- FROM 子句中包含多个表。

**注意：**在创建视图时也尽量使用限制不允许通过视图来修改表

#### 删除视图

```mysql
DROP VIEW <视图名1> [ , <视图名2> …]
```

### 3.2 子查询

子查询指一个查询语句嵌套在另一个查询语句内部的查询，在 SELECT 子句中先计算子查询，子查询结果作为外层另一个查询的过滤条件，查询可以基于一个表或者多个表。

#### 子查询和视图的关系

子查询就是将用来定义视图的 SELECT 语句直接用于 FROM 子句当中。子查询不会像视图那样保存在存储介质中， 而是在 SELECT 语句执行之后就消失了。

#### 嵌套子查询

子查询也可以再出现在其他子查询中，**但是随着子查询嵌套的层数的叠加，SQL语句不仅会难以理解而且执行效率也会很差，所以要尽量避免这样的使用。**

#### 标量子查询

标量子查询要求我们执行的SQL语句只能返回一个值，也就是要返回表中具体的**某一行的某一列**。

由于标量子查询的特性，导致标量子查询不仅仅局限于 WHERE 子句中，通常任何可以使用单一值的位置都可以使用。也就是说， 能够使用常数或者列名的地方，无论是 SELECT 子句、GROUP BY 子句、HAVING 子句，还是 ORDER BY 子句，几乎所有的地方都可以使用。

#### 关联子查询

即查询与子查询之间存在着联系。关联子查询就是通过一些标志将内外两层的查询连接起来起到过滤数据的目的

**关联查询的执行过程：**

1. 首先执行不带WHERE的主查询
2. 根据主查询讯结果匹配product_type，获取子查询结果
3. 将子查询结果再与主查询结合执行完整的SQL语句

### 3.3 各种各样的函数

函数大致分为如下几类：

- 算术函数 （用来进行数值计算的函数）
- 字符串函数 （用来进行字符串操作的函数）
- 日期函数 （用来进行日期操作的函数）
- 转换函数 （用来转换数据类型和值的函数）
- 聚合函数 （用来进行数据聚合的函数）

#### 算数函数

- ABS -- 绝对值

语法：`ABS( 数值 )`

- MOD -- 求余数

语法：`MOD( 被除数，除数 )`

- ROUND -- 四舍五入

语法：`ROUND( 对象数值，保留小数的位数 )`

#### 字符串函数

- CONCAT -- 拼接

语法：`CONCAT(str1, str2, str3)`

- LENGTH -- 字符串长度

语法：`LENGTH( 字符串 )`

- LOWER -- 小写转换
- UOOER -- 大写转换
- REPLACE -- 字符串的替换

语法：`REPLACE( 对象字符串，替换前的字符串，替换后的字符串 )`

- SUBSTRING -- 字符串的截取

语法：`SUBSTRING （对象字符串 FROM 截取的起始位置 FOR 截取的字符数）`

使用 SUBSTRING 函数 可以截取出字符串中的一部分字符串。截取的起始位置从字符串最左侧开始计算，索引值起始为1。

- **SUBSTRING_INDEX -- 字符串按索引截取**

语法：`SUBSTRING_INDEX (原始字符串， 分隔符，n)`

- **REPEAT -- 字符串按需重复多次**

语法：`REPEAT(string, number)`

#### 日期函数

- CURRENT_DATE -- 获取当前日期
- CURRENT_TIME -- 当前时间
- CURRENT_TIMESTAMP -- 当前日期和时间
- EXTRACT -- 截取日期元素

语法：`EXTRACT(日期元素 FROM 日期)`

#### 转换函数

- CAST -- 类型转换

语法：`CAST（转换前的值 AS 想要转换的数据类型）`

- COALESCE -- 将NULL转换为其他值

语法：`COALESCE(数据1，数据2，数据3……)`

### 3.4 谓词

谓词就是返回值为真值的函数。包括`TRUE / FALSE / UNKNOWN`。

谓词主要有以下几个：

- LIKE
- BETWEEN
- IS NULL、IS NOT NULL
- IN
- EXISTS

#### LIKE谓词

当需要进行字符串的部分一致查询时需要使用该谓词。

部分一致大体可以分为前方一致、中间一致和后方一致三种类型。

前方一致：xxx%

中间一致：%xxx%

后方一致：%xxx

例如

```mysql
SELECT *
FROM samplelike
WHERE strcol LIKE 'ddd%';
```



`%`是代表“零个或多个任意字符串”的特殊符号

#### BETWEEN谓词

使用 BETWEEN 可以进行范围查询。

```mysql
-- 选取销售单价为100～ 1000元的商品
SELECT product_name, sale_price
FROM product
WHERE sale_price BETWEEN 100 AND 1000;
```

BETWEEN 的特点就是结果中会包含 100 和 1000 这两个临界值，也就是闭区间。如果不想让结果中包含临界值，那就必须使用 < 和 >。



#### IS NULL、 IS NOT NULL 

为了选取出某些值为 NULL 的列的数据，不能使用 =，而只能使用特定的谓词IS NULL。

#### IN谓词

用来替换OR

```mysql
SELECT product_name, purchase_price
FROM product
WHERE purchase_price IN (320, 500, 5000);
```

**子查询的结果可以作为IN谓词的参数**

```mysql
SELECT product_name, sale_price
FROM product
WHERE product_id IN (SELECT product_id
  FROM shopproduct
                       WHERE shop_id = '000C');
```

#### EXIST 谓词

EXIT的作用就是 **“判断是否存在满足某种条件的记录”**。如果存在这样的记录就返回真（TRUE），如果不存在就返回假（FALSE）。EXIST（存在）谓词的主语是“记录”。

```mysql
SELECT product_name, sale_price
  FROM product AS p
 WHERE EXISTS (SELECT *
                 FROM shopproduct AS sp
                WHERE sp.shop_id = '000C'
                  AND sp.product_id = p.product_id);
```

### 3.5 CASE表达式

CASE 表达式是在区分情况时使用的，这种情况的区分在编程中通常称为（条件）分支。

```mysql
CASE WHEN <求值表达式> THEN <表达式>
     WHEN <求值表达式> THEN <表达式>
     WHEN <求值表达式> THEN <表达式>
     .
     .
     .
ELSE <表达式>
END
```



------



## 第二部分 习题解答

### 01.

创建出满足下述三个条件的视图（视图名称为 ViewPractice5_1）。使用 product（商品）表作为参照表，假设表中包含初始状态的 8 行数据。

- 条件 1：销售单价大于等于 1000 日元。
- 条件 2：登记日期是 2009 年 9 月 20 日。
- 条件 3：包含商品名称、销售单价和登记日期三列。

```mysql
CREATE VIEW ViewPractice5_1 AS
SELECT 
	product_name,
	sale_price,
	regist_date
FROM product
WHERE
	sale_price > 1000 AND regist_date = "2009-09-20"
```

![](https://files.catbox.moe/a4isus.png)

### 02.

向习题一中创建的视图 `ViewPractice5_1` 中插入如下数据，会得到什么样的结果？为什么？

```mysql
INSERT INTO ViewPractice5_1 VALUES (' 刀子 ', 300, '2009-11-02');
```

> 插入失败，因为向视图插入内容相当于对原表插入，对于原表来说，只提供三个值无法构成一行，即有的列内容是必须的。

### 03.

请根据如下结果编写 SELECT 语句，其中 sale_price_avg 列为全部商品的平均销售单价。

![](https://oss.linklearner.com/wonderful-sql/ch03/ch03.10-1-sale_price_avg.png)

```mysql
SELECT 
	product_id,
	product_name,
	product_type,
	sale_price,
	(
		SELECT AVG(sale_price)
		FROM product
	) AS sale_price_avg
FROM product
```

![](https://files.catbox.moe/gfcfdt.png)

### 04.

请根据习题一中的条件编写一条 SQL 语句，创建一幅包含如下数据的视图（名称为AvgPriceByType）。

![](https://oss.linklearner.com/wonderful-sql/ch03/ch03.10-2-sale_price_avg_type.png)

```mysql
CREATE VIEW AvgPriceByType AS
SELECT 
	product_id,
	product_name,
	product_type,
	sale_price,
	(
		SELECT AVG(sale_price)
		FROM product
		WHERE product_type = p.product_type
	) AS sale_price_avg_type
FROM product p
```

<img src="https://files.catbox.moe/n0bl39.png" style="zoom:80%;" />

### 05.

运算中含有 NULL 时，运算结果是否必然会变为NULL ？

> 不是
>
> null在参与算术运算(+ - * \)的时候，结果为null。
>
> null在参与比较运算（>,<,=,>=,<=,<> ）的时候，结果为false。

### 06.

对本章中使用的 `product`（商品）表执行如下 2 条 `SELECT` 语句，能够得到什么样的结果呢？

```mysql
SELECT product_name, purchase_price
  FROM product
 WHERE purchase_price NOT IN (500, 2800, 5000);
```

<img src="https://files.catbox.moe/czcfwj.png" style="zoom:67%;" />

```mysql
SELECT product_name, purchase_price
  FROM product
 WHERE purchase_price NOT IN (500, 2800, 5000, NULL);
```

> 在使用IN 和 NOT IN 时是无法选取出NULL数据的。 NULL 只能使用 IS NULL 和 IS NOT NULL 来进行判断。

### 07.

按照销售单价( `sale_price` )对练习 3.6 中的 `product`（商品）表中的商品进行如下分类。

- 低档商品：销售单价在1000日元以下（T恤衫、办公用品、叉子、擦菜板、 圆珠笔）
- 中档商品：销售单价在1001日元以上3000日元以下（菜刀）
- 高档商品：销售单价在3001日元以上（运动T恤、高压锅）

请编写出统计上述商品种类中所包含的商品数量的 SELECT 语句，结果如下所示。

执行结果

![](https://files.catbox.moe/2nof32.png)

```mysql
SELECT 
	COUNT(CASE WHEN sale_price <= 1000 THEN product_id ELSE NULL END) AS low_price,
	COUNT(CASE WHEN sale_price BETWEEN 1001 AND 3000 THEN product_id ELSE NULL END) AS mid_price,
	COUNT(CASE WHEN sale_price > 3000 THEN product_id ELSE NULL END) AS high_price
FROM product
```

![](https://files.catbox.moe/bhh5zq.png)