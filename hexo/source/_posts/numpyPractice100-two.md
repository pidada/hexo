---
title: numpyPractice100-two
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-13 23:46:28
password:
summary:
tags:
  - python库
  - 可视化
  - numpy
copyright: true
categories:
  - numpy
  - 100题
---

> 主要涉及到的知识点：
>
> - `tile()`函数的使用：复制功能。一维或者多维皆可
>
> - 标准化方阵：`(arr - np.mean(arr)) / (np.std(arr))`
>
> - 向量之间的点乘：`dot(arr1, arr2)`和`arr1@arr2`
>
> - `sum()`的用法：将里面的元素进行相加
>
> - 获取时间日期：
>
> ```python
>   - yesterday = np.datetime64('today', 'D') - np.timedelta64(1, 'D')
>   - today = np.datetime64('today', 'D')
>   - tomorrow = np.datetime64('today', 'D') + np.timedelta64(1, 'D')
> ```
>
> - `linspace()`的用法
> - `argmax()`：求出最大数

<!--MORE-->

```python
import numpy as np
```


```python
# 21  tile函数使用：相当于是复制作用
np.tile(np.array([[0,1], [1,0]]), (4,4))
```


```python
a = np.array([0, 1, 2])
np.tile(a, 2)
```


```python
# 22 : 标准化一个方阵
arr = np.random.random((5,5))
(arr - np.mean(arr)) / (np.std(arr))  # 标准化公式
```


```python
# 23
color = np.dtype([("r", np.ubyte, 1),
                  ("g", np.ubyte, 1),
                  ("b", np.ubyte, 1),
                  ("a", np.ubyte, 1)])
color
```


```python
# 24  向量之间的相乘（5*3）*（3*2）= 5*2
# dot() 方法
np.dot(np.ones((5,3)), np.ones((3,2)))  
```


```python
# python 3.5 写法：@
np.ones((5,3)) @ np.ones((3,2))
```


```python
# 25 将某个区间的数字变成相反数：乘以-1
arr = np.arange(11)
arr[(arr > 3)&(arr < 8)] *= -1
arr
```


```python
# 26 表达式的结果
# 将 sum()函数中所有数相加：1+2+3+4+（-1）=9
sum(range(5), -1)
```


```python
# 27 判断表达式的合法性
# Z**Z
# 2 << Z >> 2
# Z <- Z
# 1j*Z
# Z/1/1
# Z<Z>Z   错误
```


```python
Z = np.array([1,2,3])
Z
```


```python
Z**Z   # 对应的元素求指数即可
```


```python
2 << Z >> 2
```


    array([1, 2, 4], dtype=int32)


```python
Z <- Z   # Z和它的相反数进行比较
```


    array([False, False, False])


```python
1j*Z
```


    array([0.+1.j, 0.+2.j, 0.+3.j])


```python
Z/1/1
```


    array([1., 2., 3.])


```python
# 28  表达式的结果
print(np.array(0) / np.array(0))   # nan
print(np.array(0) // np.array(0))  # 0
print(np.array([np.nan]).astype(int).astype(float))  # [-2.14748365e+09]
```

    nan
    0
    [-2.14748365e+09]

```python
# 29
Z = np.random.uniform(-10,+10,10)
print (np.copysign(np.ceil(np.abs(Z)), Z))
```

    [ -2.   5.   5. -10.  -9. -10.   6.   1.  -3.  -7.]

```python
# 30 
# np.intersectld()：返回的是两个数组中经过排序后的相同元素
# Return the sorted, unique values that are in both of the input arrays.
z1 = np.random.randint(0,10,10)
z2 = np.random.randint(0,10,10)
print(np.intersect1d(z1,z2))
```

    [0 2 3 6 7]

```python
# 31
# How to ignore all numpy warnings 
defaults = np.seterr(all="ignore")
Z = np.ones(1) / 0

# Back to sanity
_ = np.seterr(**defaults)

with np.errstate(divide='ignore'):
    Z = np.ones(1) / 0
```


```python
# 32 
np.sqrt(-1) == np.emath.sqrt(-1)   
```


    False


```python
# 33 获得时间日期：昨天、今天，明天
yesterday = np.datetime64('today', 'D') - np.timedelta64(1, 'D')
today = np.datetime64('today', 'D')
tomorrow = np.datetime64('today', 'D') + np.timedelta64(1, 'D')
print(yesterday)
print(today)
print(tomorrow)
```

    2019-10-13
    2019-10-14
    2019-10-15

```python
# 34  获取某个时间段内的所有日期：含头不含尾
np.arange('2016-07', '2016-07-27', dtype='datetime64[D]')
```


    array(['2016-07-01', '2016-07-02', '2016-07-03', '2016-07-04',
           '2016-07-05', '2016-07-06', '2016-07-07', '2016-07-08',
           '2016-07-09', '2016-07-10', '2016-07-11', '2016-07-12',
           '2016-07-13', '2016-07-14', '2016-07-15', '2016-07-16',
           '2016-07-17', '2016-07-18', '2016-07-19', '2016-07-20',
           '2016-07-21', '2016-07-22', '2016-07-23', '2016-07-24',
           '2016-07-25', '2016-07-26'], dtype='datetime64[D]')


```python
# 35  计算两个数组之间的数量关系
a = np.ones(3) * 1
b = np.ones(3) * 2
c = np.ones(3) * 3   
```


```python
np.add(a,b,out=a)
```


    array([3., 3., 3.])


```python
np.divide(a,2,out=a)
```


    array([1.5, 1.5, 1.5])


```python
np.negative(a,out=a)
```


    array([-1.5, -1.5, -1.5])


```python
# 36
Z = np.random.uniform(0,10,10)  # 随机生成10个数字

print(Z)
print (Z - Z%1)  # 取整数
print (np.floor(Z))  
print (np.ceil(Z)-1)
print (Z.astype(int))  # 取整后为int类型
print (np.trunc(Z))
```

    [3.95803958 1.32403183 8.64610951 0.83095194 4.94494921 9.23832181
     0.37511147 8.1509876  9.82676114 8.29316378]
    [3. 1. 8. 0. 4. 9. 0. 8. 9. 8.]
    [3. 1. 8. 0. 4. 9. 0. 8. 9. 8.]
    [3. 1. 8. 0. 4. 9. 0. 8. 9. 8.]
    [3 1 8 0 4 9 0 8 9 8]
    [3. 1. 8. 0. 4. 9. 0. 8. 9. 8.]

```python
# 37 
z = np.zeros((5,5))   # 5*5的全0方阵
z += np.arange(5)    # np.arange(5) 生成一个向量0-4
print(z)
```

    [[0. 1. 2. 3. 4.]
     [0. 1. 2. 3. 4.]
     [0. 1. 2. 3. 4.]
     [0. 1. 2. 3. 4.]
     [0. 1. 2. 3. 4.]]

```python
np.arange(5)
```


    array([0, 1, 2, 3, 4])


```python
# 38
def generate():
    for x in range(10):
        yield x
#         return x

z = np.fromiter(generate(), dtype=float, count=-1)
print(z)
```

    [0. 1. 2. 3. 4. 5. 6. 7. 8. 9.]

```python
# 39
# linspace()的用法
Z = np.linspace(0,1,11,endpoint=False)[1:]   # 生成10个0-1的数字，不包含尾部
print(Z)
```

    [0.09090909 0.18181818 0.27272727 0.36363636 0.45454545 0.54545455
     0.63636364 0.72727273 0.81818182 0.90909091]

```python
# 40 
z = np.random.random(10)
z.sort()  # 排序之后输出
print(z)
```

    [0.09537726 0.11914916 0.12808735 0.28657504 0.32288421 0.39927615
     0.64877625 0.66318512 0.75063225 0.87013168]



```python
# 41
z = np.arange(10)
print(sum(z))
np.add.reduce(z)   # 速度比sum()函数快很多
```

    45
    45


```python
# 42 检验两个数组是否相等equal
a = np.random.randint(0,2,5)
b = np.random.randint(0,2,5)

equal = np.allclose(a, b)
print(equal)

equal = np.array_equal(a, b)
print(equal)
```

    False
    False

```python
# 43 创建一个只读矩阵
Z = np.zeros(10)
Z.flags.writeable = False
print(Z)
# Z[0]=1   无法修改数据，只是可读状态
```

    [0. 0. 0. 0. 0. 0. 0. 0. 0. 0.]

```python
# 44
z = np.random.random((10,2))  # 生成 10*2 的矩阵
x,y = z[:, 0],z[:, 1]   # 取出第一列和第二列的数据
r = np.sqrt(x**2+y**2)
t = np.arctan2(y,x)
print(z)
print(r)
print(t)
```

    [[0.54870349 0.71706842]
     [0.2816144  0.06322106]
     [0.85545704 0.29143293]
     [0.92798256 0.09136097]
     [0.61973184 0.38362868]
     [0.0454154  0.37239676]
     [0.25334226 0.25988723]
     [0.74275296 0.92348859]
     [0.35936247 0.84002203]
     [0.23819685 0.94926246]]
     
    [0.90291895 0.28862358 0.90373663 0.93246901 0.72886111 0.37515584
     0.36293756 1.18512157 0.91366208 0.97869145]
     
    [0.91763557 0.22083385 0.32834355 0.09813492 0.55429018 1.44944125
     0.79814999 0.89344479 1.1665554  1.32494413]

```python
# 45 将数组中最大数用0代替
z = np.random.random(10)
z[z.argmax()]= 0
print(z)
```

    [0.30695133 0.14808481 0.78878546 0.15906783 0.78569492 0.66986127
     0.5942439  0.14547699 0.         0.5825813 ]

```python
z.argmax()  # 找出最大数
```


    2

