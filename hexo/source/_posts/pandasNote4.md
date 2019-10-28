---
title: pandasNote4
date: 2019-09-11 15:33:59
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
from pandas import Series, DataFramei
import pandas_datareader.data as web
```

### 协方差和相关系数

- corr()
- cov()
- corrwith()

<!-- MORE -->



```python
all_data = {ticker: web.get_data_yahoo(ticker)
            for ticker in ['AAPL', 'IBM', 'MSFT', 'GOOG']}
price = pd.DataFrame({ticker: data['Adj Close']
                      for ticker, data in all_data.items()})
volume = pd.DataFrame({ticker: data['Volume']
                        for ticker, data in all_data.items()})
```

```python
# 计算价格的百分变化
returns = price.pct_change()
returns.head()
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
      <th>AAPL</th>
      <th>IBM</th>
      <th>MSFT</th>
      <th>GOOG</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2009-12-31</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2010-01-04</th>
      <td>0.015565</td>
      <td>0.011841</td>
      <td>0.015420</td>
      <td>0.010920</td>
    </tr>
    <tr>
      <th>2010-01-05</th>
      <td>0.001729</td>
      <td>-0.012080</td>
      <td>0.000323</td>
      <td>-0.004404</td>
    </tr>
    <tr>
      <th>2010-01-06</th>
      <td>-0.015906</td>
      <td>-0.006496</td>
      <td>-0.006137</td>
      <td>-0.025209</td>
    </tr>
    <tr>
      <th>2010-01-07</th>
      <td>-0.001849</td>
      <td>-0.003461</td>
      <td>-0.010400</td>
      <td>-0.023280</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 计算相关系数和协方差
print(returns["MSFT"].corr(returns["IBM"]))  # 通过标签的形式
print(returns["MSFT"].cov(returns["IBM"]))
```

```
0.49161308372179857
8.80330763108205e-05
```

```python
returns.MSFT.corr(returns.IBM)  # 通过属性的形式
```

```
0.49161308372179857

```

```python
# 协方差和相关系数矩阵
print(returns.corr())
print(returns.cov())

```

```
          AAPL       IBM      MSFT      GOOG
AAPL  1.000000  0.387306  0.458039  0.463707
IBM   0.387306  1.000000  0.491613  0.407577
MSFT  0.458039  0.491613  1.000000  0.539243
GOOG  0.463707  0.407577  0.539243  1.000000
          AAPL       IBM      MSFT      GOOG
AAPL  0.000267  0.000078  0.000108  0.000118
IBM   0.000078  0.000154  0.000088  0.000079
MSFT  0.000108  0.000088  0.000209  0.000121
GOOG  0.000118  0.000079  0.000121  0.000242

```

```python
# corrwith()：计算某列或者行和另一个S或者DF数据之间的相关系数
returns.corrwith(returns.IBM)

```

```
AAPL    0.387306
IBM     1.000000
MSFT    0.491613
GOOG    0.407577
dtype: float64

```



### 唯一值、值计数和成员资格

- unique()：返回未排序的值
- value_counts()：计算出现的频率，降序
  - pd.value_counts(obj.values, sort=False)
- isin()判断成员资格

```python
obj = pd.Series(['c', 'a', 'd', 'a', 'a', 'b', 'b', 'c', 'c'])

```

```python
# 值计数功能
uniques=obj.unique()
uniques

```

```
array(['c', 'a', 'd', 'b'], dtype=object)

```

```python
obj.value_counts()  # 默认是降序通过sort=False关闭降序功能pd.value_counts(obj.values, sort=False)

```

```
a    3
c    3
b    2
d    1
dtype: int64

```

```python
# 成员资格
mask = obj.isin(['b', 'c'])
mask

```

```
0     True
1    False
2    False
3    False
4    False
5     True
6     True
7     True
8     True
dtype: bool

```

```python
obj[mask]

```

```
0    c
5    b
6    b
7    c
8    c
dtype: object

```



### 相关列的柱状图

> 将pandas.value_counts传给该DataFrame的apply函数

```python
data = pd.DataFrame({'Qu1': [1, 3, 4, 3, 4],
                     'Qu2': [2, 3, 1, 2, 3],
                     'Qu3': [1, 5, 2, 4, 4]})
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
      <th></th>
      <th>Qu1</th>
      <th>Qu2</th>
      <th>Qu3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3</td>
      <td>3</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>2</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>3</td>
      <td>4</td>
    </tr>
  </tbody>
</table>

</div>

```python
result = data.apply(pd.value_counts).fillna(0)
result

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
      <th>Qu1</th>
      <th>Qu2</th>
      <th>Qu3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.0</td>
      <td>2.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2.0</td>
      <td>2.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2.0</td>
      <td>0.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>0.0</td>
      <td>0.0</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>

</div>

```python
data.apply(pd.value_counts)
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
      <th>Qu1</th>
      <th>Qu2</th>
      <th>Qu3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>NaN</td>
      <td>2.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2.0</td>
      <td>2.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2.0</td>
      <td>NaN</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>

</div>



<CENTER><span style='font-size:2.5rem'>Stay Foolish Stay Hungry</span></CENTER>