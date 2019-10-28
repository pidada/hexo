---
title: pandasNote1
date: 2019-09-09 18:08:46
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
from pandas import Series, DataFrame
```

### Series创建

- 基本知识
  - 类似于一维数组的对象
  - 由一组数据（各种`Numpy`数据类型）和数据标签（索引）组成
  - 左边索引，右边数值；
- 不指定索引的话，自动从0开始；
- 索引也可以自定义：index=['a', 'b', 'c', 'd']
- 通过`Python`的字典类型创建

<!-- MORE -->

```python
obj = pd.Series([4, 7, 8, -1])
obj
```

```
0    4
1    7
2    8
3   -1
dtype: int64
```

```python
# 指定索引值
obj1 = pd.Series([1, 2, 3, 4], 
                 index=['a', 'b', 'c', 'd'])
obj1
```

```
a    1
b    2
c    3
d    4
dtype: int64
```

```python
# 通过字典的形式创建Series数据
# S中的索引就是原来字典中的键
data1 = {"city": "shenzhen", "age": 25, "number": 123456}
obj2 = pd.Series(data1)
obj2
```

```
city      shenzhen
age             25
number      123456
dtype: object
```

### `Series`中值的获取 

- 通过索引的方式获取
  - 使用`Series`自己创建时候的索引
  - 使用默认的数值索引
  - 使用布尔型数组、标量乘法、应用函数等作为索引

```python
# 通过自己创建的索引来获取数据
obj1[['a', 'c', 'b']]
```

```
a    1
c    3
b    2
dtype: int64
```

```python
# 默认数值索引来获取数据
obj1[:3]
```

```
a    1
b    2
c    3
dtype: int64
```

```python
# 布尔型数组过滤掉不满足要求的数据
obj1[obj1 >= 2]
```

```
b    2
c    3
d    4
dtype: int64
```

### 索引操作

#### 索引多样性

- 自建索引
- 默认的数值索引
- 通过Python的字典形式来创建索引

```python
# 上面的obj1
obj1
```

```
a    1
b    2
c    3
d    4
dtype: int64
```

```python
obj1['b']
```

```
2
```

```python
# 通过切片形式，包含末端，和Python1不同
obj1["a":"c"]
```

```
a    1
b    2
c    3
dtype: int64
```

```python
obj1[["b", "a", "d"]]
```

```
b    2
a    1
d    4
dtype: int64
```

```python
obj1[1]
```

```
2
```

```python
obj1[1:4]
```

```
b    2
c    3
d    4
dtype: int64
```

```python
obj1[[1, 3]]
```

```
b    2
d    4
dtype: int64
```

```python
obj1[obj1 > 2]
```

```
c    3
d    4
dtype: int64
```

#### 索引缺失值处理

- 缺失值用`NaN`表示
- `isnull`和`notnull`检测缺失值

```python
# 上面的obj2
obj2
```

```
city      shenzhen
age             25
number      123456
dtype: object
```

```python
# sex对应的值找不到，用NaN表示
data2 = ["sex", "city", "age", "number"]
obj3 = pd.Series(obj2, index=data2)
obj3
```

```
sex            NaN
city      shenzhen
age             25
number      123456
dtype: object
```

```python
pd.isnull(obj3)
```

```
sex        True
city      False
age       False
number    False
dtype: bool
```

```python
# Series的实例方法isnull和notnull
obj3.isnull()
```

```
sex        True
city      False
age       False
number    False
dtype: bool
```

#### `Series`对象的`name`属性

- `S`数据本身和索引都有`name`属性
- 能够直接指定`name`属性的值

```python
# S的name属性
obj3.name = "person"
# S索引的name属性
obj3.index.name = "information"
obj3
```

```
information
sex            NaN
city      shenzhen
age             25
number      123456
Name: person, dtype: object
```

#### 索引就地修改

```python
# 上面栗子中的number修改为phone_num
obj3.index = ["sex", "city", "age", "phone_num"]
obj3
```

```
sex               NaN
city         shenzhen
age                25
phone_num      123456
Name: person, dtype: object
```

### DataFrame

- 表格型数据结构，含有一组有序的列
- 既有行索引也有列索引

#### DF创建

- 使用pd.DataFrame(data)
- 直接传入字典型数据
- 通过`columns`参数指定各个属性的顺序

```python
# 1.通过传入等长列表或者Numpy数组组成的字典
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada', 'Nevada'],
        'year': [2000, 2001, 2002, 2001, 2002, 2003],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9, 3.2]}
frame = pd.DataFrame(data)
frame
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
      <th>state</th>
      <th>year</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Ohio</td>
      <td>2000</td>
      <td>1.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ohio</td>
      <td>2001</td>
      <td>1.7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Ohio</td>
      <td>2002</td>
      <td>3.6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Nevada</td>
      <td>2001</td>
      <td>2.4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Nevada</td>
      <td>2002</td>
      <td>2.9</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Nevada</td>
      <td>2003</td>
      <td>3.2</td>
    </tr>
  </tbody>
</table>

</div>



```python
# 2. 指定列序列创建，通过columns参数
# 结果中3个列属性和上面的顺序不同
pd.DataFrame(data, columns=["year", "state", "pop"])
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
      <th>year</th>
      <th>state</th>
      <th>pop</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2003</td>
      <td>Nevada</td>
      <td>3.2</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 3、columns中的属性如果不存在，则结果中用缺失值代替，debt属性
# 4、在DF中传入指定的index，有one-six
frame2 = pd.DataFrame(data, columns=["year", "state", "pop", "debt"],
                      index=["one", "two", "three", "four", "five", "six"])
frame2
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
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>three</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>four</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>five</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>six</th>
      <td>2003</td>
      <td>Nevada</td>
      <td>3.2</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

</div>

### <span class="girk">DF操作1</span>

- 1、查看DF中有哪些列属性columns和索引index
- 2、查看DF中的所有数据values，通过属性的方式
- 3、查看DF中的部分数据
  - 查看列数据
    - 通过字典标记或者属性（.点）的方式
    - 获取到的其实就是个S型数据
    - frame[column]    # 更通用
    - frame.column  # 属性的形式
  - 查看行数据
    - loc  # 标签索引查看
    - iloc  # 整数索引查看
- 4、通过赋值修改某列的数据
  - 传入具体数值数据
  - 传入`numpy`生成的数据
  - 传入S型数据，长度需要和D型数据一致，否则空位上将被填上缺失值
  - 赋值新的列：如果操作的列不存在，则会自动创建

```python
# 1、获取DF中的列属性和索引
print(frame2.columns)
print(frame2.index)
```

```
Index(['year', 'state', 'pop', 'debt'], dtype='object')
Index(['one', 'two', 'three', 'four', 'five', 'six'], dtype='object')
```

```python
# 2、查看values
frame2.values
```

```
array([[2000, 'Ohio', 1.5, nan],
       [2001, 'Ohio', 1.7, nan],
       [2002, 'Ohio', 3.6, nan],
       [2001, 'Nevada', 2.4, nan],
       [2002, 'Nevada', 2.9, nan],
       [2003, 'Nevada', 3.2, nan]], dtype=object)
```

```python
# 3、查看列数据
frame2.year
```

```
one      2000
two      2001
three    2002
four     2001
five     2002
six      2003
Name: year, dtype: int64
```

```python
frame2["year"]
```

```
one      2000
two      2001
three    2002
four     2001
five     2002
six      2003
Name: year, dtype: int64
```

```python
# 4、赋值修改列
# 注意是整列的修改
frame2["debt"] = 18
frame2
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
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>18</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>18</td>
    </tr>
    <tr>
      <th>three</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>18</td>
    </tr>
    <tr>
      <th>four</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>18</td>
    </tr>
    <tr>
      <th>five</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>18</td>
    </tr>
    <tr>
      <th>six</th>
      <td>2003</td>
      <td>Nevada</td>
      <td>3.2</td>
      <td>18</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 传入numpy数据
frame2["debt"] = np.arange(6.0)
frame2
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
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>three</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>four</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>five</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>six</th>
      <td>2003</td>
      <td>Nevada</td>
      <td>3.2</td>
      <td>5.0</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 传入S型数据
val = pd.Series([1.2, 1.9, 2], index=["one", "three", "five"])
frame2["debt"] = val
frame2
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
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>1.2</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>three</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>1.9</td>
    </tr>
    <tr>
      <th>four</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>five</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>six</th>
      <td>2003</td>
      <td>Nevada</td>
      <td>3.2</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

</div>

------

### DF操作2（重点）

- 1、 **创建不存在的列**：只能通过字典标记的形式
- 2、创建布尔型数据
  - <span class="burk">如何创建一列布尔值(T/F)的数据</span>
  - <span class="burk">如何创建一个新的属性数据</span>
- 3、删除数据del
- 4、嵌套字典形式创建DF数据
  - 外层作为列索引
  - 内层作为行索引
- 5、DF转置T
- 6、DF中传入S型数据
- 7、设置DF的columns和index属性的name属性

### 创建数据

- <span class="burk">如何创建一列布尔值(T/F)的数据</span>
- <span class="burk">如何创建一个新的属性数据</span>

```python
# 1、2

# 先判断state属性的值是否为Ohio
# 如果等于，将eastern属性的值设为T，否则为F
# eastern属性是新建的，只能通过字典标记的形式
frame2["eastern"] = (frame2.state == "Ohio")
frame2
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
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
      <th>eastern</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>1.2</td>
      <td>True</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>NaN</td>
      <td>True</td>
    </tr>
    <tr>
      <th>three</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>1.9</td>
      <td>True</td>
    </tr>
    <tr>
      <th>four</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>NaN</td>
      <td>False</td>
    </tr>
    <tr>
      <th>five</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>2.0</td>
      <td>False</td>
    </tr>
    <tr>
      <th>six</th>
      <td>2003</td>
      <td>Nevada</td>
      <td>3.2</td>
      <td>NaN</td>
      <td>False</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 3 删除数据
del frame2["eastern"]
```

```python
frame2
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
      <th>year</th>
      <th>state</th>
      <th>pop</th>
      <th>debt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>one</th>
      <td>2000</td>
      <td>Ohio</td>
      <td>1.5</td>
      <td>1.2</td>
    </tr>
    <tr>
      <th>two</th>
      <td>2001</td>
      <td>Ohio</td>
      <td>1.7</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>three</th>
      <td>2002</td>
      <td>Ohio</td>
      <td>3.6</td>
      <td>1.9</td>
    </tr>
    <tr>
      <th>four</th>
      <td>2001</td>
      <td>Nevada</td>
      <td>2.4</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>five</th>
      <td>2002</td>
      <td>Nevada</td>
      <td>2.9</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>six</th>
      <td>2003</td>
      <td>Nevada</td>
      <td>3.2</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>

</div>

```python
frame2["pop"]
```

```
one      1.5
two      1.7
three    3.6
four     2.4
five     2.9
six      3.2
Name: pop, dtype: float64
```

```python
frame2.pop
```

```
<bound method NDFrame.pop of        year   state  pop  debt
one    2000    Ohio  1.5   1.2
two    2001    Ohio  1.7   NaN
three  2002    Ohio  3.6   1.9
four   2001  Nevada  2.4   NaN
five   2002  Nevada  2.9   2.0
six    2003  Nevada  3.2   NaN>
```

```python
# 4  嵌套字典创建DF：外层为列属性，内层为行
pop = {'Nevada': {2001: 2.4, 2002: 2.9},
       'Ohio': {2000: 1.5, 2001: 1.7, 2002: 3.6}}
frame3 = pd.DataFrame(pop)
frame3
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
      <th>Nevada</th>
      <th>Ohio</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000</th>
      <td>NaN</td>
      <td>1.5</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>2.4</td>
      <td>1.7</td>
    </tr>
    <tr>
      <th>2002</th>
      <td>2.9</td>
      <td>3.6</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 5、转置操作
frame3.T
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
      <th>2000</th>
      <th>2001</th>
      <th>2002</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Nevada</th>
      <td>NaN</td>
      <td>2.4</td>
      <td>2.9</td>
    </tr>
    <tr>
      <th>Ohio</th>
      <td>1.5</td>
      <td>1.7</td>
      <td>3.6</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 6 、DF中传入S型数据
pdata = {'Ohio': frame3['Ohio'][:-1],
         'Nevada': frame3['Nevada'][:2]}
pd.DataFrame(pdata)
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
      <th>Ohio</th>
      <th>Nevada</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2000</th>
      <td>1.5</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>1.7</td>
      <td>2.4</td>
    </tr>
  </tbody>
</table>

</div>

```python
# 获取DF中的所有数据
frame2.values
```

```
array([[2000, 'Ohio', 1.5, 1.2],
       [2001, 'Ohio', 1.7, nan],
       [2002, 'Ohio', 3.6, 1.9],
       [2001, 'Nevada', 2.4, nan],
       [2002, 'Nevada', 2.9, 2.0],
       [2003, 'Nevada', 3.2, nan]], dtype=object)
```



<CENTER><span style='font-size:2.5rem'>Stay Foolish Stay Hungry</span></CENTER>