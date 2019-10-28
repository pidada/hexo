---
title: numpy入门-通用函数使用
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-21 10:54:39
password:
summary:
tags:
  - python
  - numpy
  - 通用函数
copyright: true
categories:
  - numpy
  - 入门
---

> 通用函数ufunc是⼀种对ndarray中的数据执⾏元素级运算的函数，它接受一个或者多个标量值，输出一个或者多个标量值。

- sqrt：开平方
- square：平方
- exp：求e指数
- add：求和
- max、min、mean：聚合函数
- abs：求绝对值
- log：默认底数是
- sign：符号函数，整数是1，负数是-1
- subtract(x,y)：两个数组中对应的元素相减

<!--MORE-->

---------



```python
import numpy as np
from numpy import pi
```


```python
a = np.arange(4)
a
```


    array([0, 1, 2, 3])


```python
np.exp(a)   # 自然e的次方
```


    array([ 1.        ,  2.71828183,  7.3890561 , 20.08553692])


```python
b = np.array([1, 4, 9, 16])
b
```


    array([ 1,  4,  9, 16])


```python
np.sqrt(b)   # 开方函数
```


    array([1., 2., 3., 4.])


```python
np.add(a, b)  # add函数
```


    array([ 1,  5, 11, 19])


```python
x = np.array([[1,5], [6,8]])
x
```


    array([[1, 5],
           [6, 8]])


```python
y = np.array([[4,7], [5,3]])
y
```


    array([[4, 7],
           [5, 3]])


```python
np.maximum(x,y)   # 比较两个数，取大值
```


    array([[4, 7],
           [6, 8]])


```python
np.minimum(x,y)
```


    array([[1, 5],
           [5, 3]])


```python
np.abs(x)   # 求绝对值
```


    array([[1, 5],
           [6, 8]])


```python
np.sqrt(x) # 开方
```


    array([[1.        , 2.23606798],
           [2.44948974, 2.82842712]])


```python
np.square(x)  # 求平方
```


    array([[ 1, 25],
           [36, 64]], dtype=int32)


```python
np.exp(x)
```


    array([[2.71828183e+00, 1.48413159e+02],
           [4.03428793e+02, 2.98095799e+03]])


```python
res = np.log(x)  # 默认的底数是e
res
```


    array([[0.        , 1.60943791],
           [1.79175947, 2.07944154]])


```python
np.exp(res)
```


    array([[1., 5.],
           [6., 8.]])


```python
np.log10(x)
```


    array([[0.        , 0.69897   ],
           [0.77815125, 0.90308999]])


```python
np.log2(x)   # 指定底数2
```


    array([[0.        , 2.32192809],
           [2.5849625 , 3.        ]])


```python
np.sign(x)   # 符号函数，正数为1，负数为-1
```


    array([[1, 1],
           [1, 1]])




```python
np.ceil(x)  # 大于该值的最小整数
```


    array([[1., 5.],
           [6., 8.]])


```python
np.modf(x)  # 将数组的小数和整数部分以两个独立数组的形式返回
```


    (array([[0., 0.],
            [0., 0.]]), array([[1., 5.],
            [6., 8.]]))


```python
np.add(x,y)
```


    array([[ 5, 12],
           [11, 11]])


```python
np.subtract(x,y)  #  对应x中的元素减去y中的元素

```


    array([[-3, -2],
           [ 1,  5]])


```python
np.divide(x,y)  # 除法向下取整（丢弃余数）

```


    array([[0.25      , 0.71428571],
           [1.2       , 2.66666667]])


```python
np.floor_divide(x,y)  # 取整操作
```


    array([[0, 0],
           [1, 2]], dtype=int32)


```python
np.mod(x,y)  # 相除求余数
```


    array([[1, 5],
           [1, 2]], dtype=int32)


```python
np.multiply(x,y)  # 数组中的对应元素相乘
```


    array([[ 4, 35],
           [30, 24]])

