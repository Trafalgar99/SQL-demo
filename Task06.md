## Section A

### 练习一: 各部门工资最高的员工（难度：中等）

创建Employee 表，包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id。

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
+----+-------+--------+--------------+
```

创建Department 表，包含公司所有部门的信息。

```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

编写一个 SQL 查询，找出每个部门工资最高的员工。例如，根据上述给定的表格，Max 在 IT 部门有最高工资，Henry 在 Sales 部门有最高工资。

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| Sales      | Henry    | 80000  |
+------------+----------+--------+
```

```mysql
USE test;
CREATE TABLE Employee (
Id INTEGER NOT NULL,
Name VARCHAR(10) NOT NULL,
Salary INTEGER NOT NULL,
DepartmentId INTEGER NOT NULL,
PRIMARY KEY(Id)
);

INSERT INTO Employee VALUES(1,'Joe',70000,1);
INSERT INTO Employee VALUES(2,'Henry',80000,2);
INSERT INTO Employee VALUES(3,'Sam',60000,2);
INSERT INTO Employee VALUES(4,'Max',90000,1);

CREATE TABLE Department(
Id INTEGER NOT NULL,
Name VARCHAR(10) NOT NULL,
PRIMARY KEY(Id)
);

INSERT INTO Department VALUES(1,'IT');
INSERT INTO Department VALUES(2,'Sales');

```

```mysql
USE test;
SELECT Department.Name AS Department,
		Employee.Name AS Employee,
		Employee.Salary AS Salary
	FROM Employee
	INNER JOIN Department
	ON Employee.DepartmentId =Department.Id
WHERE Employee.Salary IN 
(SELECT MAX(Employee.Salary) FROM Employee
GROUP BY DepartmentId)
ORDER BY Employee.Salary DESC

```

### 练习二: 换座位（难度：中等）

小美是一所中学的信息科技老师，她有一张 seat 座位表，平时用来储存学生名字和与他们相对应的座位 id。

其中纵列的**id**是连续递增的

小美想改变相邻俩学生的座位。

你能不能帮她写一个 SQL query 来输出小美想要的结果呢？

请创建如下所示seat表：

**示例：**

```
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
```

假如数据输入的是上表，则输出结果如下：

```
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |
+---------+---------+
```

**注意：**
如果学生人数是奇数，则不需要改变最后一个同学的座位。

```mysql
SELECT
    (CASE MOD(id,2)
         WHEN 0 THEN id - 1
		 WHEN (1 AND id != (SELECT MAX(id) FROM seat)) THEN id + 1
	 ELSE id END) AS id,student
FROM seat
ORDER BY id;
```

### 练习三: 分数排名（难度：中等）

假设在某次期末考试中，二年级四个班的平均成绩分别是 `93、93、93、91`，请问可以实现几种排名结果？分别使用了什么函数？排序结果是怎样的？（只考虑降序）

```
+-------+-----------+
| class | score_avg |
+-------+-----------+
|    1  |       93  |
|    2  |       93  |
|    3  |       93  |
|    4  |       91  |
```

```mysql
SELECT 
    *
FROM
    avg_score
ORDER BY score_avg desc;
```

```mysql
SELECT 
	class, 
	score_avg, 
	rank() over (ORDER BY score_avg DESC) AS ranking 
FROM 
	avg_score;
```

### 练习四：连续出现的数字（难度：中等）

编写一个 SQL 查询，查找所有至少连续出现三次的数字。

```
+----+-----+
| Id | Num |
+----+-----+
| 1  |  1  |
| 2  |  1  |
| 3  |  1  |
| 4  |  2  |
| 5  |  1  |
| 6  |  2  |
| 7  |  2  |
+----+-----+
```

例如，给定上面的 Logs 表， 1 是唯一连续出现至少三次的数字。

```
+-----------------+
| ConsecutiveNums |
+-----------------+
| 1               |
```

```

```

### 练习五：树节点 （难度：中等）

对于**tree**表，*id*是树节点的标识，*p_id*是其父节点的*id*。

```
+----+------+
| id | p_id |
+----+------+
| 1  | null |
| 2  | 1    |
| 3  | 1    |
| 4  | 2    |
| 5  | 2    |
+----+------+
```

每个节点都是以下三种类型中的一种：

- Root: 如果节点是根节点。
- Leaf: 如果节点是叶子节点。
- Inner: 如果节点既不是根节点也不是叶子节点。

写一条查询语句打印节点id及对应的节点类型。按照节点id排序。上面例子的对应结果为：

```
+----+------+
| id | Type |
+----+------+
| 1  | Root |
| 2  | Inner|
| 3  | Leaf |
| 4  | Leaf |
| 5  | Leaf |
+----+------+
```

**说明**

- 节点’1’是根节点，因为它的父节点为NULL，有’2’和’3’两个子节点。
- 节点’2’是内部节点，因为它的父节点是’1’，有子节点’4’和’5’。
- 节点’3’，‘4’，'5’是叶子节点，因为它们有父节点但没有子节点。

下面是树的图形：

```
    1         
  /   \ 
 2    3    
/ \
4  5
```

**注意**

如果一个树只有一个节点，只需要输出根节点属性。

```

```

