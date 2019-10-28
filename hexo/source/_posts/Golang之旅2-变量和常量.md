---
title: Golang之旅2-变量和常量
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-16 11:44:51
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

### 语法特点

- 花括号必须跟在函数的末尾，不能单独成行
- 每个语句的结束没有分号
- 函数外面可以声明变量，但是不能进行函数赋值
- 变量名推荐使用[驼峰体](https://baike.baidu.com/item/骆驼命名法)
- 赋值语句必须放在函数体内
- 变量声明之后如果没有使用，也会报错
- 自带格式化代码工具`go fmt`，不用担心空格和tab键的使用

```
go fmt main.go
```

### 注释

- 单行注释

利用`反斜线`实现

```
//单行注释内容
```

- 多行注释

利用一对`反斜线和星号`实现

```
/*
多行注释内容1
多行注释内容2
*/
```

### 标识符和关键字

#### 标识符

- 字母、数字和下划线_组成
- 不能用数字开头

#### 关键字

25个关键字 + 37个保留字

![img](https://s2.ax1x.com/2019/09/16/nRsC4I.png)

### 变量

程序运行过程中的数据都是保存在内存中，通过找到内存地址找到变量，从而找到对应的数据。

- `GO`是静态类型语言
- 变量功能是存储数据，常用的变量数据类型：`整型、浮点型、布尔型`等
- 以`var`开头，行尾没有分号

```
var 变量名 变量类型
```

- 声明之后再进行使用，同一个作用域内不支持重复声明，且**必须使用**

#### 声明方式

```go
package main //非注释的第一行：声明当前的文件属于哪个包

import "fmt" //导入fmt包，包含格式化输出的函数

func main() {
	//标准声明
	var name string
	var age int
	var isOk bool
	fmt.Println(name, age, isOk)

	//批量声明
	var(
		a string
		b int
		c bool
		d float64
	)
	fmt.Println(a,b,c,d)

	//声明变量指定初始值
	var name1 string = "xiaowangzi"
	var age1 int = 18
	var name3, age3 = "Tom", 28
	fmt.Println(name1, age1, name3, age3)

	//类型推导：系统根据后面的类型自行推导类型；可以在全局中声明
	var name2 = "zhangsan"
	var age2 = 20
	fmt.Println(name2, age2)

	//短变量声明：在函数内部声明
	m := 10
	fmt.Println(m)

	//匿名变量：通过下划线_来实现，不占用命名空间，不会分配内存
}
```

#### 注意

- 函数外的每个语句都必须以关键字开始（`var、const、func`等）
- 短变量`:=`不能使用在函数外。
- 匿名变量`_`多用于占位，表示忽略值。

### 常量

#### `const`

将`var`变成`const`，常量在定义的时候`必须赋值`

```go
package main

import "fmt"

//常量
const pi = 3.14
const e = 2.71

//同时设置多个变量
//const (
//	pi = 3.14
//	e = 2.71
//)

const (
	//变量的值相同，可以略写；只能在常量中使用
	n1 = 10
	n2
	n3
)

func main(){
	fmt.Println(pi, e, n1, n2, n3)
}
```

#### iota

`iota`是`GO`的常量计数器，只能在常量的表达式中使用。

- 出现`const`关键字，`iota`被重置为0
- `const`中每增加一行常量声明，`iota`计数增加一次
- 使用`iota`简化定义，常用于枚举中

```go
package main

import "fmt"

//每出现一次iota，计数加1
const (
	n1 = iota   //0
	n2 = iota   //1
	n3 = iota   //2
	n4 = iota   //3

	//使用下划线跳过某些值；值是存在的，但不需要用
	_
	n6 = iota   //5
	_

	//iota声明中间插队
	n8 = 28
)

//重新出现再次归0
const n9 = iota


//定义单位数量级
//其中<<表示左移操作符号，1<<10表示1的二进制左移10位，变成10000000000，也就是十进制的1024
//2<<2：表示2的二进制左移2位，10变成1000，十进制的8

const (
	_  = iota  //出现const，iota置为0
	KB = 1 << (10 * iota) //iota=1，1<<10
	MB = 1 << (10 * iota) //iota=2，1<<20
	GB = 1 << (10 * iota)
	TB = 1 << (10 * iota)
	PB = 1 << (10 * iota)
)

const (
	a, b = iota + 1, iota + 2  //0+1,0+2
	c, d  //等价于c, d = iota + 1, iota + 2，结果为1+1,1+2
	e, f  //2+1, 2+2
)


func main(){
	fmt.Println(n1, n2, n3, n4, n6, n8, n9)
	fmt.Println(a,b,c,d,e,f)
}

//result
D:\go\src\code\iota>go build
D:\go\src\code\iota>iota.exe
0 1 2 3 5 28 0
1 2 2 3 3 4
```

