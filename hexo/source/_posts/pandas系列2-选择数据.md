---
title: pandas系列2_选择数据
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-04 20:17:32
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

### 选择数据

如何从众多数据选择出我们所需要的数据，是数据分析中重点。本文中使用的方法

> - `loc`：通过标签获取，等同于`.at`
> - `iloc`：通过数字索引获取，等同于`.iat`

#### 总结

- `df.loc[[......]]`：可以使用数字索引，也可以使用标签索引，还可以用切片的形式

- `df.iloc[[.....]]`：只能使用数字索引，可以是非连续或者连续（等差形式也OK）

- 布尔索引：`df2[df2['E'].isin(['two', 'four'])]`

- 同时指定行和列：

  - `df.loc[:, ["A","B"]]`

  - `df.iloc[[1, 2, 4], [0, 2]]`

    <!--MORE--> 

#### 查看指定的行列数据

```python
# 指定列属性查看数据，多个列属性放在列表中
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
      <td>-0.362323</td>
      <td>1.678106</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>-2.261810</td>
      <td>-1.035994</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-1.472062</td>
      <td>1.081443</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.292800</td>
      <td>-0.593975</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>0.002751</td>
      <td>-0.233792</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>1.001527</td>
      <td>1.521685</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 通过切片形式，指定行标签查看指定的行数据
df[1:3]   # 默认数字索引
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
      <td>1.438213</td>
      <td>-2.261810</td>
      <td>-1.035994</td>
      <td>0.433404</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>-1.472062</td>
      <td>1.081443</td>
      <td>1.109993</td>
    </tr>
  </tbody>
</table>

</div>

```python
df["20190924":"20190927"]     # 使用的是创建的索引
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
      <td>-0.693593</td>
      <td>-0.362323</td>
      <td>1.678106</td>
      <td>-1.180693</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>1.438213</td>
      <td>-2.261810</td>
      <td>-1.035994</td>
      <td>0.433404</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>-1.472062</td>
      <td>1.081443</td>
      <td>1.109993</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>-0.189173</td>
      <td>0.292800</td>
      <td>-0.593975</td>
      <td>-0.171872</td>
    </tr>
  </tbody>
</table>

</div>



#### `loc` 标签索引

根据标签(不是自带的数字索引)查看数据

```python
df.loc[dates[0]]
```

```
A   -0.693593
B   -0.362323
C    1.678106
D   -1.180693
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
df.loc[:, ["A","B"]]   # 选择所有行，然后AB两个列
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
      <td>-0.693593</td>
      <td>-0.362323</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>1.438213</td>
      <td>-2.261810</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>-1.472062</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>-0.189173</td>
      <td>0.292800</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>0.561579</td>
      <td>0.002751</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>-1.037907</td>
      <td>1.001527</td>
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
      <td>-0.693593</td>
      <td>-0.362323</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>1.438213</td>
      <td>-2.261810</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>-1.472062</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>-0.189173</td>
      <td>0.292800</td>
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
      <td>-0.693593</td>
      <td>-0.362323</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>1.438213</td>
      <td>-2.261810</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>-1.472062</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>-0.189173</td>
      <td>0.292800</td>
    </tr>
  </tbody>
</table>

</div>



#### `iloc`数字索引

记忆放法：`iloc`记为`intloc`，`int`为整型，表示通过数字来进行索引

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
      <td>-0.693593</td>
      <td>-0.362323</td>
      <td>1.678106</td>
      <td>-1.180693</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>1.438213</td>
      <td>-2.261810</td>
      <td>-1.035994</td>
      <td>0.433404</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>-1.472062</td>
      <td>1.081443</td>
      <td>1.109993</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>-0.189173</td>
      <td>0.292800</td>
      <td>-0.593975</td>
      <td>-0.171872</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>0.561579</td>
      <td>0.002751</td>
      <td>-0.233792</td>
      <td>1.624140</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>-1.037907</td>
      <td>1.001527</td>
      <td>1.521685</td>
      <td>-0.049556</td>
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
      <td>1.438213</td>
      <td>-2.261810</td>
      <td>-1.035994</td>
      <td>0.433404</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>-1.472062</td>
      <td>1.081443</td>
      <td>1.109993</td>
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
      <td>1.438213</td>
      <td>-2.261810</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>-1.472062</td>
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
      <td>1.438213</td>
      <td>-1.035994</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>1.081443</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>0.561579</td>
      <td>-0.233792</td>
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
      <td>-0.362323</td>
      <td>1.678106</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>-2.261810</td>
      <td>-1.035994</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>-1.472062</td>
      <td>1.081443</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.292800</td>
      <td>-0.593975</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>0.002751</td>
      <td>-0.233792</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>1.001527</td>
      <td>1.521685</td>
    </tr>
  </tbody>
</table>

</div>

#### 获取具体位置的元素

```python
df.iloc[1,2]
```

```
-1.035993773960585
```

```python
df.iat[1,2]    # 等同上面
```

```
-1.035993773960585
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
      <td>-0.693593</td>
      <td>-0.362323</td>
      <td>1.678106</td>
      <td>-1.180693</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>1.438213</td>
      <td>-2.261810</td>
      <td>-1.035994</td>
      <td>0.433404</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>-1.472062</td>
      <td>1.081443</td>
      <td>1.109993</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>-0.189173</td>
      <td>0.292800</td>
      <td>-0.593975</td>
      <td>-0.171872</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>0.561579</td>
      <td>0.002751</td>
      <td>-0.233792</td>
      <td>1.624140</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>-1.037907</td>
      <td>1.001527</td>
      <td>1.521685</td>
      <td>-0.049556</td>
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
      <th>2019-09-25</th>
      <td>1.438213</td>
      <td>-2.261810</td>
      <td>-1.035994</td>
      <td>0.433404</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>-1.472062</td>
      <td>1.081443</td>
      <td>1.109993</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>0.561579</td>
      <td>0.002751</td>
      <td>-0.233792</td>
      <td>1.624140</td>
    </tr>
  </tbody>
</table>

</div>

```python
df[df > 0]   # 满足bool条件的DF中选取值
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
      <td>NaN</td>
      <td>NaN</td>
      <td>1.678106</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>1.438213</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>0.433404</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>NaN</td>
      <td>1.081443</td>
      <td>1.109993</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>NaN</td>
      <td>0.292800</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>0.561579</td>
      <td>0.002751</td>
      <td>NaN</td>
      <td>1.624140</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>NaN</td>
      <td>1.001527</td>
      <td>1.521685</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

</div>

#### `isin`方法过滤

```python
df2 = df.copy()
```

```python
df2['E'] = ['one', 'one', 'two', 'three', 'four', 'three']
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-24</th>
      <td>-0.693593</td>
      <td>-0.362323</td>
      <td>1.678106</td>
      <td>-1.180693</td>
      <td>one</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>1.438213</td>
      <td>-2.261810</td>
      <td>-1.035994</td>
      <td>0.433404</td>
      <td>one</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>-1.472062</td>
      <td>1.081443</td>
      <td>1.109993</td>
      <td>two</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>-0.189173</td>
      <td>0.292800</td>
      <td>-0.593975</td>
      <td>-0.171872</td>
      <td>three</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>0.561579</td>
      <td>0.002751</td>
      <td>-0.233792</td>
      <td>1.624140</td>
      <td>four</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>-1.037907</td>
      <td>1.001527</td>
      <td>1.521685</td>
      <td>-0.049556</td>
      <td>three</td>
    </tr>
  </tbody>
</table>

</div>

```python
df2['E'].isin(['two', 'four'])    # 得到bool值
```

```
2019-09-24    False
2019-09-25    False
2019-09-26     True
2019-09-27    False
2019-09-28     True
2019-09-29    False
Freq: D, Name: E, dtype: bool
```

```python
df2[df2['E'].isin(['two', 'four'])]  
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
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>-1.472062</td>
      <td>1.081443</td>
      <td>1.109993</td>
      <td>two</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>0.561579</td>
      <td>0.002751</td>
      <td>-0.233792</td>
      <td>1.624140</td>
      <td>four</td>
    </tr>
  </tbody>
</table>

</div>

#### 生成新的列属性

```python
df2['F'] = df2['A'] + df['B']    # 只能通过类似字典的形式，不能通过对象的属性形式
```

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
      <th>2019-09-24</th>
      <td>-0.693593</td>
      <td>-0.362323</td>
      <td>1.678106</td>
      <td>-1.180693</td>
      <td>one</td>
      <td>-1.055915</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>1.438213</td>
      <td>-2.261810</td>
      <td>-1.035994</td>
      <td>0.433404</td>
      <td>one</td>
      <td>-0.823597</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.710651</td>
      <td>-1.472062</td>
      <td>1.081443</td>
      <td>1.109993</td>
      <td>two</td>
      <td>0.238589</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>-0.189173</td>
      <td>0.292800</td>
      <td>-0.593975</td>
      <td>-0.171872</td>
      <td>three</td>
      <td>0.103627</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>0.561579</td>
      <td>0.002751</td>
      <td>-0.233792</td>
      <td>1.624140</td>
      <td>four</td>
      <td>0.564329</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>-1.037907</td>
      <td>1.001527</td>
      <td>1.521685</td>
      <td>-0.049556</td>
      <td>three</td>
      <td>-0.036380</td>
    </tr>
  </tbody>
</table>

</div>

