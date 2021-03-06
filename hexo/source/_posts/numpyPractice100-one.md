---
title: numpyPractice100_one
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-26 00:33:53
password:
summary:
copyright: true
tags:
  - python库
  - 可视化
  - numpy
categories:
  - numpy
  - 100题
---

> 这个100题系列来源于`github上`的题目，分为`4`天完成，主要介绍的是`numpy`的常见操作。

```python
# 导入numpy包
import numpy as np
```

```python
# 查看版本
print(np.__version__)
```

```
1.16.3
```

```python
# 创造全0矩阵
Z = np.zeros((5,5))
Z
```

```
array([[0., 0., 0., 0., 0.],
       [0., 0., 0., 0., 0.],
       [0., 0., 0., 0., 0.],
       [0., 0., 0., 0., 0.],
       [0., 0., 0., 0., 0.]])
```

```python
# 查看数组内存大小
Z = np.zeros((5,5))
print("%d bytes" % (Z.size * Z.itemsize))
```

<!-- MORE -->

```
200 bytes
```

```python
Z.itemsize
```

```
8
```

```python
# 修改数组指定位置的值
Z = np.zeros(10)
Z[4] = 5
Z
```

```
array([0., 0., 0., 0., 5., 0., 0., 0., 0., 0.])
```

```python
# 如何反转数组
Z = np.arange(10,21)
Z = Z[::-1]   # 实现反转功能
Z
```

```
array([20, 19, 18, 17, 16, 15, 14, 13, 12, 11, 10])
```

```python
# 改变数组形状
Z = np.arange(9).reshape(3,3)
Z
```

```
array([[0, 1, 2],
       [3, 4, 5],
       [6, 7, 8]])
```

```python
# 创建非0数组：自动将数组中的0去掉
nz = np.nonzero([1,2,0,0,4,0])
nz
```

```
(array([0, 1, 4], dtype=int64),)
```

```python
# 创建单位矩阵
Z = np.eye(3)   
Z
```

```
array([[1., 0., 0.],
       [0., 1., 0.],
       [0., 0., 1.]])
```

```python
# 找出数组中的最大和最小值
Z = np.random.random((10,10))  # random生成的是0-1之间的数
Zmin, Zmax = Z.min(), Z.max()   # 找出最小和最大值
print(Zmin, Zmax)

```

```
0.00804609668099976 0.9958444566621576
```

```python
 # 求平均值
z = np.random.random(30)
m = z.mean() 
m
```

```
0.5543031427314672
```

```python
# 行中的索引为1到索引为-2（含头不含尾），列同样，赋值为0
z = np.ones((10,10))
z[1:-1, 1:-1] = 0    #  行和列的指定位置同时赋值为0
z
```

```
array([[1., 1., 1., 1., 1., 1., 1., 1., 1., 1.],
       [1., 0., 0., 0., 0., 0., 0., 0., 0., 1.],
       [1., 0., 0., 0., 0., 0., 0., 0., 0., 1.],
       [1., 0., 0., 0., 0., 0., 0., 0., 0., 1.],
       [1., 0., 0., 0., 0., 0., 0., 0., 0., 1.],
       [1., 0., 0., 0., 0., 0., 0., 0., 0., 1.],
       [1., 0., 0., 0., 0., 0., 0., 0., 0., 1.],
       [1., 0., 0., 0., 0., 0., 0., 0., 0., 1.],
       [1., 0., 0., 0., 0., 0., 0., 0., 0., 1.],
       [1., 1., 1., 1., 1., 1., 1., 1., 1., 1.]])
```

```python
# 数组外围的填充
Z = np.ones((5,5))  # 全1矩阵
# 将5*5变成7*7的矩阵，外围填充的数字是0，模式是常量
Z = np.pad(Z, pad_width=2, mode='constant', constant_values=0) 
Z
```

```
array([[0., 0., 0., 0., 0., 0., 0., 0., 0.],
       [0., 0., 0., 0., 0., 0., 0., 0., 0.],
       [0., 0., 1., 1., 1., 1., 1., 0., 0.],
       [0., 0., 1., 1., 1., 1., 1., 0., 0.],
       [0., 0., 1., 1., 1., 1., 1., 0., 0.],
       [0., 0., 1., 1., 1., 1., 1., 0., 0.],
       [0., 0., 1., 1., 1., 1., 1., 0., 0.],
       [0., 0., 0., 0., 0., 0., 0., 0., 0.],
       [0., 0., 0., 0., 0., 0., 0., 0., 0.]])

```

```python
# nan的神器之处
print(0 * np.nan)   # nan
print(np.nan == np.nan)  # F
print(np.inf > np.nan)   # F
print(np.nan - np.nan)   # nan 
print(np.nan in set([np.nan])) # T
print(0.3 == 3 * 0.1)  # F

```

```
nan
False
False
nan
True
False

```

```python
Z = np.diag(1+np.arange(4),k=1)
print(Z)
```

```
[[0 1 0 0 0]
 [0 0 2 0 0]
 [0 0 0 3 0]
 [0 0 0 0 4]
 [0 0 0 0 0]]
```

```python
Z = np.diag(1+np.arange(4),k=-1)  # 观察k值的变化
print(Z)
```

```
[[0 0 0 0 0]
 [1 0 0 0 0]
 [0 2 0 0 0]
 [0 0 3 0 0]
 [0 0 0 4 0]]
```

```python
Z = np.diag(1+np.arange(4),k=-2)
print(Z)
```

```
[[0 0 0 0 0 0]
 [0 0 0 0 0 0]
 [1 0 0 0 0 0]
 [0 2 0 0 0 0]
 [0 0 3 0 0 0]
 [0 0 0 4 0 0]]
```

```python
Z = np.zeros((8,8),dtype=int)
Z[1::2,::2] = 1
Z[::2,1::2] = 1
print(Z)
```

```
[[0 1 0 1 0 1 0 1]
 [1 0 1 0 1 0 1 0]
 [0 1 0 1 0 1 0 1]
 [1 0 1 0 1 0 1 0]
 [0 1 0 1 0 1 0 1]
 [1 0 1 0 1 0 1 0]
 [0 1 0 1 0 1 0 1]
 [1 0 1 0 1 0 1 0]]
```

```python
print(np.unravel_index(99,(6,7,8)))
```

```
(1, 5, 3)
```

```python
Z = np.tile( np.array([[0,1],[1,0]]), (4,4))
print(Z)
```

```
[[0 1 0 1 0 1 0 1]
 [1 0 1 0 1 0 1 0]
 [0 1 0 1 0 1 0 1]
 [1 0 1 0 1 0 1 0]
 [0 1 0 1 0 1 0 1]
 [1 0 1 0 1 0 1 0]
 [0 1 0 1 0 1 0 1]
 [1 0 1 0 1 0 1 0]]

```

```python
Z = np.random.random((5,5))
Z = (Z - np.mean (Z)) / (np.std (Z))  # 标准分布
print(Z)
```

```
[[ 1.41352327 -1.10568196  0.42998927 -1.13603058  1.6252364 ]
 [-1.38790781 -0.38326045 -0.48561531 -0.91306084  0.99511305]
 [-0.95801467  0.72817058  0.42855968  0.40835484  1.69929003]
 [ 0.36861659  0.05906306 -1.31997441 -0.92392559  1.00674601]
 [ 0.69583865 -0.71638163 -1.14414993 -0.83217088  1.44767262]]

```

```python
# 数组的点乘
Z = np.dot(np.ones((5,3)), np.ones((3,2)))  # dot表示点乘
print(Z)

print("-----------")

# Alternative solution, in Python 3.5 and above
Z = np.ones((5,3)) @ np.ones((3,2))
print(Z)
```

```
[[3. 3.]
 [3. 3.]
 [3. 3.]
 [3. 3.]
 [3. 3.]]
-----------
[[3. 3.]
 [3. 3.]
 [3. 3.]
 [3. 3.]
 [3. 3.]]
```

```python
# 条件表达式的使用
Z = np.arange(11)
Z[(3 < Z) & (Z <= 8)] *= -1
print(Z)
```

```
[ 0  1  2  3 -4 -5 -6 -7 -8  9 10]
```

```python
Z = np.arange(11)
Z[(3 < Z)] = 10   # 将大于3的赋值为10
print(Z)
```

```
[ 0  1  2  3 10 10 10 10 10 10 10]
```