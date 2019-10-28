---
title: k均值聚类算法
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-25 20:39:54
password:
summary:
copyright: true
tags:
  - k均值聚类
  - 无监督学习
  - 机器学习
categories:
  - Machine learning
---

### 吴恩达老师-K均值聚类

K均值聚类算法中主要是有两个关键的步骤：簇分配和移动聚类中心。

- 簇分配
  - 假设有一个样本集合，需要将其分成两个类（簇：cluster，红色和蓝色）
  - 首先随机生成两个聚类中心：红色和蓝色两个点
  - 遍历每个样本绿色的点，求出和两个聚类中心的距离，判断和哪个更接近，则归属于哪个类（簇）
- 移动聚类中心
  - 将两个聚类中心（红色和蓝色的叉）**移动到同色点的均值**处，找到所有红色（蓝色）点的均值

重复上述的步骤：簇分配和移动聚类中心，直到颜色的点不再改变，具体算法过程如下各图所示：



<!-- MORE -->

![](https://s2.ax1x.com/2019/09/25/uZjuD0.md.png)

![-](https://s2.ax1x.com/2019/09/25/uZj6xA.png)





![](https://s2.ax1x.com/2019/09/25/uZjfVf.png)

![](https://s2.ax1x.com/2019/09/25/uZjqrq.png)

![](https://s2.ax1x.com/2019/09/25/uZv9z9.png)

![](https://s2.ax1x.com/2019/09/25/uZvVIO.png)



### 算法

#### 输入

- K值：分成K个簇
- 训练样本

![](https://s2.ax1x.com/2019/09/25/ueK3gs.md.png)

#### 簇分配和移动聚类中心

总共K个簇，用$u_1,...,u_K$表示，求出每个样本$x^{(i)}$和某个聚类中心之间距离的最小值，**采用的是欧式距离的平方**，则该样本归属于其类
$$
c_i=\min ||x^{(i)}-u_k||^2
$$


![](https://s2.ax1x.com/2019/09/25/ueQ0XR.md.png)

### 代价损失函数

![](https://s2.ax1x.com/2019/09/25/ue1Ei8.md.png)

![](https://s2.ax1x.com/2019/09/25/ue1yWD.md.png)

### 算法特性

- 基于划分的聚类算法，k值需要预先指定；
- 欧式距离的平方表示样本和聚类中心之间的距离，以中心或者样本的均值表示类别
- 算法是迭代算法，不能得到全局最优解
- 选择不同的初始中心，会得到不同的聚类结果
- 聚类结果的质量一般是通过类的平均直径来进行衡量的
- `k`的选择：一般的，当类别数增加平均直径会减小，当到达某个值后平均直径不再变化，此时的值就是`k`值



### 代码实现

```python
import numpy as np

def kmeans(data, n, m, k):
    rarray = np.random.random(size=k)    # 设置簇的个数k
    rarray = np.floor(rarray*n)   # 
    rarray.astype(int)
    cls = np.zeros([1,n],np.int)            
    center = np.take(data,rarray)
    pcenter = np.zeros([k,m])
    end = True
    while end:
        for i in xrange(n):
            tmp = data[i] - center
            tmp = np.square(tmp)
            tmp = np.sum(tmp,axis=1)
            cls[i] = np.argmin(tmp)
        center = np.zeros([k,m])
        count = np.zeros([1,k],np.int)
        for i in xrange(n):
            center[cls[i]]=center[cls[i]]+data[i]
            count[cls[i]]= count[cls[i]]+1
        if np.sum(center/count - pcenter) <= 1e-4:
            end = False
        pcenter = center/count
```

```python
from numpy import *
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans


#dataSet:聚类数据集
#k:指定的k个类
def kmeans(dataSet, k):
    #得到数据样本的维度n
    sampleNum, col = dataSet.shape
     #初始化为一个(k,n)的矩阵=簇
    cluster = mat(zeros((sampleNum, 2)))
    #生成全零阵
    centroids = zeros((k, col))
    #选择质心
    for i in range(k):
        #索引为随机生成的int类型的且在0-sampleNum间的数
        index = int(random.uniform(0, sampleNum))
        centroids[i, :] = dataSet[index, :]
    #设置flag，查看聚类结果是否发生变化
    clusterChanged = True
     #只要聚类结果一直发生变化，就一直执行聚类算法，直至所有数据点聚类结果不变化
    while clusterChanged:
        #聚类结果变化布尔类型置为false
        clusterChanged = False
         #遍历数据集每一个样本向量
        for i in range(sampleNum):
            #使用sqrt函数（二分法）
            minDist = sqrt(sum(power(centroids[0, :] - dataSet[i, :], 2)))
            minIndex = 0
            #计算点到质心的距离
            for j in range(1,k):
                distance = sqrt(sum(power(centroids[j, :] - dataSet[i, :], 2)))
                #当前最小距离一定时，索引为J
                if distance < minDist:
                    minDist  = distance
                    minIndex = j

            # 当前聚类结果中第i个样本的聚类结果发生变化：布尔类型置为true，继续聚类算法
            if cluster[i, 0] != minIndex:
                clusterChanged = True
                #更新聚类结果和平方误差
                cluster[i, :] = minIndex, minDist**2
        #遍历每一个质心
        for j in range(k):
            #筛选出属于当前质心类的所有样本
            pointsInCluster = dataSet[nonzero(cluster[:, 0].A == j)[0]]
            #计算列的均值（使用axis=0：求列的均值）
            centroids[j, :] = mean(pointsInCluster, axis = 0)

    return centroids, cluster

#创建简单数据集以及进行可视化
dataSet = [[1,1],[3,1],[1,4],[2,5],[11,12],[14,11],[13,12],[11,16],[17,12],[28,10],[26,15],[27,13],[28,11],[29,15]]
dataSet = mat(dataSet)
k = 3
centroids, cluster = kmeans(dataSet, k)
sampleNum, col = dataSet.shape
mark = ['or', 'ob', 'og']

for i in range(sampleNum):
    markIndex = int(cluster[i, 0])
    plt.plot(dataSet[i, 0], dataSet[i, 1], mark[markIndex])

mark = ['+r', '+b', '+g']
for i in range(k):
    plt.plot(centroids[i, 0], centroids[i, 1], mark[i], markersize=12)

plt.show()
```

