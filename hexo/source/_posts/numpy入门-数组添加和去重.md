---
title: numpy入门-数组中添加和删除元素
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-14 11:13:21
password:
summary:
tags:
  - python库
  - append
  - insert
  - numpy
copyright: true
categories:
  - numpy
  - 入门
---

> 添加和删除元素的方法主要是
>
> - append：只能追加在末尾
> - insert：可以在指定位置插入
> - delete：删除元素
> - unique：数组中元素去重

<!--MORE-->

### append

**numpy.append(arr,values,axis=None)**

- arr：输入向量
- values：将values值插到arr后面；values和arr应该维度相同
- axis：在哪个维度上进行增加元素；默认是返回的的是一个被拉平的向量

```python
import numpy as np 
a = np.array([[1,2,3], [4,5,6]])
np.append(a, [7,8,9])  # 不能通过a.append(),与Python的append方法不同；变成一维数组
```


    array([1, 2, 3, 4, 5, 6, 7, 8, 9])


```python
np.append(a, [[17,18,19]], axis=0)   # axis=0表示按行插入；2层中括号[]：numpy的括号好严格  
```


    array([[ 1,  2,  3],
           [ 4,  5,  6],
           [17, 18, 19]])

### insert

 **numpy.insert(arr,obj,value,axis=None) **

- arr：目标向量
- obj：目标位置
- values：想插入的元素
- axis：插入的维度，0行1列


```python
a = np.array([[1,2], [3,4],[5,6]])
a   
```


    array([[1, 2],
           [3, 4],
           [5, 6]])


```python
np.insert(a, 3, [7,8])      # 第3号数据前面插入，索引从0开始；数组变成一维
```


    array([1, 2, 3, 7, 8, 4, 5, 6])


```python
a  # 原来的数组不变
```


    array([[1, 2],
           [3, 4],
           [5, 6]])


```python
np.insert(a, 2, [11,12], axis=0)  # axis=0按行插入：第2行前面
```


    array([[ 1,  2],
           [ 3,  4],
           [11, 12],
           [ 5,  6]])


```python
np.insert(a, 1, [9], axis=1)   # 操作是在原来的数组a上，不是上一步变化之后的数组，注意维度的一致性
```


    array([[1, 9, 2],
           [3, 9, 4],
           [5, 9, 6]])


```python
np.insert(a, 1, [9,8,7], axis=1)   # axis=1表示按照列插入
```


    array([[1, 9, 2],
           [3, 8, 4],
           [5, 7, 6]])

### delete

**numpy.delete(arr,obj,axis=None) **

- arr：输入向量
- obj：表明哪个子向量应该被删除，可以是整数或者int型的向量
- axis：删除的轴；默认是返回的的是一个被拉平的向量


```python
b =  np.arange(12).reshape(3,4)   # 创建3行4列的数组
b
```


    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])


```python
np.delete(b,5)  # 删除数组中指定的元素5；变成一维数组
```


    array([ 0,  1,  2,  3,  4,  6,  7,  8,  9, 10, 11])


```python
np.delete(b,1,axis=0)  #  axis=0：删除数组中指定的行，索引=1
```


    array([[ 0,  1,  2,  3],
           [ 8,  9, 10, 11]])


```python
np.delete(b,1,axis=1)  # axis=1：删除数组中指定的列，第二个参数:索引=1
```


    array([[ 0,  2,  3],
           [ 4,  6,  7],
           [ 8, 10, 11]])

```python
b  # 原来的数组不变
```


    array([[ 0,  1,  2,  3],
           [ 4,  5,  6,  7],
           [ 8,  9, 10, 11]])

### unique

进行去重功能


```python
a = np.array([1,2,4,1,6,2,7,9,0])
a
```


    array([1, 2, 4, 1, 6, 2, 7, 9, 0])


```python
np.unique(a)    # 自动去重
```


    array([0, 1, 2, 4, 6, 7, 9])


```python
a  # 原来数组不变
```


    array([1, 2, 4, 1, 6, 2, 7, 9, 0])


```python
b = np.array([[1,1,3], [1,1,6], [1,1,5]])
b
```


    array([[1, 1, 3],
           [1, 1, 6],
           [1, 1, 5]])


```python
np.unique(b,axis=1)   # 删除相同的列
```


    array([[1, 3],
           [1, 6],
           [1, 5]])


```python
b = np.array([[0,1,3], [2,4,6], [9,8,3], [2,4,6]])
np.unique(b,axis=0)  # 将相同的一行数据删除掉
```


    array([[0, 1, 3],
           [2, 4, 6],
           [9, 8, 3]])
