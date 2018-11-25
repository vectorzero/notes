# SQL高级教程

## TOP 子句
用于规定要返回的记录的数目。

对于拥有数千条记录的大型表来说，TOP 子句是非常有用的。

注释：并非所有的数据库系统都支持 TOP 子句。

### SQL Server 的语法
<details>
<summary>点击查看</summary>

`SELECT TOP number|percent column_name(s) FROM table_name`

</details>

### MySQL 语法
<details>
<summary>点击查看</summary>

`SELECT column_name(s) FROM table_name LIMIT number`
</details>

### Oracle 语法
<details>
<summary>点击查看</summary>

`SELECT column_name(s) FROM table_name WHERE ROWNUM <= number`
</details>

### 实例
从 "Persons" 表中选取头两条记录。
<details>
<summary>点击查看</summary>

`SELECT TOP 2 * FROM Persons`
</details>

从 "Persons" 表中选取 50% 的记录。
<details>
<summary>点击查看</summary>

`SELECT TOP 50 PERCENT * FROM Persons`
</details>

## LIKE 操作符
用于在 WHERE 子句中搜索列中的指定模式。
<details>
<summary>点击查看</summary>

`SELECT column_name(s) FROM table_name WHERE column_name LIKE pattern`
</details>

### 实例
从 "Persons" 表中选取居住在以 "N" 开始的城市里的人：

`SELECT * FROM Persons WHERE City LIKE 'N%'`

提示："%" 可用于定义通配符（模式中缺少的字母）。

从 "Persons" 表中选取居住在以 "g" 结尾的城市里的人：

`SELECT * FROM Persons WHERE City LIKE '%g'`

从 "Persons" 表中选取居住在包含 "lon" 的城市里的人：

`SELECT * FROM Persons WHERE City LIKE '%lon%'`

从 "Persons" 表中选取居住在不包含 "lon" 的城市里的人：

`SELECT * FROM Persons WHERE City NOT LIKE '%lon%'`

## 通配符
在搜索数据库中的数据时，SQL 通配符可以替代一个或多个字符。

SQL 通配符必须与 LIKE 运算符一起使用。

在 SQL 中，可使用以下通配符：

|通配符	|描述	|
|----------|---------|
|%	    |替代一个或多个字符	    |
|_	    |仅替代一个字符	    |
|[charlist]	    |字符列中的任何单一字符	    |
|[^charlist]或者[!charlist]    |不在字符列中的任何单一字符	|

### 使用 % 通配符
从 "Persons" 表中选取居住在以 "Ne" 开始的城市里的人：

`SELECT * FROM Persons WHERE City LIKE 'Ne%'`

从 "Persons" 表中选取居住在包含 "lond" 的城市里的人：

`SELECT * FROM Persons WHERE City LIKE '%lond%'`

### 使用 _ 通配符
从 "Persons" 表中选取名字的第一个字符之后是 "eorge" 的人：

`SELECT * FROM Persons WHERE FirstName LIKE '_eorge'`

从 "Persons" 表中选取的这条记录的姓氏以 "C" 开头，然后是一个任意字符，然后是 "r"，然后是任意字符，然后是 "er"：

`SELECT * FROM Persons WHERE LastName LIKE 'C_r_er'`

### 使用 [charlist] 通配符

从 "Persons" 表中选取居住的城市以 "A" 或 "L" 或 "N" 开头的人：

`SELECT * FROM Persons WHERE City LIKE '[ALN]%'`

从 "Persons" 表中选取居住的城市不以 "A" 或 "L" 或 "N" 开头的人：

`SELECT * FROM Persons WHERE City LIKE '[!ALN]%'`

## IN 操作符
允许我们在 WHERE 子句中规定多个值。

<details>
<summary>点击查看</summary>

`SELECT column_name(s) FROM table_name WHERE column_name IN (value1,value2,...)`

</details>

### 实例
从表中选取姓氏为 Adams 和 Carter 的人：

`SELECT * FROM Persons WHERE LastName IN ('Adams','Carter')`

## BETWEEN 操作符
操作符 `BETWEEN ... AND` 会选取介于两个值之间的数据范围。这些值可以是数值、文本或者日期。

<details>
<summary>点击查看</summary>

`SELECT column_name(s) FROM table_name WHERE column_name BETWEEN value1 AND value2`

</details>

### 实例
以字母顺序显示介于 "Adams"（包括）和 "Carter"（不包括）之间的人：

`SELECT * FROM Persons WHERE LastName BETWEEN 'Adams' AND 'Carter'`

如需使用上面的例子显示范围之外的人，请使用 `NOT` 操作符：

`SELECT * FROM Persons WHERE LastName NOT BETWEEN 'Adams' AND 'Carter'`

## Alias（别名）
可以为列名称和表名称指定别名

### 表的 SQL Alias 语法

<details>
<summary>点击查看</summary>

`SELECT column_name(s) FROM table_name AS alias_name`

</details>

### 列的 SQL Alias 语法

<details>
<summary>点击查看</summary>

`SELECT column_name AS alias_name FROM table_name`

</details>

### 实例
使用一个列名别名：

`SELECT LastName AS Family, FirstName AS Name FROM Persons`

## JOIN

## INNER JOIN 关键字

## LEFT JOIN 关键字

## RIGHT JOIN 关键字

## FULL JOIN 关键字

## UNION 和 UNION ALL 操作符

## SELECT INTO 语句

## CREATE DATABASE 语句

## CREATE TABLE 语句

## 约束 (Constraints)

## NOT NULL 约束

## UNIQUE 约束

## PRIMARY KEY 约束

## FOREIGN KEY 约束

## CHECK 约束

## DEFAULT 约束

## CREATE INDEX 语句

## 撤销索引、表以及数据库

## ALTER TABLE 语句

## AUTO INCREMENT 字段

## VIEW（视图）

## Date 函数

## NULL 值

## NULL 函数

## 数据类型

## 服务器 - RDBMS