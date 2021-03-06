---
title: MySQL学习17_索引B+树
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-17 13:39:38
password:
summary:
copyright: true
tags:
  - MySQL
  - index
  - B+树
categories:
  - MySQL
  - 进阶
---

### 树

- 无序树：任意节点的子节点之间没有任何的顺序关系，称之为无序树，也叫自由树

- 有序树：子节点之间由顺序关系

  - 二叉树：**每个节点最多含有两个子树的树**
    - 完全二叉树：若一棵树的深度为d，除去第d层外，其他各层的节点数目达到了最大值，且第d层所有节点从左向右连续紧密的排列的二叉树
    - 满二叉树：所有层的节点数达到了最大数
    - 平衡二叉树：当且仅当任何节点之间的`两颗子树的高度差不大于1`的二叉树
    - 排序二叉树：二叉搜索树，二叉查找树，性质：**任何节点左边的数比节点上的数小，右边比节点上的数大**
  - 霍夫曼树：用于信息编码
  - $B$树/$B^+$树：在`MySQL`的索引中使用

  <!--MORE-->

#### 应用场景

- HTML文件
- 路由协议
- mysql索引
- 文件目录的目录结构
- AI算法都是树搜索，比如决策树

### 树的遍历

**层次遍历是从上到下，从左到右遍历的方式**

深度遍历的三种遍历顺序（左节点一定在右节点前面）：

- 前序遍历：根--->左--->右

- 中序遍历：左--->根--->右

- 后序遍历：左--->右--->根

  ```python
  # 先序遍历：根---左---右
  根---0
  左---0,1,3,7
  右1---0,1,3,7,8（根左右）
  右2---0,1,3,7,8,4,9
  右3---0,1,3,7,8,4,9,2,5,6
  
  # 中序遍历：左---根---右
  左---7
  根---7,3,8（左根右）
  右1---7,3,8,1,9,4
  右2---7,3,8,1,9,4,0,5,2,6
  
  # 后序遍历：左右根
  左---7
  右---7,8,3(左右根)
  根1---7,8,3,9,4,1
  根2---7,8,3,9,4,1,5,6,2,0
  ```

  

![](https://s2.ax1x.com/2019/10/17/KAQOzT.png)

#### 如何通过遍历结果反推树的结构（必须给出中序遍历）

只有给定中序才能将左、右子树分开

```python
# 反推demo

# 前：0,1,3,7,8,4,9,2,5,6
# 中：7,3,8,1,9,4,0,5,2,6
```

- 前序遍历：`根左右`规则确定**第一个**是根节点为0
- 中序遍历：
  - 根据根节点0，将中序结果分成三部分：`7,3,8,1,9,4 + 0 + 5,2,6`
  - 所以在中序遍历结果中，左子树：738194 ；右子树：526
- 前序遍历：根据上步，分成三部分：`0+1,3,7,8,4,9+2,5,6`；左子树：`1,3,7,8,4,9`；右子树：`2,5,6`
  - 左子树：`1,3,7,8,4,9`，左根节点是1
  - 右子树：`2,5,6`，右根节点是2
- 中序遍历：根据上步，确定了三个根节点，将中序结果分成多块：`7,3,8 + (1) + 9,4 + (0) + 5 + (2) + 6`
  - 左子树：`7,3,8 + (1) + 9,4 `中根据中序的`左根右`规则，7,3,8是左子树，9,4为右子树
    - 根据`左根右`规则，依次对应：`左7根3右8`；94中9是左子树，根是4
  - 右子树：`5 + (2) + 6`，5是左节点，6是右节点

-----

### 索引

> 索引是加速`MySQL`对表中数据行的高效获取而创建的一种分散存储的数据结构，正确地创建合适的索引作用是提高数据查询性能的基础。

B+树查询时间，树的高度有关

- 平均查询时间是$O(logn)$
- 哈希存储索引$O(1)$

[图解MySQL索引]( https://www.cnblogs.com/liqiangchn/p/9060521.html )

### 索引实现

> `mysql`默认存储引擎`innodb`只显式支持B-Tree( 从技术上来说是B+Tree)索引。
>
> 对于频繁访问的表，`innodb`会透明建立自适应hash索引，即在B树索引基础上建立hash索引，可以显著提高查找效率，对于客户端是透明的，不可控制的，隐式的。 

#### 哈希索引

基于哈希表实现。存储引擎会对所有的列计算一个哈希码， Hash索引将所有的哈希码存储在索引中，同时在索引表中保存指向每个数据行的指针

![](https://s2.ax1x.com/2019/10/17/KAaH2j.md.png)

### B+ Tree

B树是一种多路搜索树，每个节点可以拥有多于两个子节点。M路的B树最多拥有M个子节点。 B+树中的B代表平衡（balance），而不是二叉（binary），因为B+树是从最早的平衡二叉树演化而来的。

#### 二叉查找树

- 左节点小于根节点

- 右节点大于根节点

  ![](https://s2.ax1x.com/2019/10/17/KAgPAA.png)





 

![](https://s2.ax1x.com/2019/10/17/KA0BV0.md.png)