---
title: Spark笔记1-入门Hadoop
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-23 15:07:58
password:
summary:
copyright: true
tags:
  - 大数据
  - spark
  - Hadoop
categories:
  - 大数据
  - Spark
---

### 大数据4大特性

- Volume：大量化
- Velocity：快速化
- Variety：多样化
- Value：价值密度低

<!--MORE-->

### 关键技术

数据采集、数据存储和管理、数据处理和分析和数据隐私和安全。主要关注点是：

#### 分布式存储

解决数据存储问题，代表：

- GFS/HDFS
- Big Table
- NoSql
- NewSQL

#### 分布式处理

解决数据高效计算问题，带表

- MapReduce
- Spark
- Flink

### 大数据计算模式

- 批处理计算模式：MapReduce

- 流计算：实时处理，实时做出响应：Storm\Flume\S4

- 图计算：地理信息系统，社交网络等：Pregel

  ![](https://s2.ax1x.com/2019/10/23/KYUukV.md.png)

- 查询分析计算：google Dremel、Hive、Cassandra

  ![](https://s2.ax1x.com/2019/10/23/KYURtf.md.png)

### Hadoop

`Hadoop`中相关组件有

- **HDFS：海量分布式文件管理系统，针对数据存储**
- YARN：`资源调度管家`，一个集群支持多种框架。管理系统，系统、`CPU`和内存等，解决开发成本高和集群资源利用率等问题

![](https://s2.ax1x.com/2019/10/23/KY0qr4.png)

- **MapReduce：分布式计算框架，针对数据计算**

  - 编程容易：屏蔽了底层分布式并行编程细节
  - 分而治之：将大任务分成多个子任务，并行执行任务

  ![](https://s2.ax1x.com/2019/10/23/KYwzjg.md.png)

- Hive：数据仓库，查询时候写的SQL语句；编程接口，将SQL语句自动转成HDFS对应的查询分析

- Pig： 数据流处理，和Hive联合处理

- Mahout：数据挖掘库，实现分类、聚类和回归等

  - 调用接口，传参数，较少工作量
  - 针对海量数据进行数据挖掘分析

- Ambari：安装、部署、配置和管理工具

- Zookeeper：分布式协作服务

- HBase：分布式数据库，**一主多从架构**

- Flume：日志收集分析功能

- Sqoop：数据库`ETL`，完成各个组件之间的互联互通功能

![](https://s2.ax1x.com/2019/10/23/KYaE9O.md.png)

> Hadoop的缺点是：
>
> - 表达能力有限：不管应用如何，总是抽象成map和reduce两个函数，降低了分布式应用开发的复杂性
> - 磁盘IO开销大：各种迭代功能
> - 延迟高

### Spark

spark（2009年）是一个单纯的计算框架，比MapReduce更佳，取而代之，本身不具备存储能力。**火的原因：社区好、企业支持早**

#### 优势

**操作多样化**

Spark中的操作不再仅限于map和reduce两个操作，操作类型多，表达能力更强，操作还包含：

1. groupby
2. join
3. filter
4. .....

**提供内存计算**

数据生成之后，将数据写入内存中，下次直接在内存中进行调用即可。

##### 底层架构

底层是`spark core`，`spark`框架图：

- spark SQL：分析关系数据，进行查询
- spark streaming：流计算
- MLlib：机器学习算法库
- GraphX：编写图计算应用程序

![](https://s2.ax1x.com/2019/10/23/KYBdQU.png)

### Flink

 **Apache Flink** 是一个分布式大数据处理引擎，2008年诞生，也是一个计算框架。可对有限数据流和无限数据流进行有状态或无状态的计算，能够部署在各种集群环境，对各种规模大小的数据进行快速计算。 

![](https://s2.ax1x.com/2019/10/23/KYyMhn.md.png)

#### spark和Flink对比

`Flink`更适合做流计算

![](https://s2.ax1x.com/2019/10/23/KYyTu8.md.png)





### Beam

`Beam`是谷歌公司提出来的，想将各种框架统一起来。

![](https://s2.ax1x.com/2019/10/23/KY6Ii9.md.png)