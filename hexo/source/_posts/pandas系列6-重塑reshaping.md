---

title: pandas系列6-重塑reshaping
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-11 08:16:03
password:
summary:
copyright: true
tags:
  - pandas
  - 数据分析
  - 可视化
  - 重塑
categories:
  - pandas
  - 文档
---

> 重新排列表格型数据的基础运算称之为重塑`reshape`或者轴向旋转`pivot`

- `stack：`将数据的列旋转成行，AB由列属性变成行索引
- `unstack:`将数据的行旋转成列，AB由行索引变成列属性

### 重点知识

- stack和unstack的用法
- 如何实现行和列的位置互换

![](https://s2.ax1x.com/2019/10/11/uHmwVS.png)

![](https://s2.ax1x.com/2019/10/11/uHmRbT.png)

<!--MORE-->

### 层次化索引 `MultiIndex`

- 数据分散在不同的文件或者数据库中
- 层次化索引在⼀个轴上拥有多个（两个以上）索引级别
- 低维度形式处理高维度数据

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```

```python
data = pd.DataFrame(np.arange(6).reshape(2,3)
                   ,index=pd.Index(['Inhio','Colorado'], name='state')
                   ,columns=pd.Index(['one', 'two', 'three']
                   ,name='number'   # name 参数在column里面，不是单独的，相当于是给columns起名字
                     )    
                   )

data
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
      <th>number</th>
      <th>one</th>
      <th>two</th>
      <th>three</th>
    </tr>
    <tr>
      <th>state</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Inhio</th>
      <td>0</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>Colorado</th>
      <td>3</td>
      <td>4</td>
      <td>5</td>
    </tr>
  </tbody>
</table>

</div>

#### 列行的互转

```python
res = data.stack()   # 列转成行
res
```

```
state     number
Inhio     one       0
          two       1
          three     2
Colorado  one       3
          two       4
          three     5
dtype: int32
```

```python
res.index
```

```
MultiIndex(levels=[['Inhio', 'Colorado'], ['one', 'two', 'three']],
           codes=[[0, 0, 0, 1, 1, 1], [0, 1, 2, 0, 1, 2]],
           names=['state', 'number'])
```

```python
type(res)     # res 是S型对象
```

```
pandas.core.series.Series
```

```python
res.unstack()   # 行转成列：one、two、three 变成列属性
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
      <th>number</th>
      <th>one</th>
      <th>two</th>
      <th>three</th>
    </tr>
    <tr>
      <th>state</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Inhio</th>
      <td>0</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>Colorado</th>
      <td>3</td>
      <td>4</td>
      <td>5</td>
    </tr>
  </tbody>
</table>

</div>

```python
res.unstack(1)   # 默认操作最内层
# res.unstack()  
# res.unstack('number')  同上；恢复原状
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
      <th>number</th>
      <th>one</th>
      <th>two</th>
      <th>three</th>
    </tr>
    <tr>
      <th>state</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Inhio</th>
      <td>0</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>Colorado</th>
      <td>3</td>
      <td>4</td>
      <td>5</td>
    </tr>
  </tbody>
</table>

</div>

#### 实现行索引和列属性的位置互换

```python
res.unstack(0)   # 实现了将行索引和列属性的位置互换
# res.unstack('state')    同上功能
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
      <th>state</th>
      <th>Inhio</th>
      <th>Colorado</th>
    </tr>
    <tr>
      <th>number</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>two</th>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>three</th>
      <td>2</td>
      <td>5</td>
    </tr>
  </tbody>
</table>

</div>

```python
s1 = pd.Series([0,1,2,3], index=['a','b','c','d'])
s2 = pd.Series([4,5,6], index=['c','d','e'])
```

```python
data1 = pd.concat([s1,s2], keys=['one', 'two'])
data1
```

```
one  a    0
     b    1
     c    2
     d    3
two  c    4
     d    5
     e    6
dtype: int64
```

```python
data1.unstack()  # 行索引转成列属性，unstack引入缺失值
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
      <th>one</th>
      <td>0.0</td>
      <td>1.0</td>
      <td>2.0</td>
      <td>3.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>two</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>4.0</td>
      <td>5.0</td>
      <td>6.0</td>
    </tr>
  </tbody>
</table>

</div>

```python
data1.unstack().stack()   # 操作可逆，stack操作默认过滤缺失值
```

```
one  a    0.0
     b    1.0
     c    2.0
     d    3.0
two  c    4.0
     d    5.0
     e    6.0
dtype: float64
```

```python
data1.unstack().stack(dropna=False)   # 通过dropna参数保留缺失值
```

```
one  a    0.0
     b    1.0
     c    2.0
     d    3.0
     e    NaN
two  a    NaN
     b    NaN
     c    4.0
     d    5.0
     e    6.0
dtype: float64
```

### 官网demo

```python
tuples = list(zip(*[['bar', 'bar', 'baz', 'baz',
                     'foo', 'foo', 'qux', 'qux'],
                    ['one', 'two', 'one', 'two',
                     'one', 'two', 'one', 'two']]))
tuples
```

```
[('bar', 'one'),
 ('bar', 'two'),
 ('baz', 'one'),
 ('baz', 'two'),
 ('foo', 'one'),
 ('foo', 'two'),
 ('qux', 'one'),
 ('qux', 'two')]
```

#### `zip`用法

```python
zip(*[['bar', 'bar', 'baz', 'baz',
   'foo', 'foo', 'qux', 'qux'],
  ['one', 'two', 'one', 'two',
   'one', 'two', 'one', 'two']])
```

```
<zip at 0x1d68ccb0d08>
```

```python
list(zip(*[["name", "age", "fee"],    # zip函数的用法
         ["xiaoming", 18, "28.22"]]))
```

```
[('name', 'xiaoming'), ('age', 18), ('fee', '28.22')]
```

```python
index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second'])  # 错层次索引如何创建
```

```python
# index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second'])
index
```

```
MultiIndex(levels=[['bar', 'baz', 'foo', 'qux'], ['one', 'two']],
           codes=[[0, 0, 1, 1, 2, 2, 3, 3], [0, 1, 0, 1, 0, 1, 0, 1]],
           names=['first', 'second'])
```

```python
df = pd.DataFrame(np.random.randn(8, 2), index=index, columns=['A', 'B'])
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
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
    <tr>
      <th>first</th>
      <th>second</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">bar</th>
      <th>one</th>
      <td>-0.904164</td>
      <td>0.001987</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.299222</td>
      <td>-1.895165</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">baz</th>
      <th>one</th>
      <td>2.029077</td>
      <td>-0.618868</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-3.423209</td>
      <td>0.621895</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">foo</th>
      <th>one</th>
      <td>0.278142</td>
      <td>-0.029705</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-1.875680</td>
      <td>-1.225554</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">qux</th>
      <th>one</th>
      <td>0.593413</td>
      <td>-1.241949</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.727231</td>
      <td>0.394393</td>
    </tr>
  </tbody>
</table>

</div>

```python
df2 = df[:4]
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
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
    <tr>
      <th>first</th>
      <th>second</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">bar</th>
      <th>one</th>
      <td>-0.904164</td>
      <td>0.001987</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-0.299222</td>
      <td>-1.895165</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">baz</th>
      <th>one</th>
      <td>2.029077</td>
      <td>-0.618868</td>
    </tr>
    <tr>
      <th>two</th>
      <td>-3.423209</td>
      <td>0.621895</td>
    </tr>
  </tbody>
</table>

</div>

```python
stacked = df2.stack()   # 列---->行
stacked
```

```
first  second   
bar    one     A   -0.904164
               B    0.001987
       two     A   -0.299222
               B   -1.895165
baz    one     A    2.029077
               B   -0.618868
       two     A   -3.423209
               B    0.621895
dtype: float64
```

```python
stacked.unstack()  # 行----->列
# stacked.unstack(0)
# stacked.unstack('first')
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
      <th>first</th>
      <th>bar</th>
      <th>baz</th>
    </tr>
    <tr>
      <th>second</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">one</th>
      <th>A</th>
      <td>-0.904164</td>
      <td>2.029077</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.001987</td>
      <td>-0.618868</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">two</th>
      <th>A</th>
      <td>-0.299222</td>
      <td>-3.423209</td>
    </tr>
    <tr>
      <th>B</th>
      <td>-1.895165</td>
      <td>0.621895</td>
    </tr>
  </tbody>
</table>

</div>

```python
stacked.unstack(1)
# stacked.unstack('second')
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
      <th>second</th>
      <th>one</th>
      <th>two</th>
    </tr>
    <tr>
      <th>first</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th rowspan="2" valign="top">bar</th>
      <th>A</th>
      <td>-0.904164</td>
      <td>-0.299222</td>
    </tr>
    <tr>
      <th>B</th>
      <td>0.001987</td>
      <td>-1.895165</td>
    </tr>
    <tr>
      <th rowspan="2" valign="top">baz</th>
      <th>A</th>
      <td>2.029077</td>
      <td>-3.423209</td>
    </tr>
    <tr>
      <th>B</th>
      <td>-0.618868</td>
      <td>0.621895</td>
    </tr>
  </tbody>
</table>

</div>

#### 关于unstack的数字标签

- `unstack(1) =unstack("second")`，默认是最里层标签
- `unstack(0) =unstack("first")`

![](https://s2.ax1x.com/2019/10/11/uHwla4.md.png)

![](https://s2.ax1x.com/2019/10/11/uHwJR1.png)

### pivot

#### 本质

`DF的pivot`本质上就是`set_index`先创建层次化索引，再利用`unstack`进行重塑。

[Pandas透视表详解](https://www.cnblogs.com/onemorepoint/p/8425300.html)

![](https://s2.ax1x.com/2019/10/11/ubfFr4.png)

> 左边的表格类似于是`Excel`或者`MySQL`中的存储形式，通过轴向转换变成右边的`DataFrame`型数据。

```python
df[df['bar'] == 'A']     # select out everything for variable A 

df.pivot(index='foo', columns='bar', values='baz')   # 分别指定行索引、列属性还有value值
```



![](https://s2.ax1x.com/2019/10/11/uHc4GF.png)



#### 官网demo

```python
In [1]: df
Out[1]: 
         date variable     value
0  2000-01-03        A  0.469112
1  2000-01-04        A -0.282863
2  2000-01-05        A -1.509059
3  2000-01-03        B -1.135632
4  2000-01-04        B  1.212112
5  2000-01-05        B -0.173215
6  2000-01-03        C  0.119209
7  2000-01-04        C -1.044236
8  2000-01-05        C -0.861849
9  2000-01-03        D -2.104569
10 2000-01-04        D -0.494929
11 2000-01-05        D  1.071804

In [2]: df.pivot(index='date', columns='variable', values='value')
Out[2]: 
variable           A         B         C         D
date                                              
2000-01-03  0.469112 -1.135632  0.119209 -2.104569
2000-01-04 -0.282863  1.212112 -1.044236 -0.494929
2000-01-05 -1.509059 -0.173215 -0.861849  1.071804

In [3]: df[df['variable'] == 'A']
Out[3]: 
        date variable     value
0 2000-01-03        A  0.469112
1 2000-01-04        A -0.282863
2 2000-01-05        A -1.509059
    
In [4]: df['value2'] = df['value'] * 2
In [5]: pivoted = df.pivot(index='date', columns='variable')

In [6]: pivoted
Out[6]: 
               value                                  value2                              
variable           A         B         C         D         A         B         C         D
date                                                                                      
2000-01-03  0.469112 -1.135632  0.119209 -2.104569  0.938225 -2.271265  0.238417 -4.209138
2000-01-04 -0.282863  1.212112 -1.044236 -0.494929 -0.565727  2.424224 -2.088472 -0.989859
2000-01-05 -1.509059 -0.173215 -0.861849  1.071804 -3.018117 -0.346429 -1.723698  2.143608
```