---
title: Spark笔记4-RDD运行原理
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-23 20:30:47
password:
summary:
copyright: true
tags:
  - RDD
  - spark
  - shuffle
  - 宽窄依赖
categories:
  - 大数据
  - Spark
---

### 概念

`Hadoop`不善于处理迭代场景：逻辑斯蒂回归、模拟退火算法、遗传算法等。`MapReduce`是将中间结果写入磁盘中，下次使用直接从磁盘中取出来，产生两个问题：

- 磁盘`IO`开销大
- 序列化和反序列化的开销（序列化：将数据对象转成可保存和传输的相应格式，比如Java对象转成二进制或者字符串等）

<!--MORE-->

> RDD提供了抽象的数据结构，不必在意底层数据的特性，只需要将数据变成一个个RDD转换，不同的转换之间通过依赖关系，形成了DAG有向无环图。
>
> 通过采用一定的优化原理，形成管道化（流水线化）操作，数据不再需要保存在磁盘或者其他存储器中，可以直接进行使用。

`RDD`是一个分布式对象的集合，本质上是一个只读的分区记录集合，是一个高度受限的共享内存模型。**只有在转换的过程中能够进行修改。**比如，学生的成绩从60分，加5分之后变成65的过程是可以进行修改的。

**只能通过生成新的RDD来达到数据修改的目的**

![](https://s2.ax1x.com/2019/10/23/KtYzss.png)



### 操作

两种主要类型的操作如下，它们都是**粗粒度**的操作：**只支持对RDD中所有的数据进行操作，不支持向数据库那样的单条数据的操作。**

- Action：动作类型操作
- Transformation：转换类型操作

`Spark`中提供了各种`RDD`的`API`，程序员可以通过调用`API`来实现对`RDD`的各种操作。

![](https://s2.ax1x.com/2019/10/23/KtJ4H0.md.png)

#### 惰性调用机制

转换过程不会真正产生数据的输出，只记录转换的轨迹，只有通过动作类型的操作才会发生计算，产生最终的结果。

![](https://s2.ax1x.com/2019/10/23/Kt6spR.png)

![](https://s2.ax1x.com/2019/10/23/Kt6xhj.md.png)

#### 流水线优化

从输出产生的结果不会写入磁盘中，直接持久化到`RDD.cache()`中再进行输出,避免了不必要的磁盘开销。

![](https://s2.ax1x.com/2019/10/23/KtcGUe.md.png)



###  特性

- 只读：本身不能修改，只能通过转换进行数据修改
- 粗粒度：一次性全集操作
- 高效的容错性
- 避免不必要的序列化和反序列化



### RDD之间的依赖关系

在实际中一个应用`application`被分成多个作业`task`，一个task被分成多个阶段`stage`，一个阶段是多个任务`task`的集合。

> 一个作业为什么要分成多个阶段stage？

#### shuffle操作

**是否包含shuffle操作（洗牌操作）**是区分宽依赖和窄依赖的关键。只要发生了`shuffle`操作，一定是发生了很多数据的来回分发动作。数据一定会被写入磁盘中，其中发生等待操作，则无法发生流水线的操作。

![](https://s2.ax1x.com/2019/10/24/KNEuKe.png)

窄依赖：不断加入阶段，可以进行流水线操作

宽依赖：必须划分多个阶段，发生了`shuffle`操作，不能进行流水线操作

#### 窄依赖

没有进行`shuffle`操作，表现为一个父`RDD`分区对应为一个子`RDD`分区或者多个`RDD`分区对应一个子`RDD`分区。

总结：只能有一个子`RDD`分区

![](https://s2.ax1x.com/2019/10/24/KNEarQ.png)

![](https://s2.ax1x.com/2019/10/24/KNEsP0.png)

#### 宽依赖

产生了`shuffle`操作，一个父`RDD`分区对应多个子`RDD`分区

![](https://s2.ax1x.com/2019/10/24/KNERr4.png)



-----

![](https://s2.ax1x.com/2019/10/24/KNE4aR.md.png)

### Spark优化原理

优化是通过`fork/join`机制。多个分区通过fork并行执行转化操作，再通过join操作

![](https://s2.ax1x.com/2019/10/24/KNE7RK.png)

#### 优化栗子



![](https://s2.ax1x.com/2019/10/24/KNVJoR.md.png)

**宽依赖中，shuffle操作发生（数据的写磁盘和等待操作），产生数据来回转换并发生等待，则无法流水线化操作。**

![](https://s2.ax1x.com/2019/10/24/KNVyTA.md.png)





去掉在上海的`join`过程，管道化处理：

![](https://s2.ax1x.com/2019/10/24/KNVuWV.png)

### RDD运行原理全过程

- 将写的代码提交给整个`Spark`框架，生成有向无环图`DAG`
- `DAG`提交给`DAG Scheduler`，分解成多个阶段，每个阶段包含多个任务`task`
- 每个任务分配给`Task  Scheduler`,`Task  Scheduler`将任务分发给工作节点`WorkerNode`上的`Excutor`进程
- 通过`Excutor`进程派发的每个线程去执行任务

![](https://s2.ax1x.com/2019/10/24/KNZzgf.md.png)

### Spark部署方式

主要有`单机部署`和`集群部署`两种方式，集群部署的3中方式包含：

- `Standalone`：`Spark`自带的集群管理器，效率不高
- `YARN`：目前最常用的方式
- `Mesos`：性能匹配好

**Spark和MapReduce是对等的关系，已经取而代之**