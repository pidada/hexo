---
title: Golang之旅9-函数
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-18 09:42:14
password:
summary:
copyright: true
tags: 
  - go
  - google
categories:
  - go
---

### 函数定义

Go语言中支持函数、匿名函数和闭包，通过func关键字进行定义。

```go
func 函数名(参数)(返回值){
    函数体
}

//简单的求和函数
func intSum(x int, y int) int {
	return x + y
}

func div(a,b int) (q,r int){
    q = a / b
    r = a % b
    return  // 默认是返回q, r
    // return q, r
}
```

- 函数名：由`字母、数字、下划线`组成。但函数名的第一个字母不能是数字。在同一个包内，函数名也称不能重名
- 参数：参数由`参数变量`和`参数变量的类型`组成，多个参数之间使用`,`分隔。
- 返回值：返回值由返回值变量和其变量类型组成，也可以只写返回值的类型，多个返回值必须用`()`包裹，并用`,`分隔。
- 函数体：实现指定功能的代码块。

·

### 返回值return

#### 仅指定类型

- 仅指定返回值类型，函数体内必须声明和初始化才能使用
- return语句中必须带上返回值名称

![](https://s2.ax1x.com/2019/09/18/nTudbt.png)

#### 指定类型和名称

- 类型和名称用()括起来
- 函数体内直接使用，不必初始化
- return语句中不必带上返回值，系统自动查找

![](https://s2.ax1x.com/2019/09/18/nTKpGD.png)

### 可变参数

- 可变参数通过`...`来实现

- 可变参数必须在固定参数的后面

- 可变参数可以用来进行遍历`for range`

  ```go
  func intSum4(a int, b...int) int {  //b是int类型的可变参数
  	ret := a
  	for _, arg := range b{
  		ret += arg
  	}
  	return ret
  }
  ```

  ```go
  func sum(values ...int) int{
      sum := 0
      for i:= range values{
          sum += values[i]   // 将values中每个元素进行遍历，再相加
      }
      return sum
  }
  ```
  
  
  
  

```go
package main

import "fmt"

//函数：没有默认参数
//没有参数和返回值
func sayHello(){
	fmt.Println("hello Peter")
}

//带上参数的函数
func sayHello2(name string){
	fmt.Println("hello", name)
}

//定义参数和返回值的函数
//参数类型简写：func intSum (a, b int) int{ 
func intSum (a int, b int) int{
	ret := a + b   //声明并且赋值
	return ret   //返回ret
}

//效果同上：指定了返回值的名称和类型，则函数体内不需要进行
func intSum2 (a int, b int) (ret int){
	ret = a + b   //直接使用
	return  //不必写返回，系统自动找到返回值
}

//接收可变参数，参数名后面...表示是可变参数
//可变参数在函数体内是切片类型
func intSum3(a ...int) int {
	//fmt.Printf("%T\n", a)  //查看a的类型
	ret := 0   //类似Python中的sum=0
	for _, arg := range a{
		ret += arg
	}
	return ret
}

//同时接收固定和可变参数，可变参数必须在固定参数后面
func intSum4(a int, b...int) int {
	//fmt.Printf("%T\n", a)  //查看a的类型
	ret := a
	for _, arg := range b{
		ret += arg
	}
	return ret
}


func main(){
	//函数调用
	sayHello()

	n := "小明"
	sayHello2(n)

	r := intSum(20,10)
	fmt.Println(r)

	m := intSum2(20,10)
	fmt.Println(m)

	//根据传入的参数进行计算
	r1 := intSum3()
	r2 := intSum3(10)
	r3 := intSum3(10,20)
	r4 := intSum3(10,20,30)
	fmt.Println(r1)
	fmt.Println(r2)
	fmt.Println(r3)
	fmt.Println(r4)

	//固定+可变参数
	r5 := intSum4(0)  //a=0,b=[]
	r6 := intSum4(10,20) //a=10,b=20
	r7 := intSum4(10,20,30)  //a=10,b=[20,30]
	fmt.Println(r5)
	fmt.Println(r6)
	fmt.Println(r7)
}
```



### 多个返回值

- 允许多个返回值
- 如果不想使用某个返回值，就用下划线`_`代替；若用于返回错误

```go
package main

import "fmt"

//多返回值支持类型简写
func calc(a, b int)(sum, sub int){    //同时定义参数和返回值类型
	sum = a + b
	sub = a - b
	return  // 自动返回的是sum和sub
	//return sum, sub
}

func main(){
	x, y := calc(20,10)  //进行返回值的声明和初始化
	fmt.Println(x)
	fmt.Println(y)
}
```



### defer语句

#### 特点

- 将`defer`语句进行延迟处理
- 场景：处理资源释放问题，资源清理、文件关闭、解锁及记录时间
- 处理顺序是`倒序`，类似`栈`结构，先进后出`FILO`
- 仅仅是`defer`语句的倒序输出，其他语句正常顺序

```go
package main

import "fmt"

func intSum(a, b int)(ret int){
	ret = a + b
	return
}

//defer语句的打印输出类似于栈的先进后出FILO
func main(){
	fmt.Println("start")
	defer fmt.Println("1")
	fmt.Println("middle")
	defer fmt.Println("2")
	x := intSum(10,20)
	fmt.Println(x)
	defer fmt.Println("3")
	fmt.Println("end")
}

//result
start
middle
30
end
3
2
1
```

#### 执行时机

在`Go`语言的函数中`return`语句在底层并不是原子操作，它分为给返回值赋值和`RET`指令两步。而`defer`语句执行的时机就在返回值赋值操作后，`RET`指令执行前

![](https://www.liwenzhou.com/images/Go/func/defer.png)