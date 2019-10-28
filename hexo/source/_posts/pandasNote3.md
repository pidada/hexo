---
title: pandasNote3
date: 2019-09-10 08:33:59
tags: 
  - pandas
  - python库
categories: 
  - pandas
  - Python-for-data 
copyright: true
---



```python
import numpy as np
import pandas as pd
from pandas import Series, DataFrame
```

### 主要内容

- 填充值处理 fill_value
- 翻转功能r
- 广播机制
- apply和applymap
- 排名和排序
- 重复标签
- 汇总和统计

### 填充值处理

- 使用add方法，填入`fill_value`参数，添加指定元素
- 翻转参数，用r实现
  - add/radd
  - sub/rsub
  - div/rdiv
  - floor/rfloor
  - mul/rmul
  - pow/rpow

<!-- MORE -->

```python
df1 = pd.DataFrame(np.arange(12.).reshape((3,4)),
                   columns=list("abcd"))
```

```python
df2 = pd.DataFrame(np.arange(20.).reshape((4,5)),
                             columns=list("abcde"))
```

```python
df1
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>7.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8.0</td>
      <td>9.0</td>
      <td>10.0</td>
      <td>11.0</td>
    </tr>
  </tbody>
</table>

</div>

```python
df2
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>8.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10.0</td>
      <td>11.0</td>
      <td>12.0</td>
      <td>13.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>15.0</td>
      <td>16.0</td>
      <td>17.0</td>
      <td>18.0</td>
      <td>19.0</td>
    </tr>
  </tbody>
</table>

</div>

```python
df1+df2
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9.0</td>
      <td>11.0</td>
      <td>13.0</td>
      <td>15.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>20.0</td>
      <td>22.0</td>
      <td>24.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 使用add方法，填入fill_value参数
df1.add(df2, fill_value=0)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>6.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9.0</td>
      <td>11.0</td>
      <td>13.0</td>
      <td>15.0</td>
      <td>9.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>18.0</td>
      <td>20.0</td>
      <td>22.0</td>
      <td>24.0</td>
      <td>14.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>15.0</td>
      <td>16.0</td>
      <td>17.0</td>
      <td>18.0</td>
      <td>19.0</td>
    </tr>
  </tbody>
</table>

</div>

```python
# r翻转实现
1 / df1
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>inf</td>
      <td>1.000000</td>
      <td>0.500000</td>
      <td>0.333333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.250</td>
      <td>0.200000</td>
      <td>0.166667</td>
      <td>0.142857</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.125</td>
      <td>0.111111</td>
      <td>0.100000</td>
      <td>0.090909</td>
    </tr>
  </tbody>
</table>

</div>

```python
df1.rdiv(1)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>inf</td>
      <td>1.000000</td>
      <td>0.500000</td>
      <td>0.333333</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.250</td>
      <td>0.200000</td>
      <td>0.166667</td>
      <td>0.142857</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.125</td>
      <td>0.111111</td>
      <td>0.100000</td>
      <td>0.090909</td>
    </tr>
  </tbody>
</table>

</div>

```python
df1.reindex(columns=df2.columns, fill_value=0)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
      <td>7.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8.0</td>
      <td>9.0</td>
      <td>10.0</td>
      <td>11.0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>

</div>

--------

### 广播机制

Series和DataFrame之间进行运算的时候，会作用于每行。

- 二维数组和一维向量之间
- DF数据和S型数据之间

```python
arr = np.arange(12.).reshape((3, 4))
arr
```

```
array([[ 0.,  1.,  2.,  3.],
       [ 4.,  5.,  6.,  7.],
       [ 8.,  9., 10., 11.]])
```

```python
arr[0]
```

```
array([0., 1., 2., 3.])
```

```python
# 每行都要执行减操作
arr - arr[0]
```

```
array([[0., 0., 0., 0.],
       [4., 4., 4., 4.],
       [8., 8., 8., 8.]])
```

```python
# DF和S型数据 
frame = pd.DataFrame(np.arange(12.).reshape((4, 3)),
                     columns=list('bde'),
                     index=['Utah', 'Ohio', 'Texas', 'Oregon'])
frame
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Utah</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>Ohio</th>
      <td>3.0</td>
      <td>4.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>6.0</td>
      <td>7.0</td>
      <td>8.0</td>
    </tr>
    <tr>
      <th>Oregon</th>
      <td>9.0</td>
      <td>10.0</td>
      <td>11.0</td>
    </tr>
  </tbody>
</table>

</div>

```python
series = frame.iloc[0]
series
```

```
b    0.0
d    1.0
e    2.0
Name: Utah, dtype: float64
```

```python
frame - series
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Utah</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>Ohio</th>
      <td>3.0</td>
      <td>3.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>6.0</td>
      <td>6.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>Oregon</th>
      <td>9.0</td>
      <td>9.0</td>
      <td>9.0</td>
    </tr>
  </tbody>
</table>

</div>

```python
series2 = pd.Series(range(3), index=['b', 'e', 'f'])
series2
```

```
b    0
e    1
f    2
dtype: int64
```

```python
# 相同的数据则直接相加，只在一个类型中存在，填充为NaN
frame + series2
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
      <th>f</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Utah</th>
      <td>0.0</td>
      <td>NaN</td>
      <td>3.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Ohio</th>
      <td>3.0</td>
      <td>NaN</td>
      <td>6.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>6.0</td>
      <td>NaN</td>
      <td>9.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Oregon</th>
      <td>9.0</td>
      <td>NaN</td>
      <td>12.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 在列上进行广播机制
series3 = frame["d"]
series3
```

```
Utah       1.0
Ohio       4.0
Texas      7.0
Oregon    10.0
Name: d, dtype: float64
```

```python
# axis="index"等价于axis=0
frame.sub(series3, axis="index")
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Utah</th>
      <td>-1.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>Ohio</th>
      <td>-1.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>-1.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>Oregon</th>
      <td>-1.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>

</div>

### ufuncs

- Numpy的元素级数组方法用在pandas操作中：abs\sum\cos\sin...
- apply()方法作用于DF型数据
- applymap()方法作用于S型数据

```python
frame = pd.DataFrame(np.random.randn(4, 3), columns=list('bde'),
                     index=['Utah', 'Ohio', 'Texas', 'Oregon'])
frame
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Utah</th>
      <td>0.644845</td>
      <td>0.878886</td>
      <td>0.505433</td>
    </tr>
    <tr>
      <th>Ohio</th>
      <td>-0.230029</td>
      <td>-0.268866</td>
      <td>-0.107758</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>0.429735</td>
      <td>0.016485</td>
      <td>-0.940844</td>
    </tr>
    <tr>
      <th>Oregon</th>
      <td>0.318921</td>
      <td>-0.910421</td>
      <td>0.945280</td>
    </tr>
  </tbody>
</table>

</div>

```python
# ufuncs函数
np.abs(frame)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Utah</th>
      <td>0.644845</td>
      <td>0.878886</td>
      <td>0.505433</td>
    </tr>
    <tr>
      <th>Ohio</th>
      <td>0.230029</td>
      <td>0.268866</td>
      <td>0.107758</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>0.429735</td>
      <td>0.016485</td>
      <td>0.940844</td>
    </tr>
    <tr>
      <th>Oregon</th>
      <td>0.318921</td>
      <td>0.910421</td>
      <td>0.945280</td>
    </tr>
  </tbody>
</table>

</div>

```python
np.exp(frame)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Utah</th>
      <td>1.905692</td>
      <td>2.408215</td>
      <td>1.657703</td>
    </tr>
    <tr>
      <th>Ohio</th>
      <td>0.794511</td>
      <td>0.764246</td>
      <td>0.897845</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>1.536850</td>
      <td>1.016622</td>
      <td>0.390298</td>
    </tr>
    <tr>
      <th>Oregon</th>
      <td>1.375642</td>
      <td>0.402355</td>
      <td>2.573535</td>
    </tr>
  </tbody>
</table>

</div>

```python
# apply()：将自己实现的函数直接传给apply方法，作用于每行，返回S型数据
func = lambda x: x.max() - x.min()  
frame.apply(func)
```

```
b    0.874874
d    1.789307
e    1.886124
dtype: float64
```

```python
def f(x):
    return pd.Series([x.min(), x.max()], index=['min', 'max'])

frame.apply(f)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>min</th>
      <td>-0.230029</td>
      <td>-0.910421</td>
      <td>-0.940844</td>
    </tr>
    <tr>
      <th>max</th>
      <td>0.644845</td>
      <td>0.878886</td>
      <td>0.945280</td>
    </tr>
  </tbody>
</table>

</div>

```python
# applymap()方法：S的方法
format_x = lambda x: '{:.2f}'.format(x)
frame.applymap(format_x)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>d</th>
      <th>e</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Utah</th>
      <td>0.64</td>
      <td>0.88</td>
      <td>0.51</td>
    </tr>
    <tr>
      <th>Ohio</th>
      <td>-0.23</td>
      <td>-0.27</td>
      <td>-0.11</td>
    </tr>
    <tr>
      <th>Texas</th>
      <td>0.43</td>
      <td>0.02</td>
      <td>-0.94</td>
    </tr>
    <tr>
      <th>Oregon</th>
      <td>0.32</td>
      <td>-0.91</td>
      <td>0.95</td>
    </tr>
  </tbody>
</table>

</div>



```python
# S数据的map方法
frame['e'].map(format_x)
```

```
Utah       0.51
Ohio      -0.11
Texas     -0.94
Oregon     0.95
Name: e, dtype: object
```



### 排序和排名

- sorting
- sort_index()：按照索引进行排序
  - axis指定行和列
  - ascending指定升序和降序
- sort_values()：
  - 按照值对S型数据进行排序：缺失值放到末尾
  - 对DF数据进行排序，通过by指定某个列属性
  - 多个列进行排序，传入名称的列表

```python
obj = pd.Series(range(4), index=['d', 'a', 'b', 'c'])
obj.sort_index()
```

```
a    1
b    2
c    3
d    0
dtype: int64
```

```python
# DF数据的排序
frame = pd.DataFrame(np.arange(8).reshape((2, 4)),
                     index=['three', 'one'],
                     columns=['d', 'a', 'b', 'c'])
# 默认是行排序，升序(one, three)
frame.sort_index()
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>d</th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>4</td>
      <td>5</td>
      <td>6</td>
      <td>7</td>
    </tr>
    <tr>
      <th>three</th>
      <td>0</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 指定列排序和降序（dcba）
frame.sort_index(axis=1, ascending=False)
```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}
```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>d</th>
      <th>c</th>
      <th>b</th>
      <th>a</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>three</th>
      <td>0</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>one</th>
      <td>4</td>
      <td>7</td>
      <td>6</td>
      <td>5</td>
    </tr>
  </tbody>
</table>

</div>

```python
# sort_values()
frame = pd.DataFrame({'b': [4, 7, -3, 2], 'a': [0, 1, 0, 1]})
frame.sort_values(by='b')

```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}

```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>a</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>-3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7</td>
      <td>1</td>
    </tr>
  </tbody>
</table>

</div>

```python
frame.sort_values(by=['a', 'b'])  # 同时指定多个列属性

```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}

```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>b</th>
      <th>a</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>-3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>0</th>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>7</td>
      <td>1</td>
    </tr>
  </tbody>
</table>

</div>

#### rank

- 排名从1开始
- 返回的是平均排名，打破平级关系,method参数
  - average：默认平均值
  - min：使用分组中的最小值
  - max：使用最大值
  - first：出现顺序
  - dense：
- obj.rank(ascending=False, method='min/max')指定降序和取排名的大小值

```python
obj = pd.Series([7, -5, 7, 4, 2, 0, 4])
obj.rank()

```

```
0    6.5
1    1.0
2    6.5
3    4.5
4    3.0
5    2.0
6    4.5
dtype: float64

```

```python
# 根据出现的顺序返回排名
obj.rank(method='first')

```

```
0    6.0
1    1.0
2    7.0
3    4.0
4    3.0
5    2.0
6    5.0
dtype: float64

```

```python
# 降序排列：[7,7,4,4,2,0,-5]，相同元素排名加1
obj.rank(ascending=False, method='min')  # 第一个7排名为1，取min值1

```

```
0    1.0
1    7.0
2    1.0
3    3.0
4    5.0
5    6.0
6    3.0
dtype: float64

```

```python
obj.rank(ascending=False, method='max')  # 第一个7排名为1，取max值2

```

```
0    2.0
1    7.0
2    2.0
3    4.0
4    5.0
5    6.0
6    4.0
dtype: float64

```

```python
obj.rank(ascending=False, method='first')

```

```
0    1.0
1    7.0
2    2.0
3    3.0
4    5.0
5    6.0
6    4.0
dtype: float64

```

```python
# 类似min方法，但是不加1，重复元素排名相同
obj.rank(ascending=False, method='dense')

```

```
0    1.0
1    5.0
2    1.0
3    2.0
4    3.0
5    4.0
6    2.0
dtype: float64

```



### 重复标签的索引

- 直接查看索引index的is_unique()属性
- 索引重复的标签返回多个值

### 汇总和统计

- sum()：返回含有列的和的S型数据
- 传⼊axis='columns'或axis=1将会按⾏进⾏求和
- axis=0：表示行；axis=1：表示列
- skipna：排除缺失值，默认值是True
- idxmax()/idxmin()：返回最大值或者最小值的索引
- describe：返回多个统计值

```python
df = pd.DataFrame([[1.4, np.nan], [7.1, -4.5],
                   [np.nan, np.nan], [0.75, -1.3]],
                  index=['a', 'b', 'c', 'd'],
                  columns=['one', 'two'])
df

```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}

```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>1.40</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>b</th>
      <td>7.10</td>
      <td>-4.5</td>
    </tr>
    <tr>
      <th>c</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>d</th>
      <td>0.75</td>
      <td>-1.3</td>
    </tr>
  </tbody>
</table>

</div>

```python
df.sum(axis='columns')

```

```
a    1.40
b    2.60
c    0.00
d   -0.55
dtype: float64

```

```python
# 含有一个NA值自动排除
df.mean(axis='columns', skipna=False)

```

```
a      NaN
b    1.300
c      NaN
d   -0.275
dtype: float64

```

```python
df.mean(axis='columns')

```

```
a    1.400
b    1.300
c      NaN
d   -0.275
dtype: float64

```

```python
df.idxmax()

```

```
one    b
two    d
dtype: object

```

```python
df

```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}

```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>1.40</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>b</th>
      <td>7.10</td>
      <td>-4.5</td>
    </tr>
    <tr>
      <th>c</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>d</th>
      <td>0.75</td>
      <td>-1.3</td>
    </tr>
  </tbody>
</table>

</div>

```python
df.idxmin()

```

```
one    d
two    b
dtype: object

```

```python
df.cumsum()  # 累积求和

```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}

```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>a</th>
      <td>1.40</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>b</th>
      <td>8.50</td>
      <td>-4.5</td>
    </tr>
    <tr>
      <th>c</th>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>d</th>
      <td>9.25</td>
      <td>-5.8</td>
    </tr>
  </tbody>
</table>

</div>

```python
df.describe()

```

<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

```
.dataframe tbody tr th {
    vertical-align: top;
}

.dataframe thead th {
    text-align: right;
}

```

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>one</th>
      <th>two</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>3.000000</td>
      <td>2.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>3.083333</td>
      <td>-2.900000</td>
    </tr>
    <tr>
      <th>std</th>
      <td>3.493685</td>
      <td>2.262742</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.750000</td>
      <td>-4.500000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.075000</td>
      <td>-3.700000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1.400000</td>
      <td>-2.900000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>4.250000</td>
      <td>-2.100000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>7.100000</td>
      <td>-1.300000</td>
    </tr>
  </tbody>
</table>

</div>



<CENTER><span style='font-size:2.5rem'>Stay Foolish Stay Hungry</span></CENTER>
