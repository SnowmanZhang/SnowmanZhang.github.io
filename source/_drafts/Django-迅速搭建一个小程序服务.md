---
title: Django-迅速搭建一个小程序服务
date: 2018-02-17 20:21:58
tags:
- 新坑
- Django
- 小程序
categories:
- 编程
---

小程序以其简洁的语法、丰富的功能及广阔前景备受喜爱，回家过年时，老爸问我能否做一个功能简单的小程序，我一口应允了下来，经过年前两天的折腾，基本把框架涉及的原理弄清楚了，现在和大家分享一下。

和以前的规矩一样，我会把一些常用的要点记录下来，但就详细程度来说，是远不及官方文档的，给自己起到 的也只是督促与待查的作用。

<!--more-->

# 功能与环境说明

本项目搭建于腾讯云服务器上，是为了做好与小程序的请求对接。小程序有如下request需求

1. domain/location?latitude=number1&longtitude=number2
    
    该请求发送一个经纬度坐标，从数据库中查询得到离该坐标最近的店家信息并返回，店家信息包括

    - 经纬度坐标
    - 店名
    - 店铺地址
    - 店铺简介

2. domain/prodution

    该请求返回产品列表，列表不多，应该只有约十余个。返回一个列表，包含如下信息

    - id(依据id排序)
    - 产品名称
    - 产品简介
    - 当前有该项产品服务的最近的店铺信息(同1)


3. domain/business

    该请求返回店家列表，应该可以只放置六七个，每个包含如下信息

    - 店名
    - 店铺地址
    - 店铺成功案例

服务器相关环境如下

- Linux CentOS7
- Django 1.11.9
- Python 2.7

# 需求分析

在这里我们需要创建一个案例、店家管理页面，因为今后店家会动态增减，具体来说需要创建两个表(在Django中可以说是模型)

- 店家表
    - 经纬度坐标
    - 店铺名称
    - 店铺地址
    - 店铺简介
    - 店铺成功案例(如果没有则输入0)
- 产品表
    - 产品id
    - 产品名称
    - 产品简介

然后根据route判断并返回相应的信息


# 从零开始

### 1. 新建mysite目录 

`$ django-admin startproject mysite`

会在当前目录下新建一个名为mysite的站点，其目录结构为

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```



# 参考文档

- [Django1.8.2 中文文档](http://python.usyiyi.cn/documents/django_182/intro/tutorial01.html)
- 