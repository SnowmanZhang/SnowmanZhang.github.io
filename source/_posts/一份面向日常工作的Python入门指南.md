---
title: 一份面向日常工作的Python入门指南
date: 2018-01-21 13:49:17
tags:
- 大纲
- python
categories:
- 生活
---

近日准备一份关于面向日常工作的Python入门指南，所谓面向日常工作，意思即希望利用python开发一些解放日常工作的脚本。比如文件读写、数据处理、文本分析以及简单的爬虫技术，掌握了这些技术可以做什么呢？它可以批量化处理你手头的任务，可以自动化分析案头的任务，让你从繁重的手工劳动中解脱出来，更甚而言之，掌握一门编程语言，可以让你重新了解你的计算机，更好地利用它。

为什么要加上一个“面向日常工作”的定语？我们知道，Python作为C语言系的一门编程语言，在当下获得了前所未有的关注与热度，Python不仅在科学计算、爬虫等领域有着很好的特性，又堪称胶水语言，可以与许多编程语言及应用程序实现对接，这都极大地扩展了python的受众，但就其细节来说，开发者必须掌握相当程度的python高级特性及其它相关知识。对于本指南来说，笔者不希望涉足以上的内容，而是专注于如何在很短的时间内，教会读者如何迅速上手python并且利用其解决当下自己本职工作面临的一些问题。这同样是非常有价值的且饶有趣味的。

本指南立足于零基础且偏经管类的研究生群体，指南的内容会尽量贴合通俗化语言，且不涉及深层次的语法特性，带着任务做事情，以期达到事半功倍的效果。

<!--more-->

# 1. Python 语法基础

>本篇章主要讲解Python的一些基本计算、判断语句结构。为基础章节，为之后的应用打下基础。

所谓编程语言的基础，主要涵盖这样几部分内容

- python环境(Shell及解释器)及IDE(集成开发环境)简介
- 数据结构(在程序里面，有几种存储数据的形式)
- 运算符(数据与数据间的运算)
- 流程控制(程序依据一些规则对数据进行处理)

掌握了以上几部分，人们就有了基本的写出程序代码进行工作的能力。也具备了在此基础上深入学习的能力。

## 基础数据结构与赋值

- 数字
- 字符串
- 赋值

## 算数及逻辑运算符

### 算数运算符 加减乘除

```python
>>> 2+2
4
>>> # 这是注释
... 2+2
4
>>> 2+2 # 代码同一行的注释
4
>>> (50-5*6)/4
5.0
>>> 8/5 # 整数相除时并不会丢失小数部分
1.6
>>> 7//3
2
>>> 7//-3
-3
```

### 逻辑运算符 与或非等

逻辑运算符来源于集合论的主要术语，与(&)或(|)非(!)等(==)，逻辑运算符旨在用于判定一个条件表达式是真还是假。

## 容器简介(数据结构简介)

所谓容器，即存储数据的结构，因此有的书本称之为数据结构教程。

### 列表

列表是一种可变化的容器，它用于存储一系列相同等级的数据，如班级同学名单、物品清单。

- 索引
    - 点取元素
    - 截取列表
    - 负向点取
    - 二维列表
- 方法
    - append
    - index
    - pop
    - del

**需要说明的是，什么是可变化与不可变化，可变化的意思是说，这个容器里的内容是可以变更的，一个列表可以在建立以后在最后面添加元素，也可以删减元素，而不可变化的容器，一旦建立完毕，则不可更改其内容。**

### 字典

字典是一种可变化的容器，它将数据以键值对的方式存储，键在字典中是唯一的，值可以不唯一。字典这一数据结构可以用于存储"什么是什么"这类数据。

- 索引
    - 通过键获取
    - 通过遍历获取
- 方法
    - items
    - key
    - del

### 元组

元组是一种不可更改的容器

### 字符串

- 索引
- 切片
- 相连
- len


## 流程控制语句

### if

```python

a = 15

if a > 10:
    print("这是一个大于10的数字")
    if a > 20:
        print("这是一个大于20的数字")
    else:
        print("这是一个不大于20的数字")
elif a > 5:
    print("这是一个大于5的数字")
else:
    print("这是一个不大于5的数字")


```

### while

```python

a = 1

while a < 10:
    print(a)
    a += 1


```

### for

```python

a = [1,2,3,4,5,6,7]

for i in a:
    print(i)

```

## 输入与输出

```python

url = input("please input the url：\n")
print(url)

f = open("something.txt","r",encoding="utf8")

f.read()

f.close()

with open("something.txt",'r',encoding='utf8') as f:
    f.read()

with open("otherthing.txt",'w',encoding='utf8') as f:
    f.write("some word\n")

import pickle

    a = [1,2,3,4,5,6]
    with open("teaching.pkl","wb") as f:
        pickle.dump(a,f)
    with open("teaching.pkl","rb") as f:
        new_list = pickle.load(f)


```

# Spyder IDE简介

> 对我们所常用的Spyder这一集成开发环境进行简介，方便大家更好地理解自己写的程序是怎样运行的。提高学习效率。

## 变量查看区

![](variable_explorer.png)

## IPython控制台

![](ipython_console.png)

## 信号提示与报错查看

![](file_explorer.png)

![](history_log.png)

## debug

![](debug.png)

# Python 函数与类

> 函数与类，这是Python中抽象化的概念，了解这方面的知识可以使你更好地使用外部库函数，从类再深讲，Python就会体现其高级特性，本指南不会涉及，因此对于类的概念，我们也是但求理解，不求实践。

## 自定义函数

```python 

def myfirstfunc(a,b,c,d):
    return ( a + b ) / (c + d)

answer = myfirstfunc(1,2,3,4)

def myfunc(a):
    a.append(1)
    return a

first = [1,2,3]
second = myfunc(first)
print(first)
print(second)

```

## 命名空间

```python

def scope_test():
    def do_local():
        spam = "local␣spam"
    def do_nonlocal():
        nonlocal spam
        spam = "nonlocal␣spam"
    def do_global():
        global spam
        spam = "global␣spam"

    spam = "test␣spam"
    do_local()
    print("After␣local␣assignment:", spam)
    do_nonlocal()
    print("After␣nonlocal␣assignment:", spam)
    do_global()
    print("After␣global␣assignment:", spam)

spam = "something"
print("the global assignment is ",spam)

```

## 面向对象：类的初印象

## 回头看：容器的方法

# 案例一：填充一份财经日报

涉及内容：格式化字符串

>一份日常的财经日报，是否可以从遵循一定的书写范式中抽象出来，由收集到的数据自动添加呢？如果可以，我们就能使用Python批量化生成日报周报，人所做的工作就大幅减轻了。

```python




```

# 案例二：爬取并分析一个网页

涉及内容： request BeautifulSoup

>本案例对Python爬虫进行一个简单的实战，主要展现Python惊人的解析网页的能力与方便快捷的爬虫方法。



```python

# -*- coding: utf-8 -*-
import requests
from bs4 import BeautifulSoup


if __name__ == "__main__":
    
    url = r"http://www.cpppc.org:8082/efmisweb/ppp/projectLibrary/getProjInfoNational.do?projId=f3928af9527049518581b8b0b7320c2b"
    
    RequestClass = requests.get(url)
    print(RequestClass)  # 返回一个名为request 的类
    print(RequestClass.text) # 返回这个相应的文本
    print(RequestClass.cookies) # 返回这个相应携带的cookies
    
    '''
    接下来该做什么？是将RequestClass.text里的内容存入文本文档手工分析吗？
    不，我们使用BeautifulSoup来解析这段html代码
    '''
    
    BeautyClass = BeautifulSoup(RequestClass.text,"lxml")
    
    print(BeautyClass)   # 这是一个BeautifulSoup类，实际上是一种文档树
    print(BeautyClass.title) # 返回这个网页的名称Tag，即解析出<title>balabala</title>的内容
    InfoList = BeautyClass.find_all("td",colspan="2",style="text-align: left") #find_all 方法可以在当前这个BeautifulSoup类中进行检索，寻找符合要求的标签Tag。如果有多个符合，则返回一个列表，列表中的每个元素均为符合要求的Tag
    print(InfoList)  
    print(InfoList[0].parent)  # 标签Tag的parent方法返回它的父节点
    print(InfoList[0].next_element)  # 标签Tag的next_element方法返回这个节点的下一个节点
    


```

# 案例三：模拟并估计价格二叉树

涉及内容：Demical 、 迭代

>价格二叉树是金融工程中较常见的一种习题，在这里我们引入一点特别的东西，在讲解如何实现二叉树的同时，学习怎样在Python中书写递归函数以及计算机底层的一些特性，递归函数是编程语言最富有魅力的地方，同时也是数据算法的启蒙钥匙。


```python
from decimal import Decimal
import decimal
import numpy as np     



def prettyway(price,pro,depth):  #计算第depth层的价格情况及相应概率。
    if depth == 10:
        return price,pro
    rawarray = multiply(price,0.9) + multiply(price,1.1)
    rawpro = multiply(pro,0.6) + multiply(pro,0.4)
    cleanarray = []
    cleanpro = []
    for i in set(rawarray):
        indexlist = searchindex(i,rawarray)
        tempsum = 0
        for index in indexlist:
            tempsum = tempsum + rawpro[index]
        cleanarray.append(i)
        cleanpro.append(tempsum)
    print("have get the depth "+str(depth))
    return prettyway(cleanarray,cleanpro,depth+1)
        
def searchindex(index,array):  #返回列表array中所有值等于index的元素的下标，以列表形式返回。
    return [a for a in range(len(array)) if array[a] == index]

def multiply(array,i):  #对Demical类的元素组成的列表array乘以统一的系数i
    ans = []
    for single in array:
        ans.append(single*Decimal(str(i)))
    return ans


decimal.getcontext().prec = 1006
price,pro=prettyway([Decimal(str(100)),],[Decimal(str(1.0)),],0)    

```

