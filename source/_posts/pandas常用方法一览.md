---
title: pandas常用方法一览
date: 2018-03-16 11:20:51
tags:
- python
- pandas
- BI
categories:
- 编程
---

<!--more-->

## 引入包

```python

# -*- coding: utf-8 -*-

import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

```


## 常用方法一览

### 序列化一维列表

```python
    s = pd.Series([1,3,5,np.nan,6,8])  
    
    print(s)
    print(s.__class__)
```


    '''
    通过对列表进行序列化，得到序列化对象Series,默认元素类型float64
    0    1.0
    1    3.0
    2    5.0
    3    NaN
    4    6.0
    5    8.0
    dtype: float64
    <class 'pandas.core.series.Series'>
    
    '''

### date_range序列化时间序列


```python    
    dates = pd.date_range('20130101',periods=6)
    
    print(dates)
    print(dates.__class__)
    print(dates[0])
    print(dates[0].__class__)
```

    '''
    使用date_range函数对日期进行序列化，只需要一个字符串形式的起始日期和天数即可自动排演
    生成的dates对象为DatetimeIndex类，元素为Timestamp类
    DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
                   '2013-01-05', '2013-01-06'],
                  dtype='datetime64[ns]', freq='D')
    <class 'pandas.core.indexes.datetimes.DatetimeIndex'>
    2013-01-01 00:00:00
    <class 'pandas._libs.tslib.Timestamp'>
    '''

### DataFrame序列化二维数组

第一个参数为np的二维6*4，6行4列，index 参数为索引，columns为列表.

```python
    dates = pd.date_range('20130101',periods=6)    
    df = pd.DataFrame(np.random.randn(6,4),index = dates,columns = list('ABCD'))
    
    
    print(df)
    print(df.__dict__)
    print(df['A'])
    print(df['A'].__class__)
```

可以看到，`df['A']`是一个一维序列

    '''
                       A         B         C         D
    2013-01-01  1.209109  0.230996 -1.257127 -0.768172
    2013-01-02 -0.576515  1.131800  0.397343  0.574513
    2013-01-03 -1.232799  1.035937 -0.449225 -0.095214
    2013-01-04 -0.586036 -2.797878  1.924726 -0.852538
    2013-01-05 -0.037847 -0.835513  0.388737 -0.697447
    2013-01-06  0.599082  1.496589  0.490352 -0.743327
    {'is_copy': None, '_data': BlockManager
    Items: Index(['A', 'B', 'C', 'D'], dtype='object')
    Axis 1: DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04','2013-01-05', '2013-01-06'], dtype='datetime64[ns]', freq='D')
    FloatBlock: slice(0, 4, 1), 4 x 6, dtype: float64, '_item_cache': {}, '_iloc': <pandas.core.indexing._iLocIndexer object at 0x00000216066B7F28>}
    2013-01-01    1.209109
    2013-01-02   -0.576515
    2013-01-03   -1.232799
    2013-01-04   -0.586036
    2013-01-05   -0.037847
    2013-01-06    0.599082
    Freq: D, Name: A, dtype: float64
    <class 'pandas.core.series.Series'>
    
    '''


再次用DataFrame序列化一个二维数组，其中给入一个字典参数，每个值可以为一个单值，字符串，Timestamp对象，一维Series对象，np的一维数组，以及特殊的Categories的Series对象(一种枚举类)

```python
    df2 = pd.DataFrame({'A':1.,
                        'B':pd.Timestamp('20130102'),
                        'C':pd.Series(1,index=list(range(4)),dtype='float32'),
                        'D':np.array([3]*4,dtype='int32'),
                        'E':pd.Categorical(["test","train","test","train"]),
                        'F':'foo'})
    
    print(df2)
    print(df2.__class__)
    print(df2.dtypes)
    print(df2.dtypes.__class__)
    print(df2.index)
    print(df2.index.__class__)
    print(df2['A'])
    print(df2['A'].__class__)
    print(df2['B'])
    print(df2['B'].__class__)
    print(df2['C'])
    print(df2['C'].__class__)
    print(df2['D'])
    print(df2['D'].__class__)
    print(df2['E'])
    print(df2['E'].__class__)
    print(df2['E'][1])
    print(df2['B'][2])
    print(df2.values)
```


    '''
    df2 以分列指定数据的方法产生DateFrame类
    由下展示可得，以字典形式点选的类均为Series序列类，且可以二重索引，先索引列再索引序号
    
         A          B    C  D      E    F
    0  1.0 2013-01-02  1.0  3   test  foo
    1  1.0 2013-01-02  1.0  3  train  foo
    2  1.0 2013-01-02  1.0  3   test  foo
    3  1.0 2013-01-02  1.0  3  train  foo
    <class 'pandas.core.frame.DataFrame'>
    A           float64
    B    datetime64[ns]
    C           float32
    D             int32
    E          category
    F            object
    dtype: object
    <class 'pandas.core.series.Series'>
    Int64Index([0, 1, 2, 3], dtype='int64')
    <class 'pandas.core.indexes.numeric.Int64Index'>
    0    1.0
    1    1.0
    2    1.0
    3    1.0
    Name: A, dtype: float64
    <class 'pandas.core.series.Series'>
    0   2013-01-02
    1   2013-01-02
    2   2013-01-02
    3   2013-01-02
    Name: B, dtype: datetime64[ns]
    <class 'pandas.core.series.Series'>
    0    1.0
    1    1.0
    2    1.0
    3    1.0
    Name: C, dtype: float32
    <class 'pandas.core.series.Series'>
    0    3
    1    3
    2    3
    3    3
    Name: D, dtype: int32
    <class 'pandas.core.series.Series'>
    0     test
    1    train
    2     test
    3    train
    Name: E, dtype: category
    Categories (2, object): [test, train]
    <class 'pandas.core.series.Series'>
    train
    2013-01-02 00:00:00
    
    [[1.0 Timestamp('2013-01-02 00:00:00') 1.0 3 'test' 'foo']
     [1.0 Timestamp('2013-01-02 00:00:00') 1.0 3 'train' 'foo']
     [1.0 Timestamp('2013-01-02 00:00:00') 1.0 3 'test' 'foo']
     [1.0 Timestamp('2013-01-02 00:00:00') 1.0 3 'train' 'foo']]
    
    '''

### 常用函数

首先构建一个二维对象，对其使用如下函数

- head    取前部分数据
- tail    取最后n个数据
- index   返回索引轴
- columns 返回列轴
- values  返回面板数据
- describe描述性统计
- T       转置
- sort_index 按轴排序
- sort_values按值排序
    
```python
    dates = pd.date_range('20130101',periods=6)    
    df = pd.DataFrame(np.random.randn(6,4),index = dates,columns = list('ABCD'))
    
    
    print(df.head())
    print(df.tail(2))
    print(df.index)
    print(df.columns)
    print(df.values)
    print(df.describe())
    print(df.T)
    print(df.sort_index(axis = 1,ascending=False))
    print(df.sort_values(by='B'))
```
    
    '''
    
                       A         B         C         D
    2013-01-01  0.156707  2.244479 -0.222424  0.439458
    2013-01-02  0.989163  0.484985  0.012343  0.042710
    2013-01-03 -2.071021  0.290205 -1.584422 -0.175207
    2013-01-04 -0.352580 -0.606124  0.314183 -2.700581
    2013-01-05 -0.337049  0.455282  1.402899 -1.426662
                       A         B         C         D
    2013-01-05 -0.337049  0.455282  1.402899 -1.426662
    2013-01-06 -0.990442 -1.642256  1.213499 -1.161018
    DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
                   '2013-01-05', '2013-01-06'],
                  dtype='datetime64[ns]', freq='D')
    Index(['A', 'B', 'C', 'D'], dtype='object')
    [[ 0.15670686  2.24447949 -0.22242431  0.43945792]
     [ 0.98916341  0.4849854   0.01234314  0.04271049]
     [-2.07102059  0.29020529 -1.58442181 -0.17520655]
     [-0.35258002 -0.60612394  0.31418319 -2.7005807 ]
     [-0.33704866  0.45528226  1.40289931 -1.4266622 ]
     [-0.99044199 -1.64225638  1.21349905 -1.16101843]]
                  A         B         C         D
    count  6.000000  6.000000  6.000000  6.000000
    mean  -0.434203  0.204429  0.189346 -0.830217
    std    1.037287  1.294453  1.084834  1.163319
    min   -2.071021 -1.642256 -1.584422 -2.700581
    25%   -0.830977 -0.382042 -0.163732 -1.360251
    50%   -0.344814  0.372744  0.163263 -0.668112
    75%    0.033268  0.477560  0.988670 -0.011769
    max    0.989163  2.244479  1.402899  0.439458
       2013-01-01  2013-01-02  2013-01-03  2013-01-04  2013-01-05  2013-01-06
    A    0.156707    0.989163   -2.071021   -0.352580   -0.337049   -0.990442
    B    2.244479    0.484985    0.290205   -0.606124    0.455282   -1.642256
    C   -0.222424    0.012343   -1.584422    0.314183    1.402899    1.213499
    D    0.439458    0.042710   -0.175207   -2.700581   -1.426662   -1.161018
                       D         C         B         A
    2013-01-01  0.439458 -0.222424  2.244479  0.156707
    2013-01-02  0.042710  0.012343  0.484985  0.989163
    2013-01-03 -0.175207 -1.584422  0.290205 -2.071021
    2013-01-04 -2.700581  0.314183 -0.606124 -0.352580
    2013-01-05 -1.426662  1.402899  0.455282 -0.337049
    2013-01-06 -1.161018  1.213499 -1.642256 -0.990442
                       A         B         C         D
    2013-01-06 -0.990442 -1.642256  1.213499 -1.161018
    2013-01-04 -0.352580 -0.606124  0.314183 -2.700581
    2013-01-03 -2.071021  0.290205 -1.584422 -0.175207
    2013-01-05 -0.337049  0.455282  1.402899 -1.426662
    2013-01-02  0.989163  0.484985  0.012343  0.042710
    2013-01-01  0.156707  2.244479 -0.222424  0.439458
    
    '''


### 常用属性

构建一个二维对象。以下是一些常用方法

1. 取某列---类似字典的getattr
2. 数字切片是在切观测值
3. 也可以使用index的内容直接进行切片(时间轴为index)
4. loc方法用于检索二维数组里的值，如直接选取某一index值索引到该观测值
5. loc也可以使用双重索引，形如`df.loc[:,['A','B']]`
6. at方法用于索引到具体的值



```python
    dates = pd.date_range('20130101',periods=6)    
    df = pd.DataFrame(np.random.randn(6,4),index = dates,columns = list('ABCD'))
    
    
    print(df['A'])
    print(df[0:1])
    print(df['20130102':'20130104'])
    print(df.loc)
    print(dates[0].__class__)
    print(df.loc[dates[0]])
    print(df.loc['20130102'])
    print(df.loc[:,['A','B']])
    print(df.loc[dates[0]:dates[2],['A','B']]) #亦可将dates[0] 换为字符串时间戳，或者其切片形式
    print(df.at[dates[0],'A'])
```


    '''
    可以看见，使用loc方法怎样截取FrameData中的部分数据，包括：展示单列，索引切片，时间戳切片，
    loc方法的时间戳缩影，展示多列以及对观测值与列变量同时选取，在这里请注意哪些是无效命令
    
    - df[0]
    - df.loc[1]
    - df.loc[1:2]
    
    即loc方法只对:进行了重载，没有对切片方法进行重载
    或者说其重载切片的支持对象仅限于index轴的元素对象方可
    (在本例中为Timestamp类)
    
    2013-01-01    0.287177
    2013-01-02    1.241335
    2013-01-03    0.344942
    2013-01-04    0.907778
    2013-01-05   -0.819140
    2013-01-06    0.945255
    Freq: D, Name: A, dtype: float64
                       A         B         C         D
    2013-01-01  0.287177  0.005274 -0.347833 -0.362417
                       A         B         C         D
    2013-01-02  1.241335 -0.525782 -0.648236 -0.111174
    2013-01-03  0.344942  0.072402 -0.681797  1.539014
    2013-01-04  0.907778  1.452052  0.926188 -2.202420
    <pandas.core.indexing._LocIndexer object at 0x0000021606725A20>
    <class 'pandas._libs.tslib.Timestamp'>
    A    0.287177
    B    0.005274
    C   -0.347833
    D   -0.362417
    Name: 2013-01-01 00:00:00, dtype: float64
    A    1.241335
    B   -0.525782
    C   -0.648236
    D   -0.111174
    Name: 2013-01-02 00:00:00, dtype: float64
                       A         B
    2013-01-01  0.287177  0.005274
    2013-01-02  1.241335 -0.525782
    2013-01-03  0.344942  0.072402
    2013-01-04  0.907778  1.452052
    2013-01-05 -0.819140 -1.025594
    2013-01-06  0.945255 -1.030329
                       A         B
    2013-01-01  0.287177  0.005274
    2013-01-02  1.241335 -0.525782
    2013-01-03  0.344942  0.072402
    0.287176849629
    
    '''
    

### 常用属性

iloc意为integer location ，即使用整型数字提取目标数据
iloc使用扩强版的下标索引实现，如果只使用整型数字、切片则默认为对观测值序列进行操作
如果给出两个参数，如`[1,2]`则认为取第2个观测值第3列的值，同样有效的还有
    
- `[3:5,1:2]` 对row 和col 同时切片
- `[[1,2,3],[1,2]]` 对row和col同时使用列表点取其中的观测值
- `[:,2]`选取全部row，选取第3列进行展示
    
PS: iat方法用于选取标量

```python
    dates = pd.date_range('20130101',periods=6)    
    df = pd.DataFrame(np.random.randn(6,4),index = dates,columns = list('ABCD'))
    
    print(df.iloc[3])
    print(df.iloc[3:5,0:2])
    print(df.iloc[[1,2,3],[0,2]])
    print(df.iloc[1,2])  #取第2个观测值第3列的值
    print(df.iloc[:,2])
    print(df.iat[1,1])
    
    
```

    '''
    
    
    
    A    0.670621
    B   -0.544727
    C    2.358465
    D   -0.229754
    Name: 2013-01-04 00:00:00, dtype: float64
                       A         B
    2013-01-04  0.670621 -0.544727
    2013-01-05  1.625488  1.053147
                       A         C
    2013-01-02 -0.276596 -0.470091
    2013-01-03  0.002827  0.701587
    2013-01-04  0.670621  2.358465
    -0.470090765774
    2013-01-01   -0.927538
    2013-01-02   -0.470091
    2013-01-03    0.701587
    2013-01-04    2.358465
    2013-01-05   -1.338166
    2013-01-06    0.004396
    Freq: D, Name: C, dtype: float64
    1.05324619603
    '''

































































































