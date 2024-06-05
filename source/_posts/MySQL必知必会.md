---
title: MySQL必知必会
date: 2023-08-21 09:27:39
tags:
  - MySQL
---

书籍下载地址：[《MySQL 必知必会》](https://qinweirz-my.sharepoint.com/:b:/g/personal/qinwei_qinweirz_onmicrosoft_com/ERvXFKVNW0dErHJc5EWtOhIBrjFcFVd-Va13QWYN2_WyJQ?e=PZfkdX)
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

**通配符：** 用来匹配值的一部分的特殊字符。

为了搜索子句中使用通配符，必须使用`LIKE`操作符。`LIKE`指示`MySQL`，后跟的搜索模式利用通配符匹配而不是直接相等匹配进行比较。

### 8.1.1 百分号（%）通配符

在搜索串中，`%`表示任何字符出现任意次数。例如，为了找出所有以词`jet`起头的产品，可以使用以下`SELECT`语句：
输入：

```sql
SELECT prod_id,prod_name
FROM products
WHERE prod_name LIKE 'jet%';
```

分析：此例子使用了搜索模式`jet%`。在执行这条子句时，将检索任意以`jet`起头的单词。`%`告诉 MySQL 接受 jet 之后的任意字符，不管它有多少字符。

通配符可在搜索模式中任意位置使用，并且可以使用多个通配符。

输入：

```SQL
SELECT prod_id,prod_name
FROM products
WHERE prod_name LIKE '%anvil%';
```

分析：搜索模式`'%anvil%'`表示匹配任何位置包含文本`anvil`的值，而不论它之前或之后出现什么字符。

---

输入:

```SQL
SELECT prod_name
FROM products
WHERE prod_name LIKE 's%e';
```

分析：上述例子是找出以`s`起头以`e`结尾的所有产品。

**<span style="background:yellow;">`%`代表搜索模式中给定位置的 0 个、1 个或多个字符</span>**。

**注意 NULL** 虽然似乎`%`通配符可以匹配任何东西，但有一个例外，即`NULL`。即使`WHERE prod_name LIKE '%'`也不能匹配用值`NULL`作为产品名的行。

### 8.1.2 下划线（\_）通配符

下划线的用途与`%`一样，但下划线只匹配单个字符而不是多个字符。
输入：

```SQL
SELECT prod_id,prod_name
FROM products
WHERE prod_name LIKE '_ ton anvil';
```

分析：此`WHERE`子句中的搜素模式给出了后面跟有文本的两个通配符。对于`.5 ton anvil`产品没有匹配，因为搜索模式要求匹配一个通配符而不是两个。

# 第 9 章 用正则表达式进行搜索

## 9.1 使用 MySQL 正则表达式

### 9.1.1 基本字符匹配

下面语句检索列`prod_name`包含文本 1000 的所有行：

输入：

```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '1000'
ORDER BY prod_name;
```

---

输入：

```SQL
SELECT prod_name
FROM products
WHERE prod_name REGEXP '.000'
ORDER BY prod_name;
```

分析：这里使用了正则表达式`.000`。`.`是正则表达式语言中的一个特殊的字符，它表示匹配任意一个字符。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308231121556.png)

> **匹配不区分大小写** MySQL 中的正则表达式匹配（自版本 3.23.4 后）不区分大小写（即，大写和小写都匹配）。为区分大小写，可使用`BINARY`关键字，如`WHERE prod_name REGEXP BINARY 'JetPack .000'`。

### 9.1.2 使用`OR`匹配

为搜索两个串之一（或者为这个串，或者为另一个串），使用`|`。
输入:

```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '1000|2000'
ORDER BY prod_name;
```

分析：`|`为正则表达式的`OR`操作符，它表示匹配其中之一。

### 9.1.3 匹配几个字符之一

匹配任何单一字符，可以通过指定一组`[`和`]`括起来的字符来完成。
输入：

```SQL
SELECT prod_name
FROM products
WHERE prod_name REGEXP `[123] Ton`
ORDER BY prod_name;
```

分析：这里，使用了正则表达式`[123] Ton`。`[123]`定义一组字符，它的意思是匹配 1 或 2 或 3，因此，数据`1 ton`、`2 Ton`和`3 Ton`都会被匹配。【MySQL 不区分大小写】。

正如所见，`[]`是另一种形式的`OR`语句。事实上，正则表达式`[123] Ton`。事实上，正则表达式`[123] Ton`为`[1|2|3] Ton`的缩写。

`[^123] Ton`匹配除了字符`1`、`2`、`3`之外的任何字符。

### 9.1.4 匹配范围

集合可以用来定义要匹配的一个或多个字符。

匹配数字 0 到 9：`[0123456789]` 或者 `[0-9]`
匹配任意字母字符：`[a-z]`

### 9.1.5 匹配特殊字符

为了匹配特殊字符，必须用`\\`为前导。例如：`\\-`表示查找`-`，`\\.`表示查找`.`。
输入：

```sql
SELECT vend_name
FROM vendors
WHERE vend_name REGEXP '\\.'
ORDER BY vend_name;
```

`\\`也用来引用元字符（具有特殊含义的字符）。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308231450883.png)

> **匹配 \\** 为了匹配反斜杠（\\）字符本身，需要使用`\\\`

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308231526883.png)

### 9.1.6 匹配字符类

存在找出你自己经常使用的数字、所有字母字符或所有数字字母字符等的匹配。为更方便工作，可以使用预定义的字符集，成为字符类。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308231541472.png)

### 9.1.7 匹配多个实例

到目前为止，使用的所有正则表达式都试图匹配单次出现。如果存在一个匹配，该行被检索出来，如果不存在，检索不出任何行。但有时需要对匹配的数目进行更强的控制，需要限制字符出现的次数。可以通过重复元字符来实现。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308231550477.png)

输入：

```SQL
SELECT prod_name
FROM products
WHERE prod_name REGEXP '\\([0-9] sticks?\\)'
ORDER BY prod_name;
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308231552346.png)
分析：正则表达式`\\([0-9] sticks?\\)`解说如下，`\\(`匹配`)`，`[0-9]`匹配任意数字（这个例子中为 1 和 5），`sticks?`匹配`stick`和`sticks`（`s`后的`?`使得`s`可选，因为`?`匹配它前面的任何字符的 0 次或 1 次出现），`\\)`匹配`)`。没有`?`，匹配`stick`和`sticks`会非常困难。

匹配连在一起的 4 位数字：
输入：

```sql
SELECT prod_name
FROM products
WHERE prod_name REGEXP '[[:digit:]]{4}'
ORDER BY prod_name;
```

分析：`[[:digit:]]`匹配任意数字，因而它为数字的一个集合。`{4}`确切的要求它前面的字符（任意数字）出现 4 次。

### 9.1.8 定位符

目前为止所给出的例子都是匹配一个字符串中的任意位置的文本。为了匹配特定位置的文本，需要使用定位符。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308231606264.png)

输入：

```SQL
SELECT prod_name
FROM products
WHERE prod_name REGEXP '^[0-9\\.]'
ORDER BY prod_name；
```

分析：`^`匹配串的开始。因此，`^[0-9\\.]`只在`.`或者任意数字为串中第一个字符时才匹配它们。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308231614247.png)

# 第 10 章 创建计算字段

## 10.1 计算字段

<i style="color:red;">字段（filed）</i> 基本上与列（column）的意思相同，经常互换使用，不过数据库列一般称为列，而术语字段通常用在计算字段的连接上。

只有数据库知道`SELECT`语句中哪些列是实际的表列，哪些列是计算字段。

## 10.2 拼接字段

【需求】需要将数据库中的供货商信息按照`name(location)`的形式返回。
输入：

```sql
SELECT Concat(vend_name,'(',vend_country,')')
FROM vendors
ORDER BY vend_name;
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308231627912.png)
分析：`Concat()`拼接串，即把多个串连接起来形成一个较长的串。

> **Trim 函数** 该函数的作用是去除掉串两边的空格，除了该函数外，还有`RTrim()`去除串右边的空格、`LTrim()`去除掉串左边的空格。

**使用别名**
从上面的案例中可以发现，`SELECT`语句拼接地址字段的工作做得很好，但是新计算得到的列的名字十分冗长，如果想要替换这个冗长的列名，可以通过别名来实现。

<i style="color:red;">别名（alias）</i> 一个字段或值的替换。

输入：

```sql
SELECT Concat(RTrime(vend_name),'(',RTrime(vend_country),')') AS
vend_title
FROM vendors
ORDER BY vend_name;
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308240941666.png)

## 10.3 执行算数计算

输入：

```sql
SELECT prod_id,
       quantity,
       item_price,
       quantity*item_price AS expanded_price
FROM orderitems
WHERE order_num = 2005;
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308240947204.png)
分析：上述`sql`语句功能是汇总物品的价格（单价乘以订购数量）。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308240948788.png)

# 第 11 章 使用数据处理函数

## 11.1 函数

与其他大多数计算机语言一样，`SQL`支持利用函数来处理数据。函数一般是在数据上执行的，它给数据的转换和处理提供方便。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308240951598.png)

## 11.2 使用函数

大多数`SQL`实现以下类型的函数：文本处理函数、日期和时间处理函数、数值处理函数

### 11.2.1 文本处理函数

常见的文本处理函数：
| 函数 | 说明 |
| ----------- | ----------- |
| Left( ) | 返回串左边的字符 |
| Length( ) | 返回串的长度 |
| Locate( ) | 找出串的一个子串 |
| Lower( ) | 将串转换为小写 |
| LTrim( ) | 去掉串左边的空格 |
| RTrim( ) | 去掉串右边的空格 |
| Soundex( ) | 返回串的 SOUNDEX 值 |
| SubString( ) | 返回子串的字符 |
| Upper( ) | 将串转换为大写 |

说明：`SOUNDEX`是一个将任何文本串转换为描述其语音表示的字母数字模式的算法。`SOUNDEX`考虑了类似的发音字符和音节，使得能对串进行发音比较而不是字母比较。
`SOUNDEX()`函数的主要目的是根据声音比较字符串之间的相似性。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241006421.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241006695.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241011238.png)

---

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241536140.png)

### 11.2.2 日期和时间处理函数

日期和时间采用相应的数据类型和特殊的格式存储，以便能快速和有效地排序或过滤，并节省物理存储空间。
一般应用程序不能使用用来存储日期和时间的格式，因此日期和时间函数就用来读取、统计和处理这些值。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241018885.png)

`MySQL`使用的日期格式必须为`yyyy-mm-dd`，这种格式排除了多义性。

输入：

```sql
SELECT cust_id,order_num
FROM orders
WHERE order_date = '2005-09-01';
```

分析：此`SELECT`语句检索订单记录，检索出的订单记录的`order_date`为`2005-09-01`。

但是如果表中的订单记录的`oeder_date`是日期与时间的组合（即`2005-09-01 11:31:56`），那么通过上述`SQL`语句就无法匹配该天的订单记录，因此就要使用`Date()`函数去提取日期部分。
更加可靠的输入应为：

```sql
SELECT cust_id,order_num
FROM orders
WHERE Date(order_date) = '2005-09-01';
```

匹配当前月份内的所有订单，可以使用`BETWEEN`。
输入：

```sql
① 需要了解当前月份多少天
SELECT cust_id,order_num
FROM orders
WHERE Date(order_date) BETWEEN '2005-09-01' AND '2005-09-30';

② 不需要了解当前月份多少天
SELECT cust_id,order_num
FROM orders
WHERE Year(order_date) = 2005  AND Month(order_date) = 9;
```

### 11.2.3 数值处理函数

数值处理函数仅处理数值数据。这些函数一般主要用于代数、三角或几何运算。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241038467.png)

# 第 12 章汇总数据

## 12.1 聚集函数

我们经常需要汇总数据而不是把它们实际检索出来，为此`MySQL`提供了专门的函数。
这种类型的检索例子一般包括：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241040168.png)

<i style="color:red;">聚集函数（aggregate function）</i> 运行在行组上，计算和返回单个值的函数。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241042023.png)

### 12.1.1 AVG( ) 函数

`AVG( )`通过对表中行数计数并计算特定列值之和，求得该列的平均值。

输入：

```sql
SELECT AVG(prod_price) AS avg_price
FROM products;
```

> **只用于单个列** `AVG( )`只能用来确定特定数值列的平均值，而且列名必须作为函数参数给出。为了获得多个列的平均值，必须使用多个`AVG( )`函数。

> **NULL 值** `AVG( )`函数忽略列值为`NULL` 的行。

### 12.1.2 COUNT（ ）函数

`COUNT( )`函数用于计数。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241105231.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241107050.png)

> **`NULL`值** 如果指定列名，则指定列的值为空的行被 `COUNT（）` 函数忽略，但如果`COUNT（）` 函数 中用的是星号（\*）， 则不忽略。

### 12.1.3 MAX( )函数

`MAX( )`返回指定列中的最大值，`MAX( )`要求指定列名。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241111012.png)

> **对非数值数据使用 MAX（）** 虽然`MAX( )`一般用来找出最大的数值或日期值。但`MySQL`允许将它用于返回任意列值最大的值，包括返回文本列中的最大值。在用于文本数据时，如果数据按相应的列排序，则`MAX()`返回最后一行。

对于`NULL`值，`MAX( )`函数忽略列值为`NULL`的行。

### 12.1.4 MIN( ) 函数

与`MAX( )`函数的用法一致。

### 12.1.5 SUM( )函数

`SUM( )`用来返回指定列值的和（总计）。

输入：

```sql
SELECT SUM(quantity) AS items_ordered
FROM orderitems
WHERE order_num = 2005;
```

分析：函数`SUM(quantity)` 返回订单中所有物品数量之和。

## 12.2 聚集不同值

以上 5 个聚集函数都可以如下使用：

- 对所有的行执行计算，指定`ALL`参数或不给参数（因为`ALL`是默认行为）
- 只包含不同的值，指定`DISTINCT`参数

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241123668.png)

**⚠️ 注意：** 如果指定列名，则`DISTINCT` 只能用于`COUNT( )`。`DISTINCT`不能用于`COUNT(*)`，因此不允许使用`COUNT(DISTINCT)`，否则会产生错误。

## 12.3 组合聚集函数

到目前为止，所有聚集函数的例子都只涉及单个函数。但实际上`SELECT`语句可根据需要包含多个聚集函数。

输入：

```sql
SELECT COUNT(*) AS num_items,
       MIN(prod_price) AS price_min,
       MAX(prod_price) AS price_max,
       AVG(prod_price) AS price_avg
FROM products;
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241130927.png)

# 第 13 章 分组数据

## 13.1 数据分组

从上一章中可以知道`SQL`聚集函数可以用来汇总数据。到目前为止，所有的计算都是在表的所有数据或者匹配特定的`WHERE`子句的数据上进行的。
例如，返回供应商 1003 提供的产品数据，`SQL`语句如下所示：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241509894.png)

但如果要返回每个供应商提供的产品数目怎么办？或者返回只提供单项产品的供应商所提供的产品。此时就需要使用到分组。

<i style="color:red;">分组：</i> 分组允许把数据分为多个逻辑组，以便能对每个组进行聚集计算。

## 13.2 创建分组

分组是在`SELECT`语句的`GROUP BY`子句中建立的。
输入：

```sql
SELECT vend_id,COUNT(*) AS num_prods
FROM products
GROUP BY vend_id;
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241557344.png)
分析：上面的`SELECT`语句指定了两个列，`vend_id`包含产品供应商的 ID，`num_prods` 为计算字段（用`COUNT(*)`函数建立）。`GROUP BY`子句指示`MySQL`按照`vend_id`排序并进行分组数据。这导致对每个`vend_id`而不是整个表计算`nums_prods`一次。从输出中可以看到，供应商 1001 有 3 个产品，供应商 1002 有 2 个产品，供应商 1003 有 7 个产品，供应商 1005 有 2 个产品。

因为使用`GROUP BY`，就不必指定要计算和估值的每个组了。系统会自动完成。`GROUP BY`子句指示`MySQL`分组数据，然后对每个组而不是整个结果进行聚集。

使用`GROUP BY`子句的一些重要规定：

1. <span style="color:red;">`GROUP BY`子句可以包含任意数目的列</span>。这使得能对分组进行嵌套，为数据分组提供了更加细致的控制。
   ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308250950812.png)
2. <span style="color:red;">如果在`GROUP BY`子句中嵌套了分组，数据将在最后规定的分组上进行汇总。</span> 换句话说，在建立分组时，指定的所有列都一起计算（所以不能从个别列中取回数据）。
   ![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308250959999.png)
3. <span style="color:red;">`GROUP BY`子句中列出的每个列都必须是检索列或有效的表达式（但不能是聚集函数）</span>。如果在`SELECT`中使用表达式，则必须在`GROUP BY`子句中指定相同的表达式。不能使用别名。

4. <span style="color:red;">除聚集计算语句外，`SELECT`语句中的每个列都必须在`GROUP BY`子句中给出</span>。

5. <span style="color:red;">如果分组列中具有`NULL`值，则 NULL 将作为一个分组返回。如果列中有多行`NULL`值，它们将分为一组</span>。

6.<span style="color:red;">`GROUP BY`子句必须出现在`WHERE`子句之后，`ORDER BY`子句之前</span>。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308241618103.png)

## 13.3 过滤分组

除了能用`GROUP BY`分组数据之外，`MySQL`还允许过滤分组，规定包括哪个分组，排除哪个分组。
<i>例如，可能要列出至少有 2 个订单的顾客。</i>
我们在第 6 章中了解了`WHERE`子句的作用。但是在这个例子中`WHERE`不能完成任务，<span style="color:red;">因为`WHERE`过滤的是行，而不是分组</span>。事实上，`WHERE`没有分组的概念。

`MySQL`提供了`HAVING`子句，用于过滤分组。到目前为止，所学习的`WHERE`子句的功能都能使用`HAVING` 子句来进行替代。

**<span style="color:red;">WHERE 过滤行，HAVING 过滤分组。</span>**
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308251014805.png)
输入：

```sql
SELECT cust_id,COUNT(*) AS orders
FROM orders
GROUP BY cust_id
HAVING COUNT(*) >= 2;
```

分析：这条`SELECT`语句的前 3 行类似于上面的语句。最后一行增加了`HAVING`子句，它过滤`COUNT(*) >= 2`（两个及以上的订单）的那些分组。

> **`HAVING` 和 `WHERE`的差别** `WHERE`在数据分组前进行过滤，`HAVING`在数据分组后进行过滤。

输入：

```sql
SELECT vend_id,COUNT(*) AS num_prods
FROM products
WHERE prod_price >=10
GROUP BY vend_id
HAVING COUNT(*) >= 2;
```

分析：上述`SQL`语句列出 2 个（含）以上、价格为 10（含）以上的产品的供应商。

## 13.4 分组和排序

虽然`GROUP BY`和`ORDER BY`经常完成相同的工作，但是它们是非常不同的。
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308251036753.png)
我们经常发现用`GROUP BY`分组的数据确实是以分组顺序输出，但是结果并不总是这样的，它并不是`SQL`规范所要求的。

> **不要忘记 `ORDER BY`** 一般在使用`GROUP BY`子句时，应该也给出`ORDER BY`子句。这是保证数据正确排序的唯一方法。千万不要依赖`GROUP BY`排序数据。

为说明 `GROUP BY` 和 `ORDER BY` 的使用方法， 请看一个例子。 下面的 `SELECT`语句类似于前面那些例子。它检索总计订单价格大于等于 50 的订 单的订单号和总计订单价格：
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308251045284.png)
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308251046686.png)

## 13.5 `SELECT`子句顺序

`SELECT`子句及其顺序：
| 子句 | 说明 | 是否必须使用 |
| ---- | ---- |---- |
| `SELECT` | 要返回的列或者表达式 | 是 |
| `FROM` | 从中检索数据的表 | 仅在从表选择数据时使用 |
| `WHERE` | 行级过滤 | 否 |
| `GROUP BY` | 分组说明 | 仅在按组计算聚集时使用 |
| `HAVING` | 组级过滤 | 否 |
| `ORDER BY` | 输出排序顺序 | 否 |
| `LIMIT` | 要检索的行数 | 否 |

# 第 14 章 使用子查询

## 14.1 子查询

<i style="color:red;">查询（query）</i> 任何`SQL`语句都是查询。但此术语一般指`SELECT`语句。

`SQL` 还允许创建子查询（subquery），即嵌套在其他查询中的查询。

## 14.2 利用子查询进行过滤

在此之前所学习的`SQL`语句都是在同一个数据库表中进行操作，并未跨表操作。
现有三个数据库表格：
① `orders`表：包含订单号、客户 ID、订单日期
② `orderitems`表：各订单物品
③ `customers`表：实际的客户信息

需求：列出订购物品`TNT2`的所有客户。

实现步骤：
（1） 检索包含物品`TNT2`的所有订单的编号。
（2） 检索具有前一步骤列出的订单编号的所有客户的 ID。
（3） 检索前一个步骤返回的所有客户 ID 的客户信息。

---

分开检索：

```sql
(1)  检索`prod_id`为`TNT2`的所有订单物品
SELECT order_num
FROM orderitems
WHERE prod_id = 'TNT2';
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308251133678.png)

```sql
(2) 查询具有订单 2005 和 2007 的客户 ID
SELECT cust_id
FROM orders
WHERE order_num IN (2005,1007);
```

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308251134343.png)

```sql
（3）检索这些客户 ID 的客户信息
SELECT cust_name,cust_contact
FROM customers
WHERE cust_id IN (10001,10004);
```

合并检索：

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308251420912.png)
子查询中的`WHERE`子句与前面使用的`WHERE`子句稍有不同，因为它使用了完全限定列名。
下面的语句告诉`SQL`比较`orders`表中的`cust_id` 与当前正从`customer`表中检索的`cust_id`：`WHERE orders.cust_id = customers.cust_id`

<i style="color:red;">相关子查询（correlated subquery）</i> 涉及外部查询的子查询。任何时候只要列名可能有多义性，就必须使用这种语法（表名和列名由一个句号分隔）。

# 第 15 章 联结表

## 15.1 联结

`SQL`最强大的功能之一就是能在数据检索查询的执行中联结（join）表。联结是利用`SQL`的`SELECT`能执行的最重要的操作。

在能够有效地使用联结前，必须了解关系表以及关系数据库设计的一些基础知识。

### 15.1.1 关系表

关系表的设计就是要保证把信息分解成多个表，一类数据一个表。各表通过某些常用的值（即关系设计中的 关系（relational））互相关联。

<i style="color:red;"> 外键（foreign key）:</i> 外键为某个表中的一列，它包含另一个表的主键值，定义了两个表之间的关系。

关系数据可以有效地存储和方便地处理。因此，关系数据库的可伸缩性远比非关系数据库要好。

<i style="color:red;">可伸缩性（scale）:</i>能够适应不断增加的工作量而不失败。设计良好的数据库或应用程序称之为 可伸缩性好（scale well）。

### 15.1.2 为什么使用联结

正如所述，分解数据为多个表能更有效地存储，更方便地处理，并且具有更大地可伸缩性。但这些好处是有代价的。如果数据存储在多个表中，怎样使用单条`SELECT`语句检索出数据？

答案是使用联结。简单来说，联结是一种机制，用来在一条`SELECT`语句中关联表。使用特殊的语法，可以联结多个表返回一组输出，联结在运行时关联表中正确的行。

## 15.2 创建联结

联结的创建非常简单，规定要联结的所有表以及它们如何关联即可。
输入：

```sql
SELECT vend_name,prod_name,prod_price
FROM vendors,products
WHERE vendors.vend_id = products.vend_id
ORDER BY vend_name,prod_name
```

### 15.2.1 WHERE 子句的重要性

在数据库表的定义中，不存在能指示 MySQL 如何对表进行联结的东西。在联结两个表时，你实际上做的是讲第一个表中的每一行与第二个表中的没一行进行配对。`WHERE`子句作为过滤条件，它只包含那些匹配给定条件（这里是联结条件）的行。没有`WHERE`子句，第一个表的每个行将与第二个表中的每个行配对，而不管它们逻辑上是否可以配对在一起。

<i style="color:red;">笛卡尔积（cartesian product）：</i>由没有联结条件的表关系返回的结果为笛卡尔积。检索出的行的数目将是第一个表中的行数乘以第二个表中的行数。

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308251645031.png)

### 15.2.2 内部联结

到目前为止，所用的联结称为等值联结（equijoin），它基于两个表之间的相等测试。这种联结也称为内部联结。
其实，对于这种联结可以使用稍微不同的语法来明确指定联结的类型。
输入：

```sql
SELECT vend_name,prod_name,prod_price
FROM vnedors INNER JOIN products
ON vendors.vend_id = products.vend_id;
```

分析：此语句中的`SELECT`子句与前面相同，不同的是`FROM`子句。在这里，两个表之间的关系是`FROM`子句的组成部分，以`INNER JOIN`指定。在使用这种语法时，联结条件用特定的`ON`子句而不是`WHERE`子句给出。传递给`ON`的实际条件与传递给`WHERE` 的相同。

### 15.2.3 联结多个表

`SQL`对一条`SELECT`语句中可以联结多少个表的数目没有限制。
输入：

```sql
SELECT prod_name,vend_name,prod_price,quantity
FROM orderitems,products,vendors
WHERE products.vend_id = vendors.vend_id
   AND orderitems.prod_id = products.prod_id
   AND order_num = 20005;
```

分析：这个例子显示编号为 20005 的订单中的物品。订单物品存储在`orderitems`表中。每个产品按其产品`ID`存储，它引用`products`表中的产品。这些产品通过供应商`ID`联结到`vendors`表中相应的供应商。

> **性能考虑** `MySQL`在运行时关联指定的每个表以处理联结。这种处理可能是非常耗费资源的，因此，应该仔细，不要联结不必要的表。联结的表越多，性能下降越厉害。

正如 14 章所述，子查询并不总是执行复杂`SELECT`操作的最有效方法，对于下面的查询输入，
![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308251420912.png)
使用联结可以同样进行实现：
输入：

```sql
SELECT cust_name,cust_contact
FROM customers,orders,orderitems
WHERE customers.cust_id = orders.cust_id
  AND orderitems.order_num = orders.order_num
  AND prod_id = 'TNT2';
```

分析：在这个查询中返回数据需要使用 3 个表。在这里我们没有在嵌套子查询中使用它们，而是使用了两个联结。

# 第 16 章 创建高级联结

本章将介绍如何对被联结的表使用表别名和聚集函数。

## 16.1 使用表别名

输入：

```sql
SELECT cust_name,cust_contact
FROM customer AS c, orders AS o,orderitems AS oi
WHERE c.cust_id = o.cust_id
   AND oi.order_num = o.order_num
   AND prod_id = 'TNT2';
```

分析：可以看到`FROM`子句中 3 个表都具有别名，表的别名用于`WHERE`子句。

**应该注意：** 表别名只在查询执行中使用。与列别名不一样，表别名不返回到客户机。

## 16.2 使用不同类型的联结

到目前为止，我们使用的只是成为“内部联结（等值联结）”的简单联结。还有另外其他 3 中联结：自联结、自然联结和外部联结。

### 16.2.1 自联结

现在存在这样一个问题：假如你发现某物品（其`ID`为`DTNTR`）存在问题，因此想要知道生产该物品的供应商生产的其他物品是否也存在这些问题。
此查询需要先找到生产 `ID`为`DTNTR` 的物品的供应商，然后找到这个供应商生产的其他物品。

输入：

```sql
 方法一：
 SELECT prod_id,prod_name
 FROM products
 WHERE vend_id = (SELECT vend_id
                  FROM products
                  WHERE prod_id = 'DTNTR');

方法二：
SELECT p1.prod_id,p1.prod_name
FROM products AS p1,products AS p2
WHERE p1.vend_id = p2.vend_id
   AND p2.prod_id = 'DTNTR';
```

分析：方法二中需要的两个表实际上是相同的表，因此`products`表在`FROM`子句中出现了两次，为了解决`products`表的引用具有二义性的问题，使用了表别名。

> **用自联结而不用子查询：** 自联结通常作为外部语句用来替代从相同表中检索数据时使用的子查询语句。虽然最终的结果是相同的，但有时候处理联结远比处理子查询快得多。

### 16.2.2 自然联结

无论何时对表进行联结，应该至少有一个列出现在不止一个表中（被联结的列）。标准的联结返回所有数据，甚至相同的列多次出现。自然联结排除多次出现，使得每个列只返回一次。
但是`SQL`不提供这项功能,自然联结的功能是需要你自己去完成的,自然联结要求你只能选择哪些唯一的列,一般通过对一个表使用通配符(`SELECT *`),而对其他表的列使用明确的子集来完成。

输入：

```sql
SELECT c.*,o.order_num,o.order_date,oi.prod_id,oi.quantity,oi.item_price
FROM customer AS c,order AS o,orderitems AS oi
WHERE c.cust_id = o.cust_id
  AND oi.order_num = o.order_num
  AND prod_id = 'FB';
```

分析：在这个例子中，通配符只对第一个表使用。所有其他列明确列出，所以没有重复的列被检索出来。
事实上，迄今为止我们建立的每个内部联结都是自然联结。

### 16.2.3 外部联结

外部联结是指联结包含在那些表中没有关联行的行。
输入：

```sql
SELECT customers.cust_id,orders.order_num
FROM customers LEFT OUTER JOIN orders
ON customers.cust_id = orders.cust_id;
```

分析：类似于内部联结，这条`SELECT`语句 使用了关键字`OUTER JOIN`来指定联结的类型（而不是在`WHERE` 子句中指定）。但是与内部联结两个表中的行不同的是，外部联结还包括没有关联的行。

在使用`OUTER JOIN` 语法时，必须使用`RIGHT`或者`LEFT`关键字指定包括其包含所有行的表（`RIGHT` 指出的是`OUTER JOIN`右边的表，而`LEFT`指出的是`OUTER JOIN` 左边的表）。上面的例子使用`LEFT OUTER JOIN` 从`FROM` 子句的左边表（`customers`表）中选择所有行。

## 16.3 使用带聚集函数的联结

聚集函数是用来汇总数据，虽然至今为止聚集函数的所有例子只是从单个表汇总数据，但这些函数也可以与联结一起使用。
输入：

```sql
SELECT customers.cust_name,customers.cust_id,COUNT(orders.order_num) AS num_ord
FROM customers INNER JOIN orders
ON customers.cust_id = orders.cust_id
GROUP BY customers.cust_id;
```

分析：此条`SELECT` 语句使用`INNER JOIN` 将`customers`和`orders`表相互关联。`GROUP BY` 子句按客户分组数据，因此，函数调用`COUNT(orders.order_num)`对每个客户的订单计数，将它作为`num_ord`返回。

## 16.4 使用联结和联结条件

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202308281456699.png)

# 第 17 章 组合查询

_本章讲述如何利用 UNION 操作符将多条 SELECT 语句组合成一个结果集_

## 17.1 组合查询

多数`SQL`语句都只包含从一个或多个表中返回数据的单条`SELECT`语句。`MySQL`也允许执行多个查询（多条`SELECT`语句），并将结果作为单个查询结果集返回。这些组合查询通常称为 并（union） 或 复合查询（compound query）。

有两种基本情况，其中需要使用组合查询：

- 在单个查询中从不同的表返回类似结构的数据
- 对单个表执行多个查询，按单个查询返回数据

![](https://cdn.jsdelivr.net/gh/qw-null/BlogImages/202309102037167.png)

## 17.2 创建组合查询

可以使用`UNION`操作符来组合数条`SQL`查询。利用`UNION`，可给出多条`SELECT`语句，将它们的结果组合成单个结果集。

### 17.2.1 使用`UNION`

`UNION`的使用很简单。所需要做的只是给出每条`SELECT`语句，在各条语句之间放上关键字`UNION`。

举一个例子，假如
