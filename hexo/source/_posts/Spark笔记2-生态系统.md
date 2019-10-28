---
title: Spark笔记2-生态系统
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-23 19:08:10
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

### 三大应用常景

#### 场景

- 复杂的批处理：`MapReduce`
- 交互式查询
- 基于实时数据流的流处理：`storm`

#### 缺陷

1. 数据无法无缝共享，数据格式需要进行转换
2. 维护成本高
3. 资源利用率低下：每个资源框架都有自己的调度管家

<!--MORE-->

### Spark生态

一个软件栈满足不同的应用场景，通过YRAN作为公共的资源调度管家。

- `SQL`吉时查询
- 流式计算
- 机器学习
- 图计算

![](https://s2.ax1x.com/2019/10/23/KYgPh9.png)

#### 构成

![](https://s2.ax1x.com/2019/10/23/Kt9JKI.md.png)

#### 机器学习算法库

![](https://s2.ax1x.com/2019/10/23/Kt9yMn.png)

#### 各个组件应用

![](https://s2.ax1x.com/2019/10/23/Kt9WIU.png)