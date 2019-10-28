---
title: pandas小结
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-13 15:25:13
password:
summary:
copyright: true
tags:
  - pandas
  - 数据分析
  - 可视化
categories:
  - pandas
  - 文档
---

本篇博文主要是对之前的几篇关于`pandas`使用技巧的小结，内容包含：

- 创建`S`型或者`DF`型数据，以及如何查看数据
- 选择特定的数据
- 缺失值处理
- `apply`使用
- 合并和连接
- 分组`groupby`机制
- 重塑`reshaping`
- 透视表使用

<!--MORE-->

-----------

### 创建数据

#### S型数据

```python
import numpy as np
import pandas as pd

pd.Series([1, 3, 5, np.nan, 6, 89])  # 普通形式
pd.date_range('20190924', periods=6)  # 时间间隔形式
```

#### DF型数据

指定3个参数

- values
- index
- columns

```python
pd.DataFrame(np.random.randn(6,4), index=dates, columns=list("ABCD"))
df

pd.DataFrame({'A': 1.,   #  某列的值相同
                    'B': pd.Timestamp('20130102'),  # 时间戳的创建
                    'C': pd.Series(1, index=list(range(4)), dtype='float32'),  # 某列值可以是S型数据
                    'D': np.array([3] * 4, dtype='int32'),  # 使用numpy数组
                    'E': pd.Categorical(["test", "train", "test", "train"]), # 不同的类
                    'F': 'foo'})  # 使用布尔值
```

#### 选择数据

- head()，默认是头5行
- tail()
- df.index/df.columns
- df.describe()  查看各种统计信息
- df.T  转置
- df.sort_index(axis=0, ascending=False)，行索引降序排列
-  df.sort_values(by="age")，某个属性的降序排列

### 查看数据

- loc和iloc

  - `loc`：通过标签获取，等同于`.at`
  - `iloc`：通过数字索引获取，等同于`.iat`

  ```python
  df.loc[[......]]：可以使用数字索引，也可以使用标签索引，还可以用切片的形式
  
  df.iloc[[.....]]：只能使用数字索引，可以是非连续或者连续（等差形式也OK）
  
  布尔索引：df2[df2['E'].isin(['two', 'four'])]
  
  同时指定行和列：
  
  df.loc[:, ["A","B"]]
  
  df.iloc[[1, 2, 4], [0, 2]]
  ```

### 缺失值处理

- `df.dropna()`删除缺失值

  - `axis`：维度，`0`表示`index`行，`1`表示`columns`列，默认为0

  - `how：`

    ```
    - `all：`全部为缺失值则删除该行或者列
    - `any：`至少有一个则删除
    ```

  - `thresh`：指定至少出现了thresh个才删除

  - `subset`：指定在某些列的子集中选择出现了缺失值的列删除，不在子集中不会删除(axis决定行\列）

  - `inplace`：刷选过缺失值得到的新数据是存为副本还是直接在原数据上进行修改。

  

- `df.fillna()`填充缺失值

  - value：使用什么填充缺失值，不同的列可以使用不同的值

  - axis：指定填充维度

  - `method`：填充方法，**不能和value同时出现，避免冲突**

    ```
    - `ffill`：用缺失值的前一个值进行填充，axis=1：横向填充（前后），axis=0：纵向填充（上下）
    - `backfill、bfill`：缺失值后面的一个填充代替前一个
    ```

  - `limit`：填充的次数限制

- `df.isnull()`和`df.isna()`

 二者都是判断是不是缺失值 

-----

### apply用法

```python
# 求出每列的max 和 min
def f(x):    
    return pd.Series([x.min(), x.max()], index=["min", "max"])
df.apply(f)
```

```python
f = lambda x: x.max() - x.min()
df.apply(f)# df.apply(f, axis="columns") 表示在行上执行
```

### 合并和连接

####  合并`concat`

- axis
  - `axis=0`：默认是`Series`
  - `axis=1`：得到`DF`数据，缺值用`NaN`补充
- join
  - `outer`：合并，缺值用`nan`
  - `inner`：求交集，非交集部分直接删除
- `keys`：用于层次化索引
- `ignore_index`：不保留连接轴上的索引，产生新的索引

#### 连接`merge`

可根据⼀个或多个键将不同`DataFrame`中的⾏连接起来，它实现的就是数据库的`join`操作 ，就是数据库风格的合并

常用参数表格

| 参数                    | 说明                                                     |
| ----------------------- | -------------------------------------------------------- |
| left                    | 参与合并的左侧`DF`                                       |
| right                   | 参与合并的右侧`DF`                                       |
| how                     | 默认是inner，`inner、outer、right、left`                 |
| on                      | 用于连接的列名，默认是相同的列名                         |
| left_on \right_on       | 左侧、右侧`DF`中用作连接键的列                           |
| sort                    | 根据连接键对合并后的数据进行排序，默认是T                |
| suffixes                | 重复列名，直接指定后缀，用元组的形式('_left', '_right')  |
| left_index、right_index | 将左侧、右侧的行索引`index`作为连接键（用于index的合并） |



### 分组

#### groupby

- 拆分：`groupby`，按照某个属性`column`分组，得到的是一个分组之后的对象
- 应用：对上面的对象使用某个函数，可以是自带的也可以是自己写的函数，通过`apply(function)`
- 合并：最终结果是个S型数据

![](https://s2.ax1x.com/2019/10/10/uT4XOH.md.png)

**如何找出每一种职业的平均年龄?并按照平均年龄从大到小排序?**

- 分组用`groupby`
- 求平均`mean()`
- 排序`sort_values`，默认是升序`asc`
- 操作某个列属性，通过属性的方式`df.column`

```python
df.groupby("occupation").age.mean().sort_values(ascending=False)  # 默认是升序
# df.groupby(df["occupation"]).age.mean().sort_values(ascending=False)
# df.groupby(by="occupation").age.mean().sort_values(ascending=False)  by可以省略
```

```python
# 按照职业分组，再对年龄求均值
df['age'].groupby(df['occupation']).mean()
```

#### 避免层次化索引

- 分组和聚合之后使用`reset_index()`

- 在分组时，使用`as_index=False`

  

### 重塑reshaping 

- `stack：`将数据的列旋转成行，AB由列属性变成行索引
- `unstack:`将数据的行旋转成列，AB由行索引变成列属性

![](https://s2.ax1x.com/2019/10/11/uHmwVS.png)

![](https://s2.ax1x.com/2019/10/11/uHmRbT.png)



### 透视表

- `data`: a DataFrame object，要应用透视表的数据框
- `values`: a column or a list of columns to aggregate，要聚合的列，相当于“值”
- `index`: a column, Grouper, array which has the same length as data, or list of them. Keys to group by on the pivot table index. If an array is passed, it is being used as the same manner as column values，聚合值的分组，相当于“行”
- `columns`: a column, Grouper, array which has the same length as data, or list of them. Keys to group by on the pivot table column. If an array is passed, it is being used as the same manner as column values，聚合值的分组，相当于是”列”
- `aggfunc`: function to use for aggregation, defaulting to `numpy.mean`，要应用的聚合函数，默认函数是均值

![](https://s2.ax1x.com/2019/10/13/ujrD4P.png)

**关于`pivot_table`函数结果的说明**

- `df`是需要进行透视表的数据框
- `values`是生成的透视表中的数据
- `index`是透视表的**层次化索引**，多个属性使用**列表**的形式
- `columns`是生成透视表的列属性