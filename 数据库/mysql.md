[TOC]




















# 基础
> [MySQL/数据库 知识点总结](https://gitee.com/SnailClimb/JavaGuide/blob/master/docs/database/MySQL.md)

> [一千行 MySQL 学习笔记](https://shockerli.net/post/1000-line-mysql-note/)

> [drop，truncate，delete 三者的区别](https://blog.csdn.net/GRAY_KEY/article/details/86742248)

SQL是Structured Query Language（结构化查询语言）的缩写，是使用关系型数据库的应用语言。美国国家标准局（ANSI）于1986年制定了第一个SQL标准，叫做SQL-86。大多数关系型数据库系统都支持SQL语言。

MySQL是一种关系型数据库，并对标准SQL进行了一些扩展。MySQL数据库在企业级开发中非常常用。

MySQL是开放源代码的，因此任何人都可以在许可下下载并根据需要对其进行修改或扩展。

MySQL的默认端口号是3306。

## 常用SQL

SQL语句主要可以划分为以下3个类别：

- DDL(Data Definition Language)语句：数据定义语言，主要用来定义数据库、表、列、索引等数据库对象。常用的语句关键字主要包括create、drop、alter等。
- DML(Data Manipulation Language)语句：数据操纵语句，用于添加、删除、更新和查询数据库记录。常用的语句关键字主要包括insert、delete、update和select等。
- DCL(Data Control Language)语句：数据控制语句，用于定义数据库、表、字段、用户的访问权限和安全级别。主要的语句关键字包括grant、revoke等。

创建数据库：

```mysql
CREATE DATABASE database_name;
```

创建数据表：
```mysql
CREATE TABLE table_name (column_name column_type);

CREATE TABLE IF NOT EXISTS `tbl`(
   `id` INT UNSIGNED AUTO_INCREMENT,
   `title` VARCHAR(100) NOT NULL,
   `author` VARCHAR(40) NOT NULL,
   `submission_date` DATE,
   PRIMARY KEY (`runoob_id`)
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

删除数据表以及表中的所有数据：

```mysql
DROP TABLE table_name;
```

查询：

```mysql
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
```mysql
INSERT INTO table_name VALUES (value1, value2, ..., valueN);

INSERT INTO table_name (field1, field2,...fieldN) VALUES (value1, value2, ..., valueN);
```

更新：
```mysql
UPDATE table_name SET field1=value1, field2=value2
[WHERE Clause]
```

删除：
```mysql
DELETE FROM table_name [WHERE Clause]
```

清空数据表，即删除数据表中的所有数据，但不删除表结构：

```mysql
TRUNCATE TABLE table_name;
```

## 数据类型

> 深入浅出MySQL 数据库开发、优化与管理维护

### 数值类型

MySQL支持所有标准SQL中的数值类型，包括严格数值类型（整数）和近似数值数据类型（浮点数），并以此为基础做了扩展。

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20210319095021.png" alt="mysql数值类型" style="zoom:80%;" />

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20210319095034.png" alt="mysql数值类型2" style="zoom:80%;" />

### 日期时间类型

MySQL中有多种数据类型可以用于日期和时间的表示。下表列出了MySQL5.0中所支持的日期和时间类型

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20210319105017.png" alt="mysql日期和时间类型" style="zoom:80%;" />

这些数据类型的主要区别如下：

- 如果要用来表示年月日，通常用DATE来表示。
- 如果要用来表示年月日时分秒，通常用DATETIME来表示。
- 如果要用来表示时分秒，通常用TIME来表示。
- 如果需要经常插入或更新日期为当前系统时间，通常使用TIMESTAMP来表示。
- 如果只是表示年份，可以用YEAR来表示。

从上表可以看出，每种日期时间类型都有一个有效值范围。如果超出这个范围，系统会进行错误提示，并将以零值来进行存储。不同日期类型零值的表示如下表所示。

![mysql日期和时间类型的零值表示](https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20210319140635.png)

TIMESTAMP和DATETIME的表示方法非常类似，主要区别如下：

- TIMESTAMP存储大小为4字节，支持的时间范围较小，而DATETIME占8字节，支持的范围更大。
- 表中的第一个TIMESTAMP列自动设置为系统时间。如果在写数据时指定为NULL或没有明确赋值，则MySQL会自动设置该列为系统当前的日期和时间。
- TIMESTAMP的插入和查询都受当地时区的影响，更能反映出实际的日期，而DATETIME则只能反映插入时当地的时区。

### 字符串类型

MySQL提供了多种对字符数据的存储类型，比如CHAR、VARCHAR、BINARY、TEXT等。

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20210319143039.png" alt="mysql字符串类型" style="zoom:80%;" />

## 运算符

MySQL支持多种类型的运算符。

- 算术运算符：+、-、*、/（DIV）、%（MOD）。

- 比较运算符：=、<>或!=、<=>（NULL安全的等于）、<、<=、>、>=、BETWEEN、IN、IS NULL、IS NOT NULL、LIKE、REGEXP。
- 逻辑运算发：NOT或!（逻辑非）、AND或&&（逻辑与）、OR或||（逻辑或）、XOR（逻辑异或）。
- 位运算符：&、|、^、~、>>、<<。

## 常用函数

>  深入浅出MySQL 数据库开发、优化与管理维护

函数可以用在SELECT语句及其子句（例如WHERE、ORDER BY等）中，也可以用在UPDATE、DELETE语句机器子句中。

### 字符串函数

字符串函数包括CONCAT、INSERT、LOWER、UPPER、LEFT、RIGHT、LPAD、RPAD等。

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20210318202023.png" alt="mysql字符串函数" style="zoom:80%;" />

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20210318202114.png" alt="mysql字符串函数2" style="zoom:80%;" />

### 数值函数

数值函数包括ABS、CEIL、FLOOR、MOD、RAND、ROUND、TRUNCATE等。

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20210319090406.png" alt="mysql数值函数" style="zoom:80%;" />

### 日期和时间函数

日期和时间函数包括CURDATA、CURTIME、NOW、UNIX_TIMESTAMP、FROM_UNIXTIME、YEAR等。

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20210319091152.png" alt="mysql日期和时间函数" style="zoom:80%;" />

### 流程函数

流程函数也是很常用的一类函数，可以用来实现条件选择等功能。流程函数包括IF、IFNULL等。

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20210319091559.png" alt="mysql流程函数" style="zoom:80%;" />

### 其他函数

MySQL还有很多其他函数，比如DATABASE、VERSION、USER、INET_ATON、INET_NTOA、PASSWORD等。

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20210319091919.png" alt="mysql其他常用函数" style="zoom:80%;" />


## 视图
MySQL 从 5.0.1 版本开始提供视图功能。

视图（View）是一种虚拟存在的表。视图并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。通俗的讲，视图就是一条 SELECT 语句执行后返回的结果集。所以我们在创建视图的时候，主要的工作就落在创建这条 SQL 查询语句上。

视图相对于普通的表的优势主要包括以下几项。
- 简单：使用视图的用户完全不需要关心后面对应的表的结构、关联条件和筛选条件，对用户来说已经是过滤好的复合条件的结果集。
- 安全：使用视图的用户只能访问他们被允许查询的结果集，对表的权限管理并不能限制到某个行某个列，但是通过视图就可以简单的实现。
- 数据独立：一旦视图的结构确定了，可以屏蔽表结构变化对用户的影响，源表增加列对视图没有影响；源表修改列名，则可以通过修改视图来解决，不会造成对访问者的影响。

### 创建或修改视图

创建视图需要有 CREATE VIEW 的权限，并且对于查询涉及的列有 SELECT 权限。如果使用 CREATE OR REPLACE 或者 ALTER 修改视图，那么还需要该视图的 DROP 权限。

创建视图：

```mysql
CREATE [OR REPLACE] [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
VIEW view_name [(column_list)]
AS select_statement
[WITH [CASCADED | LOCAL] CHECK OPTION]
```

修改视图：
```mysql
ALTER [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
VIEW view_name [(column_list)]
AS select_statement
[WITH [CASCADED | LOCAL] CHECK OPTION]
```

### 删除视图

用户可以一次删除一个或多个视图，前提是必须有该视图的 DROP 权限。

删除视图：

```mysql
DROP VIEW [IF EXISTS] view_name [, view_name] ...[RESTRICT | CASCADE]
```

### 查看视图

从 MySQL 5.1 版本开始，使用 SHOW TABLES 命令的时候不仅显示表的名字，同时也会显示视图的名字，而不存在单独显示视图的 SHOW VIEWS 命令。

查看表（包括视图）：
```mysql
SHOW tables;
```

查看视图的定义：

```mysql
SHOW CREATE VIEW [view_name];
```

## 存储过程和函数

MySQL 从 5.0 版本开始支持存储过程和函数。

存储过程和函数是事先经过编译并存储在数据库中的一段 SQL 语句的集合，调用存储过程和函数可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。

存储过程和函数的区别在于函数必须有返回值，而存储过程没有。存储过程的参数可以使用 IN、OUT、INOUT 类型，而函数的参数只能是 IN 类型的。

换句话说，函数是一个有返回值的过程 ，过程是一个没有返回值的函数 。

创建存储过程或函数：
```mysql
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
```mysql
delimiter $
CREATE PROCEDURE pro_test1()
begin
    select 'Hello Mysql';
end $
delimiter ;
```

> DELIMITER。该关键字用来声明SQL语句的分隔符，告诉 MySQL 解释器，该段命令已经结束。默认情况下，delimiter 是分号。在命令行客户端中，如果有一行命令以分号结束，那么回车后，MySQL 将会执行该命令。由于中间的 SQL 语句需要使用分号，所以整个命令的分隔符必须修改为其他符号。

调用存储过程：
```mysql
CALL procedure_name();
```

删除存储过程或函数：

```mysql
DROP [PROCEDURE | FUNCTION] [IF EXISTS] sp_name;
```

查看存储过程或函数：

```mysql
SHOW [PROCEDURE | FUNCTION] status [LIKE 'pattern'];
```

查询存储过程或函数的定义：

```mysql
SHOW CREATE [PROCEDURE | FUNCTION] sp_name;
```

## 触发器

MySQL 从 5.0.2 版本开始支持触发器。

触发器是与表有关的数据库对象，在满足条件时（INSERT、UPDATE、DELETE之前或之后）触发，并执行触发器中定义的语句集合。触发器的这种特性可以协助应用在数据库端确保数据的完整性、记录日志等 。

使用别名 OLD 和 NEW 来引用触发器中发生变化的记录内容，这与其他的数据库是相似的。现在触发器还只支持行级触发，不支持语句级触发。

|触发器类型|NEW和OLD的使用|
|---|---|
|INSERT 型触发器 | NEW 表示将要或者已经新增的数据|
|UPDATE 型触发器 | OLD 表示修改之前的数据，NEW 表示将要或已经修改后的数据|
|DELETE 型触发器 | OLD 表示将要或者已经删除的数据|

创建触发器：
```mysql
CREATE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE
ON table_name
[FOR EACH ROW] -- 行级触发器
begin
    trigger_stmt;
end;
```

查看触发器：
```mysql
SHOW TRIGGERS;
```

删除触发器：
```mysql
DROP TRIGGER [schema_name.] trigger_name;
```




# 索引
> [MySQL索引——分类、何时使用、何时不使用、何时失效](https://blog.csdn.net/weixin_39420024/article/details/80040549)

> [数据库两大神器【索引和锁】](https://juejin.im/post/6844903645125820424)

索引（index）是帮助MySQL高效获取记录的数据结构。在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法，这种数据结构就是索引。通常的索引结果如下图所示：

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030153654.png?token=AE4F4YNWSDL53EYEXXIQSHC7TPBS6" alt="MySQL索引示例"  />

一般来说索引本身也很大，不可能全部存储在内存中，因此索引往往以索引文件的形式存储在磁盘上。索引是数据库中用来提高性能的最常用的工具。

索引的优点有：
- 减少检索数据的数量，提高检索数据的效率，降低数据库的IO
- 通过索引对数据进行排序，降低数据排序的成本，降低了CPU的消耗
- 把随机IO变为顺序IO，加速表和表之间的连接

索引的缺点有：
- 占用一定空间，如果索引过多，容易造成空间浪费
- 降低插入、更新和删除操作的速度，不仅要保存数据，还要更新索引

适合建索引的条件：
- 表比较大，查询操作占多数的表适合建索引
- 经常查询的字段适合建索引
- 经常作为where查询条件的字段适合建索引
- 经常需要排序的字段适合建索引
- 与其他表关联的字段，比如外键，适合建索引

不适合建索引的条件：
- 表记录少的
- 增删改操作占多数的表或者字段不应该建立索引
- where条件里用不到的字段不应该创建索引
- 过滤性不好的不适合建索引

## 索引结构
索引是在MySQL的存储引擎层中实现的，而不是在服务器层实现的。所以每种存储引擎的索引都不一定完全相同，也不是所有的存储引擎都支持所有的索引类型的。MySQL目前提供了以下4种索引：
- BTREE索引：最常见的索引类型，一般使用 B+ 树来实现。
- HASH索引：只有Memory引擎支持，使用场景简单。
- R-tree索引（空间索引）：空间索引是MyISAM引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少，不做特别介绍。
- Full-text（全文索引）：全文索引也是MyISAM的一个特殊索引类型，主要用于全文索引，InnoDB从Mysql5.6版本开始支持全文索引。

我们平常所说的索引，如果没有特别指明，都是指B+树（一种多路搜索树）结构组织的索引。基于B+树结构的索引包括：
- 普通索引：主要目的是为了加快查询数据，一张表允许建多个普通索引，允许数据重复和NULL。
- 唯一索引：类似普通索引，索引列的值必须唯一，可以为NULL。唯一索引可以保证该属性列的数据的唯一性，也可以提高查询效率。
- 主键索引：特殊的唯一索引，只能有一个，不允许为NULL，不允许重复，一般是在建表时指定。对于InnoDB存储引擎，如果没有显式指定主键，InnoDB会自动先检查表中是否有唯一索引的字段，如果有，则选择该字段为默认的主键，否则InnoDB将会自动创建一个自增主键。
- 联合索引（复合索引）：在多个字段上创建索引，可以同时创建多个索引，遵循最左匹配原则。

哈希索引使用了哈希算法，通过键值计算出一个哈希值，检索时不需要类似B+树那样从根节点到叶子节点逐级查找，只需一次哈希算法即可立刻定位到相应的位置，速度非常快。但是哈希索引也有一些局限：
- 不支持范围查询。
- 没办法利用索引完成排序。
- 在有大量重复键值情况下，哈希索引的效率也是极低的（哈希碰撞问题）。


### B树
B树又叫多路平衡搜索树，一颗m叉的B树特性如下：
- 树中每个节点最多包含m个孩子。
- 除根节点与叶子节点外，每个节点至少有[ceil(m/2)]个孩子。
- 若根节点不是叶子节点，则至少有两个孩子。
- 所有的叶子节点都在同一层。
- 每个非叶子节点由n个key与n+1个指针组成，其中[ceil(m/2)-1] <= n <= m-1

以5叉B树为例，key的数量：公式推导[ceil(m/2)-1] <= n <= m-1，所以 2 <= n <=4。当n>4时，中间节点分裂到父节点，两边节点分裂。

BTREE树和二叉树相比，查询数据的效率更高，因为对于相同的数据量来说，BTREE的层级结构比二叉树小，因此搜索速度快。


### B+树
B+树是B树的变种，两者的主要区别在于：
- B树的非叶子结点存放了N个关键字和N+1个指针，B+树存放了N个关键字和N个指针；
- B树的非叶子结点既存放了关键字也存放了数据，B+树的非叶子结点只存放关键字，所有的数据都存在叶子结点；
- B树的检索过程可能没有到达叶子结点就结束了，B+树的检索效率比较稳定，任何查找都是从根结点到叶结点；
- B树非叶子节点是独立的，B+树为所有叶子结点增加一个链指针；

由于B+树只有叶子节点保存key信息，查询任何key都要从根节点走到叶子。所以B+树的查询效率更加稳定。


### MySQL中的B+树
MySQL索引的底层数据结构对经典的B+树进行了优化。在原B+树的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的B+树，提高区间访问的性能。

MySQL中的B+树索引结构如下图所示：

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030153742.png?token=AE4F4YPQG2MGIL2E6FGHSEK7TPBWE" alt="MySQL索引底层结构"  />

MySQL的基本存储结构是页，页的大小一般是16KB（一个磁盘块是4KB），页中存储了数据记录、页目录等信息，其中页目录就是索引，可以指向其他的页。

InnoDB存储引擎的页结构如下图所示：

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030153833.png?token=AE4F4YMRUY7VWJ5DV2ZTK4K7TPBZG" alt="MySQL InnoDB页结构" style="zoom: 80%;" />

通过索引来查找数据时，先读取根节点的页，使用二分查找搜索其中合适的索引，找到下一层的页，进而找到叶子节点，在叶子节点的数据中查找目标数据。

不通过索引查找数据时，只能通过叶子节点的双向链表遍历所有的页，查找每一个页中的每一条记录。


## 索引语法
索引可以在创建表的时候创建， 也可以在建好的表上增加新的索引。

创建索引：
```mysql
CREATE [UNIQUE|FULLTEXT|SPATIAL] INDEX index_name
[USING index_type]
ON tbl_name(index_col_name,...)

index_col_name : column_name[(length)][ASC | DESC]
```

查看索引：
```mysql
show index from table_name[\G];
```

删除索引：
```mysql
DROP INDEX index_1 name ON tbl_name;
```

用ALTER命令来添加索引：
```mysql
# 该语句添加一个主键，这意味着索引值必须是唯一的，且不能为NULL
alter table tb_name add primary key(column_list);
# 这条语句创建索引的值必须是唯一的（除了NULL外，NULL可能会出现多次）
alter table tb_name add unique index_name(column_list);
# 添加普通索引， 索引值可以出现多次。
alter table tb_name add index index_name(column_list);
# 该语句指定了索引为FULLTEXT，用于全文索引
alter table tb_name add fulltext index_name(column_list);
```

## 聚簇索引/非聚簇索引
MyISAM存储引擎的数据文件和索引文件是分开储存的。索引底层结构的B+树的叶子节点的data域存放的是**数据记录的地址**，非叶节点存放的是索引。在检索数据的时候，首先按照B+树搜索索引，如果指定的Key存在，则取出其data域的值，然后以data域的值为地址读取相应的记录。这被称为非聚簇索引或非聚集索引，即索引结构和数据分开存放的索引。

InnoDB存储引擎的主键索引是聚簇索引（聚集索引），因为数据文件和主键索引是一起存储的，而且数据文件本身就是按主键索引底层的B+树进行组织的，树的叶节点data域保存了**完整的数据记录**，非叶节点存放的是索引（主键key）。

InnoDB的非主键索引（辅助索引）是非聚簇索引。非主键索引底层也是B+树，叶子节点存放的是**主键值和索引列**（和MyISAM不同），非叶子结点存放的是索引。非聚集索引的优点是不存储完整的数据记录，更新时或排序时代价较小。

对于InnoDB存储引擎：
- 在根据主索引查询时，直接找到key所在的结点即可取出数据；
- 在根据辅助索引查找时，需要先取出主键的值，再走一遍主索引，这称为二次回表。
- 建议使用比较短的主键，比如自增主键，因为过长的字段会导致辅助索引过大；
- 建议使用有序的主键，比如自增主键，因为没有顺序的主键在插入时需要进行排序，会造成主索引频繁分裂。


## 联合索引
如果用多个非主键列建立索引，该索引称为联合索引，并且可以一次性建立多个索引。比如在（a,b,c）三个字段上建立索引，可以同时建立（a），（a,b），（a,b,c）三个索引。

联合索引的好处是建立一棵B+树上得到多个索引，减少了索引的空间。如果对多个字段建立单列索引，会建立多棵B+树，占用磁盘空间。

联合索引遵循最左匹配原则。最左匹配原则是指，按联合索引建立时列的顺序进行连续匹配，遇到范围查询时（>,<,between,like）停止匹配。举例，对于联合索引（a,b,c），当查询条件是`a=1 and b<10 and c=3`，根据最左匹配原则可以使用（a,b）索引，无法使用（a,b,c）索引。

如果查询条件的顺序和联合索引的顺序不同，MySQL会自动优化为相同的，再进行最左匹配。举例，对于联合索引（a,b,c），当查询条件是`a=1 and c=3 and b<10`，MySQL会自动优化为`a=1 and b<10 and c=3`。

联合索引中有一种特殊的索引，**覆盖索引**，就是包含了所有查询字段的索引。覆盖索引的好处是可以避免二次回表。对于Innodb存储引擎来说，二级索引在叶子节点中所保存的是主键值和索引列，如果是用非主键索引查询数据的话，在查找到相应的键值后，还要通过主键进行二次查询才能获取我们真实所需要的数据。在覆盖索引中，二级索引的键值中可以获取所有的数据，避免了对主键的二次查询 ，减少了IO操作，提升了查询效率。


## 其他使用索引的注意事项
- 尽量选择区分度高的列作为索引，区分度的公式是`COUNT(DISTINCT col) / COUNT(*)`。表示字段不重复的比率，比率越大我们扫描的记录数就越少。
- 避免在where子句中对字段施加函数，这会造成无法命中索引。
- 删除长期未使用的索引，不用的索引的存在会造成不必要的性能损耗。
- 避免冗余索引。冗余索引指的是索引的功能相同，MySQL只能使用一个索引，会从多个索引中选择一个限制最为严格的索引。
- 考虑在字符串类型的字段上使用前缀索引代替普通索引，前缀索引仅限于字符串类型，较普通索引会占用更小的空间。



# 存储引擎

和大多数的数据库不同，MySQL中有一个存储引擎的概念，针对不同的存储需求可以选择最优的存储引擎。

存储引擎就是存储数据，建立索引，更新查询数据等等技术的实现方式。存储引擎是基于表的，而不是基于库的，所以存储引擎也可被称为表类型。

Oracle，SqlServer等数据库只有一种存储引擎。MySQL提供了插件式的存储引擎架构。MySQL存在多种存储引擎，可以根据需要使用相应引擎，或者编写存储引擎。

MySQL5.0支持的存储引擎包含：InnoDB、MyISAM、BDB、MEMORY、MERGE、EXAMPLE、NDB Cluster、ARCHIVE、CSV、BLACKHOLE、FEDERATED等，其中InnoDB和BDB提供事务安全表，其他存储引擎是非事务安全表。

查看当前数据库支持的存储引擎：
```
show engines;
```

创建新表时如果不指定存储引擎，那么系统就会使用默认的存储引擎，MySQL5.5之前的默认存储引擎是MyISAM，5.5之后就改为了InnoDB。

查看Mysql数据库默认的存储引擎：
```sql
show variables like '% storage_engine%' ；
```

从事务、锁等多个方面，对比一下几种常用的存储引擎，如下表所示：
|特点|InnoDB|MyISAM|MEMORY|MERGE|NDB|
|---|---|---|---|---|---|
|存储限制| 64TB | 有 | 有 | 没有 | 有 |
|事务安全| ==支持== ||||
|锁机制| ==行锁(适合高并发)== | ==表锁== | 表锁 | 表锁 | 行锁 |
|B树索引| 支持 | 支持 | 支持| 支持 | 支持 |
|哈希索引| || 支持||
|全文索引| 支持(5.6版本之后) | 支持 ||||
|集群索引| 支持 ||||
|数据索引| 支持 || 支持 || 支持|
|索引缓存| 支持 | 支持 | 支持 | 支持 | 支持 |
|数据可压缩|| 支持 |||
|空间使用| 高 | 低 | N/A | 低 | 低 |
|内存使用| 高 | 低 | 中等 | 低 | 高 |
|批量插入速度| 低 | 高 | 高 | 高 | 高 |
|支持外键| ==支持== ||||

下面我们将重点介绍最常使用的两种存储引擎：InnoDB、MyISAM，另外两种MEMORY、MERGE，了解即可。


## InnoDB
> [Java 知识点整理：Spring、MySQL - 冠状病毒biss](https://www.nowcoder.com/discuss/440684?type=post&order=time&pos=&page=1&channel=666&source_id=search_post)

InnoDB现在是MySQL的默认存储引擎。它支持事务，具有事务提交、回滚、崩溃恢复的能力，支持外键以及行级锁。但是对比MyISAM存储引擎，InnoDB写的处理效率差一些，并且会占用更多的磁盘空间以保留数据和索引。

InnoDB默认的事务隔离级别是`REPEATABLE READ`，并且通过间隙锁策略防止幻读，到达了最高隔离级别的效果。

InnoDB的主键索引是聚簇索引，通过主键进行查询有很高的性能，通过非主键索引查询需要查出主键值，再通过主键值进行查询（二次回表）。非主键索引底层也是B+树，叶子节点存放了**主键值**，所以如果主键很大的话其他的非主键索引都会很大，因此主键应当尽可能小。

### 外键
InnoDB支持外键。外键是指子表中的某一列参考父表中某一列，子表中的这一列称为外键。在创建外键时，要求父表中被参考的列必须有对应的索引，子表会在外键上自动创建索引。子表中的外键通常会参考父表中的主键。

外键的使用条件包括：
1. 两个表必须是InnoDB表，MyISAM表暂时不支持外键；
2. 外键列必须建立了索引，MySQL4.1.2以后的版本在建立外键时会自动创建索引，但如果在较早的版本则需要显式建立；
3. 外键关系的两个表的列必须是数据类型相似，也就是可以相互转换类型的列，比如int和tinyint可以，而int和char则不可以；

外键的好处：可以使得两张表关联，保证数据的一致性和实现一些级联操作；

在子表创建外键时，可以指定在删除、更新父表时，对子表进行的相应操作，包括RESTRICT、CASCADE、SET NULL和 NO ACTION：
- RESTRICT和NO ACTION相同，表示在子表有关联记录的情况下，父表不能更新；
- CASCADE，表示父表在更新或者删除时，更新或者删除子表对应的记录；
- SET NULL，表示父表在更新或者删除的时候，子表的对应字段被SET NULL。


### 文件存储方式
InnoDB存储表文件和索引文件有以下两种方式 ：
1. 使用共享表空间存储，这种方式创建的表的表结构保存在.frm文件中，数据和索引保存在`innodb_data_home_dir`和`innodb_data_file_path`定义的表空间中，可以是多个文件。
2. 使用多表空间存储（默认），这种方式创建的表的表结构仍然存在 .frm 文件中，但是每个表的数据和索引单独保存在`.ibd`文件中。

### redo log日志
redo log又称重做日志，是Innodb存储引擎自带的日志，用于记录事务操作的变化，记录的是数据修改之后的值，不管事务是否提交都会记录下来。在实例和介质失败时，redo log文件就能派上用场，如数据库突然断电，InnoDB存储引擎会使用redo log恢复到掉电前的时刻，以此来保证数据的完整性。

redo log和binlog的区别在于：
- redo log是在InnoDB存储引擎层产生。binlog是MySQL数据库的上层产生的，并且二进制日志不仅仅针对InnoDB存储引擎，MySQL数据库中的任何存储引擎对于数据库的更改都会产生二进制日志。
- 两种日志记录的内容形式不同。MySQL的binlog是逻辑日志，记录的是对应的SQL语句。Innodb存储引擎层面的重做日志是物理日志，记录的是记录的是数据修改之后的值。
- 两种日志与记录写入磁盘的时间点不同，二进制日志只在事务提交完成后进行一次写入。InnoDB存储引擎的重做日志在事务进行中不断地被写入，并不是随事务提交的顺序进行写入的。
- binlog在写满或者重启之后，会生成新的binlog文件，redo log是循环使用的，空间固定会用完。
- binlog可以作为恢复数据使用，主从复制搭建，redo log作为异常宕机或者介质故障后的数据恢复使用。


## MyISAM
> [MYSQL有哪些存储引擎，各自优缺点。](https://blog.csdn.net/riemann_/article/details/89930883)

MyISAM不支持事务、也不支持外键，其优势是访问的速度快，对事务的完整性没有要求，以SELECT、INSERT为主的应用基本上都可以使用这个引擎来创建表。


### 文件存储方式
MyISAM把数据和索引存在不同的文件中，甚至可以把数据文件和索引文件放在不同目录。MyISAM在磁盘上存储成3个文件，其文件名都和表名相同，但扩展名分别是：
- .frm：存储表定义；
- .MYD：MYData，存储数据；
- .MYI：MYIndex，存储索引；

MyISAM的数据紧凑存储，因此可获得更小的索引和更快的全表扫描性能。


## MEMORY
MEMORY存储引擎将表的数据存放在内存中。每个MEMORY表实际对应一个磁盘文件，格式是.frm ，该文件中只存储表的结构，而数据文件存储在内存中，这样有利于数据的快速处理，提高整个表的效率。

MEMORY类型的表访问非常地快，因为数据是存放在内存中的，并且默认使用HASH索引，但是服务一旦关闭，表中的数据就会丢失。


## MERGE
MERGE存储引擎是一组MyISAM表的组合，这些MyISAM表必须结构完全相同，MERGE表本身并没有存储数据，对MERGE类型的表可以进行查询、更新、删除操作，这些操作实际上是对内部的MyISAM表进行的。

对于MERGE类型表的插入操作，是通过INSERT_METHOD子句定义插入的表，可以有3个不同的值，使用FIRST或LAST值使得插入操作被相应地作用在第一或者最后一个表上，不定义这个子句或者定义为NO，表示不能对这个MERGE表执行插入操作。


## 存储引擎对比和选择
MyISAM是MySQL5.5版本之前的默认数据库引擎。MyISAM性能比较好，提供了大量的特性，包括全文索引、压缩、空间函数等，但不支持事务和行级锁，而且最大的缺陷就是崩溃后无法安全恢复。不过，MySQL5.5版本之后，MySQL引入了InnoDB，并作为默认的存储引擎。

大多数时候我们使用的都是 InnoDB 存储引擎，但是在某些情况下使用 MyISAM 也是合适的，比如读密集的场景。

两者的对比：
- 是否支持行级锁：MyISAM只有表级锁，而InnoDB支持行级锁和表级锁，默认为行级锁。
- 是否支持事务和崩溃后的安全恢复：MyISAM强调的是性能，每次查询具有原子性，执行速度比InnoDB类型更快，但是不提供事务支持。InnoDB提供事务支持，具有事务提交回滚、崩溃修复的能力。
- 是否支持外键：MyISAM不支持，而InnoDB支持。
- 是否支持MVCC：仅InnoDB支持。应对高并发事务，MVCC比单纯的加锁更高效。



# 锁

锁是计算机协调多个进程或线程并发访问某一资源的机制（避免争抢）。

在数据库中，除传统的计算资源（如CPU、RAM、I/O等）的争用以外，数据也是一种供许多用户共享的资源。如何保证数据并发访问的一致性、有效性是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度来说，锁对数据库而言显得尤其重要，也更加复杂。

从对数据操作的角度分：
- 表锁：操作数据时，会锁定整个表。
- 行锁：操作数据时，会锁定当前数据行。
- 页面锁：操作数据是，会锁定当前数据所在的页面。

从对数据操作的类型分：
- 读锁（共享锁，S锁）：针对相同的数据，多个读操作可以同时进行而不会互相影响。
- 写锁（排他锁，X锁）：针对相同的数据，写锁会阻塞其他针对该数据的读锁和写锁。

相对其他数据库而言，MySQL的锁机制比较简单，其最显著的特点是不同的存储引擎支持不同的锁机制。不同存储引擎对锁的支持情况如下表所示：
|存储引擎|表级锁|行级锁|页面锁|
|---|---|---|---|
|MyISAM| 支持 | 不支持 | 不支持 |
|InnoDB| 支持 | 支持 | 不支持 |
|MEMORY| 支持 | 不支持 | 不支持 |
|BDB| 支持 | 不支持 | 支持 |

这3种锁的特性可大致归纳如下：
- 表级锁：MyISAM存储引擎使用，开销小，加锁快；不会出现死锁；锁定粒度大，发生锁冲突的概率最高，并发度最低。
- 行级锁：InnoDB存储引擎使用，开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度也最高。
- 页面锁：开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般。

从上述特点可见，很难笼统地说哪种锁更好，只能就具体应用的特点来说哪种锁更合适！仅从锁的角度来说：表级锁更适合于以查询为主，只有少量按索引条件更新数据的应用，如Web应用；而行级锁则更适合于有大量按索引条件并发更新少量不同数据，同时又有并查询的应用，如一些在线事务处理系统。


## MyISAM
MyISAM存储引擎只支持表锁，可以分为读锁和写锁。

对于执行查询语句（SELECT）前，MyISAM会自动给涉及的所有表加读锁；对于更新操作（UPDATE、DELETE、INSERT 等）前，MyISAM会自动给涉及的表加写锁。加锁的过程对于用户是透明的，不需要用户干预。

当然，用户也可以在操作数据之前显式地加锁：
```sql
# 加读锁
lock table table_name read;
# 加写锁
lock table table_name write；
```

MyISAM的读锁和写锁的相互兼容性如下表所示：
| 当前锁模式（行）/请求锁模式（列） | 读锁 | 写锁 |
| --------------------------------- | ---- | ---- |
| 读锁                              | 是   | 否   |
| 行锁                              | 是   | 否   |

由上表可见：
- 对MyISAM表的读操作，不会阻塞其他用户对同一表的读请求，但会阻塞其他用户对同一表的写请求；
- 对MyISAM表的写操作，不会阻塞当前用户对同一表的读请求，但会阻塞其他用户对同一表的读和写请求；

简而言之，就是读锁会阻塞写，但是不会阻塞读。写锁，既会阻塞读，又会阻塞写。

此外，MyISAM的读写锁调度是写优先，这也是MyISAM不适合做写为主的表的存储引擎的原因。因为写锁后，其他线程不能做任何操作，大量的更新会使得查询很难得到锁，从而造成查询被阻塞，查询效率低。


## InnoDB
InooDB既支持表锁也支持行锁。行锁开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度也最高。InnoDB默认使用行锁。

InnoDB与MyISAM的最大不同有两点：一是支持事务；二是采用了行级锁。

### 事务
事务就是一组SQL语句的逻辑操作单元。事务内的语句要么全部执行成功，要么全部执行失败。

事务具有4个特性（简称为ACID）：
- **原子性（Atomicity）**：一个事务在逻辑上是不可分割的最小工作单元。整个事务中的所有操作要么全部提交成功，要么全部失败，不能只执行其中的一部分。
- **一致性（Consistency）**：数据库总是从一个一致性的状态转换到另一个一致性的状态。执行事务前后，多个事务对同一个数据读取的结果是相同的。
- **隔离性（Isolation）**：针对并发事务而言，隔离性就是要隔离并发运行的多个事务之间的相互影响。一个事务所做的修改在最终提交以前，对其他事务是不可见的。
- **持久性（Durability）**：一旦事务提交成功，其修改就会永久保存到数据库中。即使系统崩溃，修改的数据也不会丢失。

在应用程序中，多个事务并发运行，经常会操作相同的数据来完成各自的任务。并发虽然是必须的，但可能会导致一些问题：
- **丢失修改（Lost to modify**）：指一个事务修改了一个数据后，还没有进行提交，如果另一个事务也修改了这个数据，那么第一个事务的修改就丢失了，这种情况称为丢失修改。
- **脏读（Dirty read）**：当一个事务正在访问一个数据并对数据进行了修改而且还没有提交，这时另外一个事务使用了这个修改过的数据。这个数据还没有提交，第二个事务读到的数据就是“脏数据”，这种情况称为脏读。
- **不可重复读（Unrepeatable read）**：指在一个事务内多次读取同一个数据的结果不一样。如果在一个事务两次读取一个数据之间，另一个事务对这个数据进行了修改，那第一个事务两次读取的结果可能不太一样，这种情况称为不可重复读。
- **幻读（Phantom read）**：幻读与不可重复读类似。它发生在一个事务读取了一些数据，接着另一个并发事务插入了一些数据。在随后的查询中，第一个事务可能发现多了一些原本不存在的记录，就好像发生了幻觉一样，这种情况称为幻读。

不可重复读和幻读的区别：不可重复读的重点是修改，比如多次读取一条记录，发现其中某些列的值被修改；幻读的重点是增加或者删除，比如多次读取某种条件的数据，发现记录增加或减少了。

为了解决上述提到的事务并发问题，数据库提供一定的事务隔离机制来解决这个问题。数据库的事务隔离越严格，并发副作用越小，但付出的代价也就越大。事务隔离实质上就是使用事务在一定程度上“串行化” 进行，这显然与“并发” 是矛盾的。

在SQL标准中定义了四种隔离级别，每一种隔离级别都规定了一个事务中所做的修改，哪些在事务内和事务间是可见的，哪些是不可见的。隔离级别从低到到高包括：
- **读取未提交（READ UNCOMMITTED）**：最低的隔离级别，允许事务读取其他事没有提交的数据修改，可能会导致脏读、幻读或不可重复读。
- **读取已提交（READ COMMITTED）**：允许事务读取其他事务已经提交的数据修改，可以阻止脏读，但是可能会导致不可重复读和幻读。
- **可重复读（REPEATABLE READ）**：MySQL默认的隔离级别。在一个事务中，多次读取同一个数据的结果是一致的，除非是自己进行修改。可以阻止脏读和不可重复读，但是可能会导致幻读。
- **可串行化（SERIALIZABLE）**：最高的隔离级别，完全服从ACID的隔离级别。所有的事务依次逐个执行，事务之间就完全不可能产生干扰。该级别可以防止脏读、不可重复读以及幻读。

不同隔离级别可能发生的问题：
| 隔离级别 | 丢失修改 | 脏读 | 不可重复读 | 幻读 |
| --- | --- | --- | --- | --- |
| READ-UNCOMMITTED | × | √ | √ | √ |
| READ-COMMITTED  | × | × | √ | √ |
| REPEATABLE-READ | × |	× | × | √ |
| SERIALIZABLE | × | × | × | × |

值得注意的是，InnoDB存储引擎默认的隔离级别是`REPEATABLE-READ`，由于使用了间隙锁，可以避免幻读问题。所以，InnoDB存储引擎默认的隔离级别已经可以保证事务的隔离性要求，达到了`SERIALIZABLE`隔离级别的要求。


### InnoDB行锁
InnoDB实现了以下两种类型的行锁：
- 读锁：读锁允许多个事务共享同一行数据的锁，都能访问这一行数据，但是只能读不能修改。
- 写锁：写锁允许获得锁的事务修改和读取一行数据，但是其他事务就不能再获取该行的其他锁，包括读锁和写锁。

对于普通查询语句（SELECT），InnoDB不会加任何锁；对于更新语句（UPDATE、DELETE和INSERT），InnoDB会自动给涉及数据集加写锁。加锁的过程对于用户是透明的，不需要用户干预。

当然，用户也可以显式给记录集加读锁或写锁：
```mysql
# 共享锁
SELECT * FROM table_name WHERE ... LOCK IN SHARE MODE
# 排他锁
SELECT * FROM table_name WHERE ... FOR UPDATE
```

为了允许行锁和表锁共存，实现多粒度锁机制，InnoDB还有两种内部使用的意向锁（Intention Locks）：
- 意向共享锁（IS）：事务在给一个数据行加共享锁前必须先取得该表的意向共享锁锁。
- 意向排他锁（IX）：事务在给一个数据行加排他锁前必须先取得该表的意向排他锁。

意向锁也是数据库隐式帮我们做了，不需要程序员操心！

**行锁升级为表锁**：InnoDB的行锁是基于索引的，如果查询数据不通过索引条件，那么InnoDB将对表中的所有记录加锁，实际效果跟表锁一样。

**间隙锁**：当我们使用范围条件，而不是相等条件查询数据时，并请求读锁或写锁时，InnoDB会给符合条件的已有数据进行加锁；对于键值在条件范围内但并不存在的记录，叫做**间隙**（GAP），InnoDB也会对这个**间隙**加锁，这种锁机制就是所谓的间隙锁（Next-Key锁）。间隙锁只能在`Repeatable read`隔离级别下使用。

间隙锁的坏处是会降低并发度，但是好处是可以避免幻读问题。

查看InnoDB行锁争用情况：
```mysql
show status like 'innodb_row_lock%';
```

解释：
- Innodb_row_lock_current_waits：当前正在等待锁定的数量
- Innodb_row_lock_time：从系统启动到现在锁定总时间长度
- Innodb_row_lock_time_avg：每次等待所花平均时长
- Innodb_row_lock_time_max：从系统启动到现在等待最长的一次所花的时间
- Innodb_row_lock_waits：系统启动后到现在总共等待的次数当等待的次数很高，而且每次等待的时长也不小的时候，我们就需要分析系统中为什么会有如此多的等待，然后根据分析结果着手制定优化计划。


### 死锁
并发事务和加锁会导致死锁的问题。

一般来说，MySQL通过回滚可以解决死锁的问题，但死锁是无法完全避免的。为了尽可能减少死锁，可以：
- 以固定的顺序访问表和行。比如对两个job批量更新的情形，简单方法是对id列表先排序，后执行，这样就避免了交叉等待锁的情形；将两个事务的SQL顺序调整为一致，也能避免死锁。
- 大事务拆小。大事务更倾向于死锁，如果业务允许，将大事务拆小。
- 一次性获取所有资源。在同一个事务中，尽可能做到一次锁定所需要的所有资源，减少死锁概率。
- 降低隔离级别。如果业务允许，将隔离级别调低也是较好的选择，比如将隔离级别从RR调整为RC，可以避免掉很多因为gap锁造成的死锁。
- 为表添加合理的索引。可以看到如果不走索引将会为表的每一行记录添加上锁，死锁的概率大大增大。


### 总结
InnoDB存储引擎默认使用行级锁，加锁和解锁的开销比表锁更高一些，但是在并发处理能力方面要优于表锁。所以，当系统并发量较高的时候，InnoDB的整体性能要高于MyISAM。但是，InnoDB的行级锁也有其脆弱的一面，当使用不当的时候，可能会让InnoDB的整体性能可能会更差。

InnoDB基于行锁还实现了MVCC多版本并发控制，MVCC在隔离级别下的`Read committed`和`Repeatable read`下工作。MVCC能够实现读写不阻塞！

优化建议：
- 尽可能让所有数据检索都能通过索引来完成，避免无索引行锁升级为表锁。
- 合理设计索引，尽量缩小锁的范围
- 尽可能减少索引条件，及索引范围，避免间隙锁
- 尽量控制事务大小，减少锁定资源量和时间长度
- 尽可能使用低级别事务隔离（但是需要业务层面满足需求）


## MVCC
> [mysql的MVCC（多版本并发控制）](https://www.cnblogs.com/myseries/p/10930910.html)

> [MySQL InnoDB MVCC 机制的原理及实现](https://zhuanlan.zhihu.com/p/64576887)

MVCC（Multi-Version Concurrency Control，多版本并发控制）可以认为是InnoDB行级锁的一个变种，它可以在很多情况下避免加锁操作，因此开销更低。InnoDB存储引擎支持MVCC（MyISAM不支持），而且MVCC只能用`于READ-COMMITTED`和`READ-COMMITTED`两种隔离级别。MVCC能够实现读操作不阻塞，写操作也只锁定必要的行。

数据库事务有不同的隔离级别，不同的隔离级别对锁的使用是不同的。换句话说，事务的隔离级别就是通过锁的机制来实现，只不过隐藏了加锁细节。对于并发事务，加锁会导致阻塞的情况，影响系统的并发量。

对于一个事务，不管其执行多长时间，其内部看到的数据是一致的。为了提高并发性，MVCC通过保存数据**在某个事务请求时间点**的一致性快照（Snapshot），并用这个快照提供一定级别（语句级别或事务级别）的一致性读取。从用户的角度来看，好像是数据库可以提供同一数据的多个版本。

数据的一致性快照有两个级别：
- 语句级别：针对于`Read committed`隔离级别
- 事务级别：针对于`Repeatable read`隔离级别

通过锁和一致性快照讲解一下不同的事务隔离级别可能发生什么问题以及如何解决问题：
- Read uncommitted：可能出现脏读问题，原因是事务操作（修改）完数据就释放锁，导致其他事务可能读取到该数据。
- Read committed：可以避免脏读问题，做法是把释放锁的位置调整到事务提交之后，在该事务提交之前，其他事务无法读取或修改该数据。但是可能出现不可重复读问题，原因是`Read committed`是语句级别的快照，每次读取的都是当前数据的最新版本。
- Repeatable read：可以避免不可重复读问题，做法是在事务开始时保存一个快照，事务每次读取的都是当前事务的快照。即使其他事务修改了当前事务读取的的数据，当前事务的快照数据也不会变。可能会发生幻读问题，导致前后读取不一致，InnoDB在该隔离级别使用了间隙锁，可以防止幻读问题。

> InnoDB的MVCC，是通过在每行记录后面保存两个隐藏的列来实现的。这两个列，一个保存了行的创建时间，一个保存行的过期时间（或删除时间）。当然，存储的并不是实际的时间值，而是系统版本号，每开始一个新的事务，系统版本号就会递增。事务开始时刻的系统版本号会作为事务的版本号，用来和查询到的每行记录的版本号进行比较。——来自《高性能MySQL》

在`Repeatable read`隔离级别下，MVCC的操作如下：
- SELECT：
    - InnoDB只查找版本**早于**当前事务版本的数据行（也就是说，行的系统版本号小于或等于事务的系统版本号），这样可以确保事务读取的行，要么是事务开始前就已存在的，要么是事务自身插入或修改过的。
    - 行的删除版本要么未定义，要么大于当前事务版本号。可以确保事务读取的行，在事务开始之前未删除。
- INSERT：将新插入的行保存当前版本号为行版本号。
- DELETE：将删除的行保存当前版本号为删除标识。
- UPDATE：为INSERT和UPDATE操作的组合，INSERT的行保存当前版本号为行版本号，UPDATE则保存当前版本号到原来的行作为删除标识。

由于旧数据并不真正的删除，所以必须对这些数据进行清理，innodb会开启一个后台线程执行清理工作，具体的规则是将删除版本号小于当前系统版本的行删除，这个过程叫做purge。


## 悲观锁和乐观锁
无论是`Read committed`还是`Repeatable read`隔离级别，都是为了解决读写冲突的问题。

解决读写冲突的方法有：
- 使用`Serializable`隔离级别，事务串行执行，没有冲突
- 使用乐观锁。乐观锁是一种思想，具体实现是在数据表中添加一个版本字段，当更新某个数据时，检查该数据的版本是否和预期相同。如果相同，可以更新，否则不可以更新。
- 使用悲观锁。悲观锁是数据库层面加锁，都会阻塞去等待锁。

使用悲观锁：
- 举例：`select * from xxx for update`，在`SELECT`语句后边加了`for update`相当于加了写锁（排他锁）。加了写锁以后，其他事务不能进行修改，需要等待当前事务执行完。

使用乐观锁：
- 乐观锁不是数据库层面上的锁，需要手动加锁，一般是添加一个版本字段来实现。
- 举例：`update A set name=xxx,version=version+1 where ID=#{id} and version=#{version}`，更新数据之前，判断version字段与当前的version字段是否相等，相等则更新成功，否则更新失败。
- 如果两个事务同时更新同一行数据，一个事务如果更新成功，另一个事务就不能更新。



# 日志

在 MySQL 中，有 4 种不同的日志，分别是错误日志、二进制日志（BINLOG 日志）、查询日志和慢查询日志，这些日志记录着数据库在不同方面的踪迹。

## 错误日志

错误日志是 MySQL 中最重要的日志之一，它记录了当 MySQL 启动和停止时，以及服务器在运行过程中发生任何严重错误时的相关信息。当数据库出现任何故障导致无法正常使用时，可以首先查看此日志。

该日志是默认开启的，默认存放目录为 MySQL 的数据目录（var/lib/mysql）, 默认的日志文件名为hostname.err（hostname是主机名）。

查看日志位置指令：

```mysql
show variables like 'log_error%';
```

查看日志内容：

```shell
tail -f /var/lib/mysql/xaxh-server.err
```


## 二进制日志

二进制日志（binary log，binlog）记录了所有的 DDL（数据定义语言）语句和 DML（数据操纵语言）语句，但是不包括数据查询语句。此日志对于灾难时的数据恢复起着极其重要的作用，MySQL的主从复制， 就是通过binlog实现的。

默认情况下，二进制日志是没有开启的，需要在MySQL的配置文件中开启，并配置MySQL日志的文件名和格式。如果不指定日志的保存路径，则默认保存在MySQL的数据目录。

```ini
# 设置开启binlog日志，日志的文件前缀为mysqlbin，生成的文件名如mysqlbin.000001,mysqlbin.000002
log_bin=mysqlbin
# 配置二进制日志的格式
binlog_format=STATEMENT/ROW/MIXED
```

二进制日志有三种格式：

- STATEMENT：该日志格式在日志文件中记录的是SQL语句（STATEMENT），每一条对数据进行修改的SQL都会记录在日志文件中。通过MySQL提供的mysqlbinlog工具，可以查看到每条语句的文本。主从复制的时候，从库（slave）会将日志解析为原文本，并在从库重新执行一次。
- ROW：该日志格式在日志文件中记录的是每一行的数据变更，而不是记录SQL语句。比如，执行SQL语句：`update
  tb_book set status='1'`, 如果是STATEMENT格式，在日志中会记录一行SQL文件； 如果是ROW，由于是对全表进行更新，也就是每一行记录都会发生变更，日志中会记录每一行的数据变更。
- MIXED：这是目前MySQL默认的日志格式，混合了STATEMENT 和 ROW两种格式。默认情况下采用STATEMENT，但是在一些特殊情况下采用ROW来进行记录。MIXED 格式能尽量利用两种模式的优点，而避开他们的缺点。

由于日志以二进制方式存储，不能直接读取，需要使用mysqlbinlog工具来查看，语法如下：

```mysql
# 查看STATEMENT格式日志
mysqlbinlog log-file;
# 查看ROW格式日志
mysqlbinlog -vv log-file;
```

对于比较繁忙的系统，由于每天生成日志量大，这些日志如果长时间不清除，将会占用大量的磁盘空间。删除日志有几种常用方法：

```mysql
# 删除全部 binlog 日志，删除之后，日志编号将从 xxxx.000001 重新开始。
Reset Master;
# 删除 ****** 编号之前的所有日志
purge master logs to 'mysqlbin.******'
# 删除 某一时间 之前产生的所有日志
purge master logs before 'yyyy-mm-dd hh24:mi:ss'
```

还有一种删除日志的方法，但是不需要手动删除日志：#在MySQL配置文件中设置日志过期参数，过了指定天数后的日志将会被自动删除。

```ini
--expire_logs_days=xxx
```


## 查询日志

查询日志中记录了客户端的所有操作语句，而二进制日志不包含查询数据的SQL语句。

查询日志默认是未开启的。如果需要开启查询日志，可以配置：

```ini
# 设置是否开启查询日志，0表示关闭，1表示开启
general_log=1
# 设置日志的文件名，如果没有指定，默认的文件名为 host_name.log
general_log_file=file_name
```


## 慢查询日志

慢查询日志记录了执行时间超过参数`long_query_time`设置值并且扫描记录数不小于`min_examined_row_limit`的SQL语句。`long_query_time`默认为 10 秒，最小为 0， 精度可以到微秒。

慢查询日志默认是关闭的。如果需要开启慢查询日志，可以配置：

```ini
# 设置是否开启慢查询日志， 0代表关闭，1代表开启
slow_query_log=1
# 设置慢查询日志的文件名
slow_query_log_file=slow_query.log
# 设置查询的时间限制，超过这个时间将认为是慢查询，需要进行日志记录，默认为10秒
long_query_time=10
```

和错误日志、查询日志一样，慢查询日志记录的格式也是纯文本，可以被直接读取。

如何查询`long_query_time`：

```mysql
show variables like 'long%';
```

如何查看慢查询日志：

```shell
# Linux
cat slow_query.log
```

如果慢查询日志内容很多，直接查看文件比较麻烦，这个时候可以借助于MySQL自带的`mysqldumpslow`工具， 来对慢查询日志进行分类汇总。



# 体系结构

> [一条SQL语句在MySQL中如何执行的](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247485097&idx=1&sn=84c89da477b1338bdf3e9fcd65514ac1&chksm=cea24962f9d5c074d8d3ff1ab04ee8f0d6486e3d015cfd783503685986485c11738ccb542ba7&token=79317275&lang=zh_CN#rd)

MySQL体系结构如下图所示：

![MySQL体系结构](https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030153533.png?token=AE4F4YPUQCBD6XAVVHQCJSC7TPBN6)

整个MySQL Server由以下组成

- Connection Pool：连接池组件
- Management Services & Utilities：管理服务和工具组件
- SQL Interface：SQL接口组件
- Parser：查询分析器组件
- Optimizer：优化器组件
- Caches & Buffers：缓冲池组件
- Pluggable Storage Engines：存储引擎
- File System：文件系统

MySQL包括：

- 连接层：最上层是一些客户端和链接服务，包含本地socket通信和大多数基于客户端/服务端工具实现的类似于TCP/IP的通信。主要完成一些类似于连接处理、授权认证、及相关的安全方案。在该层上引入了线程池的概念，为通过认证安全接入的客户端提供线程。同样在该层上可以实现基于SSL的安全链接。服务器也会为安全接入的每个客户端验证它所具有的操作权限。
- 服务层：第二层架构主要完成大多数的核心服务功能，如SQL接口，并完成缓存的查询，SQL的分析和优化，部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如过程、函数等。在该层，服务器会解析查询并创建相应的内部解析树，并对其完成相应的优化如确定表的查询的顺序，是否利用索引等，最后生成相应的执行操作。如果是select语句，服务器还会查询内部的缓存，如果缓存空间足够大，这样在解决大量读操作的环境中能够很好的提升系统的性能。
- 引擎层：存储引擎层，存储引擎真正的负责了MySQL中数据的存储和提取，服务器通过API和存储引擎进行通信。不同的存储引擎具有不同的功能，这样我们可以根据自己的需要，来选取合适的存储引擎。
- 存储层：数据存储层， 主要是将数据存储在文件系统之上，并完成与存储引擎的交互。

和其他数据库相比，MySQL有点与众不同，它的架构可以在多种不同场景中应用并发挥良好作用。主要体现在存储引擎上，插件式的存储引擎架构，将查询处理和其他的系统任务以及数据的存储提取分离。这种架构可以根据业务的需求和实际需要选择合适的存储引擎。


## 一条SQL语句的执行过程

==TODO==



# MySQL复制
> [MySql 主从复制及配置实现](https://segmentfault.com/a/1190000008942618)

复制是指将主数据库的数据定义语言（DDL）和数据操作语言（DML）操作通过二进制日志传到从库服务器中，然后在从库上对这些日志重新执行（也叫重做），从而使得从库和主库的数据保持同步。

MySQL支持一台主库同时向多台从库进行复制，从库同时也可以作为其他从服务器的主库，实现链状复制。

MySQL主从复制有三种类型：
- 基于语句的复制：主服务器上面执行的语句在从服务器上面再执行一遍。存在的问题：时间上可能不完全同步造成偏差，执行语句的用户也可能是不同一个用户。
- 基于行的复制：把主服务器上面修改后的内容直接复制过去，而不关心到底修改是由哪条语句引发的。存在的问题：如果一次性修改了多条数据，那么基于行的复制则要复制所有被修改的内容，开销比较大。
- 混合类型的复制：默认使用基于语句的复制，当基于语句的复制会引发问题的时候就会使用基于行的复制。

在MySQL主从复制架构中，读操作可以在所有的服务器上面进行，而写操作只能在主服务器上面进行。

MySQL主从复制的原理如下图所示：

![MySQL主从复制](https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030154026.jpg?token=AE4F4YLF3BA64YLV3WHZSUS7TPCAG)

上图是主从复制的一主一从模式，复制过程分成三步：
- 主库（Master）把数据更新记录在二进制日志（Binlog）文件中。具体做法是在每次准备提交事务更新数据前，主库将数据更新的事件记录到二进制日志中。日志记录好之后，主库通知存储引擎提交事务。
- 从库（Slave）开启一个I/O线程，连接到主库上，请求读取主库的二进制日志，然后把读取到的二进制日志写到本地的中继日志（Realy log）文件中。
- 从库还要开启一个SQL线程，定时检查中继日志，如果发现有更改，立即把更改的内容在本机上面执行一遍。

MySQL主从复制还有一主多从模式，原理如下图所示：

![MySQL主从复制2](https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030154053.jpg?token=AE4F4YJGCSS2WAXOZXR5RBK7TPCB4)

一主多从模式的复制过程为链式复制，此时从库需要开启二进制日志：
- 一个从库请求读取主库的二进制日志，然后把读取到的二进制日志写到本地的中继日志（Realy log）文件中。
- 该从库开启一个SQL线程，定时检查中继日志，如果发现有更改，立即把更改的内容在本机上面执行一遍。由于开启了二进制日志，所以可以得到二进制日志。
- 其他从库把它连接的从库当作自己的主库，请求二进制日志。

MySQL复制的优点主要包含以下三个方面：
- 主库出现问题，可以快速切换到从库提供服务。
- 可以在从库上执行查询操作，从主库中更新，实现读写分离，降低主库的访问压力。
- 可以在从库中执行备份，以避免备份期间影响主库的服务。


# SQL优化
> [Mysql高性能优化规范建议](https://www.cnblogs.com/huchong/p/10219318.html)

> [腾讯面试：一条SQL语句执行得很慢的原因有哪些？---不看后悔系列](https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247485185&idx=1&sn=66ef08b4ab6af5757792223a83fc0d45&chksm=cea248caf9d5c1dc72ec8a281ec16aa3ec3e8066dbb252e27362438a26c33fbe842b0e0adf47&token=79317275&lang=zh_CN#rd)

在应用的的开发过程中，由于初期数据量小，开发人员写 SQL 语句时更重视功能上的实现，没有重视SQL的效率问题。当应用系统正式上线后，随着生产数据量的急剧增长，很多 SQL 语句开始逐渐显露出性能问题，对生产的影响也越来越大，此时这些有问题的 SQL 语句就成为整个系统性能的瓶颈，因此我们必须要对它们进行优化

当面对一个有 SQL 性能问题的数据库时，一般需要进行系统分析：
1. 查看SQL执行频率
2. 定位低效率SQL
3. 分析低效率SQL

另外，还可以：
1. 使用`show profile`了解MySQL操作的时间消耗；
2. 使用`trace`追踪SQL的执行；

常见的SQL语句优化：
1. 尽量避免在where字句中使用or来连接条件，否则将导致放弃使用索引而进行全表扫描。
2. 正确使用like查询。
3. 尽量避免在where字句中对字段进行表达式操作。
4. 如果确认查询结果数量，应尽可能加上limit。
5. 正确使用复合索引。
6. 如果使用了join，请尽量使用小表join大表。


## 查看SQL执行频率
MySQL 客户端连接成功后，可以通过`show status`和`like`查询得到各种操作的执行次数：
```
# 查看服务器信息
show status;
# 查看session级别或global级别的统计结果，默认为session
show [session|global] status like 'Com_';
```

`Com_xxx`表示每种 xxx 语句执行的次数，我们通常比较关心的是以下几个统计参数：
|参数|含义|
|---|---|
|Com_select| 执行 SELECT 操作的次数，一次查询只累加 1 次。|
|Com_insert| 执行 INSERT 操作的次数，对于批量插入的 INSERT 操作，只累加 1 次。|
|Com_update| 执行 UPDATE 操作的次数。|
|Com_delete| 执行 DELETE 操作的次数。|
|Innodb_rows_read| SELECT 查询返回的行数。|
|Innodb_rows_inserted| 执行 INSERT 操作插入的行数。|
|Innodb_rows_updated| 执行 UPDATE 操作更新的行数。|
|Innodb_rows_deleted| 执行 DELETE 操作删除的行数。|
|Connections| 试图连接 MySQL 服务器的次数。|
|Uptime| 服务器工作时间。|
|Slow_queries| 慢查询的次数。|

注意：
- Com_***: 这些参数对于所有存储引擎的表操作都会进行累计。
- Innodb_***: 这些参数只针对 InnoDB 存储引擎，累加的算法也略有不同。


## 定位低效率SQL
定位执行效率较低的 SQL 语句有两种方式：
- 慢查询日志：查询执行结束以后，才能查看慢查询日志。
- show processlist：查看当前MySQL在进行的线程，包括线程的状态等信息，可以实时地查看 SQL 的执行情况。

要想通过慢查询日志定位那些执行效率较低的 SQL 语句，首先要设置开启慢查询日志：
```mysql
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

可以使用MySQL提供的日志分析工具mysqldumpslow，获取指定条件的慢SQL：
```mysql
# 获取慢日志中最多的10个SQL
mysqldumpslow -s r -t 10 /PATH/TO/慢查询日志
# 获取按照时间排序的前10条里面含有左连接的查询语句
mysqldumpslow -s t -t 10 -g "left join" 慢日志文件
```

慢查询日志在查询结束以后才进行纪录，可以使用`show processlist`命令查看当前MySQL在进行的线程，包括线程的状态、是否锁表等，可以实时地查看 SQL 的执行情况，同时对一些锁表操作进行优化。

```mysql
show processlist;
```

`show processlist`命令的结果说明：

- Id：用户登录mysql时，系统分配的`connection_id`，可以使用函数`connection_id()`查看
- User：显示当前用户。如果不是root，这个命令就只显示用户权限范围的sql语句
- Host列：显示这个语句是从哪个ip的哪个端口上发的，可以用来跟踪出现问题语句的用户
- db列，显示这个进程目前连接的是哪个数据库
- Command：显示当前连接的执行的命令，一般取值为休眠（sleep），查询（query），连接
（connect）等
- Time：显示这个状态持续的时间，单位是秒
- State：显示使用当前连接的SQL语句的状态，State描述的是语句执行中的某一个状态。一个SQL语句，以查询为例，可能需要经过copying to tmp table、sorting result、sending data等状态才可以完成
- Info：显示这个SQL语句，是判断问题语句的一个重要依据


## 分析低效率SQL（explain）
> [如何分析Mysql慢SQL](https://www.cnblogs.com/linyue09/p/9869163.html)

通过以上步骤查询到效率低的 SQL 语句后，可以通过`EXPLAIN`命令获取MySQL如何执行`SELECT`语句
的详细信息。

查询SQL语句的执行计划：
```mysql
# id有索引
explain select * from table_name where id = 1;
```

如下图所示：

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030155624.png?token=AE4F4YMGKUUAQKOAP7DRDYC7TPD4E" alt="MySQL explain" style="zoom:80%;" />

```mysql
# title没索引
explain select * from tb_item where title = '阿尔卡特 (OT-979) 冰川白 联通3G手机3';
```

如下图所示：

<img src="https://raw.githubusercontent.com/shengchaohua/MyImages/main/images/20201030155633.png?token=AE4F4YMZBFOYPXDNHH74DK27TPD4Y" alt="MySQL explain2" style="zoom:80%;" />


### id
SELECT查询的序列号，包含一组数字，表示查询中执行SELECT语句或操作表的顺序，有3种情况：
- id相同，表示从上到下顺序执行；
- id不同，如果包含子查询，id序号会递增，id值越大，优先级越高，越先执行；
- id有相同，也有不同，同时存在，id相同的认为是一组，从上往下顺序执行。在所有的组中，id值越大，优先级越高，越先执行。

### select_type
查询类型，比较复杂，包含多种情况：
- SIMPLE：简单查询，查询中不包含子查询或者UNION语句
- PRIMARY：查询中若包含任何复杂的子查询，最外层查询标记为该标识
- SUBQUERY：在SELECT或WHERE中包含的子查询
- DERIVED：在FROM中包含的子查询，被标记为DERIVED，MySQL会递归执行这些子查询，把结果放到临时表中
- UNION：若第二个SELECT出现UNION之后，则被标记为UNION；若UNION包含在FROM子句的子查询中，外层子查询将被标记为DERIVED
- UNION RESULT：从UNION表获取结果的查询

### table
显示一行数据是关于哪张表的


### type
访问类型，是较为重要的一个指标，包括多种情况：
- NULL：不访问任何表，索引，直接返回结果。
- system：表只有一行记录（等于系统表），这是const类型的特例，一般不会出现。
- const：表示通过索引一次就找到了，const用于比较主键索引或者unique索引。因为只匹配一行数据，所以很快。如果where查询条件使用了主键索引，MySQL就能将该查询转换为一个常。量
- eq_ref：唯一性索引扫描，对于每个索引键，表中只有一条记录与之匹配。常见于主键或唯一索引扫描。
- ref：非唯一性索引扫描，返回匹配某个单独值的所有行。本质上也是一种索引访问，它返回所有匹配某个单独值的行。
- range：只检索给定范围的行，使用一个索引来选择行。key列显示使用了哪个索引，一般就是在where语句中出现`between、<、>、in`等查询。这种范围扫描索引比全表扫描要好，因为只需要开始于索引的某一点，结束于另一点，不用扫描全部索引。
- index：全部索引扫描，index与ALL的区别为index类型只遍历索引树，这通常比ALL快，因为索引文件通常比数据文件小。
- all：全表扫描，遍历全表获得匹配的行。

结果值从最好到最坏以此是：
```
NULL > system > const > eq_ref > ref > fulltext > ref_or_null > index_merge >
unique_subquery > index_subquery > range > index > ALL

system > const > eq_ref > ref > range > index > ALL
```

一般来说， 我们需要保证查询至少达到 range 级别， 最好达到ref。

### possible_keys
表示可能应用在这张表中的索引，一个或多个。如果查询涉及到的字段上存在索引，则该索引将被列出，但不一定被查询实际使用。

### key
表示实际使用的索引。如果为NULL，则没有使用索引。查询中若出现了覆盖索引，则该索引仅出现在key列表中。

### key_len
表示索引中使用的字节数，该值为索引字段的最大可能长度，并非实际使用长度。在不损失精度的情况下，长度越短越好。

### ref
显示索引的哪一列被使用了，哪些列或常量被用于查找索引列上的值。

### rows
根据表统计信息及索引选用情况，大致估算出找到所需记录多需要读取的行数。

### Extra
包含不适合在其他列中显示但十分重要的额外信息：
- Using filesort：MySQL会对数据使用一个外部的索引排序，而不是按照表内的索引顺序进行读取，称为文件排序，效率低；
- Using temporary：使用了临时表保存中间结果，MySQL在对查询结果排序时使用临时表。常见于排序order by和分组查询group by；
- Using index：表示相应的SELECT操作中使用了覆盖索引，避免访问表的数据行，效率不错。 


## 使用show profile分析SQL
查看状态：SHOW VARIABLES LIKE 'profiling';
- 开启profile：set profiling=on;
- 查看结果：show profiles;

诊断SQL：show profile cpu,block io for query 上一步SQL数字号码;
- ALL：显示所有开销信息
- BLOCK IO：显示IO相关开销
- CONTEXT SWITCHES：显示上下文切换相关开销
- CPU：显示CPU相关开销
- IPC：显示发送接收相关开销
- MEMORY：显示内存相关开销
- PAGE FAULTS：显示页面错误相关开销
- SOURCE：显示和Source_function，Source_file，Source_line相关开销
- SWAPS：显示交换次数相关开销


## 使用trace追踪SQL
==TODO==


# MySQL优化
## 大表优化
### 限定数据的范围
务必禁止不带任何限制数据范围条件的查询语句。比如：我们当用户在查询订单历史的时候，我们可以控制在一个月的范围内。

### 垂直分区
垂直拆分是指根据数据表中的列的相关性，把一张列比较多的表拆分为多张表。例如，用户表中既有用户的登录信息又有用户的基本信息，可以将用户表拆分成两个单独的表，甚至放到单独的库做分库。

优点：可以使得列数据变小，在查询时减少读取的Block数，减少I/O次数。此外，垂直分区可以简化表的结构，易于维护。

缺点：主键会出现冗余，需要管理冗余列，并会引起Join操作，可以通过在应用层进行Join来解决。此外，垂直分区会让事务变得更加复杂；

### 水平拆分
水平拆分是指保持数据表的列结构不变，在水平上把一张表的数据拆成多张表来存放，达到了分布式的目的。举个例子：我们可以将用户信息表拆分成多个用户信息表，这样就可以避免单一表数据量过大降低操作的性能。

水平拆分可以支持非常大的数据量。注意的一点是，分表仅仅是解决了单一表数据过大的问题。如果拆分后的表还是在同一台机器上，其实对于提升MySQL的并发能力没有什么意义，所以水平拆分最好分库 。

水平拆分能够支持非常大的数据量存储，应用端改造也少，但分片事务难以解决 ，跨节点Join性能较差，逻辑复杂。
