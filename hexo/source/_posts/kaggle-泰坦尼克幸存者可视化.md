---
title: kaggle_泰坦尼克幸存者可视化
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-23 16:34:31
password:
summary:
copyright: true
tags:
  - 可视化
  - kaggle
  - numpy 
  - pandas
  - 决策树
categories:
  - Machine learning
---

> 泰坦尼克数据是`kaggle`中最经典的数据之一，本文通过对原数据的处理，利用决策树实现对幸存者的预测可视化。主要掌握的知识点：

- 数据的导入及清洗
- 缺失值如何处理
- 删除不必要的属性
- 如何将文字转成数字，让`sklearn`进行处理

### 导入相关模块和包

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.tree import DecisionTreeClassifier   # 决策树的类
from sklearn.model_selection import GridSearchCV, train_test_split, cross_val_score  # 网格搜索，TTS， 交叉验证
```

### 导入数据及查看信息

- `pandas`中怎么导入数据：`pd.read_csv("file_path")`
- 观察数据信息
    - `head()`查看前n行数据，默认是前5行
    - `info()`查看数据的各种属性和标签    
- 数据中部分属性存在缺失值

<!-- MORE -->

```python
data = pd.read_csv(r"D:/Python/datalearning/sklearn/day08_data.csv")
data.head()
```

![](https://s2.ax1x.com/2019/09/23/uil3dJ.png)

### 数据特点

- Cabin属性中存在大量的缺失值
- 数据中存在文字信息

![](https://s2.ax1x.com/2019/09/23/uilvmF.png)

### 数据预处理

#### 严重缺失值的删除

```python
# 将缺失值严重的数据进行删除
# axis=1：表示对列进行操作，inplace=True表示用生成的数据代替原来的数据
data.drop(["Cabin","Name","Ticket"], inplace=True, axis=1)
```



#### 少量缺失值的填充

```python
# 缺失值处理
data["Age"] = data["Age"].fillna(data["Age"].mean())   # 用均值进行填充
# 将存在缺失值数据的行进行删除，dropna默认对行进行操作
data = data.dropna()
```

- `Age`字段中缺少量的值，通过平均值来进行填充，学习下fillna函数，默认是填充0；填充不仅仅是均值
- `Embarked`字段中缺少值，将其他的字段全部`dropna`，使得每个属性的数据相同

![](https://s2.ax1x.com/2019/09/23/uiGFlq.png)





#### 查看某个属性的种类

属性的唯一性通过`unique()`来进行观察

```python
# 查看属性中有多少类：通过unique()函数进行显示，结果只有3类
data["Embarked"].unique()

[OUT]:array(['S', 'C', 'Q'], dtype=object)
```



#### 文字分类转成数字

由于`sklearn`不能处理文字，需要将上述属性的3个种类`SCQ`转成数字。

```python
# 重点：如何将输出标签中的分类转成数字
labels = data["Embarked"].unique().tolist()
data["Embarked"] = data["Embarked"].apply(lambda x: labels.index(x))
```



在`sex`属性中只有`M-F`，转成0-1

- `loc`：标签索引

- `iloc`：数值索引

  **int(True)结果为1**

```python
# data["sex"] = (data["Sex"] == "male").astype("int")
data.loc[:,"Sex"] = (data["Sex"] == "male").astype("int")   # 将生成的布尔值转成0-1， 
data.head()
```

#### 样本数据获取

```python
# 取出属性不是Survied的所有行的数据
X = data.iloc[:,data.columns != "Survived"]  
y = data.iloc[:,data.columns == "Survived"]    作为最终的输出结果
```



#### TTS

```python
Xtrain, Xtest, ytrain, ytest = train_test_split(X,y,test_size=0.3)
```



#### 乱序索引的还原

经过上面的`TTS`步骤，索引已经被打烂了，需要进行还原。

```python
# Xtrain.index = range(Xtrain.shape[0])
for i in [Xtrain, Xtest, ytrain, ytest]:
    i.index = range(i.shape[0])  # 将range函数的结果赋值给索引
```



### 模型建立

- 实例化
- 进行拟合
- 计算`score`

```python
# 普通模型
clf = DecisionTreeClassifier(random_state=25)
clf = clf.fit(Xtrain, ytrain)
score = clf.score(Xtest,ytest)
score

# 交叉验证：结果未必会比上面的好
clf = DecisionTreeClassifier(random_state=25)
score = cross_val_score(clf,X,y,cv=10).mean()
score
```



### 不同max_depth下的模型拟合情况

```python
tr = []
te = []
for i in range(10):
    clf = DecisionTreeClassifier(random_state=25
                                 ,max_depth=i+1     # max_depth不同
                                 ,criterion="entropy"  # 欠拟合使用entropy
                                 )
    clf = clf.fit(Xtrain, ytrain)
    score_tr = clf.score(Xtrain, ytrain)
    score_te = cross_val_score(clf,X,y,cv=10).mean()
    tr.append(score_tr)
    te.append(score_te)
# print(max(te))
plt.plot(range(1,11),tr,color="red",label="train")
plt.plot(range(1,11),te,color="blue",label="test")
plt.xticks(range(1,11))
plt.legend()
plt.show()
```

![](https://s2.ax1x.com/2019/09/23/uiTl8g.png)

### 网格搜索

网格搜索是将多个参数的不同取值放在一起，同时进行参数的调节，找出最匹配的值，本质上是枚举技术。

```python
# 基尼系数0-0.5；信息熵的范围是0-1；gini_threholds = np.linspace(0,0.5,50)
# [*range(1,10)]：转成列表形式
parameters = {
    "criterion":("gini","entropy")
    ,"splitter":("best","random")
    ,"max_depth":[*range(1,10)]
    ,"min_samples_leaf":[*range(1,50,5)]
    ,"min_impurity_decrease":[*np.linspace(0,0.5,50)]
}

clf = DecisionTreeClassifier(random_state=25)
GS = GridSearchCV(clf,parameters,cv=10)
GS.fit(Xtrain,ytrain)
```



![](https://s2.ax1x.com/2019/09/23/uiISMR.png)

```python
GS.best_params_   # 返回参数和参数取值列表中的最佳组合
GS.best_score_    # 网格搜索模型后的评判标准
```

