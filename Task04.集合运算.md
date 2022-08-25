# 第四章 集合运算

## 第一部分 知识提要

### 4.1 表的加减法

#### 表的加法--UNION

```mysql
SELECT product_id, product_name
  FROM product
 UNION
SELECT product_id, product_name
  FROM product2;
```

会将两个查询得出的所有的行合并（去重）

#### 包含重复行的集合运算 UNION ALL

```mysql
-- 保留重复行
SELECT product_id, product_name
  FROM Product
 UNION ALL
SELECT product_id, product_name
  FROM Product2;
```

重复的行也会被保留



#### 隐式数据类型转换

通常来说, 我们会把类型完全一致, 并且代表相同属性的列使用 UNION 合并到一起显示, 但有时候, 即使数据类型不完全相同, 也会通过隐式类型转换来将两个类型不同的列放在一列里显示

#### 差集,补集与表的减法

借助学过的NOT IN 谓词, 可以实现表的减法。

```mysql
-- 使用 NOT IN 子句的实现方法
SELECT * 
  FROM Product
 WHERE product_id NOT IN (SELECT product_id 
                            FROM Product2)
```

对于同一个表的两个查询结果而言, 他们的交INTERSECT实际上可以等价地将两个查询的检索条件用AND谓词连接来实现。

#### 对称差

两个集合A,B的对称差是指那些仅属于A或仅属于B的元素构成的集合.从直观上就能看出来, 两个集合的对称差等于 A-B并上B-A，因此可以用NOT IN 求差集再用UNION求并，即可得到对称差

```mysql
-- 使用 NOT IN 实现两个表的差集
SELECT * 
  FROM Product
 WHERE product_id NOT IN (SELECT product_id FROM Product2)
UNION
SELECT * 
  FROM Product2
 WHERE product_id NOT IN (SELECT product_id FROM Product)
```

### 4.2 连接（JOIN）

连结(JOIN)就是使用某种关联条件(一般是使用相等判断谓词"="), 将其他表中的列添加过来, 进行“添加列”的集合运算. 

#### 内连结(INNER JOIN)

```mysql
-- 内连结
FROM <tb_1> INNER JOIN <tb_2> ON <condition(s)>
```

其中 INNER 关键词表示使用了内连结

注意：

1. **进行连结时需要在 FROM 子句中使用多张表.**
2. **必须使用 ON 子句来指定连结条件.**
3. **SELECT 子句中的列最好按照 表名.列名 的格式来使用。**

#### 自连结(SELF JOIN)

际上一张表也可以与自身作连结, 这种连接称之为自连结.自连结并不是区分于内连结和外连结的第三种连结, 自连结可以是外连结也可以是内连结, 它是不同于内连结外连结的另一个连结的分类方法。

#### 自然连结(NATURAL JOIN)

自然连结并不是区别于内连结和外连结的第三种连结, 它其实是内连结的一种特例--当两个表进行自然连结时, 会按照两个表中都包含的列名来进行等值内连结, 此时无需使用 ON 来指定连接条件。

使用自然连结还可以求出两张表或子查询的公共部分。

#### 外连结(OUTER JOIN)

内连结会丢弃两张表中不满足 ON 条件的行,和内连结相对的就是外连结. 外连结会根据外连结的种类有选择地保留无法匹配到的行。

按照保留的行位于哪张表,外连结有三种形式: 左连结, 右连结和全外连结

```mysql
-- 左连结     
FROM <tb_1> LEFT  OUTER JOIN <tb_2> ON <condition(s)>
-- 右连结     
FROM <tb_1> RIGHT OUTER JOIN <tb_2> ON <condition(s)>
-- 全外连结
FROM <tb_1> FULL  OUTER JOIN <tb_2> ON <condition(s)>
```

**外连结要点 1: 选取出单张表中全部的信息**

 **外连结要点 2：使用 LEFT、RIGHT 来指定主表.**

#### 多表连结

通常连结只涉及 2 张表,但有时也会出现必须同时连结 3 张以上的表的情况, 原则上连结表的数量并没有限制。

#### 交叉连结—— CROSS JOIN(笛卡尔积)

在连结去掉 ON 子句, 就是所谓的交叉连结(CROSS JOIN), 交叉连结又叫笛卡尔积, 后者是一个数学术语. 	

交叉连结的语法有如下几种形式:

```mysql
-- 1.使用关键字 CROSS JOIN 显式地进行交叉连结
SELECT SP.shop_id
       ,SP.shop_name
       ,SP.product_id
       ,P.product_name
       ,P.sale_price
  FROM ShopProduct AS SP
 CROSS JOIN Product AS P;
-- 2.使用逗号分隔两个表,并省略 ON 子句
SELECT SP.shop_id
       ,SP.shop_name
       ,SP.product_id
       ,P.product_name
       ,P.sale_price
  FROM ShopProduct AS SP , Product AS P;
```

## 第二部分 练习题

### 4.1

找出 product 和 product2 中售价高于 500 的商品的基本信息。

```mysql
SELECT *
FROM product
WHERE sale_price > 500
UNION
SELECT *
FROM product2
WHERE sale_price > 500
```

<img src="https://files.catbox.moe/ipoy2m.png" style="zoom:67%;" />

### 4.2

借助对称差的实现方式, 求product和product2的交集。

```mysql
SELECT *
FROM product
WHERE product_id NOT IN (
	SELECT product_id 
  FROM product
		WHERE product_id NOT IN (SELECT product_id FROM product2)
)
```

![](https://files.catbox.moe/15ezc2.png)

### 4.3

每类商品中售价最高的商品都在哪些商店有售 ？

```

```



### 4.4

分别使用内连结和关联子查询每一类商品中售价最高的商品。

```mysql
SELECT
	p1.product_type,
	p2.max_price
FROM product p1
JOIN (
	SELECT 
		product_type,
		max(sale_price) max_price
	FROM product
	GROUP BY 1
) p2 
	ON p1.product_type = p2.product_type AND 
		 p1.sale_price = p2.max_price
```

```

```

![](https://files.catbox.moe/ckzq5x.png)

### 4.5

用关联子查询实现：在 product 表中，取出 product_id, product_name, sale_price, 并按照商品的售价从低到高进行排序、对售价进行累计求和。

```mysql
SELECT 
	p.product_id,
	p.product_name,
	p.product_type,
	p.sale_price,
	(
		SELECT SUM(sale_price) FROM product p1
		WHERE 
			p.sale_price > p1.sale_price OR 
			p.sale_price = p1.sale_price AND p.product_id <= p1.product_id
	) sum
FROM product AS p
ORDER BY sale_price,sum;
```

![](https://files.catbox.moe/nyxqxi.png)