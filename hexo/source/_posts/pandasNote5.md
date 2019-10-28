---
title: pandas操作
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-16 20:23:55
password:
summary:
copyright: true
tags:
  - python
  - pandas
  - 可视化
categories:
  - pandas
  - 文档
---

> 用于记录pandas中各种`cao`气的操作

- 指定列属性

- 读取文件的时候首行不当做属性值

- 指定分隔符和属性（names）

- 查看DF数据的各种信息

- groupby机制等

  <!--MORE-->

### 文件读取

```python
import pandas as pd

names = ['age', 'city', 'size', 'gender'] 
df = pd.read_table(path, header=None, seq="|", names=names)  
```

### 查看各种信息

```python
df.describe()
df.dtype
df.shape
df.head()
df.province.value_counts()
df.province.unique()   # 统计属性中不同的元素
df.province.nunique()  # 统计属性中不同元素的个数

df.age.plot(kind='hist')
df.age.value_counts().plot(kind='bar')
```

### 属性操作

```python
df ['information'] = df['age'] + df['gender']   # 生成information属性
df.rename(columns = {'old_name': 'new_name'}, inplace=True)  # 直接指定

new_columns = ['new_age', 'new_city', 'new_size']
df.columns = new_columns  # 使用上面的新属性名

df.columns = df.columns.str.replace(' ', '_')   # 将原来的属性的_用空格代替

# 删除属性
df.drop('age', axis=1, inplace=Ture)
df.drop(['city', 'state'], axis=1, inplace=True)
```

### 排序sort

`sort_values`主要是对某个属性中出现的各个元素进行排序，默认是升序，字母是a-z

```python
df.age.sort_values()   #  默认是升序，可以修改ascending=False
df['age'].sort_values()
df.sort_values('age', ascending=False)

# 多个属性的排序：先排序第一个，再第二个
df.sort_values(['age', 'state'])   
```

### 过滤相关

```python
df[df.age > 20 & df.state == 'guangdong']
df[df.state.isin(['guangdong', 'hunan', 'hubei'])]  
# df.state.isin(['guangdong', 'hunan', 'hubei'])  结果是布尔值

df[df.age > 20, 'state']
df[df.age >20]['state']
```

### axis相关

```python
df.drop('age', axis=1)   # 列上操作
df.drop(2, axis=0)   # 行上操作

df.mean(axis=0)
df.mean(axis='index')  

df.mean(axis=1)
df.mean(axis='columns')  
```

![](https://s2.ax1x.com/2019/10/16/KF5NGR.md.jpg)

### 字符串方法

```python
df.name.str.upper()  # 全部变成大写
df[df.name.str.contains('xiaoming')]
df.description.str.replace('[\[\]]', '')  # 将[]去掉
```

### 改变数据类型

```python
df['age'] = df.age.astype(float)
df = pd.read_csv(path, dtype={'age': float})  # 创建过程中指定数据类型
df.dtype
```

### groupby 机制

```python
df.groupby('province').size.agg(['count', 'mean', 'min'])  # 传入多个参数
df.groupby('province').mean().plot(kind='bar')   # 作图
```

### 缺失值处理

```python
df.isnulll()
df.notnull()
df[df.city.isnull()]
df.dropna(subset=['age', 'city', 'province'], how='any')

pd.Series([True, False, True]).sum()  # 统计出现T的次数
```

### index相关

```python
df.set_index('province', inplace=True)  # 将某个属性变成索引index

df.index.name='country'
df.reset_index(inplace=True)

```

### 处理离散数据

怎么将性别男女变成1/0

```python
df['new_sex'] = df.sex.map({'female':0, 'male':1})
```

### 日期和时间

```python
df['time'] = pd.to_datetime(df.time)
df.time.dt.hour(dayofyear,weekday_name)

ts = pd.to_datetime('11/12/2019')   # 格式化时间
df.loc[df.time >= ts, :].head()   # 选择时间位于ts之后的

(df.time.max() - df.time.min()).days   # 相隔多少天
df.Year.value_counts.sort_index().plot()   # 作图
```

### pandas中记录的显示

```python
pd.get_option('display.max_rows')   # 查看显示多少条
pd.set_option('display.max_rows',None)  # 全部显示 
pd.reset_option('display.max_rows')  # 复原显示

pd.set_option('display.max_colwidth',1000)  # DF中数据显示的最大值
pd.set_option('display.float-format', '{:, }'.foramt)  # 将数字变成每3位一个逗号的形式
```

