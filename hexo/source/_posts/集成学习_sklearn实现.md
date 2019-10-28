---
title: 集成学习2——sklearn实现
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-19 23:40:18
password:
summary:
copyright: true
tags: 
  - ML
  - 集成学习
  - sklearn
categories: 
  - Machine learning
---

### 导入数据



```python
import numpy as np
import matplotlib.pyplot as plt
```

```python
from sklearn import datasets
# 样本数，噪声设置，随机种子
# 学习make_moons随机数据集
X, y = datasets.make_moons(n_samples=500, noise=0.3, random_state=42)   
```

```python
# X[y == 0,0]：先取出y值为0的值，再取第一个属性
plt.scatter(X[y == 0,0], X[y==0, 1])
plt.scatter(X[y == 1,0], X[y==1, 1])
plt.show()
```

![image](https://s2.ax1x.com/2019/09/20/nOywHe.png)



<!-- MORE -->



### TTS实现

```
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=666)
```

###  三种方式实现

实现步骤：导入包，创建实例，拟合和得分

- LR
- SVM
- DT

```python
from sklearn.linear_model import LogisticRegression

log_clf = LogisticRegression()
log_clf.fit(X_train, y_train)
log_clf.score(X_test, y_test)
```

```
0.824
```

```python
from sklearn.svm import SVC

svc_clf = SVC()
svc_clf.fit(X_train, y_train)
svc_clf.score(X_test, y_test)
```

```
0.88
```

```python
from sklearn.tree import DecisionTreeClassifier

dt_clf = DecisionTreeClassifier()
dt_clf.fit(X_train, y_train)
dt_clf.score(X_test, y_test)
```

```
0.84
```

### 三种模型的预测

```python
y_predict1 = log_clf.predict(X_test)
y_predict2 = svc_clf.predict(X_test)
y_predict3 = dt_clf.predict(X_test)
```

```python
# y是二分类问题，只有+1，0
# 如果三个结果的平均值大于等于3，则至少有两个结果1，说明y应该是1，少数服从多数原则
y_predict = np.array((y_predict1 + y_predict2 + y_predict3)>=2, dtype="int")
y_predict[:10]
```

```
array([1, 1, 0, 0, 0, 1, 0, 1, 0, 1])
```

```python
from sklearn.metrics import accuracy_score
accuracy_score(y_test, y_predict)
```

```
0.896
```



### 调用sklearn接口实现



```python
from sklearn.ensemble import VotingClassifier

voting_clf = VotingClassifier(estimators=[
    ("log_clf", LogisticRegression()),
    ("svm_clf", SVC()),
    ("dt_clf", DecisionTreeClassifier())], 
                              voting="hard")
```

```python
voting_clf.fit(X_train,y_train)
```

```
VotingClassifier(estimators=[('log_clf',
                              LogisticRegression(C=1.0, class_weight=None,
                                                 dual=False, fit_intercept=True,
                                                 intercept_scaling=1,
                                                 l1_ratio=None, max_iter=100,
                                                 multi_class='warn',
                                                 n_jobs=None, penalty='l2',
                                                 random_state=None,
                                                 solver='warn', tol=0.0001,
                                                 verbose=0, warm_start=False)),
                             ('svm_clf',
                              SVC(C=1.0, cache_size=200, class_weight=None,
                                  coef0=0.0, decision_f...
                             ('dt_clf',
                              DecisionTreeClassifier(class_weight=None,
                                                     criterion='gini',
                                                     max_depth=None,
                                                     max_features=None,
                                                     max_leaf_nodes=None,
                                                     min_impurity_decrease=0.0,
                                                     min_impurity_split=None,
                                                     min_samples_leaf=1,
                                                     min_samples_split=2,
                                                     min_weight_fraction_leaf=0.0,
                                                     presort=False,
                                                     random_state=None,
                                                     splitter='best'))],
                 flatten_transform=True, n_jobs=None, voting='hard',
                 weights=None)
```