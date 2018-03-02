---
title: MySQL常记与常识
date: 2018-02-27 19:44:37
tags:
- MySQL
- 速查
- 笔记
- 数据库
categories:
- 编程
---

一份精要的，不失细节的MySQL命令常识与技巧

<!--more-->

## 系统操作

### 在DOS环境下设置root密码

```sql
D:\software\mysql-5.6.25-winx64\bin>mysqladmin -u root password "123456";
```

### 登录

```sql
D:\software\mysql-5.6.25-winx64\bin> mysql -u root -p
Enter password: ******
```

### 关闭服务

```sql
D:\software\mysql-5.6.25-winx64\bin>mysqladmin -u root -p shutdown
Enter password: ******
```

### 验证版本

```
D:\software\mysql-5.6.25-winx64\bin> mysqladmin --version
mysqladmin  Ver 8.42 Distrib 5.7.17, for Win64 on x86_64
```

## 管理操作

### 设置MySQL用户账户

要添加一个新用户到MySQL,只需要在mysql.user表中添加新纪录即可。

```sql
mysql> use mysql;
Database changed

mysql> INSERT INTO user 
          (host, user, password, 
           select_priv, insert_priv, update_priv) 
           VALUES ('localhost', 'yiibai', 
           PASSWORD('123456'), 'Y', 'Y', 'Y');
Query OK, 1 row affected (0.20 sec)

mysql> FLUSH PRIVILEGES;
Query OK, 1 row affected (0.01 sec)

mysql> SELECT host, user, password FROM user WHERE user = 'yiibai';
+-----------+---------+------------------+
| host      | user    | password         |
+-----------+---------+------------------+
| localhost | yiibai | *59A8740AAC5DBCB2907F38891BE42957F699CB77 |
+-----------+---------+------------------+
1 row in set (0.00 sec)

```

- PASSWORD()是加密函数
- FLUSH PRIVILEGES为重新加载授权表。
- 可以通过在INSERT新用户时以下几列的值为Y，指定给用户相应权限，也可以使用UPDATE更新它们。
    - Select_priv
    - Insert_priv
    - Update_priv
    - Delete_priv
    - Create_priv
    - Drop_priv
    - Reload_priv
    - Shutdown_priv
    - Process_priv
    - File_priv
    - Grant_priv
    - References_priv
    - Index_priv
    - Alter_priv

### 创建数据库

创建一个新的数据库tutorials

```sql
mysql>create database tutorials default character set utf8 collate utf8_general_ci;
```

也可以在DOS下直接使用mysqladmin进行操作

```
C:\Program Files\MySQL\MySQL Server 5.7\bin>mysqladmin -u root -p create tutorials1
Enter password: ******
```

### 删除数据库

删除数据库同理

```sql
D:\software\mysql-5.6.25-winx64\bin> mysqladmin -u root -p drop yiibai_tutorials1
Enter password:******

mysql> drop database yiibai_tutorials1;

```

#### 另一种创建用户的方式 GRANT

```sql

mysql> use mysql;
Database changed

mysql> GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP
    -> ON tutorials.*
    -> TO 'yiibai'@'localhost'
    -> IDENTIFIED BY '123456';
```

这会创建一个新用户yiibai，并且只在数据库tutorials上有作用。

### 一些常用的查看操作

- USE Databasename : 用于选择在MySQL工作区指定的数据库。
- SHOW DATABASES: 列出了MySQL数据库管理系统中的所有可访问的数据库。
- SHOW TABLES: 显示已经选择数据库中的表的命令。
- SHOW COLUMNS FROM tablename: 显示属性，属性类型，关键信息，NULL是否被允许，默认值和其它的表信息。
- SHOW INDEX FROM tablename: 提供所有指标的详细信息表，其中包括PRIMARY KEY.
- SHOW TABLE STATUS LIKE tablename\G: 报告MySQL的数据库管理系统的性能和统计数据的详细信息。
- SHOW PROCESSLIST 查看当前进程

## 表字段类型

MySQL使用许多不同的数据类型，总体上分为三类，数字，日期，时间和字符串类型。

- 它用来表示数据值。
- 占用的空间以及值是固定长度还是可变长度。
- 数据类型的值可以被索引。
- MySQL如何比较特定数据类型的值。


- 数字
    - INT 无符号范围0-2^32。有符号范围-2^31 ~ 2^31。容量为4个字节
    - TINYINT 无符号范围0-2^8。有符号范围-2^7 ~ 2^7。容量为1个字节
    - SMALLINT 无符号范围0-2^16。有符号范围-2^15 ~ 2^15。容量为2个字节
    - MEDIUM 无符号范围0-2^24。有符号范围-2^23 ~ 2^23。容量为3个字节
    - BIGINT 无符号范围0-2^64。有符号范围-2^63 ~ 2^63。容量为8个字节
    - FLOAT(M,D) 有符号浮点型，M是数字总数，D是小数位数，小数精度可达24个浮点，MD默认为10，2，可定义。
    - DOUBLE(M,D) 有符号浮点型，M是数字总数，D是小数位数，小数精度可达53个浮点，MD默认为16，4，可定义。
    - DECIMAL(M,D) 有符号非压缩浮点数。每个小数耗费一个字节，M,D是必须指定的。
- 日期和时间
    - DATE 以YYYY-MM-DD格式的日期，在1000-01-01和9999-12-31之间。 例如，1973年12月30日将被存储为1973-12-30。
    - DATETIME - 日期和时间组合以YYYY-MM-DD HH:MM:SS格式，在1000-01-01 00:00:00 到9999-12-31 23:59:59之间。例如，1973年12月30日下午3:30，会被存储为1973-12-30 15:30:00。
    - TIMESTAMP 1970年1月1日午夜之间的时间戳，到2037的某个时候。这看起来像前面的DATETIME格式，无需只是数字之间的连字符; 1973年12月30日下午3点30分将被存储为19731230153000(YYYYMMDDHHMMSS)。
    - TIME 存储时间在HH:MM:SS格式。
    - YEAR(M) - 以2位或4位数字格式来存储年份。如果长度指定为2(例如YEAR(2))，年份就可以为1970至2069(70〜69)。如果长度指定为4，年份范围是1901-2155，默认长度为4。(可以明显发现，以year形式存储的时间，其实所占空间更小。)
- 字符串
    - CHAR(M) 固定长度字符串，M为长度，默认为1 ，不足者空格填充至指定长度
    - VARCHAR(M) 可变长度字符串，M为最大长度，必须定义。
    - BLOB or TEXT 字段最大长度65535(2^16)，BLOB是二进制大对象，用以存储大的二进制数据，如图像。BLOB大小写敏感，TEXT不敏感。
    - TINYBLOB or TINYTEXT 最大长度255个字符。
    - MEDIUMBLOB or MEDIUMTEXT 最大长度16777215字符。
    - ENUM 枚举，即多项选择，ENUM('A','B','C')表示只有这三个字符可以用来填充该字段。

## 查询表

在从表中查询数据时，可能会收到重复的行记录，为了删除这些重复行，可以在SELECT中使用DISTINCT子句

语法如下

```sql
SELECT DISTINCT
    columns
FROM
    table_name
WHERE
    where_conditions;
```

则在查询结果中，自然被消除。

如果列具有NULL值，则只保留一个NULL值，因为DISTINCT子句将所有NULL值视为相同的值。

DISTINCT支持多列查询，并且只删去联合重复。

### DISTINCE 和 GROUP BY比较

以下两种命令效果相同

```sql
mysql> SELECT DISTINCT state FROM customers;
mysql> SELECT  state FROM customers GROUP BY state;
```

DISTINCE 是 GROUP BY的特殊情况，之间的区别是GROUP BY子句可对结果集进行排序，而DISTINCT不进行排序

如果将ORDER BY添加到使用DISTINCT子句的语句中，则效果与GROUP BY子句效果一样

```sql

SELECT DISTINCT
    state
FROM
    customers
ORDER BY state;

```

### DISTINCT和聚合函数

可以在聚合函数(例如SUM AVG COUNT)中应用DISTINCT子句，求得相应值。如下要计算美国客户的state个数，可以使用以下查询

```sql
SELECT 
    COUNT(DISTINCT state)
FROM
    customers
WHERE
    country = 'USA';

mysql> SELECT  COUNT(DISTINCT state) FROM customers WHERE country = 'USA';
+-----------------------+
| COUNT(DISTINCT state) |
+-----------------------+
|                     8 |
+-----------------------+
1 row in set
```

### DISTINCT 与 LIMIT子句

以下查询customers表中的前3个非空(NOT NULL)唯一state列的值

```sql
mysql> SELECT DISTINCT state FROM customers WHERE state IS NOT NULL LIMIT 3;
+----------+
| state    |
+----------+
| NV       |
| Victoria |
| CA       |
+----------+
3 rows in set
```

## 创建、删除及插入表

```sql
通用语法
CREATE TABLE table_name (column_name column_type);

create table tutorials_tbl(
   tutorial_id INT NOT NULL AUTO_INCREMENT,
   tutorial_title VARCHAR(100) NOT NULL,
   tutorial_author VARCHAR(40) NOT NULL,
   submission_date DATE,
   PRIMARY KEY ( tutorial_id )
);

```

- 字段使用NOT NULL属性，是因为我们不希望这个字段的值为NULL。 因此，如果用户将尝试创建具有NULL值的记录，那么MySQL会产生错误。
- 字段的AUTO_INCREMENT属性告诉MySQL自动增加id字段下一个可用编号。
- 关键字PRIMARY KEY用于定义此列作为主键。可以使用逗号分隔多个列来定义主键。


删除表更加简单，和删除数据库类似。

```sql
mysql> DROP TABLE tutorials_tbl;
```

添加新记录方法如下


```sql
通用语法 
INSERT INTO table_name ( field1, field2,...fieldN )
                       VALUES
                       ( value1, value2,...valueN );

mysql> INSERT INTO tutorials_tbl
     ->(tutorial_title, tutorial_author, submission_date)
     ->VALUES
     ->("Learn MySQL", "Saya", NOW());
```

在上面的例子中，未提供 tutorial_id 对应的值，因为在创建表时它会自动创建，这个字段我们给了AUTO_INCREMENT选项。因此MySQL会自动分配插入ID的值。 这里, NOW() 是MySQL函数，返回当前的日期和时间。

```sql
通用语法
SELECT field1, field2,...fieldN table_name1, table_name2...
[WHERE Clause]
[OFFSET M ][LIMIT N]

```

- 可以使用分隔的一个或多个逗号从多个表，以及使用WHERE子句包括各种条件，但WHERE子句是SELECT命令的可选部分
- 可以在一个SELECT命令指定读取一个或多个字段
- 可以指定星号(*)代替选择的字段。在这种情况下，将返回所有字段
- 可以指定任意的条件在 WHERE 子句后面
- 可以使用OFFSET指定一个偏移量，SELECT从那里开始返回记录。默认情况下 offset 的值是 0
- 可以使用LIMIT属性限制返回的数量

## INSERT

**插入多行**

```sql
INSERT INTO table(column1,column2...)
VALUES (value1,value2,...),
       (value1,value2,...),
...;

```

为表中的所有列指定相应列的值，则可以忽略INSERT语句中的列列表

```sql
INSERT INTO table
VALUES (value1,value2,...);

```

### INSERT ... SELECT

使用该方法，以从某个表中完全或部分复制内容。


```sql

CREATE TABLE table_1 LIKE table_2;

INSERT INTO table_1
SELECT c1, c2, FROM table_2;

```

### INSERT与ON DUPLICATE KEY UPDATE

如果在INSERT语句中指定ON DUPLICATE KEY UPDATE选项，MySQL将插入新行或使用新值更新原行记录。

下方语句将task_id = 4 的行更新为task_id = 5。subject = ...

该方法与UPDATE语句等效

```sql
INSERT INTO tasks(task_id,subject,start_date,end_date,description)
VALUES (4,'Test ON DUPLICATE KEY UPDATE','2017-01-01','2017-01-02','Next Priority')
ON DUPLICATE KEY UPDATE 
   task_id = task_id + 1, 
   subject = 'Test ON DUPLICATE KEY UPDATE';

UPDATE tasks 
SET 
    task_id = task_id + 1,
    subject = 'Test ON DUPLICATE KEY UPDATE'
WHERE
    task_id = 4;



```

### MySQL 扩展： IGNORE

在 INSERT 后加入 IGNORE 可以对报错进行保留，不立即终止命令执行

```sql
INSERT IGNORE INTO table(column_list)
VALUES( value_list),
      ( value_list),
      ...

```

## UPDATE

我们使用UPDATE语句来更新表中的现有数据。也可以使用UPDATE语句来更改表中单个行，一组行或所有行的列值。

```sql
UPDATE [LOW_PRIORITY] [IGNORE] table_name 
SET 
    column_name1 = expr1,
    column_name2 = expr2,
    ...
WHERE
    condition;

```

>注意事项

- 首先，在UPDATE关键字后面指定要更新数据的表名。
- 其次，SET子句指定要修改的列和新值。要更新多个列，请使用以逗号分隔的列表。以字面值，表达式或子查询的形式在每列的赋值中来提供要设置的值。
- 第三，使用WHERE子句中的条件指定要更新的行。WHERE子句是可选的。 如果省略WHERE子句，则UPDATE语句将更新表中的所有行。
- LOW_PRIORITY修饰符指示UPDATE语句延迟更新，直到没有从表中读取数据的连接。 LOW_PRIORITY对仅使用表级锁定的存储引擎(例如MyISAM，MERGE，MEMORY)生效。

### **UPDATE 与 SELECT 结合**

在下方例子中，子查询首先从employeeNumber中随机找到一名销售经理，然后返回给母查询，最后将salesRepEmployeeNumber全部填充完毕，使得表customers不存在salesRepEmployeeNumber为空的情况。

```sql
UPDATE customers 
SET 
    salesRepEmployeeNumber = (SELECT 
            employeeNumber
        FROM
            employees
        WHERE
            jobtitle = 'Sales Rep'
        LIMIT 1)
WHERE
    salesRepEmployeeNumber IS NULL;

```

### UPDATE 跨表更新(INNER JOIN子句和LEFT JOIN子句)

语法格式

```sql
UPDATE T1, T2,
[INNER JOIN | LEFT JOIN] T1 ON T1.C1 = T2. C1
SET T1.C2 = T2.C2, 
    T2.C3 = expr
WHERE condition

```

在这个UPDATE语句与 **具有隐式INNER JOIN子句的UPDATE JOIN**工作相同。

```sql

UPDATE T1, T2
SET T1.c2 = T2.c2,
      T2.c3 = expr
WHERE T1.c1 = T2.c1 AND condition

UPDATE T1,T2
INNER JOIN T2 ON T1.C1 = T2.C1
SET T1.C2 = T2.C2,
      T2.C3 = expr
WHERE condition
```

## DELETE

DELETE是删除表中观测值的命令，它可以删除全部观测值也可以删除单个观测值，

ORDER BY、LIMIT、WHERE等子句均可使用。


```sql
DELETE FROM table_name
ORDER BY c1, c2, ...
LIMIT row_count;

```

### DELETE 与 INNER JOIN 

```sql
DELETE T1, T2
FROM T1
INNER JOIN T2 ON T1.key = T2.key
WHERE condition

```

请注意，将T1和T2表放在DELETE和FROM关键字之间。如果省略T1表，DELETE语句仅删除T2表中的行记录。 同样，如果省略了T2表，DELETE语句将只删除T1表中的行记录。

表达式T1.key = T2.key指定了将被删除的T1和T2表之间的匹配行记录的条件。

WHERE子句中的条件确定T1和T2表中要被删除的行记录。

```sql
USE testdb;

DROP TABLE IF EXISTS t1, t2;

CREATE TABLE t1 (
    id INT PRIMARY KEY AUTO_INCREMENT
);

CREATE TABLE t2 (
    id VARCHAR(20) PRIMARY KEY,
 ref INT NOT NULL
);

INSERT INTO t1 VALUES (1),(2),(3);

INSERT INTO t2(ref) VALUES('A',1),('B',2),('C',3);

#以下语句删除t1表中id=1的行，并使用DELETE ... INNER JOIN语句删除t2表中的ref=1的行记录：

DELETE t1 , t2 FROM t1
        INNER JOIN
    t2 ON t2.ref = t1.id 
WHERE
    t1.id = 1;


```

### DELETE 与 LEFT JOIN

以下语法说明如何使用DELETE语句与LEFT JOIN子句来删除与T2表中没有相应匹配行的表T1中的行记录：

```sql
DELETE T1 
FROM T1
        LEFT JOIN
    T2 ON T1.key = T2.key 
WHERE
    T2.key IS NULL;

```

## REPLACE 

MySQL REPLACE语句是标准SQL的MySQL扩展。 MySQL REPLACE语句的工作原理如下：

- 如果给定行数据不存在，那么MySQL REPLACE语句会插入一个新行。
- 如果给定行数据存在，则REPLACE语句首先删除旧行，然后插入一个新行。 在某些情况下，REPLACE语句仅更新现有行。
- MySQL使用PRIMARY KEY或UNIQUE KEY索引来要确定表中是否存在新行。如果表没有这些索引，则REPLACE语句等同于INSERT语句。

### REPLACE第一种形式

REPLACE语句的第一种形式类似于INSERT语句，除了INSERT关键字换成REPLACE关键字以外，如下所示：

```sql
REPLACE INTO table_name(column_list)
VALUES(value_list);

```

### REPLACE第二种形式

REPLACE语句的第二种形式类似于UPDATE语句，如下所示：

```sql
REPLACE INTO table
SET column1 = value1,
    column2 = value2;
```

与UPDATE语句不同，如果不在SET子句中指定列的值，则REPLACE语句将使用该列的默认值。

### REPLACE第三种形式

REPLACE语句的第三种形式类似于INSERT INTO SELECT语句：

```sql
REPLACE INTO table_1(column_list)
SELECT column_list
FROM table_2
WHERE where_condition;
```

## PREPARED

prepare语句利用客户端/服务器二进制协议。 它将包含占位符(?)的查询传递给MySQL服务器，

当MySQL使用不同的productcode值执行此查询时，不必完全解析查询。 因此，这有助于MySQL更快地执行查询，特别是当MySQL多次执行查询时。

```sql
PREPARE stmt1 FROM 'SELECT productCode, productName
                    FROM products
                    WHERE productCode = ?';

SET @pc = 'S10_1678';
EXECUTE stmt1 USING @pc;

DEALLOCATE PREPARE stmt1;
```

首先，使用PREPARE语句准备执行语句。我们使用SELECT语句根据指定的产品代码从products表查询产品数据。然后再使用问号(?)作为产品代码的占位符。

接下来，声明了一个产品代码变量@pc，并将其值设置为S10_1678。

然后，使用EXECUTE语句来执行产品代码变量@pc的准备语句。

最后，我们使用DEALLOCATE PREPARE来发布PREPARE语句。

## WHERE及子操作符IN BETWEEN LIKE 

```sql
SELECT field1, field2,...fieldN table_name1, table_name2...
[WHERE condition1 [AND [OR]] condition2.....
```

- WHERE可以与DELETE 或 UPDATE一起使用，也可以指定一个条件使用。
- 可以指定一个以上的条件在使用AND或OR运算符。
- BINARY关键词使搜索区分大小写(默认是不区分的)
    - 如`WHERE BINARY tutorial_author='yiibai'`

下表列出了可用于在WHERE子句中形成过滤表达式的比较运算符

|操作符|描述|
|---|---|
|`=`|几乎任何数据类型都适用|
|`<>`或`!=`|不等于|
|`<`|通常适用数字和时间数据类型|
|`>`|同上|
|`<=`|同上|
|`>=`|同上|

### AND OR

当您在语句中使用多个逻辑运算符时，MySQL会在AND运算符之后再对OR运算符进行求值。 这称为运算符优先级。要更改优先级顺序，请使用括号

```sql
SELECT (true OR false) AND false;

```

### IN

IN运算符确定指定的值是否在列表中，有则返回1，没有则返回0

语法如下

```sql
SELECT 
    column1,column2,...
FROM
    table_name
WHERE 
 (expr|column_1) IN ('value1','value2',...);

```

有说明如下

- IN操作符也可以用在其他语句(如INSERT，UPDATE，DELETE等)的WHERE子句中。
- 列表中的值以逗号分割

例子

```sql
SELECT 
    officeCode, city, phone, country
FROM
    offices
WHERE
    country IN ('USA' , 'France');

```

如果列表中有很多值，使用多个OR运算符则会构造一个非常长的语句。 因此，使用IN运算符则会缩短查询并使查询更易读。

IN 同样可以和 NOT 连用

```sql
SELECT 
    officeCode, city, phone
FROM
    offices
WHERE
    country NOT IN ('USA' , 'France');

```

#### IN 与子查询

IN运算符通常用于子查询，子查询不提供常量值列表，而是提供值列表，即把所有的值展示出来，不手打输入。

例如，如果要查找总金额大于60000的订单，则使用IN运算符查询如下所示：


```sql
SELECT 
    orderNumber, customerNumber, status, shippedDate
FROM
    orders
WHERE
    orderNumber IN (SELECT 
            orderNumber
        FROM
            orderDetails
        GROUP BY orderNumber
        HAVING SUM(quantityOrdered * priceEach) > 60000);

```

### BETWEEN 运算符

BETWEEN运算符允许指定要测试的值范围。 我们经常在SELECT，INSERT，UPDATE和DELETE语句的WHERE子句中使用BETWEEN运算符。

下面说明了BETWEEN运算符的语法，一般而言，BETWEEN也可以被过滤运算符和AND OR重写：

`expr [NOT] BETWEEN begin_expr AND end_expr;`


```sql
mysql> SELECT 
    productCode, productName, buyPrice
FROM
    products
WHERE
    buyPrice NOT BETWEEN 20 AND 100;
+-------------+-------------------------------------+----------+
| productCode | productName                         | buyPrice |
+-------------+-------------------------------------+----------+
| S10_4962    | 1962 LanciaA Delta 16V              | 103.42   |
| S18_2238    | 1998 Chrysler Plymouth Prowler      | 101.51   |
| S24_2840    | 1958 Chevy Corvette Limited Edition | 15.91    |
| S24_2972    | 1982 Lamborghini Diablo             | 16.24    |
+-------------+-------------------------------------+----------+
4 rows in set

SELECT 
    productCode, productName, buyPrice
FROM
    products
WHERE
    buyPrice < 20 OR buyPrice > 100;


```

#### BETWEEN 和 日期类型数据

```sql
SELECT orderNumber,
         requiredDate,
         status
FROM orders
WHERE requireddate
    BETWEEN CAST('2013-01-01' AS DATE)
        AND CAST('2013-01-31' AS DATE);

```

因为requiredDate列的数据类型是DATE，所以我们使用转换运算符将文字字符串“2013-01-01”和“2013-12-31”转换为DATE数据类型。

### LIKE

LIKE操作符通常用于基于模式查询选择数据。MySQL提供两个通配符，用于与LIKE运算符一起使用，它们分别是：百分比符号 - %和下划线 - _。

- 百分比(%)通配符允许匹配任何字符串的零个或多个字符。
- 下划线(_)通配符允许匹配任何单个字符。
- LIKE支持 NOT 组合
- 大小写不敏感



```sql
SELECT field1, field2,...fieldN table_name1, table_name2...
WHERE field1 LIKE condition1 [AND [OR]] filed2 = 'somevalue'

mysql> SELECT * from tutorials_tbl WHERE tutorial_author LIKE '%aul';
+-------------+----------------+-----------------+-----------------+
| tutorial_id | tutorial_title | tutorial_author | submission_date |
+-------------+----------------+-----------------+-----------------+
|           3 | JAVA Tutorial  | Paul            | 2015-07-17      |
+-------------+----------------+-----------------+-----------------+
1 rows in set (0.01 sec)
```


- 可以使用LIKE子句在WHERE子句中
- 可以使用LIKE子句代替等号(=)
- 当LIKE连同%符号使用，那么它就会像一个元字符的搜索
- 可以指定一个以上的条件使用AND或OR运算符
- WHERE... LIKE子句可以使用SQL命令的DELETE 或 UPDATE ，也可以指定一个条件

#### LIKE 与 ESCAPE 子句

如果需要匹配的文本包含 通配符，则可以

- 使用ESCAPE 子句指定转义字符
- 使用默认转义字符：反斜杠\

下方两种方法效果一样。

```sql
SELECT 
    productCode, productName
FROM
    products
WHERE
    productCode LIKE '%$_20%' ESCAPE '$';


SELECT 
    productCode, productName
FROM
    products
WHERE
    productCode LIKE '%\_20%';


```

LIKE操作符强制MySQL扫描整个表以找到匹配的行记录，因此，它不允许数据库引擎使用索引进行快速搜索。因此，当要从具有大量行的表查询数据时，使用LIKE运算符来查询数据的性能会大幅降低。

### LIMIT 

LIMIT子句用来约束结果集中的行数。LIMIT子句接受一个或两个参数。两个参数的值必须为零或正整数。

语法如下

```sql

SELECT 
    column1,column2,...
FROM
    table
LIMIT offset , count;

---------

SELECT 
    column1,column2,...
FROM
    table
LIMIT count;

```

- offset参数指定要返回的第一行的偏移量。第一行的偏移量为0，而不是1。
- count指定要返回的最大行数。

#### LIMIT 与 ORDER BY 混用

要查询信用额度最高的前五名客户，请使用以下查询：

```sql
SELECT customernumber, customername, creditlimit
FROM customers
ORDER BY creditlimit DESC
LIMIT 5;

```


获得结果集中的第n个最高值

```sql
SELECT 
    column1, column2,...
FROM
    table
ORDER BY column1 DESC
LIMIT nth-1, count;

```

## ORDER BY

ORDER BY子句允许：

- 对单个列或多个列排序结果集。
- 按升序或降序对不同列的结果集进行排序。

```sql
SELECT column1, column2,...
FROM tbl
ORDER BY column1 [ASC|DESC], column2 [ASC|DESC],...
```

ASC表示升序，DESC表示降序。默认情况下，如果不明确指定ASC或DESC，ORDER BY子句会按照升序对结果集进行排序。

#### ORDER BY 允许对结果集根据表达式进行排序

如我们计算表orderdetails中quantityOrdered * priceEach的数值并根据它进行排序

```sql
SELECT
 ordernumber,
 orderlinenumber,
 quantityOrdered * priceEach AS subtotal
FROM
 orderdetails
ORDER BY
 ordernumber,
 orderLineNumber,
 subtotal;

```

#### ORDER BY 与自定义排序

ORDER BY子句允许使用FIELD()函数为列中的值定义自己的自定义排序顺序。

```sql
mysql> SELECT 
    orderNumber, status
FROM
    orders
ORDER BY FIELD(status,
        'In Process',
        'On Hold',
        'Cancelled',
        'Resolved',
        'Disputed',
        'Shipped');
+-------------+------------+
| orderNumber | status     |
+-------------+------------+
|       10420 | In Process |
|       10421 | In Process |
... ...
|       10413 | Shipped    |
|       10416 | Shipped    |
|       10418 | Shipped    |
|       10419 | Shipped    |
+-------------+------------+
326 rows in set
```

#### ORDER BY 与自然排序

考虑如下一个表items

```sql
CREATE TABLE IF NOT EXISTS items (
    id INT AUTO_INCREMENT PRIMARY KEY,
    item_no VARCHAR(255) NOT NULL
);

INSERT INTO items(item_no)
VALUES ('1'),
       ('1C'),
       ('10Z'),
       ('2A'),
       ('2'),
       ('3C'),
       ('20D');

SELECT 
    item_no
FROM
    items
ORDER BY item_no;

+---------+
| item_no |
+---------+
| 1       |
| 10Z     |
| 1C      |
| 2       |
| 20D     |
| 2A      |
| 3C      |
+---------+
7 rows in set


```

但是我们需要的排序是

```
+---------+
| item_no |
+---------+
| 1       |
| 1C      |
| 2       |
| 2A      |
| 3C      |
| 10Z     |
| 20D     |
+---------+

```

为了达到这个效果，我们考虑将item_no分成两列，prefix和suffix，再对两列进行排序以解决问题

当然如果数据格式相当标准，我们可以使用以下方法进行排序，而无需增加列

```sql
SELECT 
    item_no
FROM
    items
ORDER BY CAST(item_no AS UNSIGNED) , item_no;

```

首先使用类型转换将item_no数据转换为无符号整数。 其次，使用ORDER BY子句对数字进行数字排序，然后按字母顺序排列。

还有一个常见的问题如下

```sql
TRUNCATE TABLE items;

INSERT INTO items(item_no)
VALUES('A-1'),
      ('A-2'),
      ('A-3'),
      ('A-4'),
      ('A-5'),
      ('A-10'),
      ('A-11'),
      ('A-20'),
      ('A-30');

SELECT 
    item_no
FROM
    items
ORDER BY LENGTH(item_no) , item_no;


```

该方法首先对字符串长度进行排序，以保证个位数的数字在前，然后再对其进行排序

## UPDATE语法

```sql
UPDATE table_name SET field1=new-value1, field2=new-value2
[WHERE Clause]

mysql> UPDATE tutorials_tbl 
    -> SET tutorial_title='Learning JAVA' 
    -> WHERE tutorial_id=3;
Query OK, 1 row affected (0.04 sec)
Rows matched: 1  Changed: 1  Warnings: 0

```


## DELETE语法


```sql
DELETE FROM table_name [WHERE Clause]

mysql> DELETE FROM tutorials_tbl WHERE tutorial_id=3;
Query OK, 1 row affected (0.23 sec)
```

- 如果WHERE子句没有指定，则所有MySQL表中的记录将均被删除。
- 删除的记录是具体哪些依据WHERE子句确定

## ORDER BY将查询结果排序

当选择数据行，MySQL服务器可以自由地返回它们的顺序，除非有指示它按照怎样的结果进行排序。但是排序结果可以通过增加一个ORDER BY子句设定列名称或要排序的列。

```sql
SELECT field1, field2,...fieldN table_name1, table_name2...
ORDER BY field1, [field2...] [ASC [DESC]]

mysql> SELECT * from tutorials_tbl ORDER BY tutorial_author ASC;
+-------------+----------------+-----------------+-----------------+
| tutorial_id | tutorial_title | tutorial_author | submission_date |
+-------------+----------------+-----------------+-----------------+
|           1 | Learn PHP      | Paul            | 2015-07-17      |
|           2 | Learn MySQL    | Saya            | 2015-07-17      |
|           3 | Learning JAVA  | Yiibai          | 2007-05-06      |
+-------------+----------------+-----------------+-----------------+
3 rows in set
mysql> 

```

- 可以使用WHERE ... LIKE子句以通用的方式放置条件
- 使用关键字 ASC 或 DESC 来执行升序或降序排列。

## Join联接

```sql
mysql> SELECT * FROM tcount_tbl;
+-----------------+----------------+
| tutorial_author | tutorial_count |
+-----------------+----------------+
| mahran          |             10 |
| mahnaz          |              0 |
| Jen             |              0 |
| Gill            |             20 |
| Paul            |             10 |
| yiibai          |             10 |
+-----------------+----------------+
6 rows in set (0.01 sec)
mysql> SELECT * from tutorials_tbl;
+-------------+----------------+-----------------+-----------------+
| tutorial_id | tutorial_title | tutorial_author | submission_date |
+-------------+----------------+-----------------+-----------------+
|           1 | Learn PHP      | Paul            | 2015-07-17      |
|           2 | Learn MySQL    | Saya            | 2015-07-17      |
|           3 | Learning JAVA  | yiibai          | 2007-05-06      |
+-------------+----------------+-----------------+-----------------+
3 rows in set (0.00 sec)

#现在我们可以写一个SQL查询来连接这两个表。此查询将从表tutorials_tbl和tcount_tbl 选择所有作者的教程数量。
mysql> SELECT a.tutorial_id, a.tutorial_author, b.tutorial_count
    -> FROM tutorials_tbl a, tcount_tbl b
    -> WHERE a.tutorial_author = b.tutorial_author;
+-------------+-----------------+----------------+
| tutorial_id | tutorial_author | tutorial_count |
+-------------+-----------------+----------------+
|           1 | Paul            |             10 |
|           3 | Yiibai          |             10 |
+-------------+-----------------+----------------+
2 rows in set (0.01 sec)


```

### 如何处理NULL值查询？

 涉及NULL的条件是特殊的。不能使用= NULL或！= NULL来匹配查找列的NULL值。这样的比较总是失败，因为它是不可能告诉它们是否是true。 甚至 NULL = NULL 也是失败的。

要查找列的值是或不是NULL，使用IS NULL或IS NOT NULL。

MySQL提供了三大运算符

- IS NULL: 如果列的值为NULL，运算结果返回 true
- IS NOT NULL: 如果列的值不为NULL，运算结果返回 true
- <=>: 运算符比较值，(不同于=运算符)即使两个空值它返回 true

```sql
mysql> SELECT * from tcount_tbl 
    -> WHERE tutorial_count IS NOT NULL;
+-----------------+----------------+
| tutorial_author | tutorial_count |
+-----------------+----------------+
| mahran          |             20 |
| Gill            |             20 |
+-----------------+----------------+
2 rows in set (0.00 sec)
```

## MySQL正则表达式



```sql
通用语法
mysql> SELECT name FROM person_tbl WHERE name REGEXP 'ok$';
```

## MySQL连接表

### 列别名

有时，列的名称是一些表达式，使查询的输出很难理解。要给列一个描述性名称，可以使用列别名。

语法如下

```sql
SELECT 
 [column_1 | expression] AS `descriptive name`
FROM table_name;

# 例子

mysql> SELECT 
    CONCAT_WS(', ', lastName, firstname)
FROM
    employees;
+--------------------------------------+
| CONCAT_WS(', ', lastName, firstname) |
+--------------------------------------+
| Murphy, Diane                        |
....
+--------------------------------------+
23 rows in set

# 上方的变量名非常难以阅读，此时可以引入列别名

mysql> SELECT
 CONCAT_WS(', ', lastName, firstname) AS `Full name`
FROM
 employees;


```

\`\`是为了避免名称中含有空格

MySQL中，可以使用ORDER BY，GROUP BY和HAVING子句中的列别名来引用该列。

以下查询使用ORDER BY子句中的列别名按字母顺序排列员工的全名：

```sql
SELECT
 CONCAT_WS(' ', lastName, firstname) `Full name`
FROM
 employees
ORDER BY
 `Full name`;

```

以下语句查询总金额大于60000的订单。它在GROUP BY和HAVING子句中使用列别名。

```sql
SELECT
 orderNumber `Order no.`,
 SUM(priceEach * quantityOrdered) total
FROM
 orderdetails
GROUP BY
 `Order no.`
HAVING
 total > 60000;

```

>请注意，不能在WHERE子句中使用列别名。原因是当MySQL评估求值WHERE子句时，SELECT子句中指定的列的值可能尚未确定。

### 表别名

一般在包含INNER JOIN，LEFT JOIN，self join子句和子查询的语句中使用表别名。

因为有的时候，我们使用INNER JOIN连接表时，两个表的列名是一样的，则此时应该使用别名以示区分，书写方便。

`table_name AS table_alias`

```sql
SELECT
 customerName,
 COUNT(o.orderNumber) total
FROM
 customers c
INNER JOIN orders o ON c.customerNumber = o.customerNumber
GROUP BY
 customerName
HAVING total >=5
ORDER BY
 total DESC;

```

### 内连接 INNER JOIN

内连接用于连接主表与副表，产生新的行记录，对于t1表中的每一行，INNER JOIN子句将它与t2表的每一行进行比较，以检查它们是否都满足连接条件。当满足连接条件时，INNER JOIN将返回由t1和t2表中的列组成的新行。

```sql
SELECT column_list
FROM t1
INNER JOIN t2 ON join_condition1
INNER JOIN t3 ON join_condition2
...
WHERE where_conditions;


简化语法如下
SELECT column_list
FROM t1
INNER JOIN t2 ON join_condition;


```

通过使用INNER JOIN子句根据productline列匹配行来从两个表中查询选择数据，如下所示：

```sql
SELECT 
    productCode, 
    productName, 
    textDescription
FROM
    products t1
        INNER JOIN
    productlines t2 ON t1.productline = t2.productline;

```

由于两个表的连接列是使用相同一个列：productline，因此可以使用以下语法：

```sql

SELECT 
    productCode, 
    productName, 
    textDescription
FROM
    products
        INNER JOIN
    productlines USING (productline);

```

可以使用具有GROUP BY子句的INNER JOIN子句从orders和orderdetails表中获取订单号，订单状态和总销售额，如下所示：

```sql
SELECT 
    T1.orderNumber,
    status,
    SUM(quantityOrdered * priceEach) total
FROM
    orders AS T1
        INNER JOIN
    orderdetails AS T2 ON T1.orderNumber = T2.orderNumber
GROUP BY orderNumber;

```

### 左连接

左连接允许从多个表中联立查询数据，且语法类似于内连接，如下


```sql
SELECT 
    t1.c1, t1.c2, t2.c1, t2.c2
FROM
    t1
        LEFT JOIN
    t2 ON t1.c1 = t2.c1;

```

其与内连接的区别是，左连接会包括主表的所有记录，如果在右表中未查询到信息，则以空信息代替。故名称是左连接

要查询每个客户的所有订单，可以使用LEFT JOIN子句，如下所示：


```sql

SELECT
 c.customerNumber,
 c.customerName,
 orderNumber,
 o.status
FROM
 customers c
LEFT JOIN orders o ON c.customerNumber = o.customerNumber;

```

疑问：为什么在LEFT JOIN中的ON条件与WHERE子句不能同等替代，而对于INNER JOIN子句，ON子句中的条件等同于WHERE子句中的条件。

### CROSS JOIN

## 分组数据

### GROUP BY 

GROUP BY子句通过列或表达式的值将一组行分组为一个小分组的汇总行记录。 GROUP BY子句为每个分组返回一行。换句话说，它减少了结果集中的行数。

经常使用GROUP BY子句与聚合函数一起使用，如SUM，AVG，MAX，MIN和COUNT。SELECT子句中使用聚合函数来计算有关每个分组的信息。

假设要将订单状态的值分组到子组中，则要使用GROUP BY子句并指定按status 列来执行分组

```sql
SELECT 
    status
FROM
    orders
GROUP BY status;

+------------+
| status     |
+------------+
| Cancelled  |
| Disputed   |
| In Process |
| On Hold    |
| Resolved   |
| Shipped    |
+------------+

```

可以看到，GROUP BY子句返回状态(status)值是唯一的。它像DISTINCT运算符一样工作

### GROUP BY 与聚合函数

可使用聚合函数来执行一组行的计算并返回单个值。 GROUP BY子句通常与聚合函数一起使用以执行计算每个分组并返回单个值。

例如，如果想知道每个状态中的订单数，可以使用COUNT函数与GROUP BY子句查询语句，如下所示：

```sql

SELECT 
    status, COUNT(*) AS total_number
FROM
    orders
GROUP BY status;

+------------+--------------+
| status     | total_number |
+------------+--------------+
| Cancelled  |            6 |
| Disputed   |            3 |
| In Process |            6 |
| On Hold    |            4 |
| Resolved   |            4 |
| Shipped    |          303 |
+------------+--------------+
6 rows in set

```

也就是说，GROUP BY起到了分组处理的作用。分组计算得出聚合函数产生的结果。

```sql

desc orderdetails;
desc orders;

SELECT 
    status, SUM(quantityOrdered * priceEach) AS amount
FROM
    orders
        INNER JOIN
    orderdetails USING (orderNumber)
GROUP BY status;
```

再次提醒，INNER JOIN是从右表orderdetails中查询符合连接条件的观测值，再与左表每一条观测值合并。

### GROUP BY 用表达式示例

除了列之外，可以按表达式对行进行分组。以下查询获取每年的总销售额

```sql

SELECT 
    YEAR(orderDate) AS year,
    SUM(quantityOrdered * priceEach) AS total
FROM
    orders
        INNER JOIN
    orderdetails USING (orderNumber)
WHERE
    status = 'Shipped'
GROUP BY YEAR(orderDate);

+------+------------+
| year | total      |
+------+------------+
| 2013 | 3223095.80 |
| 2014 | 4300602.99 |
| 2015 | 1341395.85 |
+------+------------+

```

### GROUP BY 与 HAVING

可使用HAVING 子句过滤GROUP BY 子句返回的分组，如在上述基础上选择2013年以后的年销售总额

```sql

SELECT 
    YEAR(orderDate) AS year,
    SUM(quantityOrdered * priceEach) AS total
FROM
    orders
        INNER JOIN
    orderdetails USING (orderNumber)
WHERE
    status = 'Shipped'
GROUP BY year
HAVING year > 2013;

```

### GROUP BY 与标准SQL

标准SQL不允许使用GROUP BY子句中的别名，但MySQL支持此选项。以下查询从订单日期提取年份，并对每年的订单进行计数。该year用作表达式YEAR(orderDate)的别名，它也用作GROUP BY子句中的别名，此查询在标准SQL中无效。

```sql
SELECT 
    YEAR(orderDate) AS year, COUNT(orderNumber)
FROM
    orders
GROUP BY year;
```

MySQL还允许您以升序或降序(标准SQL不能提供)对组进行排序。默认顺序是升序。例如，如果要按状态获取订单数量并按降序对状态进行排序，则可以使用带有DESC的GROUP BY子句

```sql
SELECT 
    status, COUNT(*)
FROM
    orders
GROUP BY status DESC;

```

### HAVING

在SELECT语句中使用HAVING子句来指定一组行或聚合的过滤条件。

HAVING子句通常与GROUP BY子句一起使用，以根据指定的条件过滤分组。如果省略GROUP BY子句，则HAVING子句的行为与WHERE子句类似。

    请注意，HAVING子句将过滤条件应用于每组分行，而WHERE子句将过滤条件应用于每个单独的行。

HAVING 对组为单位进行过滤

```sql
SELECT 
    ordernumber,
    SUM(quantityOrdered) AS itemsCount,
    SUM(priceeach*quantityOrdered) AS total
FROM
    orderdetails
GROUP BY ordernumber
HAVING total > 50000 AND itemsCount > 600;

```

## MySQL子查询，派生表和通用表达式

### MySQL子查询

MySQL子查询是嵌套在另一个查询(如SELECT，INSERT，UPDATE或DELETE)中的查询。 另外，MySQL子查询可以嵌套在另一个子查询中。

MySQL子查询称为内部查询，而包含子查询的查询称为外部查询。 子查询可以在使用表达式的任何地方使用，并且必须在括号中关闭。


```sql

SELECT 
    lastName, firstName
FROM
    employees
WHERE
    officeCode IN (SELECT 
            officeCode
        FROM
            offices
        WHERE
            country = 'USA');

```

下方命令返回所有大于平均值的观测

```sql
SELECT 
    customerNumber, checkNumber, amount
FROM
    payments
WHERE
    amount > (SELECT 
            AVG(amount)
        FROM
            payments);

```


### FROM子句中的子查询

在FROM子句中使用子查询时，从子查询返回的结果集将用作临时表。 该表称为派生表或物化子查询。

以下子查询将查找订单表中的最大，最小和平均数：

```sql
SELECT 
    MAX(items), MIN(items), FLOOR(AVG(items))
FROM
    (SELECT 
        orderNumber, COUNT(orderNumber) AS items
    FROM
        orderdetails
    GROUP BY orderNumber) AS lineitems;

+------------+------------+-------------------+
| MAX(items) | MIN(items) | FLOOR(AVG(items)) |
+------------+------------+-------------------+
|         18 |          1 | 9                 |
+------------+------------+-------------------+
1 row in set

```

### MySQL相关子查询

与独立子查询不同，相关子查询是使用外部查询中的数据的子查询。 换句话说，相关的子查询取决于外部查询。 对外部查询中的每一行对相关子查询进行一次评估。
即对于独立子查询，其子查询部分可以独立出来，不受外部查询影响，而相关子查询则因为引入了外部条件而受外部影响。

在以下查询中，我们查询选择购买价格高于每个产品线中的产品的平均购买价格的产品。

```sql
SELECT 
    productname,
    buyprice
FROM
    products p1
WHERE
    buyprice > (SELECT 
            AVG(buyprice)
        FROM
            products
        WHERE
            productline = p1.productline);

```

### MySQL子查询与EXISTS和NOT EXISTS

当子查询与EXISTS或NOT EXISTS运算符一起使用时，子查询返回一个布尔值为TRUE或FALSE的值。

可以使用上面的查询作为相关子查询，通过使用EXISTS运算符来查找至少有一个总额大于60000的销售订单的客户信息：

```sql
SELECT 
    customerNumber, 
    customerName
FROM
    customers
WHERE
    EXISTS( SELECT 
            orderNumber, SUM(priceEach * quantityOrdered)
        FROM
            orderdetails
                INNER JOIN
            orders USING (orderNumber)
        WHERE
            customerNumber = customers.customerNumber
        GROUP BY orderNumber
        HAVING SUM(priceEach * quantityOrdered) > 60000);
```

### MySQL派生表

派生表是从SELECT语句返回的虚拟表。派生表类似于临时表，但是在SELECT语句中使用派生表比临时表简单得多，因为它不需要创建临时表的步骤。

派生表和子查询通常可互换使用。**当SELECT语句的FROM子句中使用独立子查询时，我们将其称为派生表。**

```sql
SELECT 
    column_list
FROM
    (SELECT 
        column_list
    FROM
        table_1) derived_table_name;
WHERE derived_table_name.c1 > 0;
```

### MySQL 公共表达式(CTE)

### INTERSECT

INTERSECT 是一个集合运算符，它只返回两个查询或更多查询的交集

>要将INTERSECT用于两个查询集，需要 1.列的顺序和数量必须相同 2.相应列的数据类型必须兼容或可转换。

```sql
(SELECT column_list 
FROM table_1)
INTERSECT
(SELECT column_list
FROM table_2);
```

>注意:SQL标准有三个集合运算符，包括UNION，INTERSECT和MINUS  分别是取并交减

MySQL不支持INTERSECT操作符。 但是我们可以模拟INTERSECT操作符。

```sql
SELECT DISTINCT 
   id 
FROM t1
   INNER JOIN t2 USING(id);



SELECT DISTINCT
    id
FROM
    t1
WHERE
    id IN (SELECT 
            id
        FROM
            t2);


```

## MySQL事务

事务是一组数据库指令所组成的一个处理单元，其中数据库的任意一条指令失败，则事务失败，事务是模块化的指令集合。用以完成某个特定任务。

事务具有以下四个标准属性，通常由首字母缩写ACID简称

- 原子性: 确保了工作单位中的所有操作都成功完成; 否则，事务被中止，在失败时会被回滚到事务操作以前的状态。
- 一致性：可确保数据库在正确的更改状态在一个成功提交事务。
- 隔离: 使事务相互独立地操作。
- 持久性: 确保了提交事务的结果或系统故障情况下仍然存在作用。

在MySQL中，事务以BEGIN WORK语句开始开始工作，并使用COMMIT或ROLLBACK语句结束。SQL命令在开始和结束语句之间构成大量事务。

- 当一个成功的事务完成后，COMMIT命令发出的变化对所有涉及的表将生效。
- 如果发生故障，ROLLBACK命令发出后，事务中引用的每个表将恢复到事务开始之前的状态。

当AUTOCOMMIT = 1(默认值)，则每一次SQL语句都被认为是一个完整的事务并提交。当AUTOCOMMIT设置为0，通过发出SET AUTOCOMMIT=0命令, 随后的一系列语句就像一个事务，直到一个明确的发出 COMMIT 语句，更改才被记录进数据库。

如果需要在MySQL中直接使用事务，则需要在lnnoDB类型的表上进行操作。创建lnnoDB类型的表如下

```sql
mysql> create table tcount_tbl
    -> (
    -> tutorial_author varchar(40) NOT NULL,
    -> tutorial_count  INT
    -> ) TYPE=InnoDB;
Query OK, 0 rows affected (0.05 sec)
```

以下是关于一个事务的例子

```sql
-- start a new transaction
start transaction;

-- get latest order number
select @orderNumber := max(orderNUmber) 
from orders;
-- set new order number
set @orderNumber = @orderNumber  + 1;

-- insert a new order for customer 145
insert into orders(orderNumber,
                   orderDate,
                   requiredDate,
                   shippedDate,
                   status,
                   customerNumber)
values(@orderNumber,
       now(),
       date_add(now(), INTERVAL 5 DAY),
       date_add(now(), INTERVAL 2 DAY),
       'In Process',
        145);
-- insert 2 order line items
insert into orderdetails(orderNumber,
                         productCode,
                         quantityOrdered,
                         priceEach,
                         orderLineNumber)
values(@orderNumber,'S18_1749', 30, '136', 1),
      (@orderNumber,'S18_2248', 50, '55.09', 2); 
-- commit changes    
commit;       

-- get the new inserted order
select * from orders a 
inner join orderdetails b on a.ordernumber = b.ordernumber
where a.ordernumber = @ordernumber;

```


表锁为WRITE具有以下功能：

- 只有拥有表锁定的会话才能从表读取和写入数据。
- 在释放WRITE锁之前，其他会话不能从表中读写。

## MySQL Alter增建和删除列


首先我们创建一个表testalter_tb1,其中有column i,c

```sql
mysql> create table testalter_tbl
    -> (
    -> i INT,
    -> c CHAR(1)
    -> );
Query OK, 0 rows affected (0.05 sec)
mysql> SHOW COLUMNS FROM testalter_tbl;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| i     | int(11) | YES  |     | NULL    |       |
| c     | char(1) | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
2 rows in set (0.00 sec)

# 删除列i

mysql> ALTER TABLE testalter_tbl  DROP i;

# 增建列i

mysql> ALTER TABLE testalter_tbl ADD i INT;

# 我们可以以FIRST参数或AFTER column_name 来指定新增加列的排序位置

ALTER TABLE testalter_tbl ADD i INT FIRST;
ALTER TABLE testalter_tbl ADD i INT AFTER c;

# 使用MODIFY参数或CHANGE子句用以改变列定义、
1.将c从CHAR(1)改为CHAR(10) 2.将i改名为j，类型为BIGINT。 3.更改某列类型，且默认值为100
mysql> ALTER TABLE testalter_tbl MODIFY c CHAR(10);

mysql> ALTER TABLE testalter_tbl CHANGE i j BIGINT;

mysql> ALTER TABLE testalter_tbl CHANGE j j INT NOT NULL DEFAULT 100;

```

我们也可以使用ALTER命令更改任何列的默认值

```sql
mysql> ALTER TABLE testalter_tbl ALTER i SET DEFAULT 1000;
mysql> SHOW COLUMNS FROM testalter_tbl;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| c     | char(1) | YES  |     | NULL    |       |
| i     | int(11) | YES  |     | 1000    |       |
+-------+---------+------+-----+---------+-------+
2 rows in set (0.00 sec)

mysql> ALTER TABLE testalter_tbl ALTER i DROP DEFAULT;
mysql> SHOW COLUMNS FROM testalter_tbl;
+-------+---------+------+-----+---------+-------+
| Field | Type    | Null | Key | Default | Extra |
+-------+---------+------+-----+---------+-------+
| c     | char(1) | YES  |     | NULL    |       |
| i     | int(11) | YES  |     | NULL    |       |
+-------+---------+------+-----+---------+-------+
2 rows in set (0.00 sec)
```

ALTER也可以修改表的类型

```sql
mysql> ALTER TABLE testalter_tbl TYPE = MYISAM;
mysql>  SHOW TABLE STATUS LIKE 'testalter_tbl'\G
*************************** 1. row ****************
           Name: testalter_tbl
           Type: MyISAM
           ...
1 row in set (0.00 sec)
```

ALTER重命名表

```sql
mysql> ALTER TABLE testalter_tbl RENAME TO alter_tbl;
```

## MySQL索引

要创建索引，可以使用CREATE INDEX语句。 下面说明了CREATE INDEX语句的语法：

```sql

CREATE [UNIQUE|FULLTEXT|SPATIAL] INDEX index_name
USING [BTREE | HASH | RTREE] 
ON table_name (column_name [(length)] [ASC | DESC],...)

CREATE INDEX index_officeCode ON employees(officeCode)

DROP INDEX index_name ON table_name
```


- 对于UNIQUE索引，MySQL创建一个约束，索引中的所有值都必须是唯一的。除了BDB之外，所有存储引擎都允许重复的NULL值。
- FULLTEXT索引仅由MyISAM存储引擎支持，仅在数据类型为CHAR，VARCHAR或TEXT的列中接受。
- SPATIAL索引支持空间列，可用于MyISAM存储引擎。另外，列值不能为NULL。

## MySQL 约束

### NOT NULL约束

NOT NULL约束是一个列约束，仅将列的值强制为非NULL值。

NOT NULL约束的语法如下：

```sql

column_name data_type NOT NULL;
```

**创建表时指定NOT NULL**

```sql
USE testdb;
CREATE TABLE tasks (
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE
);
```

**向现有列添加NOT NULL约束**


- 检查列的当前值。
- 将所有NULL值更新为非NULL值。
- 添加NOT NULL约束

```sql
UPDATE tasks 
SET 
    end_date = start_date + 7
WHERE
    end_date IS NULL;

ALTER TABLE table_name
CHANGE old_column_name new_column_name new_column_definition;


```


## MySQL 临时表

临时表功能在MySQL 3.23版本以后才有
临时表特点：当前客户端会话结束时，它们将会被删除。

下面的例子中，首先创建一个临时表，然后插入一个数据。


```sql
mysql> CREATE TEMPORARY TABLE SalesSummary (
    -> product_name VARCHAR(50) NOT NULL
    -> , total_sales DECIMAL(12,2) NOT NULL DEFAULT 0.00
    -> , avg_unit_price DECIMAL(7,2) NOT NULL DEFAULT 0.00
    -> , total_units_sold INT UNSIGNED NOT NULL DEFAULT 0
);
Query OK, 0 rows affected (0.00 sec)

mysql> INSERT INTO SalesSummary
    -> (product_name, total_sales, avg_unit_price, total_units_sold)
    -> VALUES
    -> ('cucumber', 100.25, 90, 2);

mysql> SELECT * FROM SalesSummary;
+--------------+-------------+----------------+------------------+
| product_name | total_sales | avg_unit_price | total_units_sold |
+--------------+-------------+----------------+------------------+
| cucumber     |      100.25 |          90.00 |                2 |
+--------------+-------------+----------------+------------------+
1 row in set (0.00 sec)
```


当发出SHOW TABLES命令，临时表不会被列在表的列表中。现在，如果注销MySQL会话，然后发出SELECT命令，那么会发现在数据库中没有可用的数据。即使是临时表也不存在了。

删除临时表与正常表同理，没有区别。

## MySQL复制表

当需要一个表的精确副本时，可以通过以下步骤处理

- 使用SHOW CREATE TABLE以获得CREATE TABLE语句用于指定源表的结构，索引和其它事项
- 修改语句用来更改表名，以克隆出一个表的框架
- 如果需要复制表的内容，再发出一个INSERT INTO ... SELECT

```sql
#1.获取有关表的完整结构
mysql> SHOW CREATE TABLE tutorials_tbl \G;
*************************** 1. row ***************************
       Table: tutorials_tbl
Create Table: CREATE TABLE `tutorials_tbl` (
  `tutorial_id` int(11) NOT NULL auto_increment,
  `tutorial_title` varchar(100) NOT NULL default '',
  `tutorial_author` varchar(40) NOT NULL default '',
  `submission_date` date default NULL,
  PRIMARY KEY  (`tutorial_id`),
  UNIQUE KEY `AUTHOR_INDEX` (`tutorial_author`)
) TYPE=MyISAM
1 row in set (0.00 sec)

#2.重命名该表，并创建另一个表
mysql> CREATE TABLE `clone_tbl` (
  -> `tutorial_id` int(11) NOT NULL auto_increment,
  -> `tutorial_title` varchar(100) NOT NULL default '',
  -> `tutorial_author` varchar(40) NOT NULL default '',
  -> `submission_date` date default NULL,
  -> PRIMARY KEY  (`tutorial_id`),
  -> UNIQUE KEY `AUTHOR_INDEX` (`tutorial_author`)
-> ) TYPE=MyISAM;
Query OK, 0 rows affected (1.80 sec)

#3.使用INSERT INTO... SELECT将旧数据转入新数据
mysql> INSERT INTO clone_tbl (tutorial_id,
    ->                        tutorial_title,
    ->                        tutorial_author,
    ->                        submission_date)
    -> SELECT tutorial_id,tutorial_title,
    ->        tutorial_author,submission_date,
    -> FROM tutorials_tbl;
Query OK, 3 rows affected (0.07 sec)
Records: 3  Duplicates: 0  Warnings: 0
```

## MySQL数据库信息

 有三个信息，经常要从MySQL获取。

- 有关查询结果的信息: 这包括任何SELECT，UPDATE或DELETE语句所影响的记录数量。
- 有关表和数据库的信息: 这包括关于表和数据库的结构的信息。
- 关于MySQL服务器的信息: 这包括数据库服务器的当前状态，版本号等。

## MySQL重复处理

1. 防止在一个表发生重复记录

>方式：指定一个主键PRIMARY KEY

下方第一个是会产生重复记录的表，第二个是不会产生重复记录的表，如果插入重复记录，则会报错

```sql
CREATE TABLE person_tbl
(
    first_name CHAR(20),
    last_name CHAR(20),
    sex CHAR(10)
);

CREATE TABLE person_tbl
(
   first_name CHAR(20) NOT NULL,
   last_name CHAR(20) NOT NULL,
   sex CHAR(10),
   PRIMARY KEY (last_name, first_name)
);


```

如果使用INSERT IGNORE 而非INSERT，当插入重复观测时则不会报错，只是丢弃

```sql
mysql> INSERT IGNORE INTO person_tbl (last_name, first_name)
    -> VALUES( 'Jay', 'Thomas');
Query OK, 1 row affected (0.00 sec)
mysql> INSERT IGNORE INTO person_tbl (last_name, first_name)
    -> VALUES( 'Jay', 'Thomas');
Query OK, 0 rows affected (0.00 sec)
```

当使用REPLACE 而非 INSERT，当插入新的记录则会记录，插入旧的记录，则会更新

```sql
mysql> REPLACE INTO person_tbl (last_name, first_name)
    -> VALUES( 'Ajay', 'Kumar');
Query OK, 1 row affected (0.00 sec)
mysql> REPLACE INTO person_tbl (last_name, first_name)
    -> VALUES( 'Ajay', 'Kumar');
Query OK, 2 rows affected (0.00 sec)

```

2. 统计和标识重复

以下是查询以统计first_name和last_name 在表中的重复记录数。

```sql

mysql> SELECT COUNT(*) as repetitions, last_name, first_name
    -> FROM person_tbl
    -> GROUP BY last_name, first_name
    -> HAVING repetitions > 1;

```

3. 删除重复

3.1 创建新表再重命名回去

```sql
mysql> CREATE TABLE tmp SELECT last_name, first_name, sex
    ->                  FROM person_tbl;
    ->                  GROUP BY (last_name, first_name);
mysql> DROP TABLE person_tbl;
mysql> ALTER TABLE tmp RENAME TO person_tbl;
```

3.2 以添加主键的方式去除重复记录

```sql
mysql> ALTER IGNORE TABLE person_tbl
    -> ADD PRIMARY KEY (last_name, first_name);
```

## MySQL数据库导出

1. SELECT ... INTO OUTFILE

常规语法将导出到txt，并使用制表符分割，换行符结尾。

```sql
mysql> SELECT * FROM tutorials_tbl 
    -> INTO OUTFILE 'C:\tutorials.txt';
```

也可以利用选项更改记录的输出格式。

```sql
mysql> SELECT * FROM passwd INTO OUTFILE 'C:\tutorials.txt'
    -> FIELDS TERMINATED BY ',' ENCLOSED BY '"'
    -> LINES TERMINATED BY '\r\n';
```

导出表作为原始数据，使用mysqldump，它可以写入表输出作为一个原始数据文件，或为一组重新创建表中的INSERT语句的记录。

```
$ mysqldump -u root -p --no-create-info \
            --tab=c:\tmp TEST tutorials_tbl
password ******
```

以SQL格式的表导出到一个文件，使用如下方法

```
$ mysqldump -u root -p test tutorials_tbl > dump.txt
password ******
```

要转储多个表，所有数据库名称参数后跟它们的名字。要转储整个数据库，不需要在数据库之后命名(附加)任何表：

```
$ mysqldump -u root -p test > database_dump.txt
password ******
```

要备份所有可用的数据库在主机上，使用以下命令：

```
$ mysqldump -u root -p --all-databases > database_dump.txt
password ******
```

复制表或数据库到另一台主机

可以使用mysql命令将之前导出的txt文件再读入，运行此命令之前，请确保已创建数据库名称在目标服务器上。


```
$ mysql -u root -p database_name < dump.txt
password *****
```

另一种方式来实现这一点，无需使用一个中间文件是来发送，mysqldump输出直接通过网络到远程MySQL服务器。如果可以从源数据库所在的主机那里连接两个服务器，使用此命令(请确保两个服务器可以访问)：

```
$ mysqldump -u root -p database_name \
       | mysql -h other-host.com database_name
```

## MySQL数据库导入

### LOAD DATA

读取默认文件，默认情况下，LOAD DATA假设数据文件包含一个行由制表符分隔范围内被换行(新行)分割行和数据值。

```sql
mysql> LOAD DATA LOCAL INFILE 'C:\dump.txt' INTO TABLE mytbl;
```

可以使用FIELDS子句描述一行内字段的特征，LINES子句指定的行结束序列。

```sql
mysql> LOAD DATA LOCAL INFILE 'C:\dump.txt' INTO TABLE mytbl
  -> FIELDS TERMINATED BY ':'
  -> LINES TERMINATED BY '\r\n';
```

LOAD DATA假定数据文件中的列具有相同的顺序在表中的列。如果不是这样，可以指定一个列表来指示哪些表列数据文件列应该被装入。 假设表中的列A，B和C，但在数据文件连续列对应于列B，C，和A。可以像这样加载文件：

```sql
mysql> LOAD DATA LOCAL INFILE 'dump.txt' 
    -> INTO TABLE mytbl (b, c, a);
```

### mysqlimport导入数据

mysqlimport命令是一个命令行工具，包装了LOAD DATA

从dump.txt加载数据到表mytbl，使用下面的命令

```
$ mysqlimport -u root -p --local database_name dump.txt
password *****
```

使用--fields-terminated-by参数和--lines-terminated-by参数来指定分割符

```
$ mysqlimport -u root -p --local --fields-terminated-by=":" \
   --lines-terminated-by="\r\n"  database_name dump.txt
password *****
```

使用--columns参数来指定列的顺序

```
$ mysqlimport -u root -p --local --columns=b,c,a \
    database_name dump.txt
password *****

```

处理引号和特殊字符

 除了TERMINATED，FIELDS子句可以指定其他格式选项。默认情况下，LOAD DATA假设值是加引号的，并解释反斜线(\)作为特殊字符转义字符。要明确注明引用字符值， 使用ENCLOSED BY; MySQL将剥离字符的数据值末端在输入处理期间。要更改默认的转义字符，可使用ESCAPED BY。

当指定ENCLOSED BY，表明引号字符应该从数据值被剥离，有可能通过加一次或通过转义字符，确实包含引号字符在数据值前。例如，如果引号和转义字符是" 和 \，输入 "a""b\"c" 将被解释为：a"b"c

对于 mysqlimport，相应的命令行选项引号和转义值是通过 --fields-enclosed-by 和 --fields-escaped-by来指定。
