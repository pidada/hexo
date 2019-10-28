---
title: LaTex公式大全
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-21 23:09:41
password:
summary:
copyright: true
tags: 
  - tool
  - 公式编辑器
  - LaTex
categories:
  - Tool
---

### LaTex简介

> **LaTeX**是一种基于[TeX](https://zh.wikipedia.org/wiki/TeX)的[排版](https://zh.wikipedia.org/wiki/排版)系统，即使用户没有排版和程序设计的知识也可以充分发挥由TeX所提供的强大功能，不必一一亲自去设计或校对，它非常适用于生成高印刷质量的[科技](https://zh.wikipedia.org/wiki/科技)和数学、物理文档。这个系统同样适用于生成从简单的信件到完整书籍的所有其他种类的文档。[LaTex_Wiki](https://zh.wikipedia.org/wiki/LaTeX)

### LaTex应用

由于LaTeX是通过语法来排版的，可以生成需要的图形，乐谱、棋谱、数学公式、化学结构式、电路图等，本文中主要介绍其数学公式中的应用。



![](https://s2.ax1x.com/2019/09/21/uSmvcR.png)



![](https://s2.ax1x.com/2019/09/21/uSmLh4.png)

![](https://s2.ax1x.com/2019/09/21/uSnnDP.png)

### LaTex编辑公式

本文中重点是介绍`LaTex`在数学公式编辑方面的应用，使用编辑器`Typora`。Markdown中使用`LaTex`基础语法有两种情况：

![](https://s2.ax1x.com/2019/09/22/upMXAP.png)

> **Tips:**以下几个字符 # $ % & ~ _ ^ \ { } 有特殊意义，当表示这些字符时，需要转义，即在每个字符前加上 \，防止发生转义。对于`\`，可以使用`\blacklash`命令得到`\`。以下例子通过段内进行展示

- `$`：用于公式的包裹
- `{}`：将特殊内容包含起来
- `\`常用于特殊字符，比如希腊字母、各种特殊符号
- `_、^`：上、下标



[在LaTeX中插入数学公式](https://blog.csdn.net/ruthywei/article/details/82454331)

[Latex数学公式表](https://blog.csdn.net/u011826404/article/details/70215074)

[LaTex神器](https://www.codecogs.com/latex/eqneditor.php?lang=zh-cn)

<!-- MORE -->



#### 上下标

- 上标`^`：`$x^2$`，表示为$x^2$
- 下标`_`：`$x_2$`，表示为$x_2$
- 混合使用：`$x_2^3$`，表示$x_2^3$；`$x^{y_z}$`，表示 $x^{y_z}$
- 支持中文下标：需要用`{}`将下标全部括起来，否则下标只是第一个元素，`$f_{最大值}$`，表示：$f_{最大值}$

#### 分数

分数常见的两种表达形式：

- `$\frac {分母}{分子}$`：`$\frac{y}{x}$`，$\frac{y}{x}$，这是推荐写法
- `${分母} \over {分子}$`：`${y} \over {x}$`，${y} \over {x}$

其中`{}`可以表示多级嵌套，栗子：

- ` $x_0+\frac{1}{x_1+\frac{1}{x_2+\frac{1}{x_3+\frac{1}{x_4}}}}$`表示为：

$ x_0+\frac{1}{x_1+\frac{1}{x_2+\frac{1}{x_3+\frac{1}{x_4}}}}$

- 分数线长度值是预设为分子分母的最大长度，如果想要使分数线再长一点，可以在分子或分母两端添加一些间隔，例如: `$ \frac{1}{2} $` , `$ \frac{\;1\;}{\;2\;} $` 表示$\frac{1}{2}$，$ \frac{\;1\;}{\;2\;} $

#### 根式

开根号的功能主要是根据二次方根来实现的

- 二次方：`$\sqrt$`，`$\sqrt{4}$`表示为$\sqrt{4}$
- n次方：`$sqrt[n]$`，`$\sqrt[3]{8}$`表示为$\sqrt[3]{8}$

#### 二元运算符

![](https://s2.ax1x.com/2019/09/22/upFwBF.png)

#### 箭头

![](https://s2.ax1x.com/2019/09/22/upF2jK.png)

#### 常用公式

- **线性模型**
  `$$h(\theta) = \sum_{j = 0} ^n \theta_j x_j$$`
  
- **均方误差**
  `$$J(\theta) = \frac{1}{2m}\sum_{i = 0} ^m(y^i - h_\theta (x^i))^2$$`
  
- **均方误差**
  `$$J(\theta) = \frac{1}{2m}\sum_{i = 0} ^m(y^i - h_\theta (x^i))^2$$`
  
- 对齐和非对齐公式

  ![](https://s2.ax1x.com/2019/09/22/upkXxx.png)

  ![](https://s2.ax1x.com/2019/09/22/upApZD.png)

#### 矩阵

```shell
$$
    \begin{matrix}
    1 & x & x^2 \\
    1 & y & y^2 \\
    1 & z & z^2 \\
    \end{matrix}
$$
```

![](https://s2.ax1x.com/2019/09/22/uS3LlD.png)

#### 行列式

```shell
$$
X=\left|
    \begin{matrix}
        x_{11} & x_{12} & \cdots & x_{1d}\\
        x_{21} & x_{22} & \cdots & x_{2d}\\
        \vdots & \vdots & \ddots & \vdots\\
        x_{m1} & x_{m2} & \cdots & x_{md}\\
    \end{matrix}
\right|
$$
```

![](C:%5CUsers%5Cadmin%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5C1569083628032.png)

```shell
$$X=\left(
        \begin{matrix}
            x_{11} & x_{12} & \cdots & x_{1d}\\
            x_{21} & x_{22} & \cdots & x_{2d}\\
            \vdots & \vdots & \ddots & \vdots\\
            x_{m1} & x_{m2} & \cdots & x_{md}\\
        \end{matrix}
    \right)
    =\left(
         \begin{matrix}
                x_1^T \\
                x_2^T \\
                \vdots\\
                x_m^T \\
            \end{matrix}
    \right)
$$
```



![](https://s2.ax1x.com/2019/09/22/uS82Nt.png)

#### 分段函数

```go
$$
f(n) =
\begin{cases}
n/2,  & \text{if $n$ is even} \\
3n+1, & \text{if $n$ is odd}
\end{cases}
$$
```

![](https://s2.ax1x.com/2019/09/22/uS3Zi6.png)



#### 方程组

```shell
$$
\left\{ 
\begin{array}{c}
    a_1x+b_1y+c_1z=d_1 \\ 
    a_2x+b_2y+c_2z=d_2 \\ 
    a_3x+b_3y+c_3z=d_3
\end{array}
\right. 
$$
```



![](https://s2.ax1x.com/2019/09/22/uS38ot.png)



#### 推导过程

```shell
$$
\begin{align}
\frac{\partial J(\theta)}{\partial\theta_j}
& = -\frac1m\sum_{i=0}^m(y^i-h_\theta(x^i)) \frac{\partial}{\partial\theta_j}(y^i-h_\theta(x^i)) \\
& = -\frac1m\sum_{i=0}^m(y^i-h_\theta(x^i)) \frac{\partial}{\partial\theta_j}(\sum_{j=0}^n\theta_jx_j^i-y^i) \\
& = -\frac1m\sum_{i=0}^m(y^i-h_\theta(x^i))x^i_j
\end{align}
$$
```



![](https://s2.ax1x.com/2019/09/22/uS81cF.png)

#### 空白距离

![](https://s2.ax1x.com/2019/09/22/uSJ5wj.png)

#### 省略号

![](https://s2.ax1x.com/2019/10/04/uDUb9O.png)

#### 希腊字母

![](https://s2.ax1x.com/2019/10/04/uDUxHI.png)

> 1、如果使用大写的希腊字母，把命令的首字母变成大写即可，例如: `$ \Gamma $表示$ \Gamma $
>
> 2、如果使用斜体大写希腊字母，再在大写希腊字母的` LaTex `命令前加上 `var` , 例如`$\varGamma$`表示$\varGamma$

#### 标注

![](https://s2.ax1x.com/2019/10/04/uDUyNV.png)

![](https://s2.ax1x.com/2019/10/04/uDU4BR.png)

#### 集合

![](https://s2.ax1x.com/2019/09/22/upAeL8.png)



#### 逻辑

![](https://s2.ax1x.com/2019/09/22/upAneS.png)

#### 数学字体

![](https://s2.ax1x.com/2019/09/22/upAKoQ.png)

#### 括号

![](https://s2.ax1x.com/2019/10/04/uDUTN6.png)

#### 斜体粗体

![](https://s2.ax1x.com/2019/09/22/uSGpDJ.png)

![](C:%5CUsers%5Cadmin%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5C1569083955119.png)

> `LaTex`的学习真的很需要耐心！以后遇到慢慢补充！:smile:

