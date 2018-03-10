---
title: MySQL-细节与小结：优化与效率提高
date: 2018-03-10 16:40:55
tags:
- MySQL
- 知识清单
categories:
- 编程
---

一篇记录了优化小细节与提高效率的文章

<!--more-->

## 优化表语句

该语句用于整理碎片观测值

语法如下：

`OPTIMIZE TABLE table_name`

## 检查表语句

数据库服务器可能会发生错误，例如，服务器意外关闭，将数据写入硬盘时出错，等等。这些情况可能导致数据库运行不正确，并且在最坏的情况下可能会崩溃。

MySQL允许您使用CHECK TABLE语句检查数据库表的完整性。

`CHECK TABLE table_name;`

## 修复表语句

REPAIR TABLE语句允许您修复数据库表中发生的一些错误。 MySQL不保证REPAIR TABLE语句可以修复表所有可能的错误。

以下是EPAIR TABLE语句的语法：

`REPAIR TABLE table_name;`

> 注： REPAIR TABLE方法仅适用于MyISAM，ARCHIVE和CSV表。
