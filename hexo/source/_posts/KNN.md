---
title: KNN算法
top: false
cover: false
toc: true
katex: true
date: 2019-09-12 17:51:12
password: 
summary:
tags: knn 
copyright: true
categories: Machine learning
---

### KNN导读



> `KNN`算法是机器学习中最简单的算法，一句经典的总结：`物以类聚`

> k-近邻算法`k-nearest neighbor, kNN`是一种基本分类和回归的算法。`k`近邻算法中的输入为实例的特征向量，输出为实例的类别，类别可以有多类。算法主要思想：

- 给定一个训练集的数据，实例的类别已定

- 对于新的实例，根据`k`个最近邻的训练实例的类别，经投票表决等方式进行预测

- 算法不具有显式的学习过程，实际上利用训练集对特征向量空间进行划分。

  

### KNN三要素

- `k`的选择：`k`值如何选择？越大越好吗？奇偶性如何？经验值是多少？
- 距离度量：选择什么距离来进行度量新实例和训练集上点的距离？
- 分类决策规则：选择怎样的规则来对距离进行分类，从而判断新实例属于哪个类？



### k近邻算法

> 直观解释：给定一个训练数据集，对于新输入的实例，在训练集数据中找出和该实例最邻近的k个实例。这k个实例中的多数属于某个类，就将新实例划分为这个类别。
> 
> **输入训练数据集**
> $$
> T=\{(x_1,y_1),(x_2,y_2),...(x_i,y_i)....(x_N,y_N)\}
> $$
>其中，x<sub>i</sub>为实例特征向量，y<sub>i</sub>为实例的类别；i=1,2,3,...N。
> 
> **输出**：实例x所属的类别y

- 根据给定的距离度量，在训练集`T`中找出与`x`最近邻的`k`个点，涵盖这个`k`个点的`x`的邻域记作：N<sub>k</sub>(x)

- 在邻域N<sub>k</sub>(x)中根据分类规则决定x的类别y
  $$
  y = \mathop {arg max}\limits_{c_j}\sum_{x_i\in{N_k(x)}}I(y_i=c_j), i=1,2...,N;j=1,2,...K
  $$
  

上式中，I为指示函数，即当：y<sub>i</sub>=c<sub>j</sub>是为1，不等则为0

- `k=1`称之为最近邻算法。对于输入的新实例，将训练集中离x**最近点**的所属类作为`x`的类别



### k近邻模型

k近邻算法的模型主要有三个要素:

- 距离度量
- `k`值的选择
- 分类决策规则的规定



### 距离度量

> 特征空间中两个实例点的距离是两个实例点相似度的反映。`k`近邻模型的特征空间一般是`n`维实数向量空间$R^n$。一般使用的欧式距离，也可以是其他距离，如：$L_p$距离或者`Minkowski`距离。

假设特征空间$X$是n维实数向量空间$R^n$，${x_i,x_j}\in X$，其中$x_i=(x_i^{(1)},x_i^{(2)},....,x_i^{(n)})$，
$x_j=(x_j^{(1)},x_j^{(2)},....,x_j^{(n)})$

$x_i,x_j$的$L_p$的距离定义为
$$
L_p(x_i,x_j)=(\sum_{l=1}^{n}|x_i^{(l)}-x_j^{(l)}|^p)^\frac{1}{p}
$$
此距离实际上就是`明科夫斯基距离`

规定：$p\geq1$

- 当`p=2`时，即为欧式距离，即：
  $$
  L_2(x_i,x_j)=(\sum_{l=1}^{n}|x_i^{(l)}-x_j^{(l)}|^2)^\frac{1}{2}
  $$
  
- 当`p=1`时，即为曼哈顿距离，即：
  $$
  L_1(x_i,x_j)=\sum_{l=1}^{n}|x_i^{(l)}-x_j^{(l)}|
  $$
  
- 当$p$趋于无穷，它是各个坐标距离的最大值：
  $$
  L_{\infty}(x_i,x_j)=\mathop {max}\limits_{l}|x_i^{(l)}-x_j^{(l)}|
  $$
  

### k值选择

 #### **k值较小**

- 用较小的邻域中的实例进行预测
- 近似误差减小，估计误差增大
- 预测结果对近邻的实例点非常敏感；如果近邻点恰好是噪声，预测出错

#### **k值较大**

- 用较大的邻域中的实例点进行预测
- 减少学习的估计误差，但是近似误差增大
- 与输入实例较远的点的训练实例也会起预测作用
- k值增大意味着整个模型变得简单

**k值一般选取较小的值，一般是奇数值；通过交叉验证来选取最优的k值**



### 分类决策的规则

`k`近邻法中分类决策通常采取的是**多数表决**，即输入实例的k个近邻的训练实例中的多数列决定输入实例的类。如果分类的损失函数是0-1分类，分类函数是
$$
f:R^n--->\{c_1,...,c_n\}
$$
误分类的概率是
$$
P(Y\neq {f(X)})=1-P(Y=f(X))
$$
多数表决规则等价于经验风险最小化。



### 数据归一化处理
将所有的数据映射到同一个尺度上
- 最值归一化：数据映射到0-1上面
    $$
    X_{scale}= \frac{x-x_{min}}{x_{max}-x_{min}}
    $$
    
    - 适合分布明显边界的情况：比如考试分数，像素边界
    - 缺点：受`outlier`影响，比如收入没有边界
    
- 均值方差归一化`standardization`：均值为`0`，方差为`1`的分布中
    $$
    x_{scale}=\frac{x-{x_{mean}}}{s}
    $$
    
    - 数据分布没有明显边界
    - 可能存在极端数据

```python
# 调用用于均值归一化的类
from sklearn.preprocessing import StandardScaler
standScaler = StandardScaler()
standScaler.fit(X_train)
standScaler.mean_
# scale表示的是方差
standScaler.scale_
standScaler.transform(X_train)
```

### TTS

>将导入的样本数据分成训练集`train`和测试集`test`两类，一般是2：8
- 分成训练集和测试集
- 需要设置随机种子`seed`

```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=666)
```

### 网格搜索

- 超参数之间可能存在相互的制约关系
- 比如，`p`只有在`method`为`distance`的条件下才有意义

```python
import numpy as np
import pandas as pd
import matplotlib 
import matplotlib.pyplot as plt
from sklearn import datasets
```

```python
# 导入数据集
digits = datasets.load_digits()
X = digits.data
y = digits.target
```

```python
from sklearn.model_selection import train_test_split
# 拆分为训练数据和测试集
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=666)
```

```python
# 导入的类就是KNN算法
from sklearn.neighbors import KNeighborsClassifier
# 传入的参数相当于是k值
knn_clf = KNeighborsClassifier(n_neighbors=3, weights='uniform')
# 数据的拟合
knn_clf.fit(X_train, y_train)
knn_clf.score(X_test, y_test)
```

```
0.9888888888888889
```

#### 网格搜索参数设置

```python
# 具体参数的设置
param_grid = [
    {
        'weights': ['uniform'],
        'n_neighbors': [i for i in range(1, 11)]
    },
    {
        'weights': ['distance'],
        'n_neighbors': [i for i in range(1, 11)],
        'p': [i for i in range(1, 6)]
    }
]
```

```python
# 实例化类
knn_clf = KNeighborsClassifier()
```

```python
from sklearn.model_selection import GridSearchCV
grid_search = GridSearchCV(knn_clf, param_grid)
```

```python
%%time
grid_search.fit(X_train, y_train)
```

```
GridSearchCV(cv='warn', error_score='raise-deprecating',
             estimator=KNeighborsClassifier(algorithm='auto', leaf_size=30,
                                            metric='minkowski',
                                            metric_params=None, n_jobs=None,
                                            n_neighbors=5, p=2,
                                            weights='uniform'),
             iid='warn', n_jobs=None,
             param_grid=[{'n_neighbors': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
                          'weights': ['uniform']},
                         {'n_neighbors': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
                          'p': [1, 2, 3, 4, 5], 'weights': ['distance']}],
             pre_dispatch='2*n_jobs', refit=True, return_train_score=False,
             scoring=None, verbose=0)
```

```python
grid_search.best_estimator_
```

```
KNeighborsClassifier(algorithm='auto', leaf_size=30, metric='minkowski',
                     metric_params=None, n_jobs=None, n_neighbors=3, p=3,
                     weights='distance')
```

```python
# 查看准确率
grid_search.best_score_0
.9853862212943633
```

```python
# 查看最终形成的最好参数
grid_search.best_params_{
'n_neighbors': 3, 'p': 3, 'weights': 'distance'}
```

```python
# 查看测试数据的准确度
knn_clf = grid_search.best_estimator_
knn_clf.score(X_test,y_test)0
# 结果
.9833333333333333
```

#### 核数和输出信息显示

```python
# n_jobs几个核，加快速度；verbose：进行算法信息的输出
grid_search = GridSearchCV(knn_clf, param_grid, n_jobs=1, verbose=2)
grid_search.fit(X_train, y_train)
```

```
c:\users\admin\venv\lib\site-packages\sklearn\model_selection\_split.py:1978: FutureWarning: The default value of cv will change from 3 to 5 in version 0.22. Specify it explicitly to silence this warning.
  warnings.warn(CV_WARNING, FutureWarning)
[Parallel(n_jobs=1)]: Using backend SequentialBackend with 1 concurrent workers.
```

```
Fitting 3 folds for each of 60 candidates, totalling 180 fits
[CV] n_neighbors=1, weights=uniform ..................................
[CV] ................... n_neighbors=1, weights=uniform, total=   0.6s
[CV] n_neighbors=1, weights=uniform ..................................
```

```
[Parallel(n_jobs=1)]: Done   1 out of   1 | elapsed:    0.5s remaining:    0.0s
```

```
[CV] ................... n_neighbors=1, weights=uniform, total=   0.6s
[CV] n_neighbors=1, weights=uniform ..................................
[CV] ................... n_neighbors=1, weights=uniform, total=   0.5s
....
[CV] ............ n_neighbors=10, p=5, weights=distance, total=   0.6s
```

```
GridSearchCV(cv='warn', error_score='raise-deprecating',
             estimator=KNeighborsClassifier(algorithm='auto', leaf_size=30,
                                            metric='minkowski',
                                            metric_params=None, n_jobs=None,
                                            n_neighbors=3, p=3,
                                            weights='distance'),
             iid='warn', n_jobs=1,
             param_grid=[{'n_neighbors': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
                          'weights': ['uniform']},
                         {'n_neighbors': [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
                          'p': [1, 2, 3, 4, 5], 'weights': ['distance']}],
             pre_dispatch='2*n_jobs', refit=True, return_train_score=False,
             scoring=None, verbose=2)
```
