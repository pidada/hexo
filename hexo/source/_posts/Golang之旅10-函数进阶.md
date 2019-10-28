---
title: Golang之旅—10-函数进阶
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-18 23:17:28
password:
summary:
copyright: true
tags: 
  - go
  - 函数变量
  - 作用域
categories:
  - go
---

### 作用域

- 在函数体中可以使用自己的变量
- 全局变量是定义在函数外部的变量，它在程序整个运行周期内都有效。 在函数中可以访问到全局变量。
- 函数在执行的过程中，先在自己内部进行查找，找到了先进行使用
- 函数中没有变量（局部变量），再找外层的变量（全局变量）
- 如果局部变量和全局变量重名，优先访问局部变量。
- `for`语句块中声明的变量，只在`for`语句块中生效，外层无法访问
- 函数能够作为变量，进行赋值

<!-- MORE -->

```go
package main

import "fmt"

//全局变量
var number = 20

//定义函数
func testGlobal() {
	number := 10
	// 可以在函数中使用变量
	// 先在自己函数中进行查找，找到了自己的函数中的变量
	// 函数中没有找到就往外层找，即找全局变量
	fmt.Println("变量number", number)   //变量number 10

	//变量i只在for语句块中生效
	for i := 0; i < 10; i++{
		fmt.Println(i)
	}
	//fmt.Println(i)    外层不能访问内部for语句中的变量
}

func main(){
	//函数能够作为变量
	a := testGlobal
	fmt.Printf("%T\n", a)    //a是func类型
	a()  //直接调用a
}
```



### 函数作为参数

```go
package main

import "fmt"

func add(x, y int) int{
	return x + y
}

func sub(x, y int) int{
	return x - y
}

//定义3个变量：a,b,op
//a,b：是int类型
//op：是func类型，func接收两个int类型，返回值也是int类型
//calc函数的返回值也是int类型
func calc(x, y int, op func(int, int) int) int{   
	return op(x, y)
}

func main(){
    //函数作为参数传递给另一个函数
	r1 := calc(10,20, add)  //30
	fmt.Println(r1)
	r2 := calc(20,10, sub)  //10
	fmt.Println(r2)
}
```

