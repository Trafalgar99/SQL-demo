# 第0章：环境搭建

> 本章重点
>
> + 在电脑上安装MySQL数据库系统
> + 安装客户端并连接到本机上的MySQL数据库
> + 使用提供的脚本创建本教程所使用的示例数据库

## 1. MySQL的安装

由于作者在此之前已经使用过MySQL数据库，所以此处就不再重复安装了。

[安装教程]([DATAWHALE - 一个热爱学习的社区 (linklearner.com)](https://linklearner.com/datawhale-homepage/index.html#/learn/detail/70))

## 2. 连接 MySQL 并执行 SQL 查询

### 2.0 使用命令行方式连接MySQL服务

首先，可以通过本教程的方式，打开对应的MySQL命令行工具，然后输入密码即可进入数据库。另一种作者更熟悉的命令登录方式如下。

1. 打开powershell

<img src="https://files.catbox.moe/co5wha.png" style="zoom:50%;" />

如上图所示，输入`mysql -u[用户名] -p[密码]`即可登录（也可以不输入密码，则接下来会要求您主动输入密码）。

2. 数据库连接成功

<img src="https://files.catbox.moe/djqnz3.png" style="zoom:50%;" />

### 2.1使用Navicat连接MySQL

作者在事先已经下载了Navicat，所以在这里选择使用Navicat连接数据库。

1. 首先打开Navicat，初始界面如图所示



<img src=".\images\1660652193459.jpg" style="zoom:50%;" />

2. 点击左上角的连接，选择MySQL，然后填写信息

<img src=".\images\1660652396912.jpg" style="zoom:50%;" />

其中主机和端口均为默认值，其他选项根据数据库实际情况决定。

3. 双击连接名即可查看数据库

<img src=".\images\1660652507745.jpg" style="zoom:50%;" />

## 3. 创建学习用的数据库



在这里选择在Navicat里运行sql文件。

1. 首先按照上文的方法登录MySQL

2. 右键连接名字，选择运行sql文件，选中shop.sql

<img src=".\images\1660653584510.jpg" style="zoom:50%;" />

3. 查看数据库

<img src=".\images\1660653734595.jpg" style="zoom:50%;" />

------

# 第一章 初识数据库

## 1.1 初识数据库

数据库管理系统的五种类型：

1. 层次数据库（Hierarchical Database，HDB）
2. **关系数据库（Relational Database，RDB）**
3. 面向对象数据库（Object Oriented Database，OODB）
4. XML数据库（XML Database，XMLDB）
5. 键值存储系统（Key-Value Store，KVS），举例：MongoDB

## 1.2 初识SQL

数据库中存储的表结构类似于excel中的行和列，在数据库中，行称为**记录**，它相当于一条记录，列称为**字段**，它代表了表中存储的数据项目。

行和列交汇的地方称为**单元格**，一个单元格中只能输入一条记录。

SQL分为以下三类：

1. **DDL** ：DDL（Data Definition Language，数据定义语言） 
   - CREATE ： 创建数据库和表等对象
   - DROP ： 删除数据库和表等对象
   - ALTER ： 修改数据库和表等对象的结构

2. **DML** :DML（Data Manipulation Language，数据操纵语言） 用来查询或者变更表中的记录。DML 包含以下几种指令。
   - SELECT ：查询表中的数据
   - INSERT ：向表中插入新数据
   - UPDATE ：更新表中的数据
   - DELETE ：删除表中的数据

3. **DCL** ：DCL（Data Control Language，数据控制语言） 
   - COMMIT ： 确认对数据库中的数据进行的变更
   - ROLLBACK ： 取消对数据库中的数据进行的变更
   - GRANT ： 赋予用户操作权限
   - REVOKE ： 取消用户的操作权限

### 1.2.1 书写规则

- SQL语句要以分号（ ; ）结尾
- SQL 不区分关键字的大小写，但是插入到表中的数据是区分大小写的
- win 系统默认不区分表名及字段名的大小写
- linux / mac 默认严格区分表名及字段名的大小写
  \* 本教程已统一调整表名及字段名的为小写，以方便初学者学习使用。
- 常数的书写方式是固定的，'abc', 1234, '26 Jan 2010', '10/01/26', '2010-01-26'......

- 单词需要用半角空格或者换行来分隔

### 1.2.2 数据库创建

创建数据库

```mysql
CREATE DATABASE < 数据库名称 > ;
```

### 1.2.3创建表

创建表

```mysql
CREATE TABLE < 表名 >
( < 列名 1> < 数据类型 > < 该列所需约束 > ,
  < 列名 2> < 数据类型 > < 该列所需约束 > ,
  < 列名 3> < 数据类型 > < 该列所需约束 > ,
  < 列名 4> < 数据类型 > < 该列所需约束 > ,
  .
  .
  .
  < 该表的约束 1> , < 该表的约束 2> ,……);
```

### 1.2.4 命名规则

- 只能使用半角英文字母、数字、下划线（_）作为数据库、表和列的名称
- 名称必须以半角英文字母开头

### 1.2.5 数据类型的指定

1. INTEGER 型
2. CHAR 型
3. VARCHAR 型
4. DATE 型

### 1.2.6 约束的设置

`NOT NULL`是非空约束，即该列必须输入数据。

`PRIMARY KEY`是主键约束，代表该列是唯一值，可以通过该列取出特定的行的数据。

### 1.2.7 表的删除和更新

删除表

```mysql
DROP TABLE < 表名 > ;
```

添加列的 ALTER TABLE 语句

```mysql
ALTER TABLE < 表名 > ADD COLUMN < 列的定义 >;
```

删除列的 ALTER TABLE 语句

```mysql
ALTER TABLE < 表名 > DROP COLUMN < 列名 >;
```

删除表中特定的行

```mysql
-- 一定注意添加 WHERE 条件，否则将会删除所有的数据
DELETE FROM product WHERE COLUMN_NAME='XXX';
```

清空表内容（相比`drop / delete`，`truncate`用来清除数据时，速度最快。）

```mysql
TRUNCATE TABLE TABLE_NAME;
```

数据的更新

```mysql
UPDATE <表名>
   SET <列名> = <表达式> [, <列名2>=<表达式2>...];  
 WHERE <条件>;  -- 可选，非常重要。
 ORDER BY 子句;  --可选
 LIMIT 子句; --可选
```

### 1.2.8 向 product 表中插入数据

基本语法

```mysql
INSERT INTO <表名> (列1, 列2, 列3, ……) VALUES (值1, 值2, 值3, ……);
```

可以通过在创建表的CREATE TABLE 语句中设置DEFAULT约束来设定默认值。

```mysql
CREATE TABLE productins
(product_id CHAR(4) NOT NULL,
（略）
sale_price INTEGER
（略）	DEFAULT 0, -- 销售单价的默认值设定为0;
PRIMARY KEY (product_id));
```

可以使用INSERT … SELECT 语句从其他表复制数据。

```mysql
-- 将商品表中的数据复制到商品复制表中
INSERT INTO productcopy (product_id, product_name, product_type, sale_price, purchase_price, regist_date)
SELECT product_id, product_name, product_type, sale_price, purchase_price, regist_date
  FROM Product;
```

### 1.2.9 索引

创建表时可以直接创建索引，语法如下：

```mysql
CREATE TABLE mytable(  
 
ID INT NOT NULL,   
 
username VARCHAR(16) NOT NULL,  
 
INDEX [indexName] (username(length))  
 
);
```

也可以使用如下语句创建：

```mysql
-- 方法1
CREATE INDEX indexName ON table_name (column_name)

-- 方法2
ALTER table tableName ADD INDEX indexName(columnName)
```

## 练习题

**1.1 编写一条 CREATE TABLE 语句，用来创建一个包含表 1-A 中所列各项的表 Addressbook （地址簿），并为 regist_no （注册编号）列设置主键约束**

```mysql
CREATE TABLE addressbook
(regist_no INTEGER NOT NULL,
 name VARCHAR(128) NOT NULL,
 address VARCHAR(256) NOT NULL,
 tel_no CHAR(10) ,
 mail_address char(20) ,
 PRIMARY KEY (regist_no));
```

<img src=".\images\1660658319448.jpg" style="zoom:50%;" />

**1.2假设在创建练习1.1中的 Addressbook 表时忘记添加如下一列 postal_code （邮政编码）了，请编写 SQL 把此列添加到 Addressbook 表中。**

**列名 ： postal_code**

**数据类型 ：定长字符串类型（长度为 8）**

**约束 ：不能为 NULL**

```mysql
 ALTER TABLE addressbook ADD COLUMN postal_code CHAR(8) NOT NULL;
```

<img src=".\images\1660658618912.jpg" style="zoom:50%;" />

**1.3 填空题**

**请补充如下 SQL 语句来删除 Addressbook 表**

```mysql
(  DROP  ) table Addressbook;
```

**1.4 判断题**

**是否可以编写 SQL 语句来恢复删除掉的 Addressbook 表？**

> 不可以，但是可以通过binlog恢复。

