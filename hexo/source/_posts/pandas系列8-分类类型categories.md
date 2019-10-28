---
title: pandas系列8-分类类型categories
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-21 11:26:52
password:
summary:
copyright: true
tags:
  - pandas
  - 数据分析
  - 分类
categories:
  - pandas
  - 文档
---

### 分类

>分类的目的是提高性能和内存的使用率

用整数表示的方法称为分类或者字典编码表示法，不同值的数组称为分类、字典或者数据集。

#### 创建分类

- take方法存储原始字符串`Series`
- 直接创建分类：`pd.Categorical(data)`
- 转变成类：`df.astype('category')`
- 分类对象属性
  - `codes`
  - `categories`

<!--MORE-->

#### 分类计算

- 面元函数`qcut`函数返回类`Categories`对象：`pd.qcut(draws, 4)`
- 通过labels标签实现汇总
- groupby提取汇总信息

```python
import numpy as np
import pandas as pd
from pandas import Series, DataFrame
```

```
results = (pd.Series(draws)
    .groupby(bins)
    .agg(['count', 'min', 'max'])
    .reset_index())
```

#### 分类方法

- `memory_usage()`：查看内存
- `cat()`：提供入口
  - `pd.cat.categories`
  - `pd.cat.codes`
- `value_counts()`：查看具体分类
- 创建虚拟变量，用0/1组成的矩阵


```python
values = pd.Series(['apple', 'orange', 'apple', 'apple'] * 2)
pd.unique(values)  # 选取唯一值
```


    array(['apple', 'orange'], dtype=object)


```python
pd.value_counts(values)   # 计算每个值出现的频率
```


    apple     6
    orange    2
    dtype: int64

>数据系统使用包含不同值的维表Dimension Table ，将主要的参数存储为引用维表整数键

- take()方法：分类

#### 去重显示


```python
values = pd.Series([0, 1, 0, 0] * 2)
dim = pd.Series(['apple', 'orange'])
```


```python
print(values)
print(dim)
```

    0    0
    1    1
    2    0
    3    0
    4    0
    5    1
    6    0
    7    0
    dtype: int64
    0     apple
    1    orange
    dtype: object
    

```python
dim.take(values)   # take方法

```


    0     apple
    1    orange
    0     apple
    0     apple
    0     apple
    1    orange
    0     apple
    0     apple
    dtype: object
    

### pandas分类类型


```python
fruits = ['apple', 'orange', 'apple', 'apple'] * 2
N = len(fruits)

```


```python
df = pd.DataFrame({'fruit': fruits,
                   'basket_id': np.arange(N),
                   'count': np.random.randint(3, 15, size=N),   # 生成3-15随机整数
                   'weight': np.random.uniform(0, 4, size=N)},  # 生成0-4的数 
                  columns=['basket_id', 'fruit', 'count', 'weight'])
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>basket_id</th>
      <th>fruit</th>
      <th>count</th>
      <th>weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>apple</td>
      <td>12</td>
      <td>1.550912</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>orange</td>
      <td>6</td>
      <td>3.457292</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>apple</td>
      <td>3</td>
      <td>3.986303</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>apple</td>
      <td>7</td>
      <td>0.955448</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>apple</td>
      <td>9</td>
      <td>1.912784</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>orange</td>
      <td>6</td>
      <td>1.405372</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>apple</td>
      <td>11</td>
      <td>2.882363</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>apple</td>
      <td>12</td>
      <td>2.137783</td>
    </tr>
  </tbody>
</table>

</div>

#### 转成分类

通过astype('category')将某个属性转成分类类型


```python
# 转换成分类
fruit_cat = df['fruit'].astype('category')
fruit_cat
```




    0     apple
    1    orange
    2     apple
    3     apple
    4     apple
    5    orange
    6     apple
    7     apple
    Name: fruit, dtype: category
    Categories (2, object): [apple, orange]




```python
print(type(fruit_cat))   # S型数据
<class 'pandas.core.series.Series'>
```



```python
c = fruit_cat.valuest
type(c)  # c是⼀个pandas.Categorical实例
```


    pandas.core.arrays.categorical.Categorical

#### 两个属性值codes 和 categories


```python
# 分类对象有categories和codes属性
print(c.categories)  # categories是具体的分类
print(c.codes)   # codes是里面的0、1值
```

    Index(['apple', 'orange'], dtype='object')
    [0 1 0 0 0 1 0 0]

```python
df
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
    

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>basket_id</th>
      <th>fruit</th>
      <th>count</th>
      <th>weight</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>apple</td>
      <td>12</td>
      <td>1.550912</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>orange</td>
      <td>6</td>
      <td>3.457292</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>apple</td>
      <td>3</td>
      <td>3.986303</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>apple</td>
      <td>7</td>
      <td>0.955448</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>apple</td>
      <td>9</td>
      <td>1.912784</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>orange</td>
      <td>6</td>
      <td>1.405372</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>apple</td>
      <td>11</td>
      <td>2.882363</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>apple</td>
      <td>12</td>
      <td>2.137783</td>
    </tr>
  </tbody>
</table>

</div>


```python
# 将列转换成分类
df['fruit'] = df['fruit'].astype("category")
df.fruit

```


    0     apple
    1    orange
    2     apple
    3     apple
    4     apple
    5    orange
    6     apple
    7     apple
    Name: fruit, dtype: category
    Categories (2, object): [apple, orange]


```python
# 通过Python其他序列创建分类
my_categories = pd.Categorical(['foo', 'bar', 'baz', 'foo', 'bar'])
my_categories
```


    [foo, bar, baz, foo, bar]
    Categories (3, object): [bar, baz, foo]


```python
# 通过from_codes构建分类
categories = ['foo', 'bar', 'baz']
codes = [0, 1, 2, 0, 0, 1]
my_cats_2 = pd.Categorical.from_codes(codes, categories)  # from_codes构造器
my_cats_2
```


    [foo, bar, baz, foo, foo, bar]
    Categories (3, object): [foo, bar, baz]


```python
# 无序的分类实例通过as_ordered进行排序
my_cats_2.as_ordered()
```


    [foo, bar, baz, foo, foo, bar]
    Categories (3, object): [foo < bar < baz]
    


```python
# 创建分类的实例直接指定顺序: ordered=True
ordered_cat = pd.Categorical.from_codes(codes, categories,
                                        ordered=True)
ordered_cat
```


    [foo, bar, baz, foo, foo, bar]
    Categories (3, object): [foo < bar < baz]

### 分类计算

重点关注`pandas`中的`Categorical`类。通过使用`pandas.qcut`面元函数，返回`pandas.Categorical`

- 创建面元
- 通过面元提取数据


```python
np.random.seed(12345)
draws = np.random.randn(1000)
draws[:5]
```


    array([-0.20470766,  0.47894334, -0.51943872, -0.5557303 ,  1.96578057])




```python
bins = pd.qcut(draws, 4)
bins
```




    [(-0.684, -0.0101], (-0.0101, 0.63], (-0.684, -0.0101], (-0.684, -0.0101], (0.63, 3.928], ..., (-0.0101, 0.63], (-0.684, -0.0101], (-2.9499999999999997, -0.684], (-0.0101, 0.63], (0.63, 3.928]]
    Length: 1000
    Categories (4, interval[float64]): [(-2.9499999999999997, -0.684] < (-0.684, -0.0101] < (-0.0101, 0.63] < (0.63, 3.928]]

#### 使用lables参数来qcut


```python
bins = pd.qcut(draws, 4, labels=['Q1', 'Q2', 'Q3', 'Q4'])
bins
```


    [Q2, Q3, Q2, Q2, Q4, ..., Q3, Q2, Q1, Q3, Q4]
    Length: 1000
    Categories (4, object): [Q1 < Q2 < Q3 < Q4]


```python
bins.codes[:10]
```


    array([1, 2, 1, 1, 3, 3, 2, 2, 3, 3], dtype=int8)


```python
# groupby 提取汇总信息
bins = pd.Series(bins, name="quratile")
results = (pd.Series(draws)
          .groupby(bins)
          .agg(["count", "min", "max"])
          .reset_index())
results
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>quratile</th>
      <th>count</th>
      <th>min</th>
      <th>max</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Q1</td>
      <td>250</td>
      <td>-2.949343</td>
      <td>-0.685484</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Q2</td>
      <td>250</td>
      <td>-0.683066</td>
      <td>-0.010115</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Q3</td>
      <td>250</td>
      <td>-0.010032</td>
      <td>0.628894</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Q4</td>
      <td>250</td>
      <td>0.634238</td>
      <td>3.927528</td>
    </tr>
  </tbody>
</table>

</div>


```python
bins = pd.Series(bins, name="quratile")
result = (pd.Series(draws)
          .groupby(bins)
          .agg(["count", "min", "max"]))
result
```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }

</style>

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>count</th>
      <th>min</th>
      <th>max</th>
    </tr>
    <tr>
      <th>quratile</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Q1</th>
      <td>250</td>
      <td>-2.949343</td>
      <td>-0.685484</td>
    </tr>
    <tr>
      <th>Q2</th>
      <td>250</td>
      <td>-0.683066</td>
      <td>-0.010115</td>
    </tr>
    <tr>
      <th>Q3</th>
      <td>250</td>
      <td>-0.010032</td>
      <td>0.628894</td>
    </tr>
    <tr>
      <th>Q4</th>
      <td>250</td>
      <td>0.634238</td>
      <td>3.927528</td>
    </tr>
  </tbody>
</table>

</div>


```python
results["quratile"]
```




    0    Q1
    1    Q2
    2    Q3
    3    Q4
    Name: quratile, dtype: category
    Categories (4, object): [Q1 < Q2 < Q3 < Q4]

### 分类提高性能

- 使用DF的列分类占用内存少
- memory_usage()：查看内存


```python
N = 10000000
draws = pd.Series(np.random.randn(N))
labels = pd.Series(['foo', 'bar', 'baz', 'qux'] * (N // 4))

```


```python
categories = labels.astype("category")
categories.memory_usage()  # 分类占用内存少
```


    10000272


```python
labels.memory_usage()   # 非分类占用内存多
```


    80000080


```python
%time _ = labels.astype('category')
```

    Wall time: 444 ms


### 分类方法

- 先使用分类入口：`cat`方法；再使用`codes，categories`方法
  - cat.codes
  - cat.categories
- set_categories：解决超出给定的数据集个数
- value_counts()：查看分类的个数
- remove_unused_categories()：删除没有看到的数据
- 常用方法汇总

| 方法                     | 作用                                                 |
| ------------------------ | ---------------------------------------------------- |
| add_categories           | 已存在分类的后面直接添加                             |
| as_ordered               | 使分类有序                                           |
| as_unordered             | 使分类无序                                           |
| remove_categories        | 移除分类，设置移除值为`null`                         |
| remove_unused_categories | 移除任意不出现在数据中的分类值                       |
| set_categories           | 用指定的新分类的名字来替换分类，可以添加或者删除分类 |


```python
s = pd.Series(['a', 'b', 'c', 'd'] * 2)
cat_s = s.astype('category')  # 如何转换成分类
cat_s

```


    0    a
    1    b
    2    c
    3    d
    4    a
    5    b
    6    c
    7    d
    dtype: category
    Categories (4, object): [a, b, c, d]


```python
# cat属性提供分类方法的入口
cat_s.cat.codes
```


    0    0
    1    1
    2    2
    3    3
    4    0
    5    1
    6    2
    7    3
    dtype: int8


```python
cat_s.cat.categories
```


    Index(['a', 'b', 'c', 'd'], dtype='object')


```python
# 实际分类超出给定数据集中的个数，通过set_catgories实现
actual_categories = ['a', 'b', 'c', 'd', 'e']
cat_s2 = cat_s.cat.set_categories(actual_categories)
cat_s2
```


    0    a
    1    b
    2    c
    3    d
    4    a
    5    b
    6    c
    7    d
    dtype: category
    Categories (5, object): [a, b, c, d, e]




```python
# 统计每个类的分类值
cat_s.value_counts()
```


    d    2
    c    2
    b    2
    a    2
    dtype: int64


```python
cat_s2.value_counts()
```




    d    2
    c    2
    b    2
    a    2
    e    0
    dtype: int64


```python
#  remove_unused_categories()
cat_s3 = cat_s[cat_s.isin(['a', 'b'])]
cat_s3
```


    0    a
    1    b
    4    a
    5    b
    dtype: category
    Categories (4, object): [a, b, c, d]


```python
cat_s.isin(['a', 'b'])
```


    0     True
    1     True
    2    False
    3    False
    4     True
    5     True
    6    False
    7    False
    dtype: bool


```python
# 查看分类，变成2个
cat_s3.cat.remove_unused_categories()
```


    0    a
    1    b
    4    a
    5    b
    dtype: category
    Categories (2, object): [a, b]
    

### 创建虚拟变量

虚拟变量创建的过程实际上是将各个分类的标签看成是列属性，存在的数据则是1，不存在则是0，创建0/1矩阵

- 分类数据转成虚拟变量：one-hot编码
- 创建值为0或者1的DF数据
- pd.get_dummies()


```python
cat_s = pd.Series(['a', 'b', 'c', 'd'] * 2, dtype='category')
pd.get_dummies(cat_s)

```


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }


    .dataframe tbody tr th {
        vertical-align: top;
    }
    
    .dataframe thead th {
        text-align: right;
    }
    

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
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>

</div>


```python
cat_s
```


    0    a
    1    b
    2    c
    3    d
    4    a
    5    b
    6    c
    7    d
    dtype: category
    Categories (4, object): [a, b, c, d]

