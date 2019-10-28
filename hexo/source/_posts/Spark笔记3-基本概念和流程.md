---
title: Spark笔记3-基本概念和流程
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-23 19:28:05
password:
summary:
copyright: true
tags:
  - 大数据
  - spark
categories:
  - 大数据
  - Spark
---

### 基本概念

RDD：**弹性分布式数据集**，数据可大可小，动态的变化分区数量，分布式地保存在多台机器的内存当中

DAG：`Directed Acyclic Graph`，有向无环图，反映和RDD之间的依赖关系

Executor：运行在工作节点上进程，负责运行`Task`

Application：用户编写的运行程序

Task：运行在`Executor`上的工作单元

Job：一个Job包含多个`RDD`，以及作用于相应`RDD`上的各种操作。

Stage：`Job`的基本调度单位，一个job分成多个task，每组task被称为stage。



<!--MORE-->

![](https://s2.ax1x.com/2019/10/23/KtiP58.md.png)



![](https://s2.ax1x.com/2019/10/23/KtFJfS.md.png)



### 架构特点

常见的架构有两种：`P2P`和一主多从架构。`Spark`采用的是：一主多从

- 主节点：`Driver Program`
- 多个从节点：`Worker  Node`

![](https://s2.ax1x.com/2019/10/23/KtiuV0.png)

![](https://s2.ax1x.com/2019/10/23/Ktiaa6.png)



### 执行过程

应用---->主节点（任务控制节点）--->作业--->阶段（每个阶段是多个任务的集合）

![](https://s2.ax1x.com/2019/10/23/Ktizz4.png)

![](https://s2.ax1x.com/2019/10/23/KtiIzQ.md.png)



### 运行基本流程

- 为应用构建基本的运行环境，找到主节点`Driver` （指挥所）
- 主节点创建一个`Spark Context` ，它被当做整个运行程序的指挥官
- 指挥官找资源管理器（YARN、Mesos）申请资源，再进行资源的分配，从而启动`Executor`进程（启动Worker Node）
- 指挥官`Spark Context`根据提交的代码，通过`RDD`的依赖关系，生成一个`DAG`有向无环图。应用程序就是针对`RDD`一次又一次的操作。
- 将DAG图交给`DAG Scheduler`，将`DAG`图分解成多个阶段`Stage`。每个阶段Stage中包含多个任务`task`
- `Stage`中任务的分发是通过`Task Scheduler`进行的，分发给不同的节点进行处理，**分发原则：计算向数据靠拢**。如果数据在机器`A`上，就将数据分发给机器`A`，实现数据的本地化处理。如果将数据分发给其他机器，会产生额外的开销。

![](https://s2.ax1x.com/2019/10/23/KtuimF.md.png)

-----

**执行完之后：**

![](https://s2.ax1x.com/2019/10/23/KtuoN9.md.png)

