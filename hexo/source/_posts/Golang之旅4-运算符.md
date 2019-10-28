---
title: Golang之旅4-运算符
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-16 16:25:45
password:
summary:
copyright: true
tags:
  - go
  - 分布式
  - 高性能
  - 高并发
categories: 
  - go
---

### 算术运算符

算术运算符包含：+、-、*、/、%(求余数)

```go
import "fmt"

func main(){
	a := 10
	b := 3
	//实现加减乘，求商和求余数
	fmt.Println(a + b)
	fmt.Println(a - b)
	fmt.Println(a * b)
	fmt.Println(a / b)   //求商
	fmt.Println(a % b)  //求余数

	a++
	fmt.Println(a)   //通过自加1，变成11
	a--
	fmt.Println(a)   //通过自减1，变成10
}
```

**a++和a--是单独的语句**

<!-- MORE -->



### 关系运算符

![](https://s2.ax1x.com/2019/09/17/nhrBdg.png)

### 位运算

- &：相与，两个数同时为1才为1
- |：相或，只要有一个为1就为1
- ^：异或运算，相异时则为1，否则为0
- <<：左移$2^n$
- \>>：右移$2^n$

![](https://s2.ax1x.com/2019/09/17/nhs8mT.png)

### 赋值运算符

a += 5 等价于a = a+5

![](https://s2.ax1x.com/2019/09/17/nhsy7D.png)



```go
package main

import "fmt"

func main(){
	a := 10
	b := 3
	//1、实现加减乘，求商和求余数
	fmt.Println(a+b)
	fmt.Println(a-b)
	fmt.Println(a*b)
	fmt.Println(a/b)   //求商
	fmt.Println(a%b)  //求余数

	a++
	fmt.Println(a)   //通过自加1，变成11
	a--
	fmt.Println(a)   //通过自减1，变成10

	//2、关系运算符
	fmt.Println(10 > 2)
	fmt.Println(10 != 2)
	fmt.Println(4 > 5)

	//3、逻辑运算符
	fmt.Println(10>5 && 1==1)
	fmt.Println(!(10>5))
	fmt.Println(1>5 || 1==1)  //管道符||表示或

	//4、位运算符
	c := 1   //001
	d := 5   //101
	fmt.Println(c & d)  //001--->1
	fmt.Println(c | d)  //101--->5
	fmt.Println(c ^ d)  //100--->4

	fmt.Println(1 << 2)  //1--->100:4
	fmt.Println(4 >> 2)  //100--->1
	fmt.Println(1 << 10) //1024表示容量

	//赋值运算符
	var f int
	f = 5
	f += 5   //f = f + 5
	fmt.Println(f)
	f *= 2
	fmt.Println(f)
}
```

