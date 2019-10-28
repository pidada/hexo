---
title: pandasNotes3_缺失值处理和apply用法
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-08 10:03:25
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

### 知识点

- 空值删除和填充
- `apply、applymap`用法
- `shift()`用法
- value_counts()和mean()：统计每个元素的出现次数和行（列）的平均值

### 缺失值和空值处理

### 概念

空值：空值就是没有任何值，`""`

缺失值：`df`中缺失值为`nan`或者`naT`(缺失时间)，在S型数据中为`none`或者`nan`

#### 相关函数

- `df.dropna()`删除缺失值
- `df.fillna()`填充缺失值
- `df.isnull()`
- `df.isna()`

[官方文档](https://pandas.pydata.org/pandas-docs/stable/reference/frame.html)

<!--MORE-->

####  `df.dropna()`

函数作用：删除含有空值的行或列，删除缺失值

```python
DataFrame.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)
```

- `axis`：维度，`0`表示`index`行，`1`表示`columns`列，默认为0

- `how：`
  	- `all：`全部为缺失值则删除该行或者列
  	- `any：`至少有一个则删除

- `thresh`：指定至少出现了thresh个才删除

- `subset`：指定在某些列的子集中选择出现了缺失值的列删除，不在子集中不会删除(axis决定行\列）

- `inplace`：刷选过缺失值得到的新数据是存为副本还是直接在原数据上进行修改。

```python
df = pd.DataFrame({"name": ['Alfred', 'Batman', 'Catwoman'],
                   "toy": [np.nan, 'Batmobile', 'Bullwhip'],
                   "born": [pd.NaT, pd.Timestamp("1940-04-25"),pd.NaT]})
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
      <th>name</th>
      <th>toy</th>
      <th>born</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alfred</td>
      <td>NaN</td>
      <td>NaT</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Batman</td>
      <td>Batmobile</td>
      <td>1940-04-25</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Catwoman</td>
      <td>Bullwhip</td>
      <td>NaT</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 默认参数：删除行，只要有空值就会删除，不替换
df.dropna()
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
      <th>name</th>
      <th>toy</th>
      <th>born</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Batman</td>
      <td>Batmobile</td>
      <td>1940-04-25</td>
    </tr>
  </tbody>
</table>

</div>

```python
df.dropna(how='any')
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
      <th>name</th>
      <th>toy</th>
      <th>born</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Batman</td>
      <td>Batmobile</td>
      <td>1940-04-25</td>
    </tr>
  </tbody>
</table>

</div>

```python
df.dropna(how='all')  # 全部是空值才会删除
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
      <th>name</th>
      <th>toy</th>
      <th>born</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Alfred</td>
      <td>NaN</td>
      <td>NaT</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Batman</td>
      <td>Batmobile</td>
      <td>1940-04-25</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Catwoman</td>
      <td>Bullwhip</td>
      <td>NaT</td>
    </tr>
  </tbody>
</table>

</div>

```python
df.dropna(thresh=2)  # 至少出现两个空值删除
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
      <th>name</th>
      <th>toy</th>
      <th>born</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Batman</td>
      <td>Batmobile</td>
      <td>1940-04-25</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Catwoman</td>
      <td>Bullwhip</td>
      <td>NaT</td>
    </tr>
  </tbody>
</table>

</div>

```python
df.dropna(subset=['name', 'born'])
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
      <th>name</th>
      <th>toy</th>
      <th>born</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>Batman</td>
      <td>Batmobile</td>
      <td>1940-04-25</td>
    </tr>
  </tbody>
</table>

</div>

#### `df.fillna()`

主要作用：填充缺失值

```python
DataFrame.fillna(value=None, method=None, axis=None, inplace=False, limit=None, downcast=None, **kwargs)
```

- value：使用什么填充缺失值，不同的列可以使用不同的值

- axis：指定填充维度

- `method`：填充方法，**不能和value同时出现，避免冲突**

  		- `ffill`：用缺失值的前一个值进行填充，axis=1：横向填充（前后），axis=0：纵向填充（上下）
  		- `backfill、bfill`：缺失值后面的一个填充代替前一个

- `limit`：填充的次数限制

  

  ```python
  df1 = pd.DataFrame([[np.nan, 2, np.nan, 0],
                    [3, 4, np.nan, 1],
                   [np.nan, np.nan, np.nan, 5],
                  [np.nan, 3, np.nan, 4]],
                   columns=list('ABCD'))
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
        <th>A</th>
        <th>B</th>
        <th>C</th>
        <th>D</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th>0</th>
        <td>NaN</td>
        <td>2.0</td>
        <td>NaN</td>
        <td>0</td>
      </tr>
      <tr>
        <th>1</th>
        <td>3.0</td>
        <td>4.0</td>
        <td>NaN</td>
        <td>1</td>
      </tr>
      <tr>
        <th>2</th>
        <td>NaN</td>
        <td>NaN</td>
        <td>NaN</td>
        <td>5</td>
      </tr>
      <tr>
        <th>3</th>
        <td>NaN</td>
        <td>3.0</td>
        <td>NaN</td>
        <td>4</td>
      </tr>
    </tbody>
  </table>

  </div>

  

  ```python
  df1.fillna(axis=1, method='ffill') # 横向填充 
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
        <th>0</th>
        <td>NaN</td>
        <td>2.0</td>
        <td>2.0</td>
        <td>0.0</td>
      </tr>
      <tr>
        <th>1</th>
        <td>3.0</td>
        <td>4.0</td>
        <td>4.0</td>
        <td>1.0</td>
      </tr>
      <tr>
        <th>2</th>
        <td>NaN</td>
        <td>NaN</td>
        <td>NaN</td>
        <td>5.0</td>
      </tr>
      <tr>
        <th>3</th>
        <td>NaN</td>
        <td>3.0</td>
        <td>3.0</td>
        <td>4.0</td>
      </tr>
    </tbody>
  </table>

  </div>

  

  ```python
  df1.fillna(axis=0, method='ffill') # 纵向填充
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
        <th>0</th>
        <td>NaN</td>
        <td>2.0</td>
        <td>NaN</td>
        <td>0</td>
      </tr>
      <tr>
        <th>1</th>
        <td>3.0</td>
        <td>4.0</td>
        <td>NaN</td>
        <td>1</td>
      </tr>
      <tr>
        <th>2</th>
        <td>3.0</td>
        <td>4.0</td>
        <td>NaN</td>
        <td>5</td>
      </tr>
      <tr>
        <th>3</th>
        <td>3.0</td>
        <td>3.0</td>
        <td>NaN</td>
        <td>4</td>
      </tr>
    </tbody>
  </table>

  </div>

  

  ```python
  df1.fillna(value=2)  # 缺失值填充2
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
        <th>0</th>
        <td>2.0</td>
        <td>2.0</td>
        <td>2.0</td>
        <td>0</td>
      </tr>
      <tr>
        <th>1</th>
        <td>3.0</td>
        <td>4.0</td>
        <td>2.0</td>
        <td>1</td>
      </tr>
      <tr>
        <th>2</th>
        <td>2.0</td>
        <td>2.0</td>
        <td>2.0</td>
        <td>5</td>
      </tr>
      <tr>
        <th>3</th>
        <td>2.0</td>
        <td>3.0</td>
        <td>2.0</td>
        <td>4</td>
      </tr>
    </tbody>
  </table>

  </div>

  

  ```python
  values = {'A':0, 'B':1, "C":2, "D":3}
  df1.fillna(value=values)
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
        <th>0</th>
        <td>0.0</td>
        <td>2.0</td>
        <td>2.0</td>
        <td>0</td>
      </tr>
      <tr>
        <th>1</th>
        <td>3.0</td>
        <td>4.0</td>
        <td>2.0</td>
        <td>1</td>
      </tr>
      <tr>
        <th>2</th>
        <td>0.0</td>
        <td>1.0</td>
        <td>2.0</td>
        <td>5</td>
      </tr>
      <tr>
        <th>3</th>
        <td>0.0</td>
        <td>3.0</td>
        <td>2.0</td>
        <td>4</td>
      </tr>
    </tbody>
  </table>

  </div>

  

  ```python
  df1.fillna(value=values, limit=2) #  限制填充次数
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
        <th>0</th>
        <td>0.0</td>
        <td>2.0</td>
        <td>2.0</td>
        <td>0</td>
      </tr>
      <tr>
        <th>1</th>
        <td>3.0</td>
        <td>4.0</td>
        <td>2.0</td>
        <td>1</td>
      </tr>
      <tr>
        <th>2</th>
        <td>0.0</td>
        <td>1.0</td>
        <td>NaN</td>
        <td>5</td>
      </tr>
      <tr>
        <th>3</th>
        <td>NaN</td>
        <td>3.0</td>
        <td>NaN</td>
        <td>4</td>
      </tr>
    </tbody>
  </table>

  </div>

  

  ```python
  df1.fillna(value=values, inplace=True)  # inplace表示用新生成的数据代替原来的，而不是副本
  df1  # 原数据已经变化了
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
        <th>0</th>
        <td>0.0</td>
        <td>2.0</td>
        <td>2.0</td>
        <td>0</td>
      </tr>
      <tr>
        <th>1</th>
        <td>3.0</td>
        <td>4.0</td>
        <td>2.0</td>
        <td>1</td>
      </tr>
      <tr>
        <th>2</th>
        <td>0.0</td>
        <td>1.0</td>
        <td>2.0</td>
        <td>5</td>
      </tr>
      <tr>
        <th>3</th>
        <td>0.0</td>
        <td>3.0</td>
        <td>2.0</td>
        <td>4</td>
      </tr>
    </tbody>
  </table>

  </div>

  

  ### `df.isnull()`和`df.isna()`

  二者都是判断是不是缺失值

  ```python
  df1.isnull()
  df1.isna()
  ```

  ![](https://s2.ax1x.com/2019/10/08/ufCMqO.png)

  ------------------------

  ### 如何增加一列数值

```python
dates
```

```
DatetimeIndex(['2019-09-24', '2019-09-25', '2019-09-26', '2019-09-27',
               '2019-09-28', '2019-09-29'],
              dtype='datetime64[ns]', freq='D')
```

```python
# 在数据df的基础上增加一列E
data = df.reindex(index=dates[0:4], columns=list(df.columns) + ['E'])  
```

```python
# 将E列的前2行设为1
data.loc[dates[0]:dates[1], 'E'] = 1 
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
      <td>-1.636419</td>
      <td>0.222190</td>
      <td>0.504764</td>
      <td>0.614839</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>-0.516706</td>
      <td>-0.084145</td>
      <td>1.191989</td>
      <td>0.532722</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>1.841656</td>
      <td>-0.917348</td>
      <td>0.064767</td>
      <td>1.574627</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>0.637159</td>
      <td>-1.165726</td>
      <td>3.050687</td>
      <td>-0.847791</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

</div>

### 统计值

```python
data.mean()  
# data.mean(0)  默认是每个列上的均值
```

```
A    0.081423
B   -0.486257
C    1.203052
D    0.468599
E    1.000000
dtype: float64
```

```python
data.mean(1)   # 求行轴上的均值
```

```
2019-09-24    0.141075
2019-09-25    0.424772
2019-09-26    0.640926
2019-09-27    0.418582
Freq: D, dtype: float64
```

```python
pd.Series([1, 3, 5, np.nan, 6, 8], index=dates)
```

```
2019-09-24    1.0
2019-09-25    3.0
2019-09-26    5.0
2019-09-27    NaN
2019-09-28    6.0
2019-09-29    8.0
Freq: D, dtype: float64
```

```python
# shift函数将结果移动两个单位，广播机制
s = pd.Series([1, 3, 5, np.nan, 6, 8], index=dates).shift(2)  
s
```

```
2019-09-24    NaN
2019-09-25    NaN
2019-09-26    1.0
2019-09-27    3.0
2019-09-28    5.0
2019-09-29    NaN
Freq: D, dtype: float64
```

```python
df.sub(s, axis='index')
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
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-09-25</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2019-09-26</th>
      <td>0.841656</td>
      <td>-1.917348</td>
      <td>-0.935233</td>
      <td>0.574627</td>
    </tr>
    <tr>
      <th>2019-09-27</th>
      <td>-2.362841</td>
      <td>-4.165726</td>
      <td>0.050687</td>
      <td>-3.847791</td>
    </tr>
    <tr>
      <th>2019-09-28</th>
      <td>-4.284321</td>
      <td>-5.942288</td>
      <td>-2.905034</td>
      <td>-4.137728</td>
    </tr>
    <tr>
      <th>2019-09-29</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

</div>

#### apply用法(重点)

```python
# 求出每列的max 和 min
def f(x):
    return pd.Series([x.min(), x.max()], index=["min", "max"])
df.apply(f)
```



```python
f = lambda x: x.max() - x.min()
df.apply(f)
# df.apply(f, axis="columns") 表示在行上执行
```

```
A    3.478075
B    1.387917
C    2.985920
D    2.643529
dtype: float64
```

**关于applymap函数：得到df数据中浮点值的格式化字符串**

```python
formatFunc = lambda x: "%.2f" % x
df.applymap(formatFunc)  # 输出的数据为2位小数
```

#### 统计每个value出现的次数

```python
s = pd.Series(np.random.randint(0,7, size=10))
s
```

```
0    3
1    5
2    2
3    4
4    6
5    5
6    4
7    4
8    1
9    3
dtype: int32
```

```python
s.value_counts()
```

```
4    3
5    2
3    2
6    1
2    1
1    1
dtype: int64
```

