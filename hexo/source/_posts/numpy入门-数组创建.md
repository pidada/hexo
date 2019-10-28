---
title: numpy入门-数组创建
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-14 10:25:05
password:
summary:
tags:
  - python库
  - 可视化
  - numpy
copyright: true
categories:
  - numpy
  - 入门
---

### Numpy 基础知识

- `Numpy`的主要对象是同质的**多维数组**。`Numpy`中的元素放在`[]`中，其中的元素通常都是数字，并且是同样的类型，由一个正整数元组进行索引。
- **每个元素在内存中占有同样大小的空间**。在`Numpy`中，维度被称为`轴`。例如对于[1, 2, 1]有一个轴，并且长度为3。而`[[ 1., 0., 0.], [ 0., 1., 2.]]`则有两个轴，第一个轴的长度为2，第二个轴的长度为3。
- `Numpy`数组类的名字叫做`ndarray`，经常简称为`array`。要注意将`numpy.array`与标准`Python`库中的`array.array`区分开，后者只处理一维数组，并且功能简单。

<!--MORE-->

### Numpy功能

>- `ndarray`，⼀个具有⽮量算术运算和复杂⼴播能⼒的快速且节
>  省空间的多维数组。
>- ⽤于对整组数据进⾏快速运算的标准数学函数（⽆需编写循
>  环）
>- ⽤于读写磁盘数据的⼯具以及⽤于操作内存映射⽂件的⼯
>  具
>- 线性代数、随机数⽣成以及傅⾥叶变换功能。
>  ⽤于集成由`C、C++、Fortran`等语⾔编写的代码的`API`



### 参数含义


```python
numpy.array(object, dtype = None, copy = True, order = None, subok = False, ndmin = 0)   # np.array的定义
```

**各参数的含义：**

- object：用于生成数组的数据对象
- dtype：指定类型，可选。
- copy：可选，默认为`True`，对象被复制。
- order：C语言风格（按行）、FORTRAN风格（按列）或A（任意，默认）。
- subok：默认情况下，返回的数组被强制为基类数组。 如果为True，则返回子类。
- ndmin：指定返回数组的最小维数

**ndarray属性**

+ ndarray.ndim：数组的轴数量
+ ndarray.shape：数组的形状。比如对于n行m列的矩阵，其shape形状就是(n,m)。而shape元组的长度则恰恰是上面的ndim值，也就是轴数。
+ ndarray.size：数组中所有元素的个数。这恰好等于shape中元素的乘积。
+ ndarray.dtype：数组中元素的数据类型。除了标准的Python类型，Numpy还提供一些自有的类型。
+ ndarray.itemsize：元素的字节大小。float64类型的itemsize为8（=64/8），而complex32的itemsize为4（=32/8）。
+ ndarray.data：包含数组实际元素的缓冲区
+ ndarray.flags: 数组对象的一些状态指示或标签

-----

### 创建ndarray

#### 一维或者多维数组


```python
import numpy as np    # 国际惯例的导入方式# 一维数组
data1 = [4, 5, 9, 1]
arr1 = np.array(data1)
arr1
```


    array([4, 5, 9, 1])


```python
# 二维数组
data2 = [[3,41,6], [9,1,4]]
arr2 = np.array(data2)
arr2
```


    array([[ 3, 41,  6],
           [ 9,  1,  4]])


```python
np.array([9,'xiaoming',0])  # 数值型自动转成字符串
```


    array(['9', 'xiaoming', '0'], dtype='<U11')

#### 全0矩阵


```python
# 全0
np.zeros(5)  # 全0向量
```


    array([0., 0., 0., 0., 0.])


```python
# 全0
arrZero = np.zeros((4,3))  # shape形状必须括起来
arrZero
```


    array([[0., 0., 0.],
           [0., 0., 0.],
           [0., 0., 0.],
           [0., 0., 0.]])


```python
np.zeros((2,4,3))
```


    array([[[0., 0., 0.],
            [0., 0., 0.],
            [0., 0., 0.],
            [0., 0., 0.]],
    
           [[0., 0., 0.],
            [0., 0., 0.],
            [0., 0., 0.],
            [0., 0., 0.]]])


```python
np.empty((2, 3, 2)) # 尽量少用empty方法
```


    array([[[1., 1.],
            [1., 1.],
            [1., 1.]],
    
           [[1., 1.],
            [1., 1.],
            [1., 1.]]])

#### 全1（类似全0）


```python
# 全1矩阵
arrOne = np.ones((4,3))  # shape 形状必须括起来
arrOne
```


```python
array([[1., 1., 1.],
       [1., 1., 1.],
       [1., 1., 1.],
       [1., 1., 1.]])
```

#### arange

类似`Python`中的`range`函数


```python
a = np.arange(15).reshape(3, 5)  # 创建3行5列的数组
a
```


```python
array([[ 0,  1,  2,  3,  4],
       [ 5,  6,  7,  8,  9],
       [10, 11, 12, 13, 14]])
```

#### 单位矩阵

对角线上全是1，其余是0

```python
np.eye(5)   # 创建单位矩阵

array([[1., 0., 0., 0., 0.],
       [0., 1., 0., 0., 0.],
       [0., 0., 1., 0., 0.],
       [0., 0., 0., 1., 0.],
       [0., 0., 0., 0., 1.]])
```

#### full

指定生成某个数的数组

```python
np.full((3,4),5)  # 创建3*4的全部是5的数组

array([[5, 5, 5, 5],
       [5, 5, 5, 5],
       [5, 5, 5, 5]])
```



#### linspace

> 在指定的范围内生成数组，endpoint表示包含尾部的元素

```python
a = np.linspace(2, 8, 10, endpoint=True, retstep=True, dtype=float)    # 自动返回步长
a

# result
(array([2.        , 2.66666667, 3.33333333, 4.        , 4.66666667,
        5.33333333, 6.        , 6.66666667, 7.33333333, 8.        ]),
 0.6666666666666666)
```

#### reshape

改成数组的形状；当参数中出现`-1`的时候，表示系统判断shape中的另一个参数

```python
a = np.floor(10*np.random.random((3, 4)))  # 先用random函数随机生成3*4的数组；再每个元素乘以10；最后floor取整
a.ravel()  # 平铺成一维数组

a.reshape(6,2)  # 变成6行2列，原来的数组a是不变的
np.reshape(a, (6,2))  # 等价同上

a.reshape(4, -1)  # 系统自动判断为4行3列
```

#### resize

大部分功能和使用与reshape函数相同

![](https://s2.ax1x.com/2019/10/14/KSV7iq.png)

#### 常用属性

- shape：几行几列，(m,n)
- ndim：维度
- size：总元素个数，m*n
- dtype：查看数据类型
- T：表示转置


```python
a.shape  # 数组形状，即几行几列
(3, 5)
```


```python
a.ndim  # 数组的轴数，维度称为轴
2
```


```python
a.dtype.name  # 数组中元素的数据类型
'int32'
```


```python
a.size   # 数组中所有元素的个数
15
```


```python
type(a)  # 查看类型
numpy.ndarray
```


```python
b = np.array([1, 2, 3, 4])   # 生成一个数组，中括号的元素看成一个整体
b
```


    array([1, 2, 3, 4])


```python
c = np.array([[1, 2], [3, 4], [5, 6]])  # 注意：有两层中括号[]
c
```


    array([[1, 2],
           [3, 4],
           [5, 6]])


```python
# 数据类型转换
int_array = np.arange(4)  # 创建0到3的一维数组
old = np.array([3.4, 2.4, 11.3])
new = old.astype(int_array.dtype)   # 转换成整数型dtype
old

[out]:array([ 3.4,  2.4, 11.3])
```

```python
np.pi  # pi
np.e  # e
```





