---
title: pandas系列0-基础操作大全
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-08 22:37:40
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

####  读取和写入文件

| 读取                        | 写入                      |
| --------------------------- | ------------------------- |
| read_csv                    | to_csv                    |
| read_excel                  | to_excel                  |
| read_hdf                    | to_hdf                    |
| read_sql                    | to_sql                    |
| read_json                   | to_json                   |
| read_msgpack (experimental) | to_msgpack (experimental) |
| read_html                   | to_html                   |
| read_gbq(experimental)      | to_gbq (experimental)     |
| read_stata                  | to_stata                  |
| read_sas                    | ro_sas                    |
| read_clipboard              | to_clipboard              |
| read_pickle                 | to_pickle／／速度比csv快  |

#### 保存文件

```python
submission = pd.DataFrame({ 'PassengerId': test_df['PassengerId'],'Survived': predictions })
submission.to_csv("submission.csv", index=False)
# index参数是否写入行names键
```

<!--MORE-->

#### 流处理

当读取大文件的时候，通过`chunksize`可以分批次读取：

```python
# 使用类似迭代器的方式
data=pd.read_csv(file, chunksize=1000000)
for sub_df in data:
    print('hello python')
```

#### 是否为空

```pythonj
pd.isnull(obj)
obj.isnull()
```

#### 转成DF数据框

```python
DataFrame(data, 
          columns=['col1','col2','col3'...],
          index = ['i1','i2','i3'...])
```

#### 查看索引和列名

```python
DataFrame.columns
DataFrame.index
```

#### 列属性和索引重排

```python
DataFrame.reindex([columns=['col1','col2','col3'...])
# 也可以同时重建index和columns
DataFrame.reindex([index=['a','b','c'...],columns=['col1','col2','col3'...])
```

#### 重命名索引和轴

```python
data.rename(index=str.title,columns=str.upper)

#修改某个索引和列名，可以通过传入字典
data.rename(index={'old_index':'new_index'},
            columns={'old_col':'new_col'})
```

#### DF选取子集

![](https://s2.ax1x.com/2019/10/08/uhhMsx.png)

#### 针对S

```python
obj[['a','b','c'...]]
obj['b':'e']=5
```

#### 针对DF

```python
#选择多列
dataframe[['col1','col2'...]]

#选择多行
dataframe[m:n]

#条件筛选
dataframe[dataframe['col3'>5]]

#选择子集
dataframe.iloc[0:3,0:5]
dataframe.ix[0:3,0:5]
```

#### 排序和排名

```python
#默认根据index排序，axis = 1 则根据columns排序
dataframe.sort_index(axis=0, ascending=False)

# 根据值排序
dataframe.sort_index(by=['col1','col2'...])

#排名，给出的是rank值

series.rank(ascending=False)
#如果出现重复值，则取平均秩次

#在行或列上面的排名
dataframe.rank(axis=0)
```

#### 成员 、唯一值、成员资格

```python
obj.unique()
obj.value_count()
obj.isin(['b','c'])
```

#### 透视表

```python
table = df.pivot_table(values=["Price","Quantity"],
            index=["Manager","Rep"],
            aggfunc=[np.sum, np.mean],
            margins=True))

#values：需要对哪些字段应用函数
#index：透视表的行索引(row)
#columns：透视表的列索引(column)
#aggfunc：应用什么函数
#fill_value：空值填充
#margins：添加汇总项

#然后可以对透视表进行筛选
table.query('Manager == ["Debra Henley"]')
table.query('Status == ["pending","won"]')
```

