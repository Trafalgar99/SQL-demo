### 5.1

请说出针对本章中使用的 product（商品）表执行如下 SELECT 语句所能得到的结果。

```mysql
SELECT  product_id
       ,product_name
       ,sale_price
       ,MAX(sale_price) OVER (ORDER BY product_id) AS Current_max_price
  FROM product;
```

> 之前n行+自身行的最高价格

### 5.2

继续使用product表，计算出按照登记日期（regist_date）升序进行排列的各日期的销售单价（sale_price）的总额。排序是需要将登记日期为NULL 的“运动 T 恤”记录排在第 1 位（也就是将其看作比其他日期都早）

```mysql
SELECT
	DISTINCT regist_date, 
	SUM(sale_price) over (PARTITION BY regist_date ORDER BY regist_date)
FROM product
```



### 5.3

思考题

① 窗口函数不指定PARTITION BY的效果是什么？

> 得到指定列的排序、聚合等结果。

② 为什么说窗口函数只能在SELECT子句中使用？实际上，在ORDER BY 子句使用系统并不会报错。

> 窗口函数是对WHERE或者GROUP BY子句处理后的结果进行操作，所以窗口函数原则上只能写在SELECT子句中。
> ORDER BY子句中能够使用窗口函数的原因（UPDATE的SET子句中也能够使用窗口函数）是因为ORDER BY子句会在SELECT子句之后执行，并且记录也不会减少。

### 5.4

使用简洁的方法创建20个与 `shop.product` 表结构相同的表，如下图所示：

```mysql
DELIMITER $$
DROP PROCEDURE IF EXISTS world.p
CREATE DEFINER=`root`@`localhost` PROCEDURE `world`.`p`()
BEGIN
DECLARE i INT;
DECLARE table_name VARCHAR(20);
DECLARE table_pre VARCHAR(20);
DECLARE sql_text VARCHAR(2000);
SET i=1;
SET table_name='';
SET table_pre='table';
SET sql_text='';
WHILE i<21 DO
IF i<10 THEN SET table_name=CONCAT(table_pre,'0',i);
ELSE SET table_name=CONCAT(table_pre,i);
END IF;
SET sql_text=CONCAT('CREATE TABLE ', table_name, '(product_id CHAR(4) NOT NULL,
 product_name VARCHAR(100) NOT NULL,
 product_type VARCHAR(32) NOT NULL,
 sale_price INTEGER ,
 purchase_price INTEGER ,
 regist_date DATE ,
 PRIMARY KEY (product_id))');
SELECT sql_text;
SET @sql_text=sql_text;
PREPARE stmt FROM @sql_text;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
SET i=i+1;
END WHILE;
END$$
DELIMITER ;
CALL p();
```

