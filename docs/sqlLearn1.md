# SQL基础教程

## 基本

### 数据操作语言 (DML)
* `SELECT` - 从数据库表中获取数据
* `UPDATE` - 更新数据库表中的数据
* `DELETE` - 从数据库表中删除数据
* `INSERT INTO` - 向数据库表中插入数据

### 数据定义语言 (DDL)
* `CREATE DATABASE` - 创建新数据库
* `ALTER DATABASE` - 修改数据库
* `CREATE TABLE` - 创建新表
* `ALTER TABLE` - 变更（改变）数据库表
* `DROP TABLE` - 删除表
* `CREATE INDEX` - 创建索引（搜索键）
* `DROP INDEX` - 删除索引

## SQL SELECT 语句
SELECT 语句用于从表中选取数据。

结果被存储在一个结果表中（称为结果集）。

### 语法
`SELECT 列名称(可多个，用逗号隔开) FROM 表名称`

`SELECT * FROM 表名称`

## SQL SELECT DISTINCT 语句
在表中，可能会包含重复值。这并不成问题，不过，有时您也许希望仅仅列出不同（distinct）的值。

关键词 DISTINCT 用于返回唯一不同的值。

### 语法
`SELECT DISTINCT 列名称 FROM 表名称`

### 实例
| Company  |  OrderNumber  |
|----------|:-------------:|
| IBM | 3532 |
| W3School | 2356   |
| Apple | 4698 |
| W3School | 6953 |

`SELECT Company FROM Orders`

| Company  |
|----------|
| IBM |
| W3School 
| Apple |
| W3School |

`SELECT DISTINCT Company FROM Orders`

| Company  |
|----------|
| IBM |
| W3School 
| Apple |

## WHERE 子句
如需有条件地从表中选取数据，可将 WHERE 子句添加到 SELECT 语句。

### 语法
`SELECT 列名称 FROM 表名称 WHERE 列 运算符 值`

|操作符|描述|
|----------|---------|
|=	     |等于|
|<>	     |不等于|
|>	     |大于|
|<	     |小于|
|>=	     |大于等于|
|<=	     |小于等于|
|BETWEEN |在某个范围内|
|LIKE	 |搜索某种模式|

注释：在某些版本的 SQL 中，操作符 `<>` 可以写为 `!=`。

### 实例
`SELECT * FROM Persons WHERE City='Beijing'`

#### 引号的使用
SQL 使用单引号来环绕文本值（大部分数据库系统也接受双引号）。

如果是数值，请不要使用引号。

## AND 和 OR 运算符
`AND` 和 `OR` 可在 `WHERE` 子语句中把两个或多个条件结合起来。

如果第一个条件和第二个条件都成立，则 `AND` 运算符显示一条记录。

如果第一个条件和第二个条件中只要有一个成立，则 `OR` 运算符显示一条记录。

### 实例
|LastName	|FirstName	|Address	    |City    |
|----------|---------|---------|---------|
|Adams	    |John	    |Oxford Street	|London  |
|Bush	    |George	    |Fifth Avenue	|New York|
|Carter	    |Thomas	    |Changan Street	|Beijing |
|Carter	    |William	|Xuanwumen 10	|Beijing |


使用 `AND` 来显示所有姓为 "Carter" 并且名为 "Thomas" 的人：

`SELECT * FROM Persons WHERE FirstName='Thomas' AND LastName='Carter'`

|LastName	|FirstName	|Address	    |City   |
|----------|---------|---------|---------|
|Carter	    |Thomas	    |Changan Street	|Beijing|

使用 `OR` 来显示所有姓为 "Carter" 或者名为 "Thomas" 的人：

`SELECT * FROM Persons WHERE firstname='Thomas' OR lastname='Carter'`

|LastName	|FirstName	|Address	    |City   |
|----------|---------|---------|---------|
|Carter	    |Thomas	    |Changan Street	|Beijing|
|Carter	    |William    |Xuanwumen 10	|Beijing|

结合 `AND` 和 `OR` 运算符

`SELECT * FROM Persons WHERE (FirstName='Thomas' OR FirstName='William') AND LastName='Carter'`

|LastName	|FirstName	|Address	    |City   |
|----------|---------|---------|---------|
|Carter	    |Thomas 	|Changan Street	|Beijing|
|Carter	    |William	|Xuanwumen 10	|Beijing|

## ORDER BY 语句
`ORDER BY` 语句用于根据指定的列对结果集进行排序。

`ORDER BY` 语句默认按照升序对记录进行排序。

如果您希望按照降序对记录进行排序，可以使用 `DESC` 关键字。

### 实例
以字母顺序显示公司名称：

`SELECT Company, OrderNumber FROM Orders ORDER BY Company`

以字母顺序显示公司名称（Company），并以数字顺序显示顺序号（OrderNumber）：

`SELECT Company, OrderNumber FROM Orders ORDER BY Company, OrderNumber`

以逆字母顺序显示公司名称：

`SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC`

以逆字母顺序显示公司名称，并以数字顺序显示顺序号：

`SELECT Company, OrderNumber FROM Orders ORDER BY Company DESC, OrderNumber ASC`

## INSERT INTO 语句
`INSERT INTO` 语句用于向表格中插入新的行。

### 语法
`INSERT INTO 表名称 VALUES (值1, 值2,....)`

我们也可以指定所要插入数据的列：

`INSERT INTO table_name (列1, 列2,...) VALUES (值1, 值2,....)`

### 实例
插入新的行

`INSERT INTO Persons VALUES ('Gates', 'Bill', 'Xuanwumen 10', 'Beijing')`

在指定的列中插入数据

`INSERT INTO Persons (LastName, Address) VALUES ('Wilson', 'Champs-Elysees')`

## Update 语句

`Update` 语句用于修改表中的数据。

### 语法
`UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值`

### 实例
更新某一行中的一个列

`UPDATE Person SET FirstName = 'Fred' WHERE LastName = 'Wilson' `

更新某一行中的若干列
`UPDATE Person SET Address = 'Zhongshan 23', City = 'Nanjing' WHERE LastName = 'Wilson'`

## DELETE 语句
`DELETE` 语句用于删除表中的行。

### 语法
`DELETE FROM 表名称 WHERE 列名称 = 值`

### 实例
删除某行

`DELETE FROM Person WHERE LastName = 'Wilson' `

删除所有行

`DELETE FROM table_name`

或者

`DELETE * FROM table_name`