# 入门

## 安装

### 普通安装

### Docker 安装

## 卸载

Mac卸载MySQL。参考：[https://www.cnblogs.com/TsengYuen/archive/2011/12/06/2278574.html](https://www.cnblogs.com/TsengYuen/archive/2011/12/06/2278574.html "https://www.cnblogs.com/TsengYuen/archive/2011/12/06/2278574.html")

# 基础

> [一千行 MySQL 学习笔记](https://shockerli.net/post/1000-line-mysql-note/ "一千行 MySQL 学习笔记")
> [drop，truncate，delete 三者的区别](https://blog.csdn.net/GRAY_KEY/article/details/86742248 "drop，truncate，delete 三者的区别")

SQL 是 Structured Query Language（结构化查询语言）的缩写，是使用关系型数据库的应用语言。美国国家标准局（ANSI）于1986年制定了第一个SQL标准，叫做 SQL-86。大多数关系型数据库系统都支持 SQL 语言。

MySQL 是一种关系型数据库，并对标准SQL进行了一些扩展。MySQL 是开源的，因此任何人都可以在许可下下载并进行修改或扩展。MySQL 的默认端口号是 3306。

## 常用SQL

SQL 语句主要可以划分为以下 3 个类别：

-   数据定义语言（Data Definition Language，DDL）：用于定义数据库、表、列、索引等数据库对象。常用的语句关键字主要包括CREATE、DROP、ALTER等。
-   数据操纵语言（Data Manipulation Language，DML）：用于添加、删除、更新和查询数据库记录。常用的语句关键字主要包括INSERT、DELETE、UPDATE和SELECT等。
-   数据控制语言 （Data Control Language，DCL）：用于定义数据库、表、字段、用户的访问权限和安全级别。常用的语句关键字包括GRANT、REVOKE等。

创建数据库：

```sql
CREATE DATABASE database_name;
```

创建表：

```sql
CREATE TABLE table_name (column_name column_type)
ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

删除表以及表中的所有数据：

```sql
DROP TABLE table_name;
```

查询：

```sql
SELECT DISTINCT
    <select list>
FROM
    <left_table> <join_type>
JOIN
    <right_table> ON <join_condition>
WHERE
    <where_condition>
GROUP BY
    <group_by_list>
HAVING
    <having_condition>
ORDER BY
    <order_by_condition>
LIMIT
    <limit_params>
```

增加：

```sql
INSERT INTO table_name VALUES (value1, value2, ..., valueN);

INSERT INTO table_name (field1, field2,...fieldN) VALUES (value1, value2, ..., valueN);
```

更新：

```sql
UPDATE table_name SET field1=value1, field2=value2 [WHERE Clause];
```

删除：

```sql
DELETE FROM table_name [WHERE Clause];
```

清空数据表，即删除数据表中的所有数据，但不删除表结构：

```sql
TRUNCATE TABLE table_name;
```

## 数据类型

> 深入浅出MySQL 数据库开发、优化与管理维护

### 数值类型

MySQL 支持所有标准 SQL 数值数据类型。这些类型包括严格数值数据类型（INTEGER、SMALLINT、DECIMAL 和 NUMERIC），以及近似数值数据类型（FLOAT、REAL 和 DOUBLE PRECISION ）。

作为 SQL 标准的扩展，MySQL 也支持整数类型 TINYINT、MEDIUMINT 和 BIGINT。另外，MySQL 还提供了关键字 INT 和 DEC，分别是 INTEGER 和 DECIMAL 的同义词。

#### 整数类型

MySQL支持 的整数类型有 TINYINT、SMALLINT、MEDIUMINT、INT、BIGINT，其属性字段可以添加 AUTO\_INCREMENT 自增约束条件以及无符号 UNSIGNED 关键字。如下表所示：

| 类型          | 大小    | 取值范围（有符号） | 说明                                 |
| ------------- | ------- | ------------------ | ------------------------------------ |
| TINYINT       | 1个字节 | -2^7～2^7-1        | 很小的整数                           |
| SMALLINT      | 2个宇节 | -2^15～2^15-1      | 小的整数                             |
| MEDIUMINT     | 3个字节 | -2^23～2^23-1      | 中等大小的整数                       |
| INT (INTEGER) | 4个字节 | -2^31～2^31-1      | 普通大小的整数                       |
| BIGINT        | 8个字节 | -2^63～2^63-1      | 大整数，无符号BIGINT通常用于表的主键 |

不同的整数类型有不同的取值范围，并且需要不同的存储空间，因此应根据实际需要选择最合适的类型，这样有利于节省存储空间。

#### 浮点数类型

| 类型    | 大小           | 取值范围       | 说明           |
| ------- | -------------- | -------------- | -------------- |
| FLOAT   | 4个字节        |                | 单精度浮点数值 |
| DOUBLE  | 8个字节        |                | 双精度         |
| DECIMAL | 依赖于M和D的值 | 依赖于M和D的值 | 最精确的浮点数 |

### 日期时间类型

MySQL 中有多种数据类型可以用于日期和时间的表示。下表列出了 MySQL5.0中所支持的日期和时间类型

| 类型      | 大小 ( Bytes) | 范围                                                         | 格式                | 用途                     |
| :-------- | :------------ | :----------------------------------------------------------- | :------------------ | :----------------------- |
| DATE      | 3             | 1000-01-01/9999-12-31                                        | YYYY-MM-DD          | 日期值                   |
| TIME      | 3             | '-838:59:59'/'838:59:59'                                     | HH:MM:SS            | 时间值或持续时间         |
| YEAR      | 1             | 1901/2155                                                    | YYYY                | 年份值                   |
| DATETIME  | 8             | '1000-01-01 00:00:00' 到 '9999-12-31 23:59:59'               | YYYY-MM-DD hh:mm:ss | 混合日期和时间值         |
| TIMESTAMP | 4             | '1970-01-01 00:00:01' UTC 到 '2038-01-19 03:14:07' UTC结束时间是第 **2147483647** 秒 | YYYY-MM-DD hh:mm:ss | 混合日期和时间值，时间戳 |

这些数据类型的主要区别如下：

-   如果要用来表示年月日，通常用 DATE 来表示。
-   如果要用来表示年月日时分秒，通常用 DATETIME 来表示。
-   如果要用来表示时分秒，通常用 TIME 来表示。
-   如果需要经常插入或更新日期为当前系统时间，通常使用 TIMESTAMP 来表示。
-   如果只是表示年份，可以用 YEAR 来表示。

从上表可以看出，每种日期时间类型都有一个有效值范围。如果超出这个范围，系统会进行错误提示，并将以零值来进行存储。

TIMESTAMP 和 DATETIME 的表示方法非常类似，主要区别如下：

-   TIMESTAMP 存储大小为 4 字节，支持的时间范围较小，而 DATETIME 占 8 字节，支持的范围更大。
-   表中的第一个 TIMESTAMP 列自动设置为系统时间。如果在写数据时指定为 NULL 或没有明确赋值，则 MySQL 会自动设置该列为系统当前的日期和时间。
-   TIMESTAMP 的插入和查询都受当地时区的影响，更能反映出实际的日期，而 DATETIME 则只能反映插入时当地的时区。

### 字符串类型

MySQL提供了多种对字符数据的存储类型，比如CHAR、VARCHAR、TEXT等。

| 类型       | 大小                  | 用途                            |
| :--------- | :-------------------- | :------------------------------ |
| CHAR       | 0-255 bytes           | 定长字符串                      |
| VARCHAR    | 0-65535 bytes         | 变长字符串                      |
| TINYBLOB   | 0-255 bytes           | 不超过 255 个字符的二进制字符串 |
| TINYTEXT   | 0-255 bytes           | 短文本字符串                    |
| BLOB       | 0-65 535 bytes        | 二进制形式的长文本数据          |
| TEXT       | 0-65 535 bytes        | 长文本数据                      |
| MEDIUMBLOB | 0-16 777 215 bytes    | 二进制形式的中等长度文本数据    |
| MEDIUMTEXT | 0-16 777 215 bytes    | 中等长度文本数据                |
| LONGBLOB   | 0-4 294 967 295 bytes | 二进制形式的极大文本数据        |
| LONGTEXT   | 0-4 294 967 295 bytes | 极大文本数据                    |

#### CHAR

CHAR 是固定长度的字符串类型，需要指定一个长度。CHAR(M) 为固定长度字符串，在定义时指定字符串列长。当保存时，在右侧填充空格以达到指定的长度。M 表示列的长度，范围是 0～255 个字符。

例如，CHAR(4) 定义了一个固定长度的字符串列，包含的字符个数最大为 4。当检索到 CHAR 值时，尾部的空格将被删除。

#### VARCHAR

VARCHAR 是变长的字符串类型，需要指定一个最大长度。VARCHAR(M) 是长度可变的字符串，M 表示最大列的长度，M 的范围是 0～65535。VARCHAR 的最大实际长度由最长的行的大小和使用的字符集确定，而实际占用的空间为字符串的实际长度加 1。

例如，VARCHAR(50) 定义了一个最大长度为 50 的字符串，如果插入的字符串只有 10 个字符，则实际存储的字符串为 10 个字符和一个字符串结束字符。VARCHAR 在值保存和检索时尾部的空格仍保留。

#### TEXT

TEXT 类型用于保存非二进制字符串，如文章内容、评论等。当保存或查询 TEXT 列的值时，不删除尾部空格。 &#x20;

TEXT 类型分为 4 种：TINYTEXT、TEXT、MEDIUMTEXT 和 LONGTEXT。不同的 TEXT 类型的存储空间和数据长度不同。 &#x20;

-   TINYTEXT 表示长度为 255（2^8-1）字符的 TEXT 列。
-   TEXT 表示长度为 65535（2^16-1）字符的 TEXT 列。
-   MEDIUMTEXT 表示长度为 16777215（2^24-1）字符的 TEXT 列。
-   LONGTEXT 表示长度为 4294967295 或 4GB（2^32-1）字符的 TEXT 列。

#### BLOB

BLOB 用于保存二进制格式存储的数据，有 4 种类型：TINYBLOB、BLOB、MEDIUMBLOB 和 LONGBLOB。

## 运算符

MySQL支持多种类型的运算符。

#### 算术运算符

算术运算符包括+、-、\*、/（DIV）、%（MOD）。

#### 比较运算符

比较运算符包括=、<>或!=、<=>（NULL安全的等于）、<、<=、>、>=、BETWEEN、IN、IS NULL、IS NOT NULL、LIKE、REGEXP。

#### 逻辑运算符

NOT或!（逻辑非）、AND或&&（逻辑与）、OR或||（逻辑或）、XOR（逻辑异或）。

#### 位运算符

位运算符包括 &、|、^、\~、>>、<<。

## 常用函数

> 深入浅出MySQL 数据库开发、优化与管理维护

函数可以用在SELECT语句及其子句（例如WHERE、ORDER BY等）中，也可以用在UPDATE、DELETE语句机器子句中。

#### 字符串函数

字符串函数包括CONCAT、INSERT、LOWER、UPPER、LEFT、RIGHT、LPAD、RPAD、LENGTH、CHAR\_LENGTH等。

#### 数值函数

数值函数包括ABS、CEIL、FLOOR、MOD、RAND、ROUND、TRUNCATE等。

#### 日期和时间函数

日期和时间函数包括CURDATA、CURTIME、NOW、UNIX\_TIMESTAMP、FROM\_UNIXTIME、YEAR等。

#### 流程函数

流程函数也是很常用的一类函数，可以用来实现条件选择等功能。流程函数包括IF、IFNULL等。

#### 其他函数

MySQL还有很多其他函数，比如DATABASE、VERSION、USER、INET\_ATON、INET\_NTOA、PASSWORD等。

## 视图

MySQL 从 5.0.1 版本开始提供视图功能。

视图（View）是一种虚拟存在的表。视图并不在数据库中实际存在，行和列数据来自定义视图的查询，并且是在使用视图时动态生成的。通俗的讲，视图就是一条 SELECT 语句执行后返回的结果集。所以，在创建视图时，主要的工作就在编写这条查询语句上。

视图相对于普通的表的优势主要包括以下几项：

-   简单：使用视图的用户完全不需要关心后面对应的表的结构、关联条件和筛选条件，对用户来说已经是过滤好的复合条件的结果集。
-   安全：使用视图的用户只能访问他们被允许查询的结果集，对表的权限管理并不能限制到某个行某个列，但是通过视图就可以简单的实现。
-   数据独立：一旦视图的结构确定了，可以屏蔽表结构变化对用户的影响，源表增加列对视图没有影响；源表修改列名，则可以通过修改视图来解决，不会造成对访问者的影响。

创建视图需要有 CREATE VIEW 的权限，并且对于查询涉及的列有 SELECT 权限。如果使用 CREATE OR REPLACE 或者 ALTER 修改视图，那么还需要该视图的 DROP 权限。

创建视图：

```sql
CREATE [OR REPLACE] [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
VIEW view_name [(column_list)]
AS select_statement
[WITH [CASCADED | LOCAL] CHECK OPTION]
```

修改视图：

```sql
ALTER [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
VIEW view_name [(column_list)]
AS select_statement
[WITH [CASCADED | LOCAL] CHECK OPTION]
```

删除视图，用户可以一次删除一个或多个视图，前提是必须有该视图的 DROP 权限。

```sql
DROP VIEW [IF EXISTS] view_name [, view_name] ...[RESTRICT | CASCADE]
```

从 MySQL 5.1 版本开始，使用 SHOW TABLES 命令的时候不仅显示表的名字，同时也会显示视图的名字，而不存在单独显示视图的 SHOW VIEWS 命令。查看表（包括视图）：

```sql
SHOW tables;
```

查看视图的定义：

```sql
SHOW CREATE VIEW [view_name];
```

## 存储过程和函数

MySQL 从 5.0 版本开始支持存储过程和函数。

存储过程和函数是事先经过编译并存储在数据库中的一段 SQL 语句的集合，调用存储过程和函数可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。

存储过程和函数的区别在于函数必须有返回值，而存储过程没有。存储过程的参数可以使用 IN、OUT、INOUT 类型，而函数的参数只能是 IN 类型的。

换句话说，函数是一个有返回值的过程 ，过程是一个没有返回值的函数 。

创建存储过程或函数：

```sql
CREATE PROCEDURE procedure_name ([proc_parameter[,...]])
begin
-- SQL语句
end;

CREATE FUNCTION function_name ([func_parameter[,...]])
begin
-- SQL语句
-- RETURNS
end;
```

示例：

```sql
delimiter $
CREATE PROCEDURE pro_test1()
begin
    select 'Hello Mysql';
end $
delimiter ;
```

> DELIMITER。该关键字用来声明SQL语句的分隔符，告诉 MySQL 解释器，该段命令已经结束。默认情况下，delimiter 是分号。在命令行客户端中，如果有一行命令以分号结束，那么回车后，MySQL 将会执行该命令。由于中间的 SQL 语句需要使用分号，所以整个命令的分隔符必须修改为其他符号。

调用存储过程：

```sql
CALL procedure_name();
```

删除存储过程或函数：

```sql
DROP [PROCEDURE | FUNCTION] [IF EXISTS] sp_name;
```

查看存储过程或函数：

```sql
SHOW [PROCEDURE | FUNCTION] status [LIKE 'pattern'];
```

查询存储过程或函数的定义：

```sql
SHOW CREATE [PROCEDURE | FUNCTION] sp_name;
```

## 触发器

MySQL 从 5.0.2 版本开始支持触发器。

触发器是与表有关的数据库对象，在满足条件时（INSERT、UPDATE、DELETE之前或之后）触发，并执行触发器中定义的语句集合。触发器的这种特性可以协助应用在数据库端确保数据的完整性、记录日志等 。

使用别名 OLD 和 NEW 来引用触发器中发生变化的记录内容，这与其他的数据库是相似的。现在触发器还只支持行级触发，不支持语句级触发。

| 触发器类型       | NEW和OLD的使用                      |
| ----------- | ------------------------------- |
| INSERT 型触发器 | NEW 表示将要或者已经新增的数据               |
| UPDATE 型触发器 | OLD 表示修改之前的数据，NEW 表示将要或已经修改后的数据 |
| DELETE 型触发器 | OLD 表示将要或者已经删除的数据               |

创建触发器：

```sql
CREATE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE
ON table_name
[FOR EACH ROW] -- 行级触发器
begin
    trigger_stmt;
end;
```

查看触发器：

```sql
SHOW TRIGGERS;
```

删除触发器：

```sql
DROP TRIGGER [schema_name.] trigger_name;
```



# 索引

> [MySQL索引——分类、何时使用、何时不使用、何时失效](https://blog.csdn.net/weixin_39420024/article/details/80040549 "MySQL索引——分类、何时使用、何时不使用、何时失效")
> [数据库两大神器【索引和锁】](https://juejin.im/post/6844903645125820424 "数据库两大神器【索引和锁】")

## 基础

索引（Index）是帮助 MySQL 高效获取记录的数据结构。在用户程序的数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据上实现高级查找算法，这种数据结构就是索引。

一般来说索引本身也很大，不可能全部存储在内存中，因此索引往往以索引文件的形式存储在磁盘上。索引是数据库中用来提高性能的最常用的工具。

索引的优点有：

-   减少检索数据的数量，提高检索数据的效率，降低数据库的 IO。
-   通过索引对数据进行排序，降低数据排序的成本，降低CPU的消耗。
-   把随机 IO 变为顺序 IO，加速表和表之间的连接。

索引的缺点有：

-   占用一定空间，如果索引过多，容易造成空间浪费。
-   降低插入、更新和删除操作的速度，不仅要保存数据，还要更新索引。

适合建索引的条件：

-   表比较大，查询操作占多数的表适合建索引。
-   经常查询的字段适合建索引。
-   经常作为 WHERE 查询条件的字段适合建索引。
-   经常需要排序的字段适合建索引。
-   与其他表关联的字段，比如外键，适合建索引。

不适合建索引的条件：

-   表记录少的。
-   增删改操作占多数的表或者字段不应该建立索引。
-   WHERE 条件里用不到的字段不应该创建索引。
-   过滤性不好的不适合建索引。

## 索引语法

索引可以在创建表的时候设置索引， 也可以在建好的表上增加新的索引。

创建索引：

```sql
CREATE [UNIQUE|FULLTEXT|SPATIAL] INDEX index_name
[USING index_type]
ON tbl_name(index_col_name,...)

index_col_name : column_name[(length)][ASC | DESC]
```

查看索引：

```sql
SHOW INDEX FROM table_name;
```

删除索引：

```sql
DROP INDEX index_name ON tbl_name;
```

用 ALTER 命令来添加索引：

```sql
# 这条语句创建索引的值必须是唯一的（除了NULL外，NULL可能会出现多次）
AlTER TABLE tb_name add unique index_name(column_list);
# 添加普通索引，索引值可以出现多次。
AlTER TABLE tb_name add index index_name(column_list);
# 该语句指定了索引为FULLTEXT，用于全文索引
AlTER TABLE tb_name add fulltext index_name(column_list);
```

## 索引分类

索引是在 MySQL 存储引擎层中实现的，而不是在 Server 层实现的。所以每种存储引擎支持的索引不完全相同，也不是所有的存储引擎都支持所有的索引类型的。

索引大致可以分为：

-   B-Tree 索引：B+ 树索引。最常见的索引类型，一般使用 B+ 树来实现。
-   Hash 索引：哈希索引。只有Memory引擎支持，使用场景简单。
-   R-Tree 索引：空间索引是MyISAM引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少，不做特别介绍。
-   Full-Text 索引：全文索引，主要用于全文搜索。另外，InnoDB 存储引擎从 MySQL5.6 版本开始支持全文索引。

比较常用的索引就是 B-Tree 索引和 Hash 索引。Hash 索引相对简单，只有 Memory 存储引擎支持 Hash 索引。

### B-Tree索引（B+ 树索引）

B-Tree索引是最常见的索引，其构造类似一棵树，能根据键值进行快速查找需要的行。需要注意的是，B-Tree 索引中的 B-Tree 不是指二叉树（Binary Tree），而是平衡树（Balanced Tree）。

这种索引可以分为几类：

-   普通索引：主要目的是为了加快查询数据，一张表允许建多个普通索引，允许数据重复和 NULL。
-   唯一索引：类似普通索引，索引列的值必须唯一，可以为 NULL。唯一索引可以保证该属性列的数据的唯一性，也可以提高查询效率。
-   主键索引：特殊的唯一索引，只能有一个，不允许为 NULL，不允许重复，一般是在建表时指定。对于InnoDB存储引擎，如果没有显式指定主键，InnoDB 会自动先检查表中是否有唯一索引的字段，如果有，则选择该字段为默认的主键，否则 InnoDB 将会自动创建一个自增主键。另外，非主键索引也可以称为辅助索引或二级索引。
-   联合索引（复合索引）：在多个字段上创建普通索引或唯一索引索引，可以同时创建多个索引，遵循最左匹配原则。

### 哈希索引

哈希索引使用了哈希算法，通过键值计算出一个哈希值，检索时不需要类似 B+ 树那样从根节点到叶子节点逐级查找，只需一次哈希算法即可立刻定位到相应的位置，速度非常快。

哈希索引也有一些局限：

-   不支持范围查询。
-   没办法利用索引完成排序。
-   在有大量重复键值情况下，哈希索引的效率也是极低的（哈希碰撞问题）。

## 索引结构

> [https://www.cnblogs.com/itwxe/p/15783164.html](https://www.cnblogs.com/itwxe/p/15783164.html "https://www.cnblogs.com/itwxe/p/15783164.html")

### B 树

B 树又叫多路平衡搜索树，一颗 m 叉的 B 树特性如下：

-   树中每个节点最多包含 m 个孩子。
-   除根节点与叶子节点外，每个节点至少有 [ceil(m/2)] 个孩子。
-   若根节点不是叶子节点，则至少有两个孩子。
-   所有的叶子节点都在同一层。
-   每个非叶子节点由 N 个 key 与 N+1 个指针组成，其中 [ceil(m/2)-1] <= N <= m-1。

以 3 叉 B 树为例，由 [ceil(m/2)-1] <= N <= m-1 可得 1<= N <=2。如下图所示，

![image_8bwblGJclN](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311272149152.png)

和二叉树相比，B 树的查询效率更高。因为对于相同的数据量来说，B树的层级结构比二叉树小。

### B+ 树

B+ 树是 B 树的变种，两者的主要区别在于：

-   B 树的非叶子结点存放了 N 个关键字和 N+1 个指针，而 B+ 树的非叶子结点的关键字与指针个数相同。
-   B 树的非叶子结点同时存放了关键字和数据，而 B+ 树的非叶子结点只存放关键字，所有的数据都存在叶子结点。
-   B 树的检索过程可能没有到达叶子结点就结束了，而 B+ 树的检索都是从根结点到叶结点。
-   B 树非叶子节点是独立的，B+ 树为所有叶子结点增加一个互相连接的指针。

3 叉 B+ 树的结构如下所示：

![image_wzeJOc_Uwc](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311272149294.png)

和 B 树相比，B+ 树的查询效率更加稳定。因为 B+ 树只有叶子节点保存 key 信息，查询任何关键字都要从根节点走到叶子。

### MySQL 中的 B+ 树

MySQL 的基本存储结构是页，页的大小一般是16KB（操作系统的磁盘块通常是 4KB）。页中存储了数据记录、页目录（Page Directory）等信息，其中页目录就是索引，可以指向其他的页。

InnoDB存储引擎的页结构如下图所示：

![image_cJi_c_u_-0](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311272149541.png)

通过索引来查找数据时，先读取根节点的页，使用二分查找搜索其中合适的索引，找到下一层的页，直到找到叶子节点，在叶子节点的数据中查找目标数据。

也就是说，当需要读一条记录的时候，并不是将这个记录本身从磁盘读出来，而是以页为单位，将其整体读入内存。

不通过索引查找数据时，只能通过叶子节点的双向链表遍历所有的页，查找每一个页中的每一条记录。

## 聚簇索引

聚簇索引（Cluster index）是指数据和索引保存在相同的底层数据结构中，也可以称为聚集索引。比如 InnoDB 存储引擎的主键索引，因为数据本身和数据的主键索引存放在同一棵 B+ 树上，而且数据本身就是按主键索引底层的 B+ 树进行组织的。B+ 树的叶节点存放的数据（data）是**完整的数据记录**，而非叶节点存放的是主键值。

和聚簇索引正好相反，非聚簇索引是指数据和索引保存在不同的底层数据中，也可以称为非聚集索引。比如 InnoDB 的非主键索引，非主键索引底层也是一棵 B+ 树，其中叶子节点存放的数据（Data）是**主键值和索引列**，非叶子结点存放的是索引列。

对于 InnoDB 存储引擎：

-   在根据主键索引查询时，如果找到 Key，就可以直接从 Key 所在的结点取出完整的数据。
-   在根据辅助索引查询时，如果找到 Key，如果查询数据不仅仅包括主键值和索引列，那么就需要在结点取出主键值，再根据主键值走一遍主键索引进行查询，这称为二次回表。

-   建议使用比较短的主键，比如自增主键，因为过长的字段会导致辅助索引过大。
-   建议按照顺序插入数据，因为没有顺序的主键索引值会造成主键索引频繁分裂。

另外，MyISAM 索引也可以认为是非聚簇索引，因为数据和对应的索引是分开储存的。MyISAM 索引底层也是一棵 B+ 树，其中叶子节点存放的数据（Data）是**数据记录的地址**，非叶节点存放的是索引列。因此，对于 MyISAM 存储引擎，在检索数据时，首先按照B+树搜索，如果指定的 Key 存在，则取出数据 Data，然后以数据 Data 的值为地址读取相应的记录。

## 联合索引

联合索引（Join Index）是指用多个非主键列建立索引，也可以称为组合索引。一个联合索引相当于一次性建立多个索引。

联合索引的好处是在一棵 B+ 树上存储多个索引，减少了索引的空间。比如，在 (a,b,c) 三个字段上建立索引，可以同时建立(a)、(a,b)、(a,b,c) 三个索引。如果这些字段建立单列索引，会建立多棵 B+ 树，占用磁盘空间较多。

### 最左匹配原则

联合索引遵循最左匹配原则。最左匹配原则是指，按联合索引建立时列的顺序进行连续匹配，遇到范围查询时（>、<、between、like）停止匹配。举例，对于联合索引 (a,b,c)，当查询条件是`a=1 and b<10 and c=3`，根据最左匹配原则可以使用 (a,b) 索引，无法使用 (a,b,c) 索引。

如果查询条件的顺序和联合索引的顺序不同，MySQL 会自动优化为相同的顺序，再进行最左匹配。举例，对于联合索引 (a,b,c)，当查询条件是`a=1 and c>3 and b=10`，MySQL会自动优化为`a=1 and b=10 and c>3`，因为可以使用完整的 (a,b,c) 索引。

举个例子，对于 (a,b) 联合索引，底层数据的顺序如下图所示：

![image_3_On7tjHft](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311272149252.png)

从小到大看，可以发现：

-   a 是有序的：1，1，2，2，3，3。
-   b 是无序的：1，2，1，4，1，2。

但是，当a的值固定时，b是有序的。例如a = 1时，b值为1，2，是有序状态。当a=2时，b的值为1，4，也是有序状态。

也就是说，联合索引是在对字段a的排序基础上，对字段b排序。所以，使用联合索引需遵循最左匹配原则。

### 覆盖索引

联合索引中有一种特殊的索引，**覆盖索引**，就是包含了所有查询字段的索引。

覆盖索引的好处是可以避免二次回表。对于 InnoDB 存储引擎来说，辅助索引在叶子节点中保存的是主键值和索引列。一般来说，如果使用非主键索引查询时，在查找到相应的结点后，还要通过主键索引进行二次查询获得全部数据。而对于覆盖索引来说，辅助索引的索引列中已经包含了全部所需数据，避免了对主键的二次查询 ，提升了查询效率。

## 注意事项

尽量选择区分度高的列作为索引。区分度是指字段不重复的比率，其计算公式是`COUNT(DISTINCT col)/COUNT(*)`。分度高越大，查询时扫描的记录数就越少。另外，创建联合索引时，应该把区分度高的列放在左边。

避免在 WHERE 子句中对字段施加函数，这会导致无法命中索引。

删除长期未使用的索引，不用的索引的存在会造成不必要的性能损耗。

避免冗余索引。冗余索引是指功能相同的索引，但是 MySQL 在查询时只能使用一个索引，而且会从多个索引中选择一个最好的索引。冗余索引并不会提高查询效率，但是却占用了更多的空间。

考虑在字符串类型的字段上使用前缀索引代替普通索引，前缀索引仅限于字符串类型，占用更小的空间。

## 常见面试题

### 普通索引和唯一索引有什么区别？

唯一索引与普通索引类似，不同的就是普通索引可以重复，唯一索引不能重复。 

唯一索引可作为数据的一个合法验证手段，例如身份证号码字段，人为规定该字段不得重复，那么就使用唯一索引。

对于不同的操作：

1）查询操作

我们知道 InnoDB 的数据是按数据页为单位来读写的。也就是说，当需要读一条记录的时候，并不是将这个记录本身从磁盘读出来，而是以页为单位，将其整体读入内存。

在 InnoDB 中，每个数据页的大小默认是 **16KB**。因为存储引擎是按页读写的，所以说，当找到 k=5 的记录的时候，它所在的数据页的数据都在内存里了。那么，对于普通索引来说，要多做一次“查找和判断下一条记录”操作，需要一次指针寻找和一次计算。

当然，如果 k=5 这个记录刚好是这个数据页的最后一个记录，那么要取下一个记录，必须读取下一个数据页，这个操作会稍微复杂一些。但是，对于整型字段，一个数据页可以放近千个 Key，因此出现这种情况的概率相对来说比较低。

所以，我们计算平均性能差异时，仍可以认为这个操作成本对于现在的 CPU 来说可以忽略不计。

2）更新操作

为了说明普通索引和唯一索引对更新语句性能的影响，需要先说下 **change buffer**。

当需要更新一个数据页时，如果数据页在内存中，则直接更新；如果这个数据页还没有在内存中的话，则在不影响数据一致性的前提下，InooDB 会将这些更新操作缓存在 change buffer 中，这样就不需要从磁盘中读入这个数据页了。

在下次查询需要访问这个数据页的时候，将数据页读入内存，然后执行 change buffer 中与这个页有关的操作。通过这种方式就能保证这个数据逻辑的正确性。

需要说明的是，虽然名字叫作 change buffer，实际上它是可以持久化的数据。也就是说，change buffer 在内存中有拷贝，也会被写入到磁盘上。将 change buffer 中的更改持久化到磁盘的过程称为 **merge**。访问这个数据页会触发 merge 外，系统有后台线程会定期 merge。在数据库正常关闭（shutdown）的过程中，也会执行 merge 操作。

显然，如果能够将更新操作先记录在 change buffer，减少读磁盘，语句的执行速度会得到明显的提升。而且，数据读入内存是需要占用 buffer pool 的，所以这种方式还能够避免占用内存，提高内存利用率。

理解了 change buffer 的机制，那么我们再一起来看看如果要在这张表中插入一个新记录 (3,300) 的话，InnoDB 的处理流程是怎样的。

第一种情况是，这个记录要更新的目标页在内存中。这时，InnoDB 的处理流程如下：

-   对于唯一索引来说，找到 3 的位置，判断到没有冲突，插入这个值，语句执行结束；
-   对于普通索引来说，找到 3 的位置，插入这个值，语句执行结束。

可以知道，普通索引和唯一索引对更新语句性能影响的差别，只是一个判断，只会耗费微小的 CPU 时间。可以忽略不计。

第二种情况是，这个记录要更新的目标页不在内存中。此时，InnoDB 的处理流程如下：

-   对于唯一索引来说，需要将数据页读入内存，判断到没有冲突，插入这个值，语句执行结束；
-   对于普通索引来说，则是将更新记录在 change buffer，语句执行就结束了。

将数据从磁盘读入内存涉及随机 IO 的访问，是数据库里面成本最高的操作之一。change buffer 因为减少了随机磁盘访问，因此对更新性能的提升是比较明显的。

### 哪些情况会导致索引失效？

1）使用不等号（!=和<>）。

2）类型不一致。如果字段类型是 varchar，查询时使用数字类型。

3）函数表达式。比如，查询条件为 WHERE ABS(num) = 10。

4）运算符表达式。比如，查询条件为 WHERE age - 1 = 20。

5）OR 条件。当where中使用了OR运算符，如果一个条件没有用到索引，那么整个查询条件都不会用到索引。

6）模糊搜索。对于前缀索引，比如查询条件为 WHERE name like "%ben"。

7）不满足最左匹配原则。



# 存储引擎

## 基础

存储引擎是指存储数据，建立索引，查询和更新数据等技术的实现方式。Oracle，Sql Server 等数据库只有一种存储引擎，而 MySQL 提供了多种存储引擎的选择。因为 MySQL 存储引擎是插件式的，所以用户可以根据需要使用最合适的存储引擎，甚至可以编写自己的存储引擎。

MySQL5.0 支持的存储引擎包含：InnoDB、MyISAM、BDB、MEMORY、MERGE、EXAMPLE、NDB Cluster、ARCHIVE、CSV、BLACKHOLE、FEDERATED等，其中InnoDB和BDB提供事务安全表，其他存储引擎是非事务安全表。

创建新表时可以通过ENGINE关键字指定存储引擎。比如，

```sql
CREATE table test_tab (
  id bigint(20) NOT NULL PRIMARY KEY AUTO_INCREMENT
) ENGINE=xxx;
```

如果不指定，那么 MySQL 就会使用默认的存储引擎。MySQL5.5 之前的默认存储引擎是 MyISAM，5.5以及之后的默认存储引擎是InnoDB。

查看 MySQL 数据库默认的存储引擎：

```sql
mysql> show variables like '%storage_engine%';
+----------------------------------+--------+
| Variable_name                    | Value  |
+----------------------------------+--------+
| default_storage_engine           | InnoDB |
| default_tmp_storage_engine       | InnoDB |
| disabled_storage_engines         |        |
| internal_tmp_disk_storage_engine | InnoDB |
+----------------------------------+--------+
4 rows in set (0.00 sec)
```

查看当前数据库支持的存储引擎：

```sql
mysql> show engines;
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
9 rows in set (0.00 sec)

```

从事务、锁等多个方面，对比一下几种常用的存储引擎，如下表所示：

| 特点     | InnoDB        | MyISAM | MEMORY | MERGE | NDB |
| ------ | ------------- | ------ | ------ | ----- | --- |
| 存储限制   | 64TB          | 有      | 有      | 没有    | 有   |
| 事务安全   | **支持**        |        |        | 表锁    | 行锁  |
| 锁机制    | **行锁（适合高并发**） | 表锁     | 表锁     | 支持    | 支持  |
| B树索引   | 支持            | 支持     | 支持     |       | 支持  |
| 哈希索引   |               | 支持     |        | 支持    | 低   |
| 全文索引   | 支持(5.6版本之后)   | 支持     |        | 低     | 高   |
| 集群索引   | 支持            |        |        | 低     | 高   |
| 数据索引   | 支持            | 支持     | 支持     | 高     |     |
| 索引缓存   | 支持            | 支持     | 支持     |       |     |
| 数据可压缩  | 支持            |        | N/A    |       |     |
| 空间使用   | 高             | 低      | 中等     |       |     |
| 内存使用   | 高             | 低      | 高      |       |     |
| 批量插入速度 | 低             | 高      |        |       |     |
| 支持外键   | **支持**        |        |        |       |     |

下面我们将重点介绍最常使用的两种存储引擎：MyISAM、InnoDB。

## MyISAM

> [MYSQL有哪些存储引擎，各自优缺点。](https://blog.csdn.net/riemann_/article/details/89930883 "MYSQL有哪些存储引擎，各自优缺点。")

MyISAM 是 MySQL5.5 以前的默认存储引擎。MyISAM 不支持事务、也不支持外键，其优势是访问的速度快，对事务的完整性没有要求或者以 SELECT、INSERT 为主的应用基本上都可以使用这个引擎。MyISAM 的优点还包括数据紧凑存储，因此可获得更小的索引和更快的全表扫描性能。

### 索引

MyISAM 索引可以认为是非聚簇索引，因为数据和对应的索引是分开储存的。MyISAM 索引底层也是一棵 B+ 树，其中非叶节点存放的是索引列，叶子节点存放的数据（Data）是**数据记录的地址**。因此，对于MyISAM存储引擎，在检索数据时，首先按照 B+ 树搜索，如果指定的Key存在，则取出数据 Data，然后以数据 Data 的值为地址读取相应的记录。

### 文件存储方式

每个MyISAM表在磁盘上存储成3个文件，其文件名都和表名相同，扩展名分别是：

-   FRM：存储表定义；
-   MYD：存储数据；
-   MYI：存储索引；

MyISAM把数据和索引存在不同的文件中，甚至可以把数据文件和索引文件放在不同的路径下，以获得更快速度。如果要设置索引文件和数据文件的路径，需要在创建表的时候通过 DATA DIRECTORY 和 INDEX DIRECTORY 语句指定。文件路径需要是绝对路径，并且要有访问权限。

MyISAM表支持3种不同的存储格式，分别是：

-   静态表：默认的存储格式。静态表中的字段都是非变长字段，这样每个记录都是固定的长度。优点是存储非常迅速，容易缓存，出现故障容易恢复；缺点是占用空间通常比动态表多。
-   动态表：动态表包含变长字段，记录不是固定长度的。优点是占用的空间相对较少，但是可能会产生碎片，需要定期制定 OPTIMIZE TABLE 语句。
-   压缩表：压缩表有 myisampack 工具创建，占据非常小的磁盘空间。

MyISAM 表可能会损坏，原因是多种多样的，损坏的表可能不能被访问，会提示需要修复或访问后返回错误的结果。MyISAM 有检查和修复的工具，可以用 CHECK TABLE 语句来检查 MyISAM 表的健康，并用 REPAIR TABLE 语句修复一个损坏的 MyISAM 表。

## InnoDB

InnoDB 是 MySQL5.5 及以后的默认存储引擎。

InnoDB 存储引擎支持外键和事务，具有事务提交、回滚、崩溃恢复的能力，而且支持行级锁。但是对比 MyISAM 存储引擎，InnoDB写的处理效率差一些，并且会占用更多的磁盘空间以保留数据和索引。

InnoDB 表的自动增长列可以手动插入，但是如果插入的值是 0 或者 NULL，则实际插入的将是自动增长后的值。

InnoDB 的默认事务隔离级别是 REPEATABLE READ，并且通过间隙锁策略防止幻读，到达了最高隔离级别的效果。

### 索引

InnoDB 的主键索引是聚簇索引。底层 B+ 树的非叶节点存放的是主键，叶子节点存放的数据（Data）是**完整的数据记录**。所以，通过主键进行查询有很高的性能。

非主键索引不是聚簇索引，底层 B+ 树的非叶节点存放的是索引列，叶子节点存放的数据（Data）是**索引列和主键值**。通过非主键索引查询会查出主键值，如果信息不足够再通过主键值进行查询（二次回表）。需要注意的是，如果主键很大，那么非主键索引也会很大，因此主键应当尽可能小。

### 外键

外键是指创建一个表（子表）时，使某一列参考另一个表（父表）中的某一列，其中子表中的这一列称为外键。

在所有存储引擎中，只有 InnoDB 支持外键。在创建外键时，要求父表中被参考的列必须有对应的索引，子表会在外键上自动创建索引。子表中的外键通常会参考父表中的主键。

举例：

```sql
CREATE country_tab (
  id int UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  country_id int(11) UNSIGNED
) ENGINE=Innodb;

CREATE city_tab (
  id int UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  city_id int(11) UNSIGNED PRIMARY KEY AUTO_INCREMENT,
  country_id int(11) UNSIGNED NOT NULL,
  CONSTRAINT `fk_city_country` FOREIGN KEY (country_id) REFERENCES country_tab(country_id)
) ENGINE=Innodb;
```

外键的使用条件包括：

1.  父表和子表两个表必须是InnoDB表，MyISAM表暂时不支持外键；
2.  外键列必须建立了索引，MySQL4.1.2以后的版本在建立外键时会自动给父表中的列创建索引，但如果在较早的版本则需要显式建立；
3.  建立外键关系的两个列必须是数据类型相似的，也就是两个列的数据类型必须可以相互转换，比如int和tinyint可以，而int和char则不可以；

在子表创建外键时，可以指定在删除、更新父表时，对子表进行的相应操作，包括RESTRICT、CASCADE、SET NULL和 NO ACTION：

-   RESTRICT和NO ACTION相同，表示在子表有关联记录的情况下，父表不能更新；
-   CASCADE，表示父表在更新或者删除时，更新或者删除子表对应的记录；
-   SET NULL，表示父表在更新或者删除的时候，子表的对应字段被SET NULL。

外键的优点：可以使得两张表关联，保证数据的一致性和实现一些级联操作。

外键的缺点：表之间存在硬性的关联，删除或更新父表可能会导致额外的操作。所以，**不推荐使用外键**。

### 文件存储方式

InnoDB存储表文件和索引文件有以下两种方式 ：

-   共享表空间存储。这种方式创建的表的表结构保存在`.frm`文件中，数据和索引保存在 innodb\_data\_home\_dir 和 innodb\_data\_file\_path 定义的表空间中，可以是多个文件。
-   多表空间存储（默认）。这种方式创建的表的表结构同样存在 `.frm` 文件中，但是每个表的数据和索引单独保存在`.ibd`文件中。

查看是否使用多表空间存储：

```mysql
mysql> Show variables like 'innodb_file_per_tab%';
+-----------------------+-------+
| Variable_name         | Value |
+-----------------------+-------+
| innodb_file_per_table | ON    |
+-----------------------+-------+
1 row in set (0.01 sec)
```

多表空间的数据文件没有大小限制，不需要设置初始大小，也不需要设置文件的最大限制、扩展大小等参数。对于使用多表空间特性的表，可以比较方便地进行单表备份和恢复操作。

### 事务

事务是一组SQL语句组成的逻辑操作单元。

事务具有4个特性（简称为ACID）：

-   原子性（Atomicity）：事务在逻辑上是不可分割的最小工作单元。事务中的所有操作要么全都执行，要么全都不执行，不能只执行其中的一部分。
-   一致性（Consistency）：在事务开始和完成时，数据都必须保持一致状态。这意味着所有相关的数据规则都必须应用于事务的修改，事务结束时，所有的内部数据结构（如索引或双向链表）也都必须是正确的。数据库总是从一个一致性的状态转换到另一个一致性的状态。
-   隔离性（Isolation）：数据库系统提供一定的隔离机制，保证事务在不受外部并发操作影响的“独立”环境执行。这意味着一个事务所做的修改在最终提交以前，对其他事务是不可见的。简单来说，并发运行的多个事务之间不会相互影响。
-   持久性（Durability）：事务完成之后，它对于数据的修改是永久性的。即使系统崩溃，修改的数据也不会丢失。

默认情况下，事务是默认提交的，可以查看 autocommit 变量：

```sql
mysql> show variables like 'autocommit%';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| autocommit    | ON    |
+---------------+-------+
1 row in set (0.00 sec)
```

如果需要在一个事务中包含多个语句，可以使用 BEGIN （或 START TRANSACTION）、COMMIT 命令，其中前者用于开启一个事务，后者用于提交一个事务。

另外，也可以通过设置 autocommit 变量关闭自动提交，

```sql
mysql> SET autocommit = 0;

mysql> show variables like 'autocommit%';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| autocommit    | OFF   |
+---------------+-------+
1 row in set (0.00 sec)
```

注意，设置 autommit 是 session 级别的，只是当前连接有效，对其他连接没有影响。

当事务不是自动提交时，执行一个 SELECT、INSETRT、UPDATE、DELETE 语句会开启一个事务，然后需要手动执行COMMIT命令提交事务，否则数据变动无法被其他的 session 观察到。

### redo log

redo log可以称为重做日志，是 InnoDB 存储引擎自带的日志，用于记录事务操作的变化。redo log 记录的是数据修改之后的值，不管事务是否提交都会记录下来。

在数据库服务器崩溃时，比如服务器突然断电，InnoDB 存储引擎会使用 redo log 恢复到崩溃前的时刻，以此来保证数据的完整性。

## 对比和选择

MyISAM 是 MySQL 5.5 版本之前的默认数据库引擎。MyISAM 性能比较好，支持全文索引、数据压缩、空间函数等，但不支持事务、行级锁和崩溃恢复。InnoDB 是 MySQL 5.5 及以后的默认存储引擎。InnoDB 存储引擎支持外键和事务，具有事务提交、回滚、崩溃恢复的能力，而且支持行级锁。

两者的对比：

-   外键：MyISAM不支持，而InnoDB支持。
-   事务和崩溃恢复：MyISAM 不提供事务支持。InnoDB 提供事务支持，具有事务提交回滚、崩溃修复的能力。
-   行级锁：MyISAM 只有表级锁，而 InnoDB 支持行级锁和表级锁，默认为行级锁。
-   MVCC：只有InnoDB支持。应对高并发事务，MVCC比单纯的加锁更高效。

绝大部分情况，我们都应该使用 InnoDB 存储引擎，因为其支持事务和崩溃恢复。但是，在某些情况下使用 MyISAM 也是合适的，比如读密集、对事务的完整性没有要求的场景。



# 锁

## 基础

锁是计算机程序协调多个进程或线程并发访问某一资源的机制，避免资源错误问题。

在数据库中，除传统的计算资源（如CPU、RAM、I/O等）的争用以外，数据也是一种供许多用户共享的资源。如何保证数据并发访问的一致性、有效性是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。

相对其他数据库而言，MySQL 的锁机制比较简单，其最显著的特点是不同的存储引擎支持不同的锁机制。从操作数据的角度，可以把锁分为：

-   表级锁：
    -   操作数据时，会锁定整个表。
    -   开销小，加锁快；不会出现死锁；锁定粒度大，发生锁冲突的概率最高，并发度最低。
    -   MyISAM 和 Memory 存储引擎使用表级锁。
-   行级锁：
    -   操作数据时，会锁定当前数据行。
    -   开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度也最高。
    -   InnoDB 存储引擎默认使用行级锁。
-   页面锁：
    -   操作数据是，会锁定当前数据所在的页面。
    -   开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般。
    -   BDB 存储引擎采用页面锁。

不同存储引擎对锁的支持情况如下表所示：

| 存储引擎   | 表级锁 | 页面锁 | 行级锁 |
| ------ | --- | --- | --- |
| MyISAM | 支持  | 不支持 | 不支持 |
| InnoDB | 支持  | 不支持 | 支持  |
| MEMORY | 支持  | 不支持 | 不支持 |
| BDB    | 支持  | 支持  | 不支持 |

从锁的角度来说：表级锁更适合于以查询为主，只有少量按索引条件更新数据的应用；而行级锁则更适合于有大量按索引条件并发更新数据，同时又有查询的应用，如一些在线事务处理系统。

## MyISAM

MyISAM存储引擎只支持表锁（Table Lock），可以分为读锁（表读锁，Table Read Lock）和写锁（表写锁，Table Write Lock）。

MyISAM的读锁和写锁的相互兼容性如下表所示：

| 当前锁模式/请求锁模式 | 读锁 | 写锁 |
| ----------- | -- | -- |
| 读锁          | 是  | 否  |
| 写锁          | 否  | 否  |

解释：

-   对 MyISAM 表的读操作（加读锁），不会阻塞其他用户对同一表的读请求，但会阻塞其他用户对同一表的写请求；
-   对 MyISAM 表的写操作（加写锁），不会阻塞当前用户对同一表的读和写请求，但会阻塞其他用户对同一表的读和写请求；

简单来说，MyISAM 表的读操作和写操作之间，以及写操作之间是串行的。当一个线程获得对一个表的写锁后，只有持有锁的线程可以对表进行更新操作，其他线程的读、写操作都会等待，直到锁被释放为止。

### 表锁

MyISAM存储引擎在执行查询语句（SELECT）前，会自动给涉及的所有表加读锁；在执行更新操作（UPDATE、DELETE、INSERT 等）前，会自动给涉及的所有表加写锁。加锁的过程对于用户是透明的，不需要用户干预。

当然，用户也可以在操作数据之前显式地加锁：

```sql
# 加读锁
lock table table_name read;
# 加写锁
lock table table_name write；
```

注意，显式加锁一般是为了方便说明问题，比如模拟事务操作，实现对某一时间点的一致性读取。

另外，显式加锁时必须同时取得所有涉及表的锁。也就是说，在执行 LOCK TABLE 后，只能访问显式加锁的这些表，不能访问未加锁的表。在自动加锁的情况下也是如此，MyISAM 总是一次获得 SQL 语句所需要的全部锁，这也正是 MyISAM 不会出现死锁的原因。

### 并发插入

MyISAM 的读锁和写锁是互斥的，所以 MyISAM 表的读和写操作是串行的。但是，MyISAM 存储引擎也允许设置一个系统变量 concurrent\_insert，来控制并发插入的行为。

-   0：不允许并发插入。
-   1：在 MyISAM 表没有空洞（表的中间没有被删除的行）的情况下，MyISAM允许在一个进程读取表的同时，另一个进程从表尾部插入记录。这是默认设置。
-   2：无论 MyISAM 表有没有空洞，都允许在表尾部并发插入记录。

### 锁调度策略

MyISAM 的读写锁调度的策略是写锁优先。如果一个进程请求某个 MyISAM 表的读锁，同时另一个进程也请求同一个表的写锁，那么写进程会先获得锁。即使读请求先到锁等待队列，写请求后到，写请求也会先获得到锁。

这是因为 MyISAM 认为写请求一般比读请求更重要。这也正是 MyISAM 不适合做写为主的表的存储引擎的原因。因为如果更新操作较多，会使得查询操作很难获得读锁，从而产生严重的锁等待，导致查询操作被阻塞、查询效率低。

## InnoDB

> [https://dev.mysql.com/doc/refman/8.0/en/innodb-locking-transaction-model.html](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking-transaction-model.html "https://dev.mysql.com/doc/refman/8.0/en/innodb-locking-transaction-model.html")

InnoDB 与 MyISAM 的最大不同有两点：一是支持事务，二是采用了行锁。行锁开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度也最高。InnoDB 默认使用行锁。

### 并发事务

> 参考InnoDB-事务一节。

多个事务并发运行能大大增加数据库资源的利用率，提高数据库系统的事务的吞吐量，从而可以支持更多的用户。但是，因为并发事务处理可能操作相同的数据，会导致一些问题：

-   修改丢失（Lost to modify）：指一个事务修改了一个数据后，还没有进行提交，此时另外一个事务也修改了这个数据，那么第一个事务的修改就丢失了。这种情况称为修改丢失。
-   脏读（Dirty read）：指一个事务修改了一个数据后，还没有进行提交，此时另外一个事务使用了这个修改过的数据。因为第一个事务还没有提交，我们可以认为第二个事务读到的数据是“脏数据”。这种情况称为脏读。
-   不可重复读（Unrepeatable read）：指在一个事务内多次读取同一个数据的结果不一样。如果在一个事务两次读取一个数据之间，另一个事务对这个数据进行了修改，导致第一个事务两次读取的结果不一样。这种情况称为不可重复读。
-   幻读（Phantom read）：幻读与不可重复读类似，是指一个事务以一定的查询条件读取了一些数据，之后按相同的查询条件发现多了一些原本不存在的记录，这是由于其他并发事务插入了满足该查询条件的新数据。这种情况称为幻读。

不可重复读和幻读的区别：不可重复读在于数据修改，比如多次读取一条记录，发现其中某些列的值被修改；幻读在于数据增加或者删除，比如多次读取某种条件的数据，发现记录增加或减少了。

在上面的问题中，“修改丢失”通常是应该完全避免的，而“脏读”、“不可重复读”和“幻读”其实都是数据库一致性问题，必须由数据库系统提供一定的事务隔离机制来解决。

#### 事务隔离

数据库系统实现事务隔离的方式，基本上可以分为两种：

-   锁：在操作数据前，对其加锁，阻止其他事务对数据进行修改。
-   多版本并发控制（Multi-Version Concurrency Control，简称MVCC）：MVCC不需要对数据加锁，而是通过一定机制在一个数据请求时间点，生成一个一致性数据快照（snapshot），并用这个快照来提供一定级别（语句级或事务级）的一致性读取。从用户的角度来看，好像数据库可以提供同一个数据的多个版本。

数据库的事务隔离越严格，并发副作用越小，但付出的代价也就越大。事务隔离实质上就是使用事务在一定程度上“串行化” 进行，这显然与“并发” 是矛盾的。

为了解决“隔离”与“并发”的矛盾，ISO/ANSI SQL92 标准定义了四种隔离级别，每个级别的隔离程度不同，允许出现的副作用也不同。隔离级别从低到到高分别是：

-   读未提交（Read Uncommitted）：最低的隔离级别，允许事务读取其他事没有提交的数据修改，可能会导致脏读、幻读或不可重复读。
-   读已提交（Read Committed）：允许事务读取其他事务已经提交的数据修改，可以防止脏读，但是可能会导致不可重复读和幻读。
-   可重复读（Repeatable Read）：InnoDB 存储引擎默认的隔离级别。在一个事务中，多次读取同一个数据的结果是一致的，除非是自己进行修改。该级别可以防止脏读和不可重复读，但是可能会导致幻读。
-   串行化（Serializable）：最高的隔离级别，完全服从 ACID 的隔离级别。所有的事务依次逐个执行，事务之间就完全不可能产生干扰。该级别可以防止脏读、不可重复读以及幻读。

不同隔离级别的特性如下：

| 隔离级别 | 读数据一致性   | 脏读 | 不可重复读 | 幻读 |
| ---- | -------- | -- | ----- | -- |
| 读未提交 | 最低级别     | 是  | 是     | 是  |
| 读已提交 | 语句级      | 否  | 是     | 是  |
| 可重复读 | 事务级      | 否  | 否     | 是  |
| 可串行化 | 最高级别，事务级 | 否  | 否     | 否  |

需要说明的是，不同数据库系统不一定完全实现了上述4个隔离级别。例如，Oracle 只提供 Read Committed 和 Serializable 两个标准隔离级别，另外还提供自己定义的 Read Only 隔离级别。MySQL 支持全部4个隔离级别，但在具体实现和应用时，有很多需要注意的地方。

比如，InnoDB 存储引擎默认的隔离级别是 Repeatable Read，可以查看`tx_isolation`变量：

```sql
mysql> SELECT @@tx_isolation;
+-----------------+
| @@tx_isolation  |
+-----------------+
| REPEATABLE-READ |
+-----------------+
1 row in set, 1 warning (0.00 sec)
```

由于使用了间隙锁，还可以避免幻读问题，所以 InnoDB 默认的隔离级别已经达到了 Serializable 隔离级别的要求。

### 行锁

> [https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html "https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html")

InnoDB存储引擎实现了标准的的行级锁（Row-level Locking），有两种类型：

-   共享锁（Shared Lock，简称S）：允许持有锁的事务读取一行。共享锁也可以称为读锁。
-   排他锁（Exclusive Lock，简称X）：允许持有锁的事务更新或删除一行。排他锁也可以称为写锁。

如果事务 T1 在行 r 上持有共享锁，则来自某个不同事务 T2 的请求在行 r 上的锁将按如下方式处理：

-   可以立即授予 T2 的共享锁请求。 结果，T1 和 T2 都持有 r 上的 S 锁。
-   不能立即授予 T2 对排他锁的请求。

如果事务 T1 在行 r 上持有排他锁，则无法授予来自某个不同事务 T2 对 r 上任一类型锁的请求。相反，事务 T2 必须等待事务 T1 释放它对行 r 的锁定。

为了允许行锁和表锁共存，实现多粒度锁机制，InnoDB还有两种内部使用的意向锁（Intention Locks）。这两种意向锁都是表锁：

-   意向共享锁（Intention Shared Lock，简称IS）：事务打算给数据加共享锁，在加共享锁之前必须先取得该表的意向共享锁。
-   意向排他锁（Intention Exclusive Lock，简称IX）：事务打算在数据加排他锁，在加排他锁之前必须先取得该表的意向排他锁。

上述锁模式的兼容情况如下表所示：

| 当前锁模式｜请求锁模式 | X      | IX | S      | IS |
| ----------- | ------ | -- | ------ | -- |
| X           | **冲突** | 冲突 | **冲突** | 冲突 |
| IX          | 冲突     | 兼容 | 冲突     | 兼容 |
| S           | **冲突** | 冲突 | **兼容** | 兼容 |
| IS          | 冲突     | 兼容 | 兼容     | 兼容 |

如果一个事务请求的锁模式与当前的锁兼容，InnoDB 就把请求的锁授予该事务；反之，如果两者不兼容，那么该事务就要等待锁释放。需要注意的是，意向锁是InnoDB存储引擎自动加的，不需要程序员干预！所以，我们只需要考虑共享锁和排他锁。

对于更新语句（UPDATE、DELETE 和 INSERT），InnoDB会自动给涉及的数据加排他锁。加锁的过程对于用户是透明的，不需要用户干预。对于普通查询语句（SELECT），InnoDB不会加任何锁，不过用户可以显式给数据加共享锁或排他锁：

```sql
-- 共享锁
SELECT * FROM table_name WHERE ... LOCK IN SHARE MODE;
-- 排他锁
SELECT * FROM table_name WHERE ... FOR UPDATE;
```

行锁升级为表锁：InnoDB的行锁是基于索引的，如果使用不通过索引的查询条件对数据加锁，那么 InnoDB 将对表中的所有记录加锁，实际效果跟表锁一样。

间隙锁：如果使用范围条件，而不是相等条件的查询语句请求锁，那么 InnoDB 不仅会给符合查询条件的已有数据进行加锁，也会给在查询条件范围内但并不存在的记录（间隙）加锁。这种锁机制就是所谓的间隙锁，间隙锁只能用在 Read Committed 和 Repeatable Read 隔离级别。间隙锁的好处是可以避免幻读问题，坏处是会降低并发度。

#### 行锁实现方式

InnoDB行锁是通过**给索引上的索引项加锁**来实现的。如果没有索引，InnoDB将通过隐藏的聚簇索引来对数据记录员加锁。

InnoDB行锁分为3种情形：

1）记录锁（Record lock）

记录锁对一个索引记录加锁。比如`SELECT c1 FROM t WHERE c1 = 10 FOR UPDATE;`阻止任何其他事务对该行的插入、更新和删除操作。

记录锁总是给索引记录加锁。如果一个表没有索引，InnoDB会创建一个隐藏的聚簇索引，并使用这个索引进行记录锁定。

2）间隙锁（Gap lock）

间隙锁对索引记录之间的“间隙”加锁，比如`SELECT c1 FROM t WHERE c1 >= 10 and c1 <= 20 FOR UPDATE;`阻止任何其他事务插入一条`t.c1=15`的记录，无论该列是否有等于该值的记录。

一个间隙可能跨越单个索引值、多个索引值，甚至是空的。

间隙锁是性能和并发性之间权衡的一部分，并且只用于 Read Committed 和 Repeatable Read 隔离级别。

3）邻键锁（Next-Key lock）

邻键锁是前两种锁的组合，即索引记录上的记录锁和索引记录之间的间隙上的间隙锁的组合。举例，emp\_tab表有101条记录，其emp\_id分别是1、2、...、100、101，对于`SELECT * from emp_tab where emp_id > 100 FOR UPDATE`来说，InnoDB不仅会对符合查询条件的 emp\_id 等于101的记录加记录锁，也会对 emp\_id 大于101的“间隙“加锁。

InnoDB 使用 Next-Key 锁的目的，一方面是为了防止幻读，满足相关隔离级别的要求；另一方面是为满足其恢复和复制的需要。

很显然，在使用范围条件查询并锁定记录时，InnoDB 这种加锁机制会阻塞符合条件范围内键值的并发插入，这往往会造成严重的锁等待。因此，在实际开发中，尤其是并发插入比较多的应用，应该尽量使用**相等条件**来访问更新数据，避免使用范围条件。

需要特别说明的是，如果在查询条件使用相等判断条件给一个不存在的记录加锁，InnoDB也会使用 Next-Key 锁，会阻塞其他事务插入数据。举例，如果使用相等判断条件对还不存在的主键值加锁，那么其他事务无法插入新数据，只能等待锁释放。

总结，InnoDB行锁的实现特点意味着：

-   InnoDB的行锁是基于索引的，如果使用不通过索引的查询条件对数据加锁，那么 InnoDB 将对表中的所有记录加锁，实际效果跟表锁一样。
-   行锁是针对索引记录加锁。如果不同的数据记录具有相同的索引键，那么不同的事务对这些数据记录加锁会出现锁冲突。
-   当表有多个索引时，不同的事务可以使用不同的索引锁定相同的数据记录，不论是使用主键索引、唯一索引还是普通索引，InnoDB 都会使用行锁对数据记录加锁。如果使用不同的索引对同一个数据记录加锁，那么会出现锁冲突。
-   即便在查询条件种使用了索引字段，但是是否使用索引来检索数据是由 MySQL 通过判断不同执行计划的代价来决定的。如果 MySQL 认为全表扫描效率更高，那么就不会使用索引，这种情况下会对所有记录加锁。因此，在分析锁冲突时，必须先检查 SQL 的执行计划，以确认是否真正使用了索引。
-   避免使用范围条件给数据加锁，应该尽量使用相等条件。
-   避免使用相等条件给不存在的记录加锁。

#### 行锁争用情况

通过检查 innodb_row_lock 状态变量，可以分析系统上的行锁的争用情况：

```sql
mysql> show status like 'innodb_row_lock%';
+-------------------------------+--------+
| Variable_name                 | Value  |
+-------------------------------+--------+
| Innodb_row_lock_current_waits | 0      |
| Innodb_row_lock_time          | 214513 |
| Innodb_row_lock_time_avg      | 10725  |
| Innodb_row_lock_time_max      | 51019  |
| Innodb_row_lock_waits         | 20     |
+-------------------------------+--------+
5 rows in set (0.01 sec)
```

解释：

-   innodb\_row\_lock\_current\_waits：当前正在等待行锁的数量。
-   innodb\_row\_lock\_time：从系统启动到现在，获取行锁花费的总时间（单位：毫秒）。
-   innodb\_row\_lock\_time\_avg：从系统启动到现在，获取行锁花费的平均时间（单位：毫秒）。
-   innodb\_row\_lock\_time\_max：从系统启动到现在，获取行锁花费的最大时间（单位：毫秒）。
-   innodb\_row\_lock\_waits：从系统启动到现在，等待行锁的总次数。

如果发现锁争用比较严重，比如 innodb\_row\_lock\_waits 和 innodb\_row\_lock\_time\_avg 的值比较高，可以通过查询 information\_schema 数据库中相关的表来查看锁情况，或者通过设置 InnoDB Monitors 来进一步观察发生锁冲突的表、数据等。

#### 恢复和复制对InnoDB锁机制的影响

MySQL 通过 binlog 记录执行成功的 INSERT、UPDATE、DELETE 等SQL语句，并由此实现 MySQL 数据库的恢复和主从复制。MySQL 的恢复机制有以下特点：

-   MySQL 的恢复机制是 SQL 语句级的，也就是在从数据库重新执行 binlog 中的 SQL 语句。
-   MySQL 的 binlog 是按照事务提交的先后顺序记录的，恢复也是按这个顺序进行的。

因此，要正确恢复或复制数据，就必须满足：在一个事务未提交前，其他并发事务不能插入满足其锁定条件的任何记录，也就是不允许出现幻读。这已经超过了 SQL92 标准 Repeatable Read 隔离级别的要求，实际上是要求事务串行化。这也是在许多情况下， InnoDB 要用到 Next-Key 锁的原因。比如在用范围条件更新记录时，无论是在 Read Committed 还是 Repeatable Read 隔离级别下，InnoDB 都要使用 Next-Key 锁，但这并不是隔离级别要求的。

MySQL 在处理`INSERT INTO target_tab SELECT * FROM source_tab WHERE ...`和`CREATE TABLE new_tab ...SELECT ... FROM source_tab WHERE ...`等语句时会给 source\_tab 加共享锁，而没有使用多版本数据一致性技术。加锁的主要原因是为了保证恢复和复制的正确性，具体例子参考《深入浅出MySQL》20.3.6节。但是，加锁可能会阻止对源表的并发更新，如果查询比较复杂，会造成严重的性能问题。所以，在实际中应该尽量避免使用。

#### 死锁

死锁是指多个事务都需要获取其他事务持有的排他锁才能继续完成事务，而且事务之间存在循环等待的情况。比如，两个事务都在等待获取对方持有的排他锁，无法继续各自的事务，导致事务失败。

> 死锁需要满足四个条件：
>
> - 互斥：多个事务不能使用同一个资源。
> - 持有并等待：一个事务持有资源不会释放，并等待其他资源。
> - 不可剥夺：事务持有的资源不可剥夺或抢走。
> - 循环等待：多个事务循环等待其他人持有的资源。

MyISAM 存储引擎使用表锁，而且不会发生死锁的情况，这是因为 MyISAM 总是一次性获得所需的全部锁，要么全部获得，要么等待。但是对于InnoDB存储引擎，锁是逐步获得的，导致可能发生死锁的情况。

对于 InnoDB 存储引擎，在发生死锁后，InnoDB 通常都能自动检测到，并使一个事务释放锁并回退，另一个事务获得锁，继续完成事务。查看是否开启检测死锁：

```mysql
mysql> Show variables like 'innodb_deadlock_detect';
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| innodb_deadlock_detect | ON    |
+------------------------+-------+
1 row in set (0.00 sec)
```

但是，在涉及外部锁或表锁的情况下，InnoDB 并不能完全自动检测到死锁，还需要设置锁等待超时参数 innodb\_lock\_wait\_timeout 来解决，默认是 50 秒。

```mysql
mysql> show variables like 'innodb_lock_wait%';
+--------------------------------+-------+
| Variable_name                  | Value |
+--------------------------------+-------+
| innodb_lock_wait_timeout       | 50    |
+--------------------------------+-------+
2 rows in set (0.00 sec)
```

通常来说，死锁是应用设计的问题，通过调整业务流程、数据库对象设计、事务大小以及访问数据库的SQL语句，绝大数死锁都可以避免。

为了尽可能减少死锁，有几个建议：

-   不同业务流程应该尽量以相同的顺序访问多个表。比如两个业务流程都需要更新表 A 和 B，应该约定使用相同的访问顺序。
-   在程序以批量方式处理数据时，最好先对数据排序，保证每个线程按固定的顺序来处理记录。
-   如果有更新操作，应该直接申请足够级别的锁（排他锁），而不是先申请共享锁，等到更新时再申请排他锁。
-   不要给不存在的记录加锁。在 Repeatable Read 隔离级别下，如果两个事务同时使用相同的查询条件用`SELECT ... FOR UPDATE`加排他锁，在没有符合该条件记录的情况下，两个事务都会加锁成功。如果两个事务因为发现记录不存在，都试图插入一条纪录，就那么就会发生问题：一个事务插入成功，另一个事务插入失败，返回死锁错误。
-   控制事务的大小。大的事务需要锁定的记录更多，处理时间更长，更容易产生死锁。
-   使用合理的索引。如果查询条件不走索引将会给表的所有记录加锁，死锁的概率大大增大。
-   降低隔离级别。如果业务允许，将隔离级别调低也是较好的选择，比如将隔离级别从 Repeatable Read 调整为 Read Commited，可以避免一些因为间隙锁造成的死锁。

### MVCC

> [mysql的MVCC（多版本并发控制）](https://www.cnblogs.com/myseries/p/10930910.html "mysql的MVCC（多版本并发控制）")
>
> [MySQL面试题：MVCC实现原理](https://www.bilibili.com/video/BV1xT411F7TW/)

MVCC 的全称是 Multi-Version Concurrency Control，即多版本并发控制。MVCC 通过维护同一个数据的多个版本（或快照 Snapshot），来解决**快照读**时的读写冲突。MVCC 的优点是可以避免读操作加锁导致的阻塞，减少了开销，提高了并发性能。

> 当前读：读取记录的最新版本。读取时需要保证其他并发事务不能修改当前记录，因此会对读取的记录进行加锁。比如SELECT LOCK IN SHARE MODE；SELECT FOR UPDATE；UPDATE；INSERT；DELETE。
> 快照读：非阻塞读，即不加锁的 SELECT 操作。快照读的实现是基于多版本并发控制 MVCC。具体来说，快照读是读取 MVCC 多版本数据链中的某个快照版本。

MVCC 只有 InnoDB 存储引擎支持（MyISAM 不支持），而且只能用在 Read Committed 和 Repeatable Read 两种隔离级别。对于其他两种隔离级别，Read Uncommitted 可以直接读其他事务未提交的数据，也就是总是读取最新的数据，因此不需要 MVCC；而串行级别 Serializable 根本不允许事务并发，所有事务完全按顺序执行，可以认为快照读在这个级别下会退化成当前读，因此也不需要 MVCC。

#### 底层实现和原理

> InnoDB是通过在每行记录后面保存两个隐藏的列来实现 MVCC 的。这两个列，一个保存了行的创建时间，一个保存行的过期时间（或删除时间）。当然，存储的并不是实际的时间值，而是系统版本号，每开始一个新的事务，系统版本号就会递增。事务开始时刻的系统版本号会作为事务的版本号，用来和查询到的每行记录的版本号进行比较。——来自《高性能MySQL》

具体来说，MVCC 的实现主要是依赖每行记录中的两个隐藏字段 DB\_TRX\_ID、DB\_ROLL\_PTR 以及读视图 Read View：

-   DB\_TRX\_ID：创建或修改该条记录的事务 ID。另外，事务ID会随着时间越来越大，后发生的事务ID要大于之前发生的事务。
-   DB\_ROLL\_PTR：回滚指针，指向这条记录的上一个版本。
-   Read View：快照读会创建一个读视图，其中记录并维护系统当前活跃事务的 ID，包括
    -   rw\_trx\_ids：创建 Read View 时，所有的活跃事务 ID。事务活跃意味着事务还没有提交。
    -   min\_trx\_id：活跃的事务 ID 列表 rw\_trx\_ids 中的最小事务 ID。
    -   max\_trx\_id：创建 Read View 时，将要分配给下一个事务的 ID。
    -   curr\_trx\_id：创建 Read View 的当前事务 ID。

对于读视图 Read View 来说，创建 Read View 的事务无法读取活跃的事务进行的修改，因为这些事务还没有提交。另外，读视图 Read View 在 Read Committed 和 Repeatable Read 两种隔离级别下的工作机制略有不同：

-   对于 Read Committed，每次快照读都会创建一个新的读视图 Read View，因此读视图中的活跃的事务 ID 列表可能会变化（因为其他并发事务可能已经提交）。这意味着，在这个隔离级别下，一个事务可以读取到其他并发事务提交的修改。这也就可以理解为什么这个隔离级别被称为读已提交 Read Committed。
-   对于 Repeatable Read，只在第一次快照读时创建一个读视图 Read View，并且在整个事务期间保持不变。另外，在这个隔离级别下，创建该读视图的事务只能读取 DB\_TRX\_ID 小于当前事务 ID 的那些版本的数据。

举个例子，在执行下列SQL命令后，

```sql
create table test_tab (
  id bigint PRIMARY KEY AUTO_INCREMENT,
  name varchar(50)
) engine=INNODB;

insert test_tab(id, name) values (1, "mysql"); -- DB_TRX_ID = 100

update test_tab set name = "sql" where id = 1; -- DB_TRX_ID = 200

update test_tab set name = "mvcc" where id = 1; -- DB_TRX_ID = 300
```

假设这条数据的版本链如下图所示，

![image_nraFNBOX6X](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311272150530.png)

稍微解释一下，新版本数据的事务ID大于旧版本的，并且有指针指向旧版本数据。另外，非最新版本的数据存储在 undo 日志中。

根据上面这条数据，举例说明一下快照读的原理。如果有一个快照读尝试查询`SELECT name FROM test_tab WHERE id = 1`，举几个例子

1）假设快照读所在的事务ID=301，读视图Read View的当前活跃的事务包括（205、255、300），其中事务205和255可能与数据id=1无关，那么该查询会查到name=sql。具体来说，查询从DB\_TRX\_ID=300版本的数据开始，发现对应的事务未提交（因为300在活跃事务中），然后根据DB\_ROLL\_PTR向后查找历史版本，最后发现DB\_TRX\_ID=200版本的数据已经提交（因为200不在活跃事务ID中），返回这个版本的数据。

2）假设快照读所在的事务ID=150，读视图Read View的当前活跃的事务包括（205、255、300），那么该查询在不同的隔离级别会查到不同的结果：

-   对于 Read Committed，会查到 name=sql，因为该隔离级别允许一个事务（事务ID=150）读取其他事务（事务ID=200）提交的更新。
-   对于 Repeatable Read，会查到 name=mysql，因为该隔离级别只能读取 DB\_TRX\_ID 小于当前操作事务的ID的数据。

3）假设快照读所在的事务ID=50，读视图Read View的当前活跃的事务包括（205、255、300），那么该查询在不同的隔离级别会查到不同的结果：

- 对于 Read Committed，可以查到 name=sql。
- 对于 Repeatable Read，查不到任何结果。

4）假设快照读所在的事务ID=300，读视图Read View的当前活跃的事务包括（205、255、300），那么该查询所处的事务是活跃的事务（还没有提交），可以观察到当前事务的更新，查到name=mvcc。

5）假设快照读所在的事务ID=299，读视图Read View的当前活跃的事务包括（205、255、300），

-   第一次查询，对于Read Committed和Repeatable Read，都会查到name=sql。
-   假设事务ID=300提交。
-   第二次查询，对于Read Committed，会创建一个新的读视图，活跃的事务包括（205，255），因此可以查到name=mvcc；对于Repeatable Read，读视图不会变化，因此仍然查到name=sql。这也就是为什么这个隔离级别称为可重复读Repeatable Read。

由于旧数据并不真正的删除，所以必须对这些数据进行清理，Innodb会开启一个后台线程执行清理工作，具体的规则是将删除版本号小于当前系统版本的行删除，这个过程叫做 purge。

### 悲观锁和乐观锁

一般来说，并发事务有三种数据冲突：

-   读读冲突：并发读之间互不影响。
-   读写冲突：加锁或使用 MVCC 机制。
-   写写冲突：必须加锁，包括悲观锁（Pessimistic lock）和乐观锁（Optimistic lock）。

解决写写冲突的方法有：

1）使用 Serializable 隔离级别，事务串行执行，那么就没有任何冲突。为了事务并发，大多数情况都不会使用这种方法。

2）使用悲观锁。悲观锁是在数据库层面加锁，但是等待锁会导致阻塞。

举例：`SELECT * FROM tab FOR UPDATE`，在`SELECT`语句后边加了`FOR UPDATE`相当于加写锁（排他锁）。加了写锁以后，其他事务不能进行修改，需要等待当前事务执行完。

3）使用乐观锁。

乐观锁不是数据库层面上的锁，而是一种思想，具体实现是在表中添加一个版本字段 version。当更新某个数据时，检查该数据的版本是否和预期相同。如果相同，才可以更新，否则不可以更新。

举例：`UPDATE tab SET name=xxx, version=version+1 WHERE ID={id} AND version={version}`，该语句会判断传入的 version 与当前的 version 字段是否相等，相等则更新成功，否则更新失败。所以，如果两个事务同时更新同一行数据，一个事务更新成功，其他事务就会更新失败。



# 日志

> [https://www.cnblogs.com/myseries/p/10728533.html](https://www.cnblogs.com/myseries/p/10728533.html "https://www.cnblogs.com/myseries/p/10728533.html")

在 MySQL 中，有多种不同的日志，包括错误日志、二进制日志、普通查询日志和慢查询日志，这些日志发挥着不同的作用。另外还有 redo 日志、undo 日志和 relay 日志。

## 错误日志

错误日志是 MySQL 中最重要的日志之一，它记录了当 MySQL 启动和停止时，以及服务器在运行过程中发生任何错误时的相关信息。当数据库出现任何故障导致无法正常使用时，可以先查看此日志。

错误日志是默认开启的，可以通过下面的命令查看错误日志的位置：

```sql
mysql> show variables like 'log_error%';
+---------------------+---------------------+
| Variable_name       | Value               |
+---------------------+---------------------+
| log_error           | /var/log/mysqld.log |
| log_error_verbosity | 3                   |
+---------------------+---------------------+
2 rows in set (0.03 sec)
```



## 二进制日志

> [https://www.jb51.net/article/273118.htm#\_lab2\_0\_0](https://www.jb51.net/article/273118.htm#_lab2_0_0 "https://www.jb51.net/article/273118.htm#_lab2_0_0")

二进制日志（Binary Log，binlog）记录了二进制格式的 SQL语句，包括DDL（数据定义语言）语句和 DML（数据操纵语言）语句，但是不包括数据查询语句。

binlog 主要用于主从复制，以及数据备份。具体地，MySQL 使用 binlog 在主数据库和从数据库之间同步数据，保证数据一致性。

binlog 有三种格式：

1）STATEMENT

该日志格式记录的是 SQL 语句（STATEMENT）。主从复制时，主数据库把 binlog 发送到从数据库，然后从数据库会把 binlog 解析为 SQL 语句，并重新执行。这里有一个问题，如果 SQL 中包含`update_time=now()`等类似的函数，该函数获取当前系统时间，直接执行会导致数据与原库的数据不一致。

2）ROW

该日志格式记录的是每一行的数据变更，而不是 SQL 语句。举例，对于`UPDATE test_tab SET status=1;`，如果是 STATEMENT 格式，在日志中会记录 SQL 语句； 如果是 ROW 格式，在日志中会记录每一行的数据修改，因为该 SQL 语句是对全表进行更新，也就是每一行记录都会被修改。

3）MIXED

该日志格式混合了STATEMENT 和 ROW两种格式，是一种折中的方案。MIXED 格式能尽量利用两种模式的优点，而避开他们的缺点。简单来说，MySQL 会判断这条 SQL 语句是否可能导致数据不一致，如果可能导致数据不一致就用 ROW 格式，否则就用 STATEMENT 格式。

#### 如何开启 binlog

默认情况下，二进制日志是没有开启的。下面的命令可以用来查看是否开启二进制日志，

```sql
mysql> show variables like 'log_bin%';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| log_bin       | OFF   |
+---------------+-------+
1 row in set (0.00 sec)
```

默认情况下，二进制日志的格式为 ROW。下面的命令可以用来查看二进制日志的格式，

```sql
mysql> show variables like 'binlog_format%';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| binlog_format | ROW   |
+---------------+-------+
1 row in set (0.00 sec)
```

如果要设置开启 binlog  以及binlog 的格式，需要在 MySQL 的配置文件设置 log\_bin 和 binlog\_format，其中 log\_bin 用于指定设置 binlog 的文件名，相当于开启 binlog；binlog\_format 用于指定 binlog 的格式。

```ini
# 配置binlog文件名为mysqlbin，那么生成的文件如mysqlbin.000001,mysqlbin.000002
log_bin=mysqlbin
# 配置二进制日志的格式
binlog_format=[STATEMENT|ROW|MIXED]
```

#### 如何查看 binlog

由于日志以二进制方式存储，不能直接读取，mysqlbinlog 工具可以用来解析和查看日志。

mysqlbinlog 的语法可以参考：[https://dev.mysql.com/doc/refman/5.7/en/mysqlbinlog.html](https://dev.mysql.com/doc/refman/5.7/en/mysqlbinlog.html "https://dev.mysql.com/doc/refman/5.7/en/mysqlbinlog.html")。

#### 写入磁盘

binlog 的写入磁盘流程大致如下图所示：

![image_SpKcWp0SoD](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311272150449.png)

解释：

-   在事务执行过程中，MySQL 把 binlog 写到 binlog cache。
-   当事务提交时，MySQL 把 binlog cache 写入文件系统，有两种方式：（1）只写入文件系统缓存page cache（2）先写入文件系统缓存 page cache，接着再写入磁盘的 binlog 文件中。如果是前者，写入磁盘的时间将由操作系统决定。
-   write 只是把 binlog cache 写入到文件系统缓存，并没有把数据持久化到磁盘，所以速度比较快，而 fsync是指把数据持久化到磁盘。

通过设置 sync\_binlog 参数可以控制 MySQL 写入 binlog cache 到文件系统的方式。该参数可能为0、1或者一个正整数N，默认值为 1，可以通过下面的命令查看：

```sql
mysql> show variables like 'sync_binlog%';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| sync_binlog   | 1     |
+---------------+-------+
1 row in set (0.00 sec)
```

1）当 sync\_binlog=0 时，表示每次提交事务都只执行 write 写入文件系统缓存，由系统自行判断什么时候执行 fsync。这种方式速度比较快，但是如果机器宕机，page cache 中的 binlog 可能会丢失，如下图所示。

![image_dyvPJH39m9](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311272151069.png)

2）当 sync\_binlog=1 时，表示每次提交事务都会执行 write 和 fsync。虽然这种方式速度慢一些，但是更安全。

3）当 sync\_binlog=N 时，这是一个折中方式，表示每次提交事务都write，但累积 N 个事务后才执行 fsync。在出现 IO 瓶颈的场景里，将 sync\_binlog 设置成一个比较大的值，可以提升性能。同样地，如果 MySQL 服务器宕机，会丢失最近 N 个事务的 binlog。

另外，因为一个事务的 binlog 不能被拆开，无论事务多大，也要确保一次性写入，所以系统会给每个线程分配一块内存作为 binlog cache，用于缓存事务的 binlog。另外，通过设置 binlog\_cache\_size 参数可以控制单个线程 binlog cache 大小，如果存储内容超过了这个参数，就要暂存 binlog cache 到磁盘。

下面的命令可以用来查看 binlog\_cache\_size 参数（默认是16K）：

```sql
mysql> show variables like 'binlog_cache_size%';
+-------------------+-------+
| Variable_name     | Value |
+-------------------+-------+
| binlog_cache_size | 32768 |
+-------------------+-------+
1 row in set (0.00 sec)
```

#### 过期日志删除

对于比较繁忙的系统，每天会生成大量 binlog。这些日志如果长时间不清除，将会占用大量的磁盘空间。删除日志有几种常用方法：

```mysql
# 删除全部 binlog 日志，删除之后，日志编号将从 xxxx.000001 重新开始。
reset master;
# 删除 ****** 编号之前的所有日志
purge master logs to 'mysqlbin.******'
# 删除 某一时间 之前产生的所有日志
purge master logs before 'yyyy-mm-dd hh24:mi:ss'
```

另外，还有一种定期删除日志的方法，需要在 MySQL 配置文件中设置日志过期参数，如下所示：

```ini
# 设置日志过期时间，过期日志将会被自动删除
--expire_logs_days=xxx
```



## 普通查询日志

普通查询日志（General query log）用来记录服务器接收到的每一个查询或命令。无论查询或是命令是否正确，普通查询日志都会将其记录下来 ，记录的格式为`{Time, Id, Command, Argument}`。

普通查询日志默认是关闭的，因为开启普通查询日志会导致 MySQL 服务器不断地记录日志，系统开销比较大。 下面的命令可以用来查看普通查询日志的配置，

```sql
mysql> show variables like 'general_log%';
+------------------+---------------------------------+
| Variable_name    | Value                           |
+------------------+---------------------------------+
| general_log      | OFF                             |
| general_log_file | /var/lib/mysql/9eca2f63e027.log |
+------------------+---------------------------------+
2 rows in set (0.00 sec)
```

如果要开启普通查询日志，可以在 MySQL 配置文件中添加如下的配置：

```ini
# 设置是否开启查询日志，0表示关闭，1表示开启
general_log=1
# 设置日志的文件名，如果没有指定，默认的文件名为 host_name.log
general_log_file=<file_name>
```



## 慢查询日志

慢查询日志（Slow query log）用来记录执行时间过长和没有使用索引的查询语句。

慢查询日志默认是关闭的。下面的命令可以用来查看是否开启慢查询日志以及日志文件的位置，

```sql
mysql> show variables like 'slow_query%';
+---------------------+--------------------------------------+
| Variable_name       | Value                                |
+---------------------+--------------------------------------+
| slow_query_log      | OFF                                  |
| slow_query_log_file | /var/lib/mysql/9eca2f63e027-slow.log |
+---------------------+--------------------------------------+
2 rows in set (0.00 sec)
```

具体来说，慢查询日志记录了执行时间超过 long\_query\_time 参数（单位是秒）并且扫描记录数不小于 min\_examined\_row\_limit 参数的查询语句。下面的命令可以用来查看这两个参数，

```sql
mysql> show variables like 'long_query_time%';
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| long_query_time | 10.000000 |
+-----------------+-----------+
1 row in set (0.01 sec)

mysql> show variables like 'min_examined_row_limit%';
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| min_examined_row_limit | 0     |
+------------------------+-------+
1 row in set (0.00 sec)
```

如果需要开启慢查询日志，可以在MySQL配置文件中添加如下的配置：

```ini
# 设置是否开启慢查询日志，0代表关闭，1代表开启
slow_query_log=1
# 设置慢查询日志的文件名
slow_query_log_file=slow_query.log
# 设置查询的时间限制，超过这个时间将认为是慢查询，需要进行日志记录，默认为10秒
long_query_time=10
```

和错误日志、查询日志一样，慢查询日志的内容也是纯文本，可以使用 cat 命令直接读取日志文件。如果慢查询日志比较多，查看文件比较麻烦，可以借助于 MySQL 自带的 mysqldumpslow 工具， 来对慢查询日志进行分类和汇总。



## redo log

> [https://www.jb51.net/article/273118.htm](https://www.jb51.net/article/273118.htm "https://www.jb51.net/article/273118.htm")

redo log 可以称为重做日志，是 InnoDB 存储引擎自带的日志，记录了事务操作的变化。redo log 主要用于 MySQL 的崩溃恢复，保证事务的持久性。简单来说，如果 MySQL 服务崩溃时仍然有脏页没有写入磁盘，MySQL 服务重启之后可以根据事务期间产生的 redo log 进行重做，恢复到崩溃之前的状态，从而达到事务的持久性。

> MySQL 中数据是以页为单位，查询一条数据时，会把一页的数据加载出来，加载出来的数据格式可以称为数据页，并放入到 Buffer Pool 中。
> 通常来说，查询都是先从 Buffer Pool 中找，没有找到再去硬盘加载，这样能够减少硬盘 IO 开销，提升性能。同理，在更新数据时，如果发现 Buffer Pool 里存在要更新的数据，就直接在 Buffer Pool 里更新。

redo log 是物理日志，记录的是数据修改之后的值，具体来说是“在哪个数据页做了什么修改”。在一个事务中，只要修改数据就会产生redo log 并保存到文件中。换句话说，不管事务是否提交，redo log都会记录下来。

redo log 和 binlog 的区别在于：

-   redo log 是在 InnoDB 存储引擎层产生的，binlog 是 MySQL Server 层产生的。binlog 和存储引擎没有关系，只要有数据更改，都会产生 binlog。
-   日志内容不同。redo log 是物理日志，记录的是数据修改之后的值；binlog 是逻辑日志，记录的是 SQL 语句或数据修改。
-   写入磁盘的时间点不同。binlog 只在事务提交完成后写入磁盘；redo log 在事务进行中不断地被写入。
-   日志文件生成方式不同。binlog 在一个文件写满或者 MySQL 重启之后，会创建一个新的 binlog 文件，用文件后缀标明日志文件的顺序。redo log 使用了一个日志文件组，并循环使用每一个文件，如果日志文件组中的最后一个文件写满，会重新写入日志文件组的第一个文件。
-   用途不同。binlog 主要用于主从复制，MySQL 使用 binlog 来同步数据，保证主从数据库的数据一致性。redo log 主要用于崩溃恢复，如果 MySQL 崩溃，重启之后可以使用 redo log 恢复到崩溃之前的状态。

### 写入磁盘

需要注意的是，redo log 并不会直接写入日志文件，而是先写入重做日志缓存（redo log buffer），事务提交时再把缓存中的数据写入到磁盘的日志文件中（也可以称为刷盘）。

写入磁盘的时机由 innodb\_flush\_log\_at\_trx\_commit 参数控制：

-   0，表示每次事务提交时不进行写入磁盘的操作。
-   1，表示每次事务提交时都进行写入磁盘的操作 ，是默认值。
-   2，表示每次事务提交时都只把 redo log buffer 内容写入文件系统缓存 page cache。

通过下面的命令查看：

```sql
mysql> show variables like 'innodb_flush_log_at_trx_commit';
+--------------------------------+-------+
| Variable_name                  | Value |
+--------------------------------+-------+
| innodb_flush_log_at_trx_commit | 1     |
+--------------------------------+-------+
1 row in set (0.00 sec)
```

另外，InnoDB 存储引擎有一个后台线程，每隔 1 秒会把 redo log buffer 中的内容写到文件系统缓存 page cache，然后调用 fsync 刷入到磁盘。换句话说，无论产生 redo log 的事务是否提交，redo log 都会定期写入到磁盘。如下图所示，

![image_pIx6e1LMLh](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311272151443.png)

除了后台线程每秒1次的轮询之外，当 redo log buffer 占用的空间即将达到 innodb\_log\_buffer\_size 参数一半时，后台线程也会主动把 redo log buffer 中的内容写入到磁盘。

### 日志文件组

磁盘上的 redo log 日志文件不只一个，而是以一个日志文件组的形式工作的，每个的 redo log 文件大小都是一样的。

![image_Ta1ty3dZyf](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311272151854.png)

如上图所示，redo log 的磁盘文件有固定的空间，一个文件写满后，就会使用下一个文件。

### 两阶段提交

binlog 保证了 MySQL 集群架构的数据一致性，而 redo log 让 InnoDB 存储引擎拥有了崩溃恢复能力。虽然二者都属于持久化的保证，但是侧重点不同。

redo log 与 binlog 的写入时机不一样。以基本的事务为单位，redo log 在事务执行过程中不断写入，而 binlog 只有在提交事务时才写入。正是由于这一点，二者可能产生不一致的问题。

以`UPDARE T SET c=1 where id=2;`为例，

1）假设执行过程中写完redo log日志后，binlog 日志写磁盘期间发生了异常。如下图所示，

![image_DmA8lpQun_](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311272151853.png)

由于 binlog 没写完发生了异常，binlog 里面没有对应的修改记录，但是 redo log 可能已经记下了这次修改。当 MySQL 服务器重启后，主数据库使用 redo log 可以恢复到正确的状态，c 列的值为1；而从数据库使用 binlog 恢复数据时，就会缺少这次修改，导致数据不一致。

为了解决 redo log 和 binlog 之间的逻辑不一致问题，InnoDB 存储引擎使用两阶段提交的方法。简单来说，InnoDB 将 redo log 的写入拆成了 prepare 和 commit 两个阶段，如下图所示。

![image_avYfDqrEiv](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311272152355.png)

具体地，

-   在 prepare 阶段，此时事务正在执行中，MySQL 不断写入 redo log，此时的 redo log 处于 prepare 阶段。
-   在 commit 阶段，此时事务正准备提交，MySQL 先写入 binlog，然后把 redo log 标记为 commit。

使用两阶段提交方法后，即使写入 binlog 发生异常也不会有影响。因为在MySQL重启之后，根据 redo log 恢复数据时，可以发现 redo log 还处于 prepare 阶段，而且没有对应 binlog，就会回滚该事务。

再看另一个场景，如果写入 redo log 在 commit 阶段异常，那 MySQL 会不会回滚事务呢？答案是不会。因为此时 binlog 已经完全写入，虽然 redo log 是处于 prepare 阶段，但是 MySQL 能通过事务 ID 找到完整的 binlog，所以MySQL认为是 redo log 和 binlog是一致的，就会提交事务恢复数据。

## undo log

> [https://dev.mysql.com/doc/refman/8.0/en/innodb-undo-logs.html](https://dev.mysql.com/doc/refman/8.0/en/innodb-undo-logs.html "https://dev.mysql.com/doc/refman/8.0/en/innodb-undo-logs.html")

undo log 可以称为回滚日志，也是 InnoDB 存储引擎自带的日志。undo log 是一个与读写事务相关的回滚日志记录的集合。一个undo log 记录包含了一些信息，用于撤消一个事务在一个聚簇索引的最新更改。如果另一个事务需要将原始数据视为一致读取操作的一部分，则会从undo log记录中检索未修改的数据。

> An undo log is a collection of undo log records associated with a single read-write transaction. An undo log record contains information about how to undo the latest change by a transaction to a clustered index record. If another transaction needs to see the original data as part of a consistent read operation, the unmodified data is retrieved from undo log records.

undo log 的一个用途是事务回滚。如果一个事务的执行过程中发生错误，MySQL 可以利用 undo log 中的信息回滚事务，将数据回滚到事务之前的状态。另外，undo log 会先于数据持久化到磁盘上。这保证了即使事务执行过程中遇到数据库突然宕机等极端情况，在重启之后，MySQL 还能够通过查询 undo log 来回滚还没有完成的事务。

undo log 的另一个用途是保存一条数据的历史版本。在一个快照读中，InnoDB 首先通过数据行的 DB\_TRX\_ID 和 Read View 来判断数据的可见性，如果不可见，则通过数据行的 DB\_ROLL\_PTR 在 undo log 中查找数据的历史版本。

总结，InnoDB 存储引擎使用 redo log 保证事务的持久性，使用 undo log 来保证事务的原子性。

## relay log

relay log 可以称为中继日志。MySQL 进行主从复制时，会在要复制的数据库下面产生相应的 relay log。

具体地，从数据库使用一个 I/O 线程将主数据库的 binlog 读取过来，解析后记录到从数据库的本地文件，这个文件就被称为 relay log。然后，从数据库使用一个 SQL 线程读取 relay log 并重新执行一次，从而使从数据库和主数据库的数据保持一致。

总结，中继日志相当于数据缓冲区，使从数据库可以一边从主数据库拉取 binlog，一边执行 relay log 中的内容。



# MySQL体系结构

> [mysql体系架构 mysql体系结构详解](https://blog.51cto.com/u_16099248/6356025)

MySQL 体系结构如下图所示：

![image_8JZr2azhKE](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311272152465.png)

MySQL Server 大概可以分为四层和一个服务管理工具：连接层，服务层，可插拔存储引擎，文件系统以及管理服务和工具。

| 组件名称                        | 组件作用                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| Management Services & Utilities | 管理服务和工具，提供了 MySQL 系统管理服务以及一些辅助工具。  |
| Connection Pool                 | 连接池，负责处理与用户访问有关的用户登录，线程处理，内存和进程缓存需求。 |
| SQL Interface                   | SQL 接口，用于从客户端接收命令，并把结果返回给客户端。       |
| Parser                          | 解析器，解析 SQL 语句，创建相应的内部解析树。                |
| Optimizer                       | 优化器，和解析器合作，优化 SQL 语句，比如确定表的查询的顺序，是否利用索引等。 |
| Caches & Buffers                | 缓存和缓冲，如果开启查询缓存，对于 SELECT 语句，服务器会查询内部缓存。 |
| Pluggable Storage Engines       | 可插拔存储引擎。可以根据需求使用不同的存储引擎，默认存储引擎是 InnoDB。 |
| File system                     | 文件系统。                                                   |

和其他数据库相比，MySQL 存储引擎是可插拔的，这使得它的架构可以在多种不同场景中应用并发挥良好作用。

## 连接层

第一层是连接层（Connection Pool），主要作用是用来处理来自用户（客户端）的连接，包括与用户访问有关的连接管理和限制，用户验证，内存和缓存等需求。

当 MySQL 服务启动后，可以接收并处理用户（客户端）的连接。客户端通常需要进行验证（IP，用户名，密码等），连接成功后还需要进行权限认证，比如当前连接能够操作某个库或者某个表等。

对于每一个客户端连接，服务器都会分配一个空的线程，每个线程独立拥有各自的处理空间。

MySQL 最大连接数是可配置的，默认是 151，可以通过 max_connections 变量查看

```mysql
mysql> SHOW VARIABLES LIKE 'max_connections%';
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| max_connections | 151   |
+-----------------+-------+
1 row in set (0.00 sec)
```

## 服务层

第二层是服务层（Server），主要用于完成 MySQL 的一些核心服务功能，包括处理 SQL 语句的解析、优化、查询等。

### 查询缓存

MySQL 服务层支持查询缓存，但是从 8.0 版本不再开启。因为查询缓存作用不大，一旦有 UPDATE 或者 DELETE 操作，查询缓存就会失效。

如果开启了查询缓存，对于 SELECT  语句会先查询缓存。如果该 SELECT 语句在查询缓存中存在并且未被修改过，则可以直接从缓存中获取结果而不需要重新执行这条语句。

通过下面的命令查看是否开启缓存和缓存大小：

```mysql
mysql> SHOW VARIABLES LIKE 'query_cache_type%';
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| query_cache_type | OFF   |
+------------------+-------+
1 row in set (0.00 sec)

mysql> SHOW VARIABLES LIKE 'query_cache_size%';
+------------------+---------+
| Variable_name    | Value   |
+------------------+---------+
| query_cache_size | 1048576 |
+------------------+---------+
1 row in set (0.00 sec)
```

### 优化器和解析器

TODO

## 可插拔存储引擎

存储引擎负责了对数据的存储和提取，MySQL 服务层通过 API 和存储引擎进行通信。不同的存储引擎具有不同的功能，可以根据需求来选取合适的存储引擎。

查看 MySQL 支持哪些存储引擎：

```mysql
mysql> SHOW engines;
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| MyISAM             | YES     | MyISAM storage engine                                          | NO           | NO   | NO         |
| MRG_MYISAM         | YES     | Collection of identical MyISAM tables                          | NO           | NO   | NO         |
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
| BLACKHOLE          | YES     | /dev/null storage engine (anything you write to it disappears) | NO           | NO   | NO         |
| PERFORMANCE_SCHEMA | YES     | Performance Schema                                             | NO           | NO   | NO         |
| MEMORY             | YES     | Hash based, stored in memory, useful for temporary tables      | NO           | NO   | NO         |
| ARCHIVE            | YES     | Archive storage engine                                         | NO           | NO   | NO         |
| CSV                | YES     | CSV storage engine                                             | NO           | NO   | NO         |
| FEDERATED          | NO      | Federated MySQL storage engine                                 | NULL         | NULL | NULL       |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
9 rows in set (0.00 sec)
```

查看默认的存储引擎：

```mysql
mysql> SHOW VARIABLES LIKE '%storage_engine%';
+----------------------------------+--------+
| Variable_name                    | Value  |
+----------------------------------+--------+
| default_storage_engine           | InnoDB |
| default_tmp_storage_engine       | InnoDB |
| disabled_storage_engines         |        |
| internal_tmp_disk_storage_engine | InnoDB |
+----------------------------------+--------+
4 rows in set (0.01 sec)
```

## 文件系统

数据存储层，主要是将数据存储在文件系统之上，并完成与存储引擎的交互。

查看数据存放的位置：

```mysql
mysql> Show variables like 'datadir';
+---------------+-----------------+
| Variable_name | Value           |
+---------------+-----------------+
| datadir       | /var/lib/mysql/ |
+---------------+-----------------+
1 row in set (0.01 sec)
```



## 常见问题

### 一条 SELECT 语句的执行过程

1）客户端发送查询请求到数据库服务器。如果没有连接，需要先建立连接。

2）数据库服务器接收到查询请求。通过查询「缓存查询」之前是否有查询过该 SQL（不建议开启查询缓存）。如果有缓存，就直接返回结果；没有则执行第3步。

3）解析器负责「解析 SQL 语句」。具体地，解析器会解析关键字，格式，表等信息，判断 SQL 语句是否正确。

4）优化器负责「优化 SQL 语句」，比如选择索引，JOIN 表的连接顺序。

5）执行器负责「调用存储引擎执行该 SQL」。 执行查询前会校验一下有无该表的权限，如果没有则返回错误提示，有权限就开始执行，然后返回「执行结果」。

### 一条 UPDATE 语句的执行过程

UPDATE 语句的执行过程基本和 SELECT 语句相同，也需要经过连接、分析器、优化器、执行器这些步骤。不同的是，UPDATE 执行过程中，会产生两种日志，一个是 redo log，一个是 binlog。

# MySQL 集群和复制

> [MySql 主从复制及配置实现](https://segmentfault.com/a/1190000008942618 "MySql 主从复制及配置实现")

MySQL 集群是指多个 MySQL 服务器组成一个集群对外提供服务。主数据库（Master）是 MySQL 集群中的主服务器，也可以称为主库。从数据库（Slave）是集群中的从服务器，也可以称为从库。在 MySQL 集群中，写操作只能在主数据库上面进行，而读操作可以在所有数据库上面进行。

MySQL 复制是指在一个 MySQL 集群中，主数据库把 binlog 发送到从数据库中，然后从数据库对这些日志重新执行，从而使得二者的数据保持一致。这样，从数据库可以用来支持非即时性的查询，减轻主数据库的负担。另外，MySQL 复制也可以用来实现数据备份。

MySQL 复制的优点主要包含以下三个方面：

-   故障切换：如果主库出现问题，可以快速切换到从库提供服务。
-   读写分离：在主库上执行更新操作和即时性的查询，在从库上执行非即时性的查询，实现读写分离，降低主库的访问压力。
-   数据备份：可以在从库中执行数据备份的任务，以避免备份期间影响主库的服务。

MySQL 复制基于 binlog，所以每一种 binlog 格式对应着一种 MySQL 复制类型：

-   基于语句的复制：binlog 格式为 STATEMENT。主数据库上执行的语句在从数据库上面再执行一遍。但是，时间上可能产生偏差，导致数据不一致。
-   基于行的复制：binlog 格式为 ROW。主数据库修改的内容直接复制到从数据库，而不关心修改的 SQL 语句。但是，如果一次性修改了多条数据，那么要复制所有被修改的内容，开销比较大。
-   混合类型的复制：binlog 格式为 MIXED。当基于语句的复制会引发数据不一致的问题，就使用基于行的复制。

## 复制模式

MySQL 复制支持一主一从模式，以及扩展的一主多从模式。一主多从模式是指一台主库同时向一台从库进行复制，从库同时作为其他从服务器的主库，实现链状复制。

MySQL 复制的一主一从模式如下图所示：

![](https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030154026.jpg?token=AE4F4YLF3BA64YLV3WHZSUS7TPCAG)

一主一从模式的复制过程大致分成三步：

-   主库（Master）有一个 binlog dump 线程，把数据更新记录在 binlog 文件中。具体来说，在每次提交事务时，主库把数据更新的事件记录到 binlog。binlog记录好之后，存储引擎再提交事务。
-   从库（Slave）开启一个 I/O线程，请求读取主库的 binlog，然后把读取到的 binlog 写到本地的 relay log 文件中。
-   另外，从库还要开启一个 SQL 线程，定时检查 relay log。如果发现有更改，立即把更改的内容在本机上面执行一遍。

MySQL 复制的一主多从模式如下图所示：

![](https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030154053.jpg?token=AE4F4YJGCSS2WAXOZXR5RBK7TPCB4)

一主多从模式的复制过程为链式复制，此时从库需要开启 binlog：

-   一个从库请求读取主库的 binlog，然后把读取到的二进制日志写到本地的 relay log 文件中。
-   该从库开启一个 SQL 线程，定时检查中继日志，如果发现有更改，立即把更改的内容在本机上面执行一遍。由于该数据库开启了 binlog，所以可以生成 binlog。

## 并行复制

TODO



## 常见问题

### 主从复制延迟过大，有什么原因？如何排查？

> [https://zhuanlan.zhihu.com/p/370581532](https://zhuanlan.zhihu.com/p/370581532 "https://zhuanlan.zhihu.com/p/370581532")

根据前面主从复制的原理可以看出，两者之间是存在一定时间的数据不一致，也就是所谓的主从延迟。

我们来看下导致主从延迟的时间点：

-   主库 A 执行完成一个事务，写入 binlog，该时刻记为T1。
-   从库 B 请求 binlog，接收到 binlog 的时刻记为T2。
-   从库 B 执行完这个事务，该时刻记为T3。

那么所谓主从延迟，就是同一个事务，从库执行完成的时间和主库执行完成的时间之间的差值，即 T3-T1。

我们也可以通过在从库执行`show slave status`，返回结果会显示`seconds_behind_master`，表示当前从库延迟了多少秒。

seconds\_behind\_master如何计算的？

-   每一个事务的 binlog 都有一个时间字段，用于记录主库上写入的时间。
-   从库取出当前正在执行的事务的时间字段，跟当前系统的时间进行相减，得到的就是`seconds_behind_master`，也就是前面所描述的 T3-T1。

#### 主从复制延迟过大的原因

1）网络延迟，导致日志从主库传给从库的时间过长。不过这个通常来说不太可能，因为主从数据库通常都在一个局域网内。

2）从库的机器性能比主库要差。比如将 20 台主库放在 4 台机器，把从库放在一台机器。这个时候进行更新操作，由于更新时会触发大量读操作，导致从库机器上的多个从库争夺资源，导致主从延迟。

3）从库的压力大。比如从库有着大量的查询，导致从库上耗费了大量的CPU资源，进而影响了同步速度，造成主从延迟。

4）大事务。一旦执行大事务，那么主库必须要等到事务完成之后才会写入 binlog。

5）主库执行 DDL 时间过长。

-   因为 DDL 操作是串行进行，如果 DDL 操作在主库执行时间很长，那么从库也会消耗同样的时间，比如在主库对一张 500W 的表添加一个字段耗费了10分钟，那么从节点上也会耗费10分钟。 &#x20;
-   如果从节点上有一个执行时间非常长的的查询正在执行，那么这个查询会堵塞来自主库的 DDL，表被锁，直到查询结束为止，进而导致了从节点的数据延迟。

最后，本质原因是系统 TPS 并发较高时，主库产生的 **DML**（也包含一部分DDL）数量超过 Slave 一个 SQL 线程所能承受的范围，效率就降低了。

#### 怎么减少主从延迟

主从同步问题永远都是一致性和性能的权衡，得看实际的应用场景，若想要减少主从延迟的时间，可以采取下面的办法：

1.  降低多线程大事务并发的概率，优化业务逻辑。
2.  优化SQL，避免慢 SQL，减少批量操作，建议以 update-sleep 这样的形式完成。
3.  提高从库机器的配置，减少主库写 binlog 和从库读 binlog 的效率差。
4.  尽量采用短的链路，也就是主库和从库服务器的距离尽量要短，提升端口带宽，减少binlog传输的网络延时。
5.  实时性要求的业务读强制走主库，从库只做灾备，备份。

另外，MySQL自5.7版本后就已经支持并行复制了，可以在从数据库开启并行复制。



# MySQL优化

## SQL优化

> [Mysql高性能优化规范建议](https://www.cnblogs.com/huchong/p/10219318.html "Mysql高性能优化规范建议")
> [腾讯面试：一条SQL语句执行得很慢的原因有哪些？---不看后悔系列](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==\&mid=2247485185\&idx=1\&sn=66ef08b4ab6af5757792223a83fc0d45\&chksm=cea248caf9d5c1dc72ec8a281ec16aa3ec3e8066dbb252e27362438a26c33fbe842b0e0adf47\&token=79317275\&lang=zh_CN#rd "腾讯面试：一条SQL语句执行得很慢的原因有哪些？---不看后悔系列")

常见的 SQL 优化可以分为几个方面：

1）表结构优化，包括

-   字段类型的选择
-   字段类型大小的限制
-   合理的增加冗余字段
-   新建字段一定要有默认值

2）索引结构优化，包括

-   索引字段的选择，尽量使用联合索引和覆盖索引。
-   如果业务上能够保证唯一，尽量使用唯一索引，否则使用普通索引。

3）查询语句优化，包括

-   限定数据的范围。尽量不要使用不带任何限制数据范围条件的查询语句。
-   尽量避免在WHERE字句中使用OR来连接条件，否则将导致不能使用索引而进行全表扫描。
-   正确使用LIKE查询。
-   尽量避免在where字句中对字段进行表达式操作。
-   如果确认查询结果数量，应尽可能加上LIMIT。
-   正确使用复合索引。
-   如果使用了JOIN，请尽量使用小表JOIN大表。

表结构和索引结构通常在开发阶段就确定了，所以当项目上线之后，如果发生了SQL性能问题，一般需要对性能有问题的SQL进行分析：

-   查看SQL执行频率。
-   定位低效率SQL。
-   使用EXPLAIN分析低效率SQL。
-   使用SHOW PROFILE了解MySQL操作的时间消耗。
-   使用TRACE追踪SQL的执行。

## 查看SQL执行频率

MySQL 客户端连接成功后，可以通过`show status`和`like`查询得到各种操作的执行次数：

```sql
# 查看服务器信息
show status;
# 查看session级别或global级别的统计结果，默认为session
show [session|global] status like 'Com_%';
```

`Com_xxx`表示每种 xxx 语句执行的次数，我们通常比较关心的是以下几个统计参数：

| 参数                     | 含义                                         |
| ---------------------- | ------------------------------------------ |
| Com\_select            | 执行 SELECT 操作的次数，一次查询只累加 1 次。               |
| Com\_insert            | 执行 INSERT 操作的次数，对于批量插入的 INSERT 操作，只累加 1 次。 |
| Com\_update            | 执行 UPDATE 操作的次数。                           |
| Com\_delete            | 执行 DELETE 操作的次数。                           |
| Innodb\_rows\_read     | SELECT 查询返回的行数。                            |
| Innodb\_rows\_inserted | 执行 INSERT 操作插入的行数。                         |
| Innodb\_rows\_updated  | 执行 UPDATE 操作更新的行数。                         |
| Innodb\_rows\_deleted  | 执行 DELETE 操作删除的行数。                         |
| Connections            | 试图连接 MySQL 服务器的次数。                         |
| Uptime                 | 服务器工作时间。                                   |
| Slow\_queries          | 慢查询的次数。                                    |

注意：

-   Com\_xxx：这些参数对于所有存储引擎的表操作都会进行累计。
-   Innodb\_xxx：这些参数只针对 InnoDB 存储引擎，累加的算法也略有不同。

## 定位低效率SQL

定位执行效率较低的 SQL 语句有两种方式：

-   慢查询日志：查询执行结束以后，才能查看慢查询日志。
-   show processlist：查看当前 MySQL 在进行的线程，包括线程的状态等信息，可以实时地查看 SQL 的执行情况。

### 开启慢查询日志

要想通过慢查询日志定位那些执行效率较低的 SQL 语句，首先要设置开启慢查询日志：

```sql
# 查看是否开启慢查询日志
SHOW VARIABLES LIKE '%slow_query_log%'
# 设置开启慢查询
SET GLOBAL slow_query_log=1;
# 查看慢查询的时间阈值
SHOW GLOBAL VARIABLES LIKE '%long_query_time%';
# 设置慢查询的时间阈值为2秒
SET GLOBAL long_query_time=2;
# 查询多少SQL超过了慢查询时间的阙值
SHOW GLOBAL STATUS LIKE '%Slow_queries%';
```

可以使用 MySQL 提供的日志分析工具 mysqldumpslow，获取指定条件的慢SQL：

```bash
# 获取慢日志中最多的10个SQL
mysqldumpslow -s r -t 10 <慢查询日志路径>
# 获取按照时间排序的前10条里面含有左连接的查询语句
mysqldumpslow -s t -t 10 -g "left join" <慢查询日志路径>
```

### 使用SHOW PROCESSLIST

慢查询日志在查询结束以后才进行纪录，可以使用`show processlist`命令查看当前MySQL在进行的线程，包括线程的状态、是否锁表等，可以实时地查看 SQL 的执行情况，同时对一些锁表操作进行优化。

```sql
show processlist;
```

`show processlist`命令的结果说明：

-   Id：用户登录mysql时，系统分配的`connection_id`，可以使用函数`connection_id()`查看。
-   User：显示当前用户。如果不是root，这个命令就只显示用户权限范围的sql语句。
-   Host列：显示这个语句是从哪个 IP 的哪个端口上发的，可以用来跟踪出现问题语句的用户。
-   db列：显示这个进程目前连接的是哪个数据库。
-   Command：显示当前连接的执行的命令，一般取值为休眠（sleep），查询（query），连接（connect）等。
-   Time：显示这个状态持续的时间，单位是秒。
-   State：显示使用当前连接的SQL语句的状态，State描述的是语句执行中的某一个状态。一个SQL语句，以查询为例，可能需要经过 copying to tmp table、sorting result、sending data 等状态才可以完成。
-   Info：显示这个SQL语句，是判断问题语句的一个重要依据。

## 使用EXPLAIN分析低效率SQL

> [如何分析Mysql慢SQL](https://www.cnblogs.com/linyue09/p/9869163.html "如何分析Mysql慢SQL")
> [https://blog.csdn.net/sinat\_41928169/article/details/124737340](https://blog.csdn.net/sinat_41928169/article/details/124737340 "https://blog.csdn.net/sinat_41928169/article/details/124737340")

通过以上步骤查询到效率低的查询语句后，可以通过EXPLAIN命令查看SQL语句的执行计划：

```sql
# id有索引
explain select * from table_name where id = 1;
# title没索引
explain select * from tb_item where title = '阿尔卡特 (OT-979) 冰川白 联通3G手机3';
```

执行计划包括多个列，有id、select\_type等。

#### id

SELECT查询的序列号，包含一组数字，表示查询中执行 SELECT 语句或操作表的顺序，有3种情况：

-   id相同，表示从上到下顺序执行；
-   id不同，如果包含子查询，id序号会递增，id值越大，优先级越高，越先执行；
-   id有相同，也有不同，同时存在，id相同的认为是一组，从上往下顺序执行。在所有的组中，id值越大，优先级越高，越先执行。

#### select\_type

查询类型，比较复杂，包含多种情况：

-   SIMPLE：简单查询，查询中不包含子查询或者UNION语句。
-   PRIMARY：查询中若包含任何复杂的子查询，最外层查询标记为该标识。
-   SUBQUERY：在SELECT或WHERE中包含的子查询。
-   DERIVED：在FROM中包含的子查询，被标记为DERIVED，MySQL会递归执行这些子查询，把结果放到临时表中。
-   UNION：若第二个SELECT出现UNION之后，则被标记为UNION；若UNION包含在FROM子句的子查询中，外层子查询将被标记为DERIVED。
-   UNION RESULT：从UNION表获取结果的查询。

#### table

显示一行数据是关于哪张表的。

#### type

访问类型，是较为重要的一个指标，包括多种情况：

-   NULL：不访问任何表，索引，直接返回结果。
-   system：表只有一行记录（等于系统表），这是const类型的特例，一般不会出现。
-   const：表示通过索引一次就找到了，const用于比较主键索引或者唯一索引。因为只匹配一行数据，所以很快。如果 WHERE 查询条件使用了主键索引或唯一索引，MySQL 就能将该查询转换为一个常量。
-   eq\_ref：唯一性索引扫描，对于每个索引键，表中只有一条记录与之匹配。常见于主键或唯一索引扫描。
-   ref：非唯一性索引扫描，返回匹配某个单独值的所有行。本质上也是一种索引访问，它返回所有匹配某个单独值的行。
-   range：只检索给定范围的行，使用一个索引来选择行。key 列显示使用了哪个索引，一般就是在 where 语句中出现`between、<、>、in`等查询。这种范围扫描索引比全表扫描要好，因为只需要开始于索引的某一点，结束于另一点，不用扫描全部索引。
-   index：全部索引扫描，index 与 all 的区别为 index 类型只遍历索引树，这通常比 all 快，因为索引文件通常比数据文件小。
-   all：全表扫描，遍历全表获得匹配的行。

结果值从最好到最坏以此是：NULL > system > const > eq_ref > ref > range > index > all。一般来说， 我们需要保证查询至少达到 range 级别， 最好达到ref。

#### possible\_keys

表示可能应用在这张表中的索引，一个或多个。如果查询涉及到的字段上存在索引，则该索引将被列出，但不一定被查询实际使用。

#### key

表示实际使用的索引。如果为 NULL，则没有使用索引。查询中若出现了覆盖索引，则该索引仅出现在key列表中。

#### key\_len

表示索引中使用的字节数，该值为索引字段的最大可能长度，并非实际使用长度。在不损失精度的情况下，长度越短越好。

#### ref

显示索引的哪一列被使用了，哪些列或常量被用于查找索引列上的值。

#### rows

根据表统计信息及索引选用情况，大致估算出找到所需记录多需要读取的行数。rows越小越好。

#### Extra

包含不适合在其他列中显示但十分重要的额外信息：

-   Using filesort：MySQL会对数据使用一个外部的索引排序，而不是按照表内的索引顺序进行读取，称为文件排序，效率低；
-   Using temporary：使用了临时表保存中间结果，MySQL在对查询结果排序时使用临时表。常见于排序order by和分组查询group by；
-   Using index：表示相应的SELECT操作中使用了覆盖索引，避免访问表的数据行，效率不错。

### 使用 SHOW PROFILE 分析SQL

查看状态：SHOW VARIABLES LIKE 'profiling'

-   开启 profile：`set profiling=on;`
-   查看结果：`show profiles;`

诊断SQL：show profile cpu,block io for query <上一步SQL数字号码>

-   ALL：显示所有开销信息。
-   BLOCK IO：显示IO相关开销。
-   CONTEXT SWITCHES：显示上下文切换相关开销。
-   CPU：显示CPU相关开销。
-   IPC：显示发送接收相关开销
-   MEMORY：显示内存相关开销。
-   PAGE FAULTS：显示页面错误相关开销。
-   SOURCE：显示和Source\_function，Source\_file，Source\_line相关开销。
-   SWAPS：显示交换次数相关开销。

### 使用TRACE追踪SQL

TODO



## 分库分表

### 垂直分表

垂直拆分是指根据数据表中的列的相关性，把一张列比较多的表拆分为多张表。例如，用户表中既有用户的登录信息又有用户的基本信息，可以将用户表拆分成两个单独的表，甚至放到单独的库做分库。

优点：可以使得列数据变小，在查询时减少读取的Block数，减少I/O次数。此外，垂直分区可以简化表的结构，易于维护。

缺点：主键会出现冗余，需要管理冗余列，并会引起Join操作，可以通过在应用层进行Join来解决。此外，垂直分区会让事务变得更加复杂；

### 水平分表

水平分表是指保持数据表的列结构不变，在水平上把一张表的数据拆成多张表来存放，达到了分布式的目的。举个例子：我们可以将用户信息表根据用户ID拆分成多个用户信息表，这样就可以避免单一表数据量过大降低操作的性能。

水平拆分可以支持非常大的数据量。需要注意的是，分表仅仅是解决了单一表数据过大的问题。如果拆分后的表还是在同一台机器上，其实对于提升MySQL的并发能力没有什么意义，所以水平拆分最好分库 。

水平拆分能够支持非常大的数据量存储，应用端改造也少，但分片事务难以解决 ，跨节点Join性能较差，逻辑复杂。

