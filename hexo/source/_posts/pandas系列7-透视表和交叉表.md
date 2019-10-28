---
title: pandas系列7-透视表和交叉表
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-11 18:08:37
password:
summary:
copyright: true
tags:
  - pandas
  - 数据分析
  - 可视化
  - 透视表
  - 交叉表
categories:
  - pandas
  - 文档
---

> 透视表`pivot_table`是各种电子表格和其他数据分析软件中一种常见的数据分析汇总工具。
>
> - 根据一个或者多个键对数据进行聚合
> - 根据行和列上的分组键将数据分配到各个矩形区域中

[一文看懂pandas的透视表]( http://www.pianshen.com/article/6458278605/ )

<!--MORE-->

### Pivot_table

#### 特点

- 灵活性高，可以随意定制你的分析计算要求
- 脉络清晰易于理解数据
- 操作性强，报表神器

#### 参数

- `data`: a DataFrame object，要应用透视表的数据框
- `values`: a column or a list of columns to aggregate，要聚合的列，相当于“值”
- `index`: a column, Grouper, array which has the same length as data, or list of them. Keys to group by on the pivot table index. If an array is passed, it is being used as the same manner as column values，聚合值的分组，相当于“行”
- `columns`: a column, Grouper, array which has the same length as data, or list of them. Keys to group by on the pivot table column. If an array is passed, it is being used as the same manner as column values，聚合值的分组，相当于是"列"
- `aggfunc`: function to use for aggregation, defaulting to `numpy.mean`，要应用的聚合函数，默认函数是均值

#### 三个非常用参数

-  **fill_value :** 有时候聚合结果里出现了NaN，想替换成0时，fill_value=0；
-  **dropna=True：**是跳过整行都是空缺值的行
- **margins :** 是否添加所有行或列的小计/总计，margins=True；
- **margins_name :** 当margins设置为True时，设置总计的名称，默认是“ALL”。 

#### demo

```python
In [1]: import datetime

In [2]: df = pd.DataFrame({'A': ['one', 'one', 'two', 'three'] * 6,
   ....:                    'B': ['A', 'B', 'C'] * 8,
   ....:                    'C': ['foo', 'foo', 'foo', 'bar', 'bar', 'bar'] * 4,
   ....:                    'D': np.random.randn(24),    # 如何生成随机数
   ....:                    'E': np.random.randn(24),
   ....:                    'F': [datetime.datetime(2013, i, 1) for i in range(1, 13)]
   ....:                    + [datetime.datetime(2013, i, 15) for i in range(1, 13)]})
   ....: 

In [3]: df
Out[3]: 
        A  B    C         D         E          F
0     one  A  foo  0.341734 -0.317441 2013-01-01
1     one  B  foo  0.959726 -1.236269 2013-02-01
2     two  C  foo -1.110336  0.896171 2013-03-01
3   three  A  bar -0.619976 -0.487602 2013-04-01
4     one  B  bar  0.149748 -0.082240 2013-05-01
..    ... ..  ...       ...       ...        ...
19  three  B  foo  0.690579 -2.213588 2013-08-15
20    one  C  foo  0.995761  1.063327 2013-09-15
21    one  A  bar  2.396780  1.266143 2013-10-15
22    two  B  bar  0.014871  0.299368 2013-11-15
23  three  C  bar  3.357427 -0.863838 2013-12-15

[24 rows x 6 columns]
```

![](https://s2.ax1x.com/2019/10/13/ujrD4P.png)

关于`pivot_table`函数结果的说明：

- `df`是需要进行透视表的数据框
- `values`是生成的透视表中的数据
- `index`是透视表的**层次化索引**，多个属性使用**列表**的形式
- `columns`是生成透视表的列属性

### Crosstab

一种用于计算分组频率的特殊透视表。

```python
# 关于小费的栗子
df = pd.read_csv(r"D:\Python\datalearning\Python for data analysis\pydata-book-2nd-edition\examples\tips.csv")
df.head()

# 目的：展示每天各种聚会规模的数据点的百分比
# 交叉表crosstab 可以按照指定的行和列统计分组频数
party_counts = pd.crosstab(df['day'], df['size'])   # 第一个参数是行索引，第二个参数是列属性

# 使用loc，定位取出固定的行和列数据
party_counts = party_counts.loc[:, 2:5]

# 数据进行规格化处理，各行加起来等于1
party_pcts = party_counts.div(party_counts.sum(1), axis=0)

party_pcts.plot.bar()
```

![](https://s2.ax1x.com/2019/10/13/uvaVxJ.png)

