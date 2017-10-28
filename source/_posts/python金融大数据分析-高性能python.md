---
title: '[python金融大数据分析]高性能python'
date: 2017-10-20 07:48:04
tags:
- python
- numpy
categories:
- 编程
---

[python金融大数据分析]是一本享有美名的动物书，本系列将就其中的重点篇章作讲义和笔记，本篇文章是其中的第八章[高性能的python]

## 内存布局与性能

在numpy初始化ndarray对象时，存在一个属性指定`order='C'`

该属性有两种选项，表示元素在内存中存储的顺序

1. order='C'
	C表示类似C（行优先）
2. order='F'
	F表示类似Fortran（列优先）

测试两种内存布局会在性能上有什么差异

```python

x = np.random.standard_normal((3,1500000))
C = np.array(x,order = 'C')
F = np.array(x,order = 'F') 

%timeit C.sum(axis = 0)
%timeit C.sum(axis = 1)

%timeit F.sum(axis = 0)
%timeit F.sum(axis = 1)

```

事实证明，C的布局优于F，这也是为什么C是order的默认值。

# 蒙特卡洛算法

>采用Black-Scholes-Merton设置下的欧式看涨期权价值蒙特卡洛估算函数


```python
import numpy as np

def bsm_mcs_valuation(strike):   # 实现Black-Scholes-Merton设置下蒙特卡洛估值的函数
	S0 = 100.0
	T = 1.0
	r = 0.05
	vola = 0.2
	M = 50
	I = 20000
	dt = T/M
	rand = np.random.stanard_normal((M + 1,I))
	S = np.zeros((M + 1,I))
	S[0] = S0
	for t in range(1,M + 1):
		S[t] = S[t-1] * np.exp((r - 0.5 * vola ** 2) * dt + vola * np.sqrt(dt) * rand[t])
	value = (np.exp(-r * T) * np.sum(np.maximum(S[-1] - strike, 0))/I)
	return value

def seq_value(n):
	strikes = np.linspace(80,120,n)
	option_values = []
	for strike in strikes:
		option_values.append(bsm_mcs_valuation(strike))
	return strikes,option_values

n = 100
```






