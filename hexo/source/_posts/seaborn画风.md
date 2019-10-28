---
title: seaborn画风和主题风格
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-23 11:32:52
password:
summary:
copyright: true
tags:
  - seaborn
  - data
  - 可视化
  - python
categories:
  - 可视化
---

本文中的主要知识点：

- `seaborn`画风的使用
- 怎么隐藏刻度线
- 多个子图怎么使用不同的风格
- 刻度轴上的数值大小和线条粗细设置

```python
import seaborn as sns   # seaborn是对matplotlib的基础上进行了封装
import numpy as np
import matplotlib as mpl
import matplotlib.pyplot as plt
%matplotlib inline
```

<!--MORE-->

----

### 默认画风


```python
def sinplot(flip=1):
    x = np.linspace(0, 14, 100)
    for i in range(1,7):
        plt.plot(x, np.sin(x + i * .5)* (7 - i)* flip)
```


```python
sinplot()  # 默认画风
```


![](https://s2.ax1x.com/2019/10/23/KJ5UXR.png)

```python
sns.set()
sinplot()
```


![png](https://s2.ax1x.com/2019/10/23/KJ5Nc9.png)


### 5种主题风格

- `darkgrid`
- `whitegrid`
- `dark`
- `white`
- `ticks`


```python
sns.set_style("whitegrid")   # 风格常用
data = np.random.normal(size=(20, 6)) + np.arange(6) / 2
sns.boxplot(data=data)
```


    <matplotlib.axes._subplots.AxesSubplot at 0x19234fef048>

![png](https://s2.ax1x.com/2019/10/23/KJ5GhF.png)

```python
sns.set_style('dark')   # 去掉默认的格子和刻度线
sinplot()
```


![png](https://s2.ax1x.com/2019/10/23/KJ5Yp4.png)

```python
sns.set_style('white')  # 背景变成白色
sinplot()
```


![png](https://s2.ax1x.com/2019/10/23/KJ5t1J.png)



```python
sns.set_style('ticks')    # 横纵坐标轴加上刻度 
sinplot()
```


![png](https://s2.ax1x.com/2019/10/23/KJ5dn1.png)


### despine参数使用

用于隐藏某个刻度轴

```
Signature:
sns.despine(
    fig=None,
    ax=None,
    top=True,
    right=True,
    left=False,
    bottom=False,
    offset=None,
    trim=False,
)
Docstring:
Remove the top and right spines from plot(s).
```


```python
sinplot()
sns.despine()  # 只保留XY轴上的刻度
```


![png](https://s2.ax1x.com/2019/10/23/KJ5w0x.png)



```python
sns.violinplot(data)
sns.despine(offset=10)   # 图形与刻度轴的距离
```


![png](https://s2.ax1x.com/2019/10/23/KJ5076.png)



```python
sns.violinplot(data)
sns.despine(offset=60)
```


![png](https://s2.ax1x.com/2019/10/23/KJ5DAK.png)



```python
sns.set_style("whitegrid")
sns.boxplot(data=data, palette="deep")
sns.despine(left=True)   # 隐藏左边的轴
```


![png](https://s2.ax1x.com/2019/10/23/KJ5rtO.png)



```python
sns.set_style("whitegrid")
sns.boxplot(data=data, palette="deep")
sns.despine(bottom=True)   
```


![png](https://s2.ax1x.com/2019/10/23/KJ5shD.png)


### 子图相关


```python
# 每个子图的风格不同

with sns.axes_style("darkgrid"):  # with语句中是指定的风格
    plt.subplot(211)
    sinplot()  # 调用函数
    
plt.subplot(212)   # 不在with里面是另一种风格
sinplot(-1)
```


![png](https://s2.ax1x.com/2019/10/23/KJ569e.png)


### 布局设置


```python
sns.set()
```


```python
sns.set_context("paper")
plt.figure(figsize=(8,6))
sinplot()
```


![png](https://s2.ax1x.com/2019/10/23/KJ5c1H.png)



```python
sns.set_context("talk")   # 线更粗 刻度值更大了点
plt.figure(figsize=(8,6))
sinplot()
```


![png](https://s2.ax1x.com/2019/10/23/KJ5gcd.png)



```python
sns.set_context("poster")   # 线更粗 刻度值更大了点
plt.figure(figsize=(8,6))
sinplot()
```


![png](https://s2.ax1x.com/2019/10/23/KJ52jA.png)



```python
sns.set_context("notebook", 
                font_scale=1.5,  # 刻度值大小 
                rc={"lines.linewidth":2.5}  # 线条粗细
               )
sinplot()
```


![png](https://s2.ax1x.com/2019/10/23/KJ5WnI.png)



```python
sns.set_context("notebook", 
                font_scale=2.5,  # 刻度值大小 
                rc={"lines.linewidth":4}  # 线条粗细
               )
sinplot()
```


![png](https://s2.ax1x.com/2019/10/23/KJ5fBt.png)














