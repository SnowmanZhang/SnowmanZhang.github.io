---
title: MySQL-细节与小结：存储过程与视图
date: 2018-03-10 13:13:26
tags:
- MySQL
- 知识清单
categories:
- 编程
---

一份讲解MySQL细节的知识清单

<!--more-->

## 存储过程

MySQL的存储过程，在功能上类似于定义函数，将一组常用命令编译为一个过程，存储在数据库目录中。存储过程是存储在数据库目录中的一段声明性SQL语句。

### 评价

**优点**

1. 通常存储过程有助于提高应用程序的性能。当创建，存储过程被编译之后，就存储在数据库中。 但是，MySQL实现的存储过程略有不同。在编译存储过程之后，MySQL将其放入缓存中。如果应用程序在单个连接中多次使用存储过程，则使用编译版本，否则存储过程的工作方式类似于查询。
2. 存储过程有助于减少应用程序和数据库服务器之间的流量，因为应用程序不必发送多个冗长的SQL语句，而只能发送存储过程的名称和参数。
3. 存储的程序对任何应用程序都是可重用的和透明的。故可重用性高。 存储过程将数据库接口暴露给所有应用程序，以便开发人员不必开发存储过程中已支持的功能。
4. 存储的程序是安全的。 数据库管理员可以向访问数据库中存储过程的应用程序授予适当的权限，而不向基础数据库表提供任何权限。

**缺点**

1. 如果使用大量存储过程，那么使用这些存储过程的每个连接的内存使用量将会大大增加。 此外，如果您在存储过程中过度使用大量逻辑操作，则CPU使用率也会增加。
2. 存储过程的构造不利于开发复杂业务逻辑。
3. 很难调试存储过程。只有少数数据库管理系统允许您调试存储过程。

### 简单示例

```sql
DELIMITER //
 CREATE PROCEDURE GetAllProducts()
   BEGIN
   SELECT *  FROM products;
   END //
DELIMITER ;

```

**要点**

1. DELIMITER语句将标准分隔符分号`;`更改为`//`。 以便将存储过程作为整体传递给服务器，而不是让mysql工具一次解释每个语句。
2. `CREATE PROCEDURE`语句创建一个新的存储过程。在`CREATE PROCEDURE`语句之后指定存储过程的名称。
3. BEGIN和END之间的部分称为存储过程的主体。将声明性SQL语句放在主体中以处理业务逻辑。

**调用存储过程**

`CALL STORED_PROCEDURE_NAME();`

### 存储过程的变量

**声明变量**语法如下

`DECLARE variable_name datatype(size) DEFAULT default_value;`

例如

`DECLARE total_sale INT DEFAULT 0;`

**分配变量**

1. SET 方式赋值
    
    `SET total_count = 10;`

2. SELECT INTO 赋值

    `SELECT COUNT(*) INTO total_products FROM products`

**变量范围**

1. 在存储过程中的变量，范围为BEGIN 和 END 以内
2. 以`@`符号结尾的变量是会话变量，直到会话结束失效

### 存储过程的参数

**模式简介**

1. IN - 是默认模式。在存储过程中定义IN参数时，调用程序必须将参数传递给存储过程。 另外，IN参数的值被保护。这意味着即使在存储过程中更改了IN参数的值，在存储过程结束后仍保留其原始值。换句话说，存储过程只使用IN参数的副本。
2. OUT - 可以在存储过程中更改OUT参数的值，并将其更改后新值传递回调用程序。请注意，存储过程在启动时无法访问OUT参数的初始值。
3. INOUT - INOUT参数是IN和OUT参数的组合。这意味着调用程序可以传递参数，并且存储过程可以修改INOUT参数并将新值传递回调用程序。

**定义**

`MODE param_name param_type(param_size)`


1. 根据存储过程中参数的目的，MODE可以是IN，OUT或INOUT。
2. param_name是参数的名称。参数的名称必须遵循MySQL中列名的命名规则。
3. 在参数名之后是它的数据类型和大小。和变量一样，参数的数据类型可以是任何有效的MySQL数据类型。
4. 如果存储过程有参数，则用逗号分隔

#### IN 参数传参示例

```sql

USE `yiibaidb`;
DROP procedure IF EXISTS `GetOfficeByCountry`;

DELIMITER $$
USE `yiibaidb`$$
CREATE PROCEDURE GetOfficeByCountry(IN countryName VARCHAR(255))
 BEGIN
 SELECT * 
 FROM offices
 WHERE country = countryName;
 END$$

DELIMITER ;

```

在这里，我们创建了一个IN 参数，用以查询countryname

如果使用，则形如

`CALL GetOfficeByCountry('USA');`

#### OUT 参数传参示例

```sql

USE `yiibaidb`;
DROP procedure IF EXISTS `CountOrderByStatus`;

DELIMITER $$
CREATE PROCEDURE CountOrderByStatus(
 IN orderStatus VARCHAR(25),
 OUT total INT)
BEGIN
 SELECT count(orderNumber)
 INTO total
 FROM orders
 WHERE status = orderStatus;
END$$
DELIMITER ;

```

调用语句如下

`CALL CountOrderByStatus('Shipped',@total);    SELECT @total;`

**要点**

1. 设置了两个参数IN OUT ，在传参OUT时，使用会话参数
2. `SELECT @total;` 是一种简易的查询语句。

### IF 语句

语法参见

```sql

IF expression THEN
   statements;
ELSEIF elseif-expression THEN
   elseif-statements;
...
ELSE
   else-statements;
END IF;
```

**说明**

利用IF语句，可以对传入的IN参数进行判断，进行不同的业务逻辑操作

### CASE 语句

**简单的CASE语句语法如下**

```sql

CASE  case_expression
   WHEN when_expression_1 THEN commands
   WHEN when_expression_2 THEN commands
   ...
   ELSE commands
END CASE;

```

**要点**

1. case_expression可以是任何有效的表达式。我们将case_expression的值与每个WHEN子句中的when_expression进行比较，例如when_expression_1，when_expression_2等。
2. 如果case_expression和when_expression_n的值相等，则执行相应的WHEN分支中的命令(commands)。


**可搜索CASE语句如下**

```sql
CASE
    WHEN condition_1 THEN commands
    WHEN condition_2 THEN commands
    ...
    ELSE commands
END CASE;
```

第一种CASE方式是匹配模式，拿case_exp 与 各个when_exp做比较，而搜索CASE可以在condition中输入表达式

### 循环语法

#### WHILE

```sql
WHILE expression DO
   statements
END WHILE


```

#### REPEAT

类似于do while

```sql
REPEAT
 statements;
UNTIL expression
END REPEAT
```

#### LEAVE 和 ITERATE LOOP

1. LEAVE语句用于立即退出循环，而无需等待检查条件。LEAVE语句的工作原理就类似PHP，C/C++，Java等其他语言的break语句一样。
2. ITERATE语句允许您跳过剩下的整个代码并开始新的迭代。ITERATE语句类似于PHP，C/C++，Java等中的continue语句。
3. LOOP语句用于执行循环代码块，并且支持灵活的标签

```sql
CREATE PROCEDURE test_mysql_loop()
 BEGIN
 DECLARE x  INT;
        DECLARE str  VARCHAR(255);

 SET x = 1;
        SET str =  '';

 loop_label:  LOOP
 IF  x > 10 THEN 
 LEAVE  loop_label;
 END  IF;

 SET  x = x + 1;
 IF (x mod 2) THEN
     ITERATE  loop_label;
 ELSE
    SET  str = CONCAT(str,x,',');
 END IF;
    END LOOP;    
    SELECT str;
END;


```

### 游标

**概述**

要处理存储过程中的结果集，请使用游标。游标允许您迭代查询返回的一组行，并相应地处理每行。

MySQL游标为只读，不可滚动和敏感。

1. 只读：无法通过光标更新基础表中的数据。
2. 不可滚动：只能按照SELECT语句确定的顺序获取行。不能以相反的顺序获取行。 此外，不能跳过行或跳转到结果集中的特定行。
3. 敏感：有两种游标：敏感游标和不敏感游标。敏感游标指向实际数据，不敏感游标使用数据的临时副本。敏感游标比一个不敏感的游标执行得更快，因为它不需要临时拷贝数据。但是，对其他连接的数据所做的任何更改都将影响由敏感游标使用的数据，因此，如果不更新敏感游标所使用的数据，则更安全。 MySQL游标是敏感的。

对于cursor来说，每次返回一个观测值结果，并下移一行，迭代输出。

**声明**

`DECLARE cursor_name CURSOR FOR SELECT_statement;`

**流程图**

![](cursor.png)

[详情链接进入传送门](https://www.yiibai.com/mysql/cursor.html)

### 管理存储过程

MySQL为提供了一些有用的语句，可以更有效地管理存储过程。这些语句包括列出存储过程并显示存储过程的源代码。

**显示存储过程**

`SHOW PROCEDURE STATUS [LIKE 'pattern' | WHERE expr];`

该语句是SQL的MySQL扩展。也可以加入一些条件约束，如

`SHOW PROCEDURE STATUS WHERE db = 'yiibaidb';`

**显示特定存储过程的源代码**

`SHOW CREATE PROCEDURE stored_procedure_name`

### 错误处理程序

当存储过程中发生错误时，重要的是适当处理它，例如：继续或退出当前代码块的执行，并发出有意义的错误消息。


### signal 和 esignal 语句

### MySQL存储函数

## 视图

**概述**

数据库视图是虚拟表或逻辑表，它被定义为 **具有连接的SQL SELECT查询语句**。 因为数据库视图与数据库表类似，它由行和列组成，因此可以根据数据库表查询数据。 大多数数据库管理系统(包括MySQL)允许您通过具有一些先决条件的数据库视图来更新基础表中的数据。


**优点**

1. 数据库视图允许简化复杂查询：数据库视图由与许多基础表相关联的SQL语句定义。 您可以使用数据库视图来隐藏最终用户和外部应用程序的基础表的复杂性。 通过数据库视图，您只需使用简单的SQL语句，而不是使用具有多个连接的复杂的SQL语句。
2. 数据库视图有助于限制对特定用户的数据访问。 您可能不希望所有用户都可以查询敏感数据的子集。可以使用数据库视图将非敏感数据仅显示给特定用户组。
3. 数据库视图提供额外的安全层。 安全是任何关系数据库管理系统的重要组成部分。 数据库视图为数据库管理系统提供了额外的安全性。 数据库视图允许您创建只读视图，以将只读数据公开给特定用户。 用户只能以只读视图检索数据，但无法更新。
4. 数据库视图启用计算列。 数据库表不应该具有计算列，但数据库视图可以这样。 假设在orderDetails表中有quantityOrder(产品的数量)和priceEach(产品的价格)列。 但是，orderDetails表没有一个列用来存储订单的每个订单项的总销售额。如果有，数据库模式不是一个好的设计。 在这种情况下，您可以创建一个名为total的计算列，该列是quantityOrder和priceEach的乘积，以表示计算结果。当您从数据库视图中查询数据时，计算列的数据将随机计算产生。
5. 数据库视图实现向后兼容。 假设你有一个中央数据库，许多应用程序正在使用它。 有一天，您决定重新设计数据库以适应新的业务需求。删除一些表并创建新的表，并且不希望更改影响其他应用程序。在这种情况下，可以创建与将要删除的旧表相同的模式的数据库视图。

**缺点**


1. 性能：从数据库视图查询数据可能会很慢，特别是如果视图是基于其他视图创建的。
2. 表依赖关系：将根据数据库的基础表创建一个视图。每当更改与其相关联的表的结构时，都必须更改视图。

### 实现与限制

#### 实现方式

1. 第一种方式，MySQL会根据视图定义语句创建一个临时表，并在此临时表上执行传入查询。
2. 第二种方式，MySQL将传入查询与查询定义为一个查询并执行组合查询。

#### 限制

1. 不能在视图上创建索引。当使用合并算法的视图查询数据时，MySQL会使用底层表的索引。
2. 在MySQL 5.7.7之前版本，是不能在SELECT语句的FROM子句中使用子查询来定义视图的。
3. 如果删除或重命名视图所基于的表，则MySQL不会发出任何错误。但是，MySQL会使视图无效。 可以使用CHECK TABLE语句来检查视图是否有效。
4. 一个简单的视图可以更新表中数据。基于既有连接，子查询等的复杂SELECT语句创建的视图无法更新。
5. MySQL不像：Oracle，PostgreSQL等其他数据库系统那样支持物理视图，MySQL是不支持物理视图的。

## 触发器

### 简介

SQL触发器是存储在数据库目录中的一组SQL语句。每当与表相关联的事件发生时，即会执行或触发SQL触发器，例如插入，更新或删除。

SQL触发器是一种特殊类型的存储过程。 这是特别的，因为它不像直接像存储过程那样调用。 触发器和存储过程之间的主要区别在于，当对表执行数据修改事件时，会自动调用触发器，而存储过程必须要明确地调用

在MySQL中，触发器是一组SQL语句，当对相关联的表上的数据进行更改时，会自动调用该语句。 触发器可以被定义为在INSERT，UPDATE或DELETE语句更改数据之前或之后调用。在MySQL5.7.2版本之前，每个表最多可以定义六个触发器。

#### 优点

- SQL触发器提供了检查数据完整性的替代方法。
- SQL触发器可以捕获数据库层中业务逻辑中的错误。
- SQL触发器提供了运行计划任务的另一种方法。通过使用SQL触发器，您不必等待运行计划的任务，因为在对表中的数据进行更改之前或之后自动调用触发器。
- SQL触发器对于审核表中数据的更改非常有用。

#### 缺点

1. SQL触发器只能提供扩展验证，并且无法替换所有验证。一些简单的验证必须在应用层完成。 例如，您可以使用JavaScript或服务器端使用服务器端脚本语言(如JSP，PHP，ASP.NET，Perl等)来验证客户端的用户输入。
2. 从客户端应用程序调用和执行SQL触发器不可见，因此很难弄清数据库层中发生的情况。
3. SQL触发器可能会增加数据库服务器的开销。

### 触发存储

MySQL在数据目录中存储触发器，例如：/data/yiibaidb/,并使用名为tablename.TRG和triggername.TRN的文件：

- tablename.TRG文件将触发器映射到相应的表。
- triggername.TRN文件包含触发器定义。

可以通过将触发器文件复制到备份文件夹来备份MySQL触发器。也可以使用mysqldump工具备份触发器。

### 触发限制

- 使用在SHOW，LOAD DATA，LOAD TABLE，BACKUP DATABASE，RESTORE，FLUSH和RETURN语句之上。
- 使用隐式或明确提交或回滚的语句，如COMMIT，ROLLBACK，START TRANSACTION，LOCK/UNLOCK TABLES，ALTER，CREATE，DROP，RENAME等。
- 使用准备语句，如PREPARE，EXECUTE等
- 使用动态SQL语句。

从MySQL 5.1.4版本开始，触发器可以调用存储过程或存储函数，在这之前的版本是有所限制的。
