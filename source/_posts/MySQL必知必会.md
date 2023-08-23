---
title: MySQL必知必会
date: 2023-08-21 09:27:39
tags:
  - MySQL
---

书籍《MySQL 必知必会》阅读笔记

# 第 1 章 了解 SQL

## 1.1 了解 SQL

### 1.1.1 什么是数据库？

<i style="color:red;">数据库（database）</i>:保存有组织的数据的容器（通常是一个文件或者是一组文件）
**注意：** 人们常用数据库这个术语来代表他们使用的数据库软件，这是不正确的。数据库软件应该称为 DBMS（数据库管理系统）。数据库是通过 DBMS 创建和操纵的容器。通常是我们使用 DBMS，它替我们访问数据库。

### 1.1.2 表

<i style="color:red;">表（table）</i>：某种特定类型数据的结构化清单。

数据库中的，每个表都有一个名字，用来标识自己。<b style="background:yellow;">此名字是唯一的</b>，这表示数据库中没有其他表具有相同的名字。

在相同数据库中不能出现相同的表名。

<i style="color:red;">模式（schema）</i>：关于数据库和表的布局及特性的信息。【表具有一些特性，这些特性定义了数据在表中如何存储，如 可以存储什么样的信息，数据如何分解，各部分信息如何命名 等等，描述表的这组信息就是所谓的模式】

### 1.1.3 列和数据类型

表由列组成，列中存储着表中某部分的信息。

<i style="color:red;">列（column）</i>：表中的一个字段。所有表都是由一个或多个列组成的。

<i style="color:red;">数据类型（datatype）</i>：所容许的数据的类型。每个表列都有相应的数据类型，它限制（或容许）该列中存储的数据。

### 1.1.4 行

表中的数据是按行存储的，所保存的每个记录存储在自己的行内。

<i style="color:red;">行（row）</i>：表中的一个记录。

你可能听到用户在提到行时将其成为记录（record）。在很大程度上，这两个术语是可以相互替代的，但从技术上来说，行才是正确的术语。

### 1.1.5 主键

<i style="color:red;">主键（primary key）</i>：一列（或一组列），其值能够唯一区分表中每个行。唯一标识表中每行的这个列（或这组列）成为主键。主键用来表示一个特定的行。没有主键，更新或者删除表中特定行很困难。

**注意：** 虽然并不是都需要主键，但大多数数据库设计人员都应该保证他们创建的每个表具有一个主键，以便于以后数据操纵和管理。

表中的任何列都可以作为主键，只要满足以下两个条件：

- 任意两行都不具有相同的主键值
- 每个行都必须具有一个主键值（主键列不允许 NULL 值）

主键最好习惯：除了 MySQL 强制实施的规则外，应该坚持的几个普遍认可的好习惯：

1. 不更新主键列中的值
2. 不重用主键列的值
3. 不在主键列中使用可能会更改的值

## 1.2 什么是 SQL

SQL 是结构化查询语言（Structure Query Language）的缩写。SQL 是一种专门用来与数据库通信的语言。

# 第 2 章 MySQL 简介

## 2.1 什么是 MySQL

数据的所有存储、检索、管理和处理实际上是由数据库软件————DBMS（数据库管理软件）完成的。MySQL 是一种 DBMS，即它是一种数据库软件。

## 2.1.1 客户机 - 服务器软件

DBMS 可以分为两类：一类为基于共享文件系统的 DBMS，另一类是基于客户机-服务器的 DBMS。

# 第 3 章 使用 MySQL

## 3.1 连接

为了连接到 MySQL，需要以下信息：

- 主机名（计算机名）- 如果连接到本地 MySQL 服务器，为`localhost`
- 端口（如果使用默认端口 3306 之外的端口）
- 一个合法的用户名
- 用户口令（如果需要）

## 3.2 选择数据库

为了使用名字为`crashcourse`数据库，应该输入以下内容： `USE crashcourse`
**记住：** 必须使用`USE`打开数据库，才能读取其中的数据。

## 3.3 了解数据库和表

数据库、表、列、用户、权限等的信息被存储在数据库和表中（MySQL 使用 MySQL 来存储这些信息）。不过，内部的表一般不直接访问。可以使用 MySQL 的`SHOW`命令来显示这些信息。

输入：`SHOW DATABASES;`

分析：返回可用数据库的一个列表，包含在这个列表中的可能是 MySQL 内部使用的数据库。

---

为了获得一个数据库内的表的列表，使用`SHOW TABLES;`

输入：`SHOW TABLES;`

分析：返回当前选择的数据库内可用表的列表

---

`SHOW`也可以用来显示表列：

输入：`SHOW COLUMNS FROM customers;`

分析：`SHOW COLUMNS`要求给出一个表名，它对每个字段返回一行，行中包含字段名、数据类型、是否允许 NULL、键信息、默认值以及其他信息。

---

其他`SHOW`语句：

- `SHOW STATUS`，用于显示广泛的服务器状态信息；
- `SHOW CREATE DATABASE` 和 `SHOW CREATE TABLE`，分别用来显示创建特定的数据库或表的 MySQL 语句；
- `SHOW GRANTS`，用来显示授予用户（所有用户或特定用户）的安全权限；
- `SHOW ERRORS`和`SHOW WARNINGS`，用来显示服务器错误或警告信息

# 第 4 章 检索数据

## 4.1 select 语句

`select`语句的作用是从一个或者多个表中检索信息。

## 4.2 检索单个列

输入：`SELECT prod_name FROM products;`
分析：上述语句利用`SELECT`语句从`products`表中检索一个名为`prod_name`的列。所需的列名在`SELECT`关键字之后给出，`FROM`关键字指出从其中检索数据的表名。

**注意：** `SQL`语句不区分大小写，但是许多开发人员喜欢对所有`SQL`关键字使用大写，而对所有列和表名使用小写，这样做便于代码的阅读和调试。

## 4.3 检索多个列

要想从一个表中检索出多个列，使用相同的`SELECT`语句。唯一不同的是必须在`SELECT`关键字之后给出多个列名，列名之间必须以逗号分隔。

输入：`SELECT prod_id,prod_name,prod_price FROM products;`

## 4.4 检索所有列

`SELECT`语句可以检索所有列，而不必将所有列名都列出，可以通过在实际列名的位置使用星号（\*）通配符来达到。

输入：`SELECT * FROM products;`
分析：如果给定一个通配符（\*），则返回表中的所有列。列的顺序一般是列在表定义中出现的顺序。但有时候并不是这样的，表的模式的变化（如添加或删除列）可能会导致顺序的变化。

## 4.5 检索不同行

正如所见，`SELECT`返回所有匹配的行。但是如果不想要每个值每次都出现，怎么办？
使用语句`SELECT vend_id FROM products;`会列出所有产品的供货商 id，即使存在相同的供货商也会将其全部列出（存在大量重复）。
解决办法是使用`DISTINCT`关键字，顾名思义，此关键字指示`MySQL`只返回不同的值。

输入：`SELECT DISTINCT vend_id FROM products;`
分析：`SELECT DISTINCT vend_id`告诉`MySQL`只返回不同（唯一）的`vend_id`行。`DISTINCT`关键字必须直接放在列名的前面。

## 4.6 限制结果

`SELECT`语句返回所有匹配的行，它们可能是指定表中的每个行。为了返回第一行或者前几行，可以使用`LIMIT`子句。
输入：`SELECT prod_name FROM products LIMIT 5;`
分析：此语句使用`SELECT`语句检索单个列。`LIMIT 5`指示`MySQL`返回不多于 5 行。

输入：`SELECT prod_name FROM products LIMIT 5,5;`
分析：`LIMIT 5,5`指示`MySQL`返回从第 5 行开始的后续 5 行。第一个数为开始位置，第二个数为要检索的行数。

**注意：** <span style="color:red;">行 0 </span> 检索出来的第一行为行 0 而不是行 1。因此，`LIMIT 1,1`将检索出第二行而不是第一行。

## 4.7 使用完全限定的表名

迄今为止使用的`SQL`例子只<i style="color:red">通过列名引用列</i>。也可能会<i style="color:red">使用完全限定的名字来引用列（同时使用表名和列名）</i>。

输入：`SELECT products.prod_name FROM crashcourse.products;`

# 第 5 章 排序检索数据

## 5.1 排序数据

在上一章中，通过`SELECT`语句检索出的数据并没有按照顺序输出。其实检索出的数据并不是完全随机显示，如果不对其进行排序，数据一般将以它在底层表中出现的顺序显示。这可以是数据最初添加到表中的顺序，但是如果数据后来进行过更新或者删除，则此顺序将会受到`MySQL`重用回收存储空间的影响。

**<i style="color:red">子句（clause）</i>** `SQL`语句由子句构成，有些子句是必需的，而有的是可选的。一个子句通常由一个关键字和所提供的数据组成。子句的例子有`SELECT`语句的`FROM`子句。

为了明确地排序用`SELECT`语句检索出的数据，可以使用`ORDER BY`子句。
输入：

```sql
SELECT prod_name
FROM products
ORDER BY prod_name;
```

分析：这条语句除了指示`MySQL`对`prod_name`列以字母顺序排列数据的`ORDER BY`子句外，与前面的语句相同。

## 5.2 按多个列排序

为了按照多个列排序，只要指定列名，列名之间用逗号分开即可。
输入：

```SQL
SELECT prod_id,prod_price,prod_name
FROM products
ORDER BY prod_price,prod_name;
```

分析：上述代码检索 3 个列，并按照其中两个列对结果进行排序——先按价格，然后再按名称排序。

## 5.3 指定排序方向

数据排序不限于升序排序（从`A`到`Z`），这只是默认的排序顺序，还可以使用`ORDER BY`子句以降序（从`Z`到`A`）顺序排序。为了进行降序排序，必须指定`DESC`关键字。
输入：

```sql
SELECT prod_id,prod_price,prod_name
FROM products
ORDER BY prod_price DESC;
```

分析：按照价格以降序排序产品（最贵的排在最前面）

如果打算用多个列进行排序怎么办？下面例子以降序排序产品（最贵的在最前面），然后再对产品名排序。
输入：

```sql
SELECT prod_id,prod_price,prod_name
FROM products
ORDER BY prod_price DESC,prod_name
```

**注意：** 如果想在多个列上进行降序排序，必须对每个列指定`DESC`关键字。

与`DESC`相反的关键字是`ASC`，在升序排序时可以指定它。但实际上，`ASC`没有多大用处，因为升序是默认的。

使用`ORDER BY`和`LIMIT`的组合，能够找出一个列中最高或者最低值。
下面例子是如何找出最昂贵的物品的值：
输入：

```sql
SELECT prod_price
FROM products
ORDER BY prod_price DESC
LIMIT 1;
```

# 第 6 章 过滤数据

## 6.1 使用 WHERE 子句

数据库表一般包含大量的数据，很少需要检索表中的所有行。通常只会根据特定操作或报告的需要提取表数据的子集。只检索所需数据需要指定 搜索条件 ，搜索条件也称为 过滤条件。
在`SELECT`语句中，数据根据`WHERE`子句中指定的搜索条件进行过滤。`WHERE`子句在表名（`FROM`子句）之后给出。

输入：

```sql
SELECT prod_name,prod_price
FROM products
WHERE prod_price = 2.50;
```

分析：这条语句从`products`表中检索两个列，但不返回所有行，只返回`prod_price`值为 2.50 的行。

> **WHERE 子句的位置** 在同时使用 ORDER BY 和 WHERE 子句时，应该让 ORDER BY 位于 WHERE 之后，否则会产生错误。

## 6.2 WHERE 子句操作符

| 操作符  | 说明             |
| ------- | ---------------- |
| =       | 等于             |
| <>      | 不等于           |
| !=      | 不等于           |
| <       | 小于             |
| <=      | 小于等于         |
| >       | 大于             |
| >=      | 大于等于         |
| BETWEEN | 在指定两个值之间 |

### 6.2.1 检查单个值

输入：

```SQL
SELECT prod_name,prod_price
FROM products
WHERE prod_name = 'fuses';
```

分析：检查`WHERE prod_name = 'fuses'`语句，它返回`prod_name`的值为`fuses`的一行。

---

输入：

```sql
SELECT prod_name,prod_price
FROM products
WHERE prod_price < 10;
```

分析：列出价格小于 10 美元的所有产品。

### 6.2.2 不匹配检查

列出不是由供货商 1003 制造的所有产品：
输入：

```SQL
①
SELECT vend_id,prod_name
FROM products
WHERE vend_id <> 1003;
②
SELECT vend_id,prod_name
FROM products
WHERE vend_id != 1003;
```

### 6.2.3 范围检查

检索价格在 5 美元和 10 美元之间的所有产品：
输入：

```SQL
SELECT prod_name,prod_price
FROM products
WHERE prod_price BETWEEN 5 AND 10;
```

分析：在使用`BETWEEN`时，必须指定两个值——所需范围低端值和高端值，这两个值之间必须使用`AND`关键字分割。

### 6.2.4 空值检查

在创建表时，表设计人员可以指定其中的列是否可以不包含空值。在一个列不包含值的时候，称其为包含空值`NULL`。

`SELECT`语句有一个特殊的`WHERE`子句，可用来检查具有`NULL`值的列。这个`WHERE`子句就是`IS NULL`子句。
输入：

```sql
SELECT prod_name
FROM products
WHERE prod_price IS NULL;
```

分析：返回没有价格（空`prod_price`字段，不是价格为`0`）的所有产品。

# 第 7 章 数据过滤

## 7.1 组合 WHERE 子句

为了进行更强的过滤控制，`MySQL`允许给出多个`WHERE`子句。这些子句可以两种方式使用：以`AND`子句的方式或`OR`子句的方式使用。

### 7.1.1 AND 操作符

为了通过不止一列进行过滤，可使用`AND`操作符给`WHERE`子句附加条件。

输入：

```SQL
SELECT prod_id,prod_price,prod_name
FROM products
WHERE vend_id = 1003 AND prod_price <= 10;
```

分析：检索由供应商 1003 制造且价格小于等于 10 美元的所有产品的名称和价格。

### 7.1.2 OR 操作符

输入：

```SQL
SELECT prod_name,prod_price
FROM products
WHERE vend_id = 1002 AND vend_id = 1003;
```

分析：检索 1002 供货商和 1003 供货商的所有产品。

### 7.1.3 计算次序

输入：

```sql
SELECT prod_name,prod_price
FROM products
WHERE vend_id = 1002 OR vend_id = 1003 AND prod_price >= 10;
```

分析：上述`SQL`语句可以理解为由供应商 1003 制造的任何价格为 10 美元（含）以上的产品，或者由供应商 1002 制造的任何产品。

`SQL`语句在处理`OR`操作符前，优先处理`AND`操作符。

若想要检索<span style="background:yellow;">由供货商 1002 或者 1003 制造的且价格都在 10 美元（含）以上的任何产品</span>，则需要通过使用圆括号明确地分组相应的操作符。
输入：

```SQL
SELECT prod_name,prod_price
FROM products
WHERE (vend_id = 1002 OR vend_id = 1003) AND prod_price >= 10;
```

## 7.2 IN 操作符

圆括号在`WHERE`子句还有另一种用法。`IN`操作符用来指定条件范围，范围中的每个条件都可以进行匹配。`IN`取合法值的由逗号分隔的清单，全部括在圆括号中。

输入：

```SQL
SELECT prod_name,prod_price
FROM products
WHERE vend_id IN (1002,1003)
ORDER BY prod_name;
```

分析：此`SELECT`语句检索供应商 1002 和 1003 制造的所有产品。

## 7.3 NOT 操作符

`WHERE`子句中的`NOT`操作符有且只有一个功能，那就是否定它之后所跟的任何条件。

输入：

```SQL
SELECT prod_name,prod_price
FROM products
WHERE vend_id NOT IN (1002,1003)
ORDER BY prod_name;
```

分析：列出除 1002 和 1003 之外的所有供应商制造的产品。

# 第 8 章 用通配符进行过滤

## 8.1 LIKE 操作符

**通配符：**
