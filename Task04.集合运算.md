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



