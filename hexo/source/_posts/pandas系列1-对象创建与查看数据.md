---
title: pandas系列1_对象创建及查看数据
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-26 08:18:00
password:
summary:
copyright: true
tags:
  - pandas
  - python库
  - 数据处理
  - 可视化
categories:
  - pandas
  - 文档
---

本文重点知识：

- 创建带有日期的索引：`dates = pd.date_range('20190924', periods=6)`
- head()、tail()
- 按轴排序：索引排序`sort_index`，默认是`ascending=True`升序
  - axis=0：行索引，可以用`index`
  - axis=1：列索引，可以用`columns`
- 按值排序：`df.sort_values(by='columns')`，默认升序

### 创建数据

```python
import numpy as np
import pandas as pd
```

```python
s = pd.Series([1, 3, 5, np.nan, 6, 89])
s
```

```
0     1.0
1     3.0
2     5.0
3     NaN
4     6.0
5    89.0
dtype: float64
```

```python
dates = pd.date_range('20190924', periods=6)
dates
```

```
DatetimeIndex(['2019-09-24', '2019-09-25', '2019-09-26', '2019-09-27',
               '2019-09-28', '2019-09-29'],
              dtype='datetime64[ns]', freq='D')
```

```python
df = pd.DataFrame(np.random.randn(6,4), index=dates, columns=list("ABCD"))
df
```

<!--MORE-->

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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
      <td>-0.758513</td>
      <td>-0.679173</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
      <td>-0.402183</td>
      <td>2.261182</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
      <td>0.324912</td>
      <td>-1.885101</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.310374</td>
      <td>-1.502089</td>
      <td>1.524401</td>
      <td>0.269953</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>-0.846488</td>
      <td>-0.235878</td>
      <td>1.398896</td>
      <td>-0.229573</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>0.975853</td>
      <td>-0.998395</td>
      <td>-0.514480</td>
      <td>-0.882704</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 同时创建多个不同的列
df2 = pd.DataFrame({'A': 1.,   #  某列的值相同
                    'B': pd.Timestamp('20130102'),  # 时间戳的创建
                    'C': pd.Series(1, index=list(range(4)), dtype='float32'),  # 某列值可以是S型数据
                    'D': np.array([3] * 4, dtype='int32'),  # 使用numpy数组
                    'E': pd.Categorical(["test", "train", "test", "train"]), # 不同的类
                    'F': 'foo'})  # 使用布尔值

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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
      <th>F</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>2013-01-02</td>
      <td>1.0</td>
      <td>3</td>
      <td>test</td>
      <td>foo</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>2013-01-02</td>
      <td>1.0</td>
      <td>3</td>
      <td>train</td>
      <td>foo</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1.0</td>
      <td>2013-01-02</td>
      <td>1.0</td>
      <td>3</td>
      <td>test</td>
      <td>foo</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1.0</td>
      <td>2013-01-02</td>
      <td>1.0</td>
      <td>3</td>
      <td>train</td>
      <td>foo</td>
    </tr>
  </tbody>
</table>

</div>

```python
df2.dtypes
```

```
A           float64
B    datetime64[ns]
C           float32
D             int32
E          category
F            object
dtype: object
```

### 查看数据

查看数据的相关信息

- 头、尾几行数据
- index、columns
- describe ,T

```python
# 前几行数据，默认是5行
df.head(3)
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
      <td>-0.758513</td>
      <td>-0.679173</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
      <td>-0.402183</td>
      <td>2.261182</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
      <td>0.324912</td>
      <td>-1.885101</td>
    </tr>
  </tbody>
</table>

</div>



```python
# 查看末几行，默认是5行
df.tail(2)
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-28</th>
      <td>-0.846488</td>
      <td>-0.235878</td>
      <td>1.398896</td>
      <td>-0.229573</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>0.975853</td>
      <td>-0.998395</td>
      <td>-0.514480</td>
      <td>-0.882704</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 查看数据的行索引
df.index
```

```
DatetimeIndex(['2019-09-24', '2019-09-25', '2019-09-26', '2019-09-27',
               '2019-09-28', '2019-09-29'],
              dtype='datetime64[ns]', freq='D')
```

```python
# 查看数据的列属性
df.columns
```

```
Index(['A', 'B', 'C', 'D'], dtype='object')

```

```python
df.to_numpy()
```

```
array([[ 0.50000505,  0.16657823, -0.75851253, -0.67917279],
       [ 0.09020909,  0.11790626, -0.40218323,  2.26118249],
       [-0.80905211, -0.17314443,  0.32491161, -1.88510066],
       [ 0.31037417, -1.50208882,  1.52440064,  0.26995285],
       [-0.8464884 , -0.23587797,  1.39889574, -0.22957318],
       [ 0.9758533 , -0.99839501, -0.51448041, -0.88270375]])
```

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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.036817</td>
      <td>-0.437504</td>
      <td>0.262172</td>
      <td>-0.190903</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.730717</td>
      <td>0.668113</td>
      <td>0.997562</td>
      <td>1.400993</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-0.846488</td>
      <td>-1.502089</td>
      <td>-0.758513</td>
      <td>-1.885101</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>-0.584237</td>
      <td>-0.807766</td>
      <td>-0.486406</td>
      <td>-0.831821</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.200292</td>
      <td>-0.204511</td>
      <td>-0.038636</td>
      <td>-0.454373</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.452597</td>
      <td>0.045144</td>
      <td>1.130400</td>
      <td>0.145071</td>
    </tr>
    <tr>
      <th>max</th>
      <td>0.975853</td>
      <td>0.166578</td>
      <td>1.524401</td>
      <td>2.261182</td>
    </tr>
  </tbody>
</table>

</div>

```python
df.T
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
      <th>2019-09-24 00:00:00</th>
      <th>2019-09-25 00:00:00</th>
      <th>2019-09-26 00:00:00</th>
      <th>2019-09-27 00:00:00</th>
      <th>2019-09-28 00:00:00</th>
      <th>2019-09-29 00:00:00</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>A</th>
      <td>0.500005</td>
      <td>0.090209</td>
      <td>-0.809052</td>
      <td>0.310374</td>
      <td>-0.846488</td>
      <td>0.975853</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.166578</td>
      <td>0.117906</td>
      <td>-0.173144</td>
      <td>-1.502089</td>
      <td>-0.235878</td>
      <td>-0.998395</td>
    </tr>
    <tr>
      <th>C</th>
      <td>-0.758513</td>
      <td>-0.402183</td>
      <td>0.324912</td>
      <td>1.524401</td>
      <td>1.398896</td>
      <td>-0.514480</td>
    </tr>
    <tr>
      <th>D</th>
      <td>-0.679173</td>
      <td>2.261182</td>
      <td>-1.885101</td>
      <td>0.269953</td>
      <td>-0.229573</td>
      <td>-0.882704</td>
    </tr>
  </tbody>
</table>

</div>



```python
# sort_index
# 索引排序`sort_index`，默认是`ascending=True`升序
# axis=0：行索引，可以用`index`
# axis=1：列索引，可以用`columns`
df.sort_index(axis=1)
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
      <td>-0.758513</td>
      <td>-0.679173</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
      <td>-0.402183</td>
      <td>2.261182</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
      <td>0.324912</td>
      <td>-1.885101</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.310374</td>
      <td>-1.502089</td>
      <td>1.524401</td>
      <td>0.269953</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>-0.846488</td>
      <td>-0.235878</td>
      <td>1.398896</td>
      <td>-0.229573</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>0.975853</td>
      <td>-0.998395</td>
      <td>-0.514480</td>
      <td>-0.882704</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 按照行索引进行降序
df.sort_index(axis=0, ascending=False)
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-29</th>
      <td>0.975853</td>
      <td>-0.998395</td>
      <td>-0.514480</td>
      <td>-0.882704</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>-0.846488</td>
      <td>-0.235878</td>
      <td>1.398896</td>
      <td>-0.229573</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.310374</td>
      <td>-1.502089</td>
      <td>1.524401</td>
      <td>0.269953</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
      <td>0.324912</td>
      <td>-1.885101</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
      <td>-0.402183</td>
      <td>2.261182</td>
    </tr>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
      <td>-0.758513</td>
      <td>-0.679173</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 按照某个属性进行排序
# 按照B属性的升序排列
df.sort_values(by="B")

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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-27</th>
      <td>0.310374</td>
      <td>-1.502089</td>
      <td>1.524401</td>
      <td>0.269953</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>0.975853</td>
      <td>-0.998395</td>
      <td>-0.514480</td>
      <td>-0.882704</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>-0.846488</td>
      <td>-0.235878</td>
      <td>1.398896</td>
      <td>-0.229573</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
      <td>0.324912</td>
      <td>-1.885101</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
      <td>-0.402183</td>
      <td>2.261182</td>
    </tr>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
      <td>-0.758513</td>
      <td>-0.679173</td>
    </tr>
  </tbody>
</table>

</div>

```python
df.sort_values(by=["B","C"])
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-27</th>
      <td>0.310374</td>
      <td>-1.502089</td>
      <td>1.524401</td>
      <td>0.269953</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>0.975853</td>
      <td>-0.998395</td>
      <td>-0.514480</td>
      <td>-0.882704</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>-0.846488</td>
      <td>-0.235878</td>
      <td>1.398896</td>
      <td>-0.229573</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
      <td>0.324912</td>
      <td>-1.885101</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
      <td>-0.402183</td>
      <td>2.261182</td>
    </tr>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
      <td>-0.758513</td>
      <td>-0.679173</td>
    </tr>
  </tbody>
</table>

</div>

### 选择数据

#### 查看指定的行列数据

```python
# 指定列属性查看数据
df[["B","C"]]

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
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>0.166578</td>
      <td>-0.758513</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.117906</td>
      <td>-0.402183</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.173144</td>
      <td>0.324912</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>-1.502089</td>
      <td>1.524401</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>-0.235878</td>
      <td>1.398896</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>-0.998395</td>
      <td>-0.514480</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 指定行标签查看指定的行数据
df[1:3]

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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
      <td>-0.402183</td>
      <td>2.261182</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
      <td>0.324912</td>
      <td>-1.885101</td>
    </tr>
  </tbody>
</table>

</div>

```python
df["20190924":"20190927"]

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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
      <td>-0.758513</td>
      <td>-0.679173</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
      <td>-0.402183</td>
      <td>2.261182</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
      <td>0.324912</td>
      <td>-1.885101</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.310374</td>
      <td>-1.502089</td>
      <td>1.524401</td>
      <td>0.269953</td>
    </tr>
  </tbody>
</table>

</div>

#### loc

根据标签(不是自带的数字索引)查看数据

```python
df.loc[dates[0]]
```

```
A    0.500005
B    0.166578
C   -0.758513
D   -0.679173
Name: 2019-09-24 00:00:00, dtype: float64

```

```python
dates[0]

```

```
Timestamp('2019-09-24 00:00:00', freq='D')

```

```python
# 选择行和列
df.loc[:, ["A","B"]]   # 选择所有行和AB两个列

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
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.310374</td>
      <td>-1.502089</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>-0.846488</td>
      <td>-0.235878</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>0.975853</td>
      <td>-0.998395</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 索引通过标签来实现
df.loc['20190924':'20190927', ['A', 'B']]

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
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.310374</td>
      <td>-1.502089</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 指定的行或者列可以是切片形式
df.loc['20190924':'20190927', 'A':'B']

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
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.310374</td>
      <td>-1.502089</td>
    </tr>
  </tbody>
</table>

</div>

#### iloc数字索引

记忆放法：`iloc`记为`intloc`，int为整型，表示通过数字来进行索引

```python
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
      <td>-0.758513</td>
      <td>-0.679173</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
      <td>-0.402183</td>
      <td>2.261182</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
      <td>0.324912</td>
      <td>-1.885101</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.310374</td>
      <td>-1.502089</td>
      <td>1.524401</td>
      <td>0.269953</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>-0.846488</td>
      <td>-0.235878</td>
      <td>1.398896</td>
      <td>-0.229573</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>0.975853</td>
      <td>-0.998395</td>
      <td>-0.514480</td>
      <td>-0.882704</td>
    </tr>
  </tbody>
</table>

</div>

```python
df.iloc[1:3]

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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
      <td>-0.402183</td>
      <td>2.261182</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
      <td>0.324912</td>
      <td>-1.885101</td>
    </tr>
  </tbody>
</table>

</div>

```python
df.iloc[1:3, 0:2]  # 切片形式，连续性
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
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
    </tr>
  </tbody>
</table>

</div>

```python
df.iloc[[1, 2, 4], [0, 2]]  # 行索引是离散的值

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
      <th>A</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>-0.402183</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>0.324912</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>-0.846488</td>
      <td>1.398896</td>
    </tr>
  </tbody>
</table>

</div>

```python
df.iloc[:, 1:3]  

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
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>0.166578</td>
      <td>-0.758513</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.117906</td>
      <td>-0.402183</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.173144</td>
      <td>0.324912</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>-1.502089</td>
      <td>1.524401</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>-0.235878</td>
      <td>1.398896</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>-0.998395</td>
      <td>-0.514480</td>
    </tr>
  </tbody>
</table>

</div>

#### 获取具体位置的元素

```python
df.iloc[1,2]
```

```
-0.4021832300071616
```

```python
df.iat[1,2]    # 等同上面
```

```
-0.4021832300071616

```

#### 布尔索引

```python
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
      <td>-0.758513</td>
      <td>-0.679173</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
      <td>-0.402183</td>
      <td>2.261182</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-0.809052</td>
      <td>-0.173144</td>
      <td>0.324912</td>
      <td>-1.885101</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.310374</td>
      <td>-1.502089</td>
      <td>1.524401</td>
      <td>0.269953</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>-0.846488</td>
      <td>-0.235878</td>
      <td>1.398896</td>
      <td>-0.229573</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>0.975853</td>
      <td>-0.998395</td>
      <td>-0.514480</td>
      <td>-0.882704</td>
    </tr>
  </tbody>
</table>

</div>

```python
df[df.A > 0]  # 将属性A中大于0的行全部选择出出来
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
      <td>-0.758513</td>
      <td>-0.679173</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
      <td>-0.402183</td>
      <td>2.261182</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.310374</td>
      <td>-1.502089</td>
      <td>1.524401</td>
      <td>0.269953</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>0.975853</td>
      <td>-0.998395</td>
      <td>-0.514480</td>
      <td>-0.882704</td>
    </tr>
  </tbody>
</table>

</div>

```python
df[df > 0]
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>0.500005</td>
      <td>0.166578</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>0.090209</td>
      <td>0.117906</td>
      <td>NaN</td>
      <td>2.261182</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.324912</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.310374</td>
      <td>NaN</td>
      <td>1.524401</td>
      <td>0.269953</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.398896</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>0.975853</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

</div>



![](https://s2.ax1x.com/2019/09/26/ueW7y4.png)