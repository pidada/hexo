---
title: numpy入门-数组运算
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-14 14:26:01
password:
summary:
tags:
  - python库
  - numpy
copyright: true
categories:
  - numpy
  - 入门
---

> 对数组做基本的算术运算，将会对整个数组的所有元组进行逐一运算，并将运算结果保存在一个新的数组内，而不会破坏原始的数组

- 数组和向量之间的运算
- 数组和数组之间的运算
- 通用函数的使用

<!--MORE-->

### 数组和向量之间的运算


```python
import numpy as np

a = np.array([20,40,50,80])
b = np.arange(4)
print(a)
print(b)
```

    [20 40 50 80]
    [0 1 2 3]

```python
c = a - b
c
```


    array([20, 39, 48, 77])


```python
b**2   # 每个元素进行平方
```


    array([0, 1, 4, 9], dtype=int32)


```python
10*np.sin(a)
```


    array([ 9.12945251,  7.4511316 , -2.62374854, -9.93888654])


```python
a < 40
```


    array([ True, False, False, False])


```python
a[a>45]
```


    array([50, 80])

### 数组和数组之间的运算


```python
A = np.array( [[1,1],
            [0,1]])
B = np.array( [[2,0],
            [3,4]])

print(A)
B
```

    [[1 1]
     [0 1]]

    array([[2, 0],
           [3, 4]])

#### 四则运算


```python
print(A - B)
A*B
```

    [[-1  1]
     [-3 -3]]

    array([[2, 0],
           [0, 4]])

#### 向量点乘的实现

- dot
- @


```python
A.dot(B)
```


    array([[5, 4],
           [3, 4]])


```python
np.dot(A,B)
```


    array([[5, 4],
           [3, 4]])


```python
A@B
```


    array([[5, 4],
           [3, 4]])

#### 自加和自乘


```python
A += B
A
```


    array([[3, 1],
           [3, 5]])


```python
A *= B
A
```


    array([[ 6,  0],
           [ 9, 20]])

#### 聚合函数


```python
A.mean()
```


    8.75


```python
A.max()
```


    20
    


```python
A.min()
```


    0

#### 指定行列的聚合


```python
c = np.array(np.arange(12).reshape(3,4))
c
```


    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])


```python
c.sum(axis=1)  # 行上求和
```


    array([ 6, 22, 38])


```python
c.mean(axis=0) # 列上求均值
```


    array([4., 5., 6., 7.])


```python
c.cumsum(axis=1)  # 行上求累加
```


    array([[ 0,  1,  3,  6],
           [ 4,  9, 15, 22],
           [ 8, 17, 27, 38]], dtype=int32)

#### sort函数

- Signature: np.sort(a, axis=-1, kind='quicksort', order=None)
- axis : int or None, optional
  Axis along which to sort. If None, the array is flattened before
  sorting. The default is -1, which sorts along the last axis.
- kind : {'quicksort', 'mergesort', 'heapsort', 'stable'}, optional
  Sorting algorithm. Default is 'quicksort'.


```python
# 排序：默认是快排，从低到高
c.sort()
c
```


    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])


```python
d = np.array([[1,6,2],[6,1,3],[1,5,2]])
d
```


    array([[1, 6, 2],
           [6, 1, 3],
           [1, 5, 2]])
    


```python
d.sort(axis=0)   # 在行index上进行排序
d
```


    array([[1, 1, 2],
           [1, 5, 2],
           [6, 6, 3]])

### 通用函数

- all
- any
- argmax
- argmin
- argsort
- average
- diff


```python
c
```


    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])


```python
np.exp(c)   # 求e的指数
```


    array([[1.00000000e+00, 2.71828183e+00, 7.38905610e+00, 2.00855369e+01],
           [5.45981500e+01, 1.48413159e+02, 4.03428793e+02, 1.09663316e+03],
           [2.98095799e+03, 8.10308393e+03, 2.20264658e+04, 5.98741417e+04]])


```python
np.sqrt(c)
```


    array([[0.        , 1.        , 1.41421356, 1.73205081],
           [2.        , 2.23606798, 2.44948974, 2.64575131],
           [2.82842712, 3.        , 3.16227766, 3.31662479]])


```python
np.add(c,c)   # 两个c相加
```


    array([[ 0,  2,  4,  6],
           [ 8, 10, 12, 14],
           [16, 18, 20, 22]])


```python
c * 2   
```


    array([[ 0,  2,  4,  6],
           [ 8, 10, 12, 14],
           [16, 18, 20, 22]])


```python
np.argsort(c, axis=0)    # 返回的是排序后的索引：axis=0 行上进行排序
# c.argsort()
```


    array([[0, 0, 0, 0],
           [1, 1, 1, 1],
           [2, 2, 2, 2]], dtype=int64)


```python
np.argsort(c, axis=1)  # axis=1 : 列上进行排序
```


    array([[0, 1, 2, 3],
           [0, 1, 2, 3],
           [0, 1, 2, 3]], dtype=int64)
