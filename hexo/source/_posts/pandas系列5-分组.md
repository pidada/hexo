---
title: pandas系列5-分组_groupby
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-10 18:18:04
password:
summary:
copyright: true
tags:
  - pandas
  - 数据分析
  - 可视化
  - 分组
categories:
  - pandas
  - 文档
---

> `groupby` 是`pandas` 中非常重要的一个函数, 主要用于数据聚合和分类计算. 其思想是`“split-apply-combine”`（拆分 - 应用 - 合并）.
>
> - 拆分：`groupby`，按照某个属性`column`分组，得到的是一个分组之后的对象
> - 应用：对上面的对象使用某个函数，可以是自带的也可以是自己写的函数，通过`apply(function)`
> - 合并：最终结果是个S型数据

[pandas分组和聚合详解](https://www.cnblogs.com/zhangyafei/p/10516773.html)

![](https://s2.ax1x.com/2019/10/10/uT4XOH.md.png)

<!--MORE-->

### 官方文档

>`DataFrame.``groupby`(*self*, *by=None*, *axis=0*, *level=None*, *as_index=True*, *sort=True*, *group_keys=True*, *squeeze=False*, *observed=False*, ***kwargs*)

#### 常用参数

- **by** : mapping, function, label, or list of labels
- **axis** : {0 or ‘index’, 1 or ‘columns’}, default 0；Split along rows (0) or columns (1).
- **level** : int, level name, or sequence of such, default None
- **as_index** : bool, default True
- **sort** : bool, default True。默认是情况下会对数据进行分组，关闭可以提高性能
- **group_keys** : bool, default True

**by和as_index最常用**

#### 返回值

>DataFrameGroupBy or SeriesGroupBy
>
>Depends on the calling object and returns groupby object that contains information about the groups.



### demo

- `groupby`后面接上分组的列属性名称（单个）
- 多个属性用列表形式表示，形成层次化索引

```python
In [1]: df = pd.DataFrame({'A': ['foo', 'bar', 'foo', 'bar',
   ....:                          'foo', 'bar', 'foo', 'foo'],
   ....:                    'B': ['one', 'one', 'two', 'three',
   ....:                          'two', 'two', 'one', 'three'],
   ....:                    'C': np.random.randn(8),
   ....:                    'D': np.random.randn(8)})
   ....: 

In [2]: df
Out[2]: 
     A      B         C         D
0  foo    one -1.202872 -0.055224
1  bar    one -1.814470  2.395985
2  foo    two  1.018601  1.552825
3  bar  three -0.595447  0.166599
4  foo    two  1.395433  0.047609
5  bar    two -0.392670 -0.136473
6  foo    one  0.007207 -0.561757
7  foo  three  1.928123 -1.623033

In [3]: df.groupby('A').sum()   # 分组，然后将sum()函数应用于分组结果
Out[3]: 
            C        D
A                     
bar -2.802588  2.42611
foo  3.146492 -0.63958

In [4]: df.groupby(['A', 'B']).sum()   # 多个属性用列表形式，形成层次化索引
Out[4]: 
                  C         D
A   B                        
bar one   -1.814470  2.395985
    three -0.595447  0.166599
    two   -0.392670 -0.136473
foo one   -1.195665 -0.616981
    three  1.928123 -1.623033
    two    2.414034  1.600434
```



### 栗子

导入数据

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

# 如何读取csv数据，对数据用|分开
url = "https://raw.githubusercontent.com/justmarkham/DAT8/master/data/u.user"
df = pd.read_csv(url, sep="|")
df.head()  # 查看前5行
```



> 现有数据特点
>
> - user_id: id号
> - age: 年龄
> - gender: 性别
> - occupation: 职业
> - zip_code: 邮政编码, 通过邮政编码可获取所在城市

![](https://s2.ax1x.com/2019/10/10/uTjalR.png)



解决问题

1. 如何找出每一种职业的平均年龄?（需要按照职业进行分组）并按照平均年龄从大到小排序?（分组之后对年龄求平均再排序）
2. 分别找出男人和女人每种职业的人数?（按照男女分组）
3. 更进一步, 如何找出男人和女人在不同职业的平均年龄?（先按男女分组，再按照不同职业分组，再求平均年龄）

-----------

#### 问题1 : 如何找出每一种职业的平均年龄?并按照平均年龄从大到小排序?

- 分组用`groupby`
- 求平均`mean()`
- 排序`sort_values`，默认是升序`asc`
- 操作某个列属性，通过属性的方式`df.column`

```python
df.groupby("occupation").age.mean().sort_values(ascending=False)  # 默认是升序
# df.groupby(df["occupation"]).age.mean().sort_values(ascending=False)
# df.groupby(by="occupation").age.mean().sort_values(ascending=False)  by可以省略

occupation
retired          63.071429
doctor           43.571429
educator         42.010526
healthcare       41.562500
librarian        40.000000
administrator    38.746835
executive        38.718750
marketing        37.615385
......
Name: age, dtype: float64 
```

- 首先`df`按照每一种`occupation`拆分成多个部分
- 然后分别计算每种`occupation`的`age`的平均值
- 最后合并成一个`Dataframe`或者`Series`
- 值得注意的是, `groupby`之后是一个对象,，直到应用一个函数（mean函数）之后才会变成一个`Series`或者`Dataframe`.

```python
type(df.groupby("occupation"))
# output
pandas.core.groupby.groupby.DataFrameGroupBy
```

#### 问题2 : 分别找出男人和女人每种职业的人数?

- 对两个属性同时进行分组
- 再进行`size`函数求和

```python
df.groupby(['occupation','gender']).size()
# Output
occupation     gender
administrator  F          36
               M          43
artist         F          13
               M          15
doctor         M           7
educator       F          26
               M          69
......
```

#### 问题3 : 如何找出男人和女人在不同职业的平均年龄?

- 先对职业和性别机型分组
- 再对年龄求平均值

```python
df.groupby(['occupation','gender']).age.mean()
# Output
occupation     gender
administrator  F         40.638889
               M         37.162791
artist         F         30.307692
               M         32.333333
doctor         M         43.571429
educator       F         39.115385
               M         43.101449
engineer       F         29.500000
               M         36.600000
```

#### 问题4：两个列属性的groupby机制

- 栗子中按照性别`gender`分组`groupby`，再求职业的总和`size`

  ```python
  df['occupation'].groupby(df['gender']).size()
  # result
  
  gender
  F    273
  M    670
  Name: occupation, dtype: int64
  ```

- 按照职业分组`groupby`，再对年龄求平均值`mean`

  ```python
  df['age'].groupby(df['occupation']).mean()
  
  # result
  occupation
  administrator    38.746835
  artist           31.392857
  doctor           43.571429
  educator         42.010526
  engineer         36.388060
  entertainment    29.222222
  executive        38.718750
  ......
  ```



### `groupby`细说

#### 最常用参数

- by：可以是列属性`column`，也可以是和`df`同行的`Series`
- as_index：是否将`groupby`的`column`作为`index`， 默认是`True`

#### groupby之后的对象应用自定义的函数

```python
demo = df[:5]
demo.groupby("gender").apply(lambda x: print(x))

# result    
    user_id  age gender occupation zip_code
1        2   53      F      other    94043
4        5   33      F      other    15213
   user_id  age gender occupation zip_code
1        2   53      F      other    94043
4        5   33      F      other    15213
   user_id  age gender  occupation zip_code
0        1   24      M  technician    85711
2        3   23      M      writer    32067
3        4   24      M  technician    43537

```

有个`DF`数据出现了两次，解释看[Stack Overflow](https://stackoverflow.com/questions/21390035/pandas-groupby-apply-method-duplicates-first-group)

#### 分组之后的操作

```python
# 分组之后进行遍历
grouped  = df.groupby(["sex", "age"])   
for name, group in grouped:
    print("name: {}".format(name))
    print("group: {}".format(group))
    print("--------------")
    
# 选择一个组
grouped  = df.groupby("sex")
grouped.get_group("male")
df.groupby(["sex", "age"]).get_group(("male", 18))

# 分组之后聚合：均值、最大最小值、计数、求和等，需要调用agg()方法
grouped = df.groupby("sex")
grouped["age"].agg(len)
grouped["age"].agg(['mean','std','count','max'])  # 能够传入多个聚合函数
grouped["age"].agg(np.max)
```

![](https://s2.ax1x.com/2019/10/10/u7FDxS.md.png)

#### 避免层次化索引

- 分组和聚合之后使用`reset_index()`
- 在分组时，使用`as_index=False`

```python
# 1
res = grouped.agg(len)    # grouped.count()
res.reset_index()  # 索引重排

# 2
grouped = df.groupby(["sex", "age"], as_index=False)  
```

