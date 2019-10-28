---
title: Spark笔记5-环境搭建和使用
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-24 09:25:10
password:
summary:
copyright: true
tags:
  - 搭建环境
  -  使用方法
  - spark
categories:
  - 大数据
  - Spark
---

### 安装环境

- 安装Java和Hadoop2.7.1

- 官网下载

- 配置spark的classpath

  ![](https://s2.ax1x.com/2019/10/24/KNmly8.md.png)



![](https://s2.ax1x.com/2019/10/24/KNmck9.png)

如果需要使用`HDFS`中的文件，则在使用`spark`前先启动`Hadoop`

### 伪分布式

将`Hadoop`配置成伪分布式，将多个节点放在同一台电脑上。`HDFS`中包含两个重要的组件：`namenode`和`datanode`

- `namenode`：管家节点，数据库的服务作用，只有一个`namenode`
- `datanode`：负责具体的存储数据相关

### PySpark

- `pyspark`提供了简单的方式来学习`spark API`
- `pyspark`可以实时、交互的方式来分析数据
- `pyspark`提供了`Python`交互式的执行环境

```python
pyspark --master <master-url>
```

### 运行模式

1. `Spark`的运行模式取决于`master url`值。

- 逻辑CPU个数 = `物理CPU的个数 * CPU的核数` 
- K指的是本地线程个数
- 集群模式：`spark://localhost:7077`，进入集群模式而且是本机独立的模式

![](https://s2.ax1x.com/2019/10/24/KN3as1.md.png)

![](https://s2.ax1x.com/2019/10/24/KN8Ufg.png)

2. 采用本地模式启动`pyspark`的命令主要参数
   - --master：表示连接到某个`master`
   - --jars：用于把相关的jar包添加到`classpath`中；多个`jar`包，用逗号分割符进行连接

```python
# demo

# 本地模式运行在4个CPU上
cd /usr/local/spark
./bin/pyspark --master local[4]

# 使用 --jar 参数
cd /usr/local/spark
./bin/pyspark --master local[4] --jars code.jar

# 执行pyspark默认是local模式
./bin/pyspark  # 进入的是local[*]

# 帮助命令
./bin/ pyspark --help

# 进入后的退出命令（>>> 提示符）
>>>exit()
```

