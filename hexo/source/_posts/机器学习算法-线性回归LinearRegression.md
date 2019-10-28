---
title: 线性回归LinearRegression
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-15 18:07:50
password:
summary:
tags:
  - ML
  - LinearRegression
copyright: true
categories:
  - Machine learning
---

### 线性回归法

#### 思想

- 解决回归问题
- 算法可解释性强
- 一般在坐标轴中：横轴是特征（属性），纵坐标为预测的结果，输出标记（具体数值）

#### 和分类问题的区别

 **分类问题中，横轴和纵轴都是样本特征属性（肿瘤大小，肿瘤发现时间）** 

**回归问题中，横轴样本点（房子大小，地段等），纵轴是预测值（房价）**

<!--MORE-->

#### 问题产生

![](https://s2.ax1x.com/2019/10/15/KCx2qI.png)

- 求解出拟合的直线$y=ax+b$

- 根据样本点$x^{(i)}$，求解预测值$\hat y^{(i)}$

- 求解真实值和预测值的差距尽量小 ，通常用`差的平方和最小`表示，损失函数为：
  $$
  \mathop {min}\sum ^{m}_{i=1} (y^{(i)}-{\hat {y^{(i)}}})^2
  $$
  
  $$
  \mathop {min}\sum ^{m}_{i=1} ({y^{i}-ax^{(i)}-b})^2
  $$
  

- 上面的损失函数`loss function`实际上就是求解$a,b$

### 最小二乘法求解$a,b$
求解损失函数$J(a,b)$的过程：
$$
J(a,b) = \mathop {min}\sum ^{m}_{i=1} ({y^{i}-ax^{(i)}-b})^2
$$
 分别对$a,b$求导，在令导数为0，进行求解最终结果为：

![](https://s2.ax1x.com/2019/10/15/KCzwOs.png)

- 先对b求导

  ![](https://s2.ax1x.com/2019/10/15/KCzDwq.png)

![](https://s2.ax1x.com/2019/10/15/KCz2pF.png)

- 对a求导：

![](https://s2.ax1x.com/2019/10/15/KCzHfO.png)

![](https://s2.ax1x.com/2019/10/15/KCzqpD.png)

$a$的另一种表示形式：

![](https://s2.ax1x.com/2019/10/15/KCzXXd.png)

### 向量化过程
向量化主要是针对$a$的式子来进行改进，将：分子看做$w^{(i)},v^{(i)}$，分母看做$w^{(i)},w^{(i)}$

![](https://s2.ax1x.com/2019/10/15/KCzz7t.png)

![](https://s2.ax1x.com/2019/10/15/KPS9tf.png)

```python
import numpy as np

class SimpleLinearRegression1(object):
    def __init__(self):
        # ab不是用户送进来的参数，相当于是私有的属性
        self.a_ = None
        self.b_ = None
    
    def fit(self, x_train,y_train):
        # fit函数：根据训练数据集来得到模型
        assert x_train.ndim == 1, \
            "simple linear regression can only solve single feature training data"
        assert len(x_train) == len(y_train), \
            "the size of x_train must be equal to the size of y_train"

        x_mean = np.mean(x_train)
        y_mean = np.mean(y_train)

        num = 0.0
        d = 0.0
        for x, y in zip(x_train, y_train):
            num += (x - x_mean) * (y - y_mean)
            d += (x - x_mean) ** 2
        
        self.a_ = num / d
        self.b_ = y_mean - self.a_ * x_mean
        
        # 返回自身，sklearn对fit函数的规范
        return self
    
    def predict(self, x_predict):
        # 传进来的是待预测的x 
        assert x_predict.ndim == 1, \
            "simple linear regression can only solve single feature training data"
        assert self.a_ is not None and self.b_ is not None, \
            "must fit before predict!"
            
        return np.array([self._predict(x) for x in x_predict])
    
    def _predict(self, x_single):
        # 对一个数据进行预测 
        return self.a_ * x_single + self.b_
    
    def __repr__(self):
        # 字符串输出
        return "SimpleLinearRegression1()"
    
  
 # 通过向量化实现
class SimpleLinearRegression2(object):
    def __init__(self):
        # a, b不是用户送进来的参数，相当于是私有的属性
        self.a_ = None
        self.b_ = None
    
    def fit(self, x_train, y_train):
        # fit函数：根据训练数据集来得到模型
        assert x_train.ndim == 1, \
            "simple linear regression can only solve single feature training data"
        assert len(x_train) == len(y_train), \
            "the size of x_train must be equal to the size of y_train"

        x_mean = np.mean(x_train)
        y_mean = np.mean(y_train)
        
        #  改成向量形式代替for循环，numpy中的.dot形式
        #  参考上面的向量化公式 
        num = (x_train - x_mean).dot(y_train - y_mean)
        d = (x_train - x_mean).dot(x_train - x_mean)
        
        self.a_ = num / d
        self.b_ = y_mean - self.a_ * x_mean
        
        # 返回自身，sklearn对fit函数的规范
        return self
    
    def predict(self, x_predict):
        # 传进来的是待预测的x 
        assert x_predict.ndim == 1, \
            "simple linear regression can only solve single feature training data"
        assert self.a_ is not None and self.b_ is not None, \
            "must fit before predict!"
            
        return np.array([self._predict(x) for x in x_predict])
    
    def _predict(self, x_single):
        # 对一个数据进行预测 
        return self.a_ * x_single + self.b_
    
    def __repr__(self):
        # 字符串函数，输出方便进行查看
        return "SimpleLinearRegression2()"
```



### 衡量标准
**衡量标准：将数据分成训练数据集`train`和测试数据集`test`，通过训练数据集得到a和b，再通过测试数据集进行衡量**

![](https://s2.ax1x.com/2019/10/15/KPSZBn.png)

- 均方误差MSE，mean squared error，存在量纲问题
  $$
  MSE=\frac {1}{m}\sum ^{m}_{i=1}(y^{(i)}_{test}-\hat y^{(i)}_{test})^2
  $$
  

- 均方根误差RMSE，root mean squared error

  
  $$
  RMSE=\sqrt{MSE_{test}}=\sqrt {\frac {1}{m}\sum ^{m}_{i=1}(y^{(i)}_{test}-\hat y^{(i)}_{test})^2}
  $$
  

- 平均绝对误差MAE，mean absolute error，
  $$
  MAE=\frac {1}{m}\sum^{m}_{i=1}|y^{(i)}_{test}-\hat y^{(i)}_{test}|
  $$

**sklearn中没有RMSE，只有MAE、MSE**

```python
import numpy as np
from math import sqrt


def accuracy_score(y_true, y_predict):
    '''准确率的封装：计算y_true和y_predict之间的准确率'''
    assert y_true.shape[0] == y_predict.shape[0], \
    "the size of y_true must be equal to the size of y_predict"

    return sum(y_true ==y_predict) / len(y_true)


def mean_squared_error(y_true, y_predict):
    # 计算y_true 和 y_predict之间的MSE
    assert len(y_true) == len(y_predict), \
        "the size of y_true must be equal to the size of y_predict"
    return np.sum((y_true - y_predict)**2) / len(y_true)


def root_mean_squared_error(y_true, y_predict):
    # 计算y_true 和 y_predict之间的RMSE
    return sqrt(mean_squared_error(y_true, y_predict))


def mean_absolute_error(y_true, y_predict):
    # 计算y_true 和 y_predict之间的MAE
    assert len(y_true) == len(y_predict), \
        "the size of y_true must be equal to the size of y_predict"
    
    return np.sum(np.absolute(y_true - y_predict)) / len(y_true)
```

![](https://s2.ax1x.com/2019/10/15/KPS3jJ.png)

### $R^2$指标
$R^2$指标的定义为
$$
R^2=1- \frac {SS_{residual}}{SS_{total}}
$$

$$
R^2=1-\frac {\sum_i{(\hat y^{(i)}-y^{(i)}})^2}{\sum_i{(\bar y-y^{(i)}})^2}
$$
![](https://s2.ax1x.com/2019/10/15/KPSouj.png)

>分子为模型预测产生的误差；分母为使用均值产生的误差（baseline model产生的误差）

**式子表示为：预测模型没有产生误差的指标**
- $R^2 \leq 1$

- $R^2$越小越好。$R^2$最大值为1，此时预测模型不犯误差。模型等于基准模型时，$R^2$为0

- 当$R^2$小于0，此时学习到的模型还不如基准模型，说明数据可能不存在线性关系

- **R^2**的另一种表示为

  ![](https://s2.ax1x.com/2019/10/15/KPpPV1.png)

### 多元线性回归
将特征数从1拓展到了N，求解思路和一元线性回归类似。

![](https://s2.ax1x.com/2019/10/15/KPpE8O.png)

 目标函数 

![](https://s2.ax1x.com/2019/10/15/KPpuqA.png)

![](https://s2.ax1x.com/2019/10/15/KPplIP.png)

![](https://s2.ax1x.com/2019/10/15/KPptMQ.png)

![](https://s2.ax1x.com/2019/10/15/KPpUqs.png)

![](https://s2.ax1x.com/2019/10/15/KPpwaq.png)

![](https://s2.ax1x.com/2019/10/15/KPpcM4.png)