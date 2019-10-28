---
title: Go学习之旅1-变量定义
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-24 08:16:57
password:
summary:
copyright: true
tags:
  - go
  - 变量
  - 常量
  - iota
categories:
  - go
---

### `Go`语言中变量定义

#### `var`关键字定义

- 初始值默认是类型的初始值

- 同时定义不同类型的变量，集中定义

- `var`声明的变量可以放在函数体内或者包内

- `var`关键字可以省略

  

#### 短变量定义

- 短变量声明是通过`:=`实现的，只能放在函数体内

- 短变量声明和`var`关键字不能同时使用

<!-- MORE -->

```go
package main

import "fmt"

// 变量定义

// 使用var定义：同时定义多个不同的变量；可以放在函数体内或者包内；
// 通过var集中定义(放在括号内)；var有时候能够省略，编译器自动识别类型；
// 使用短变量声明 :=  只能使用在函数体内部；短变量声明和var不能同时使用

//var aa = 3
//var bb = "kkk"
//var ss = true

var (
	aa = 3
	bb = "kkk"
	ss = true
)

func variableZeroValue(){
    //1. 默认初值
	var a int   //0  初始值默认是该类型的零值
	var s string  // ""  初始值默认是空字符串
	fmt.Printf("%d %q\n", a, s)
}

func variableInitialValue(){
    //2. 赋初始值
	//var a, b  int = 3, 4
	//var s string = "abc"
	var (  
		a = 3  //直接赋值，不写变量类型，编译器自动识别
		b = 4
		s = "abc"
	fmt.Println(a, b, s)
}

func variableTypeDeduction(){
    //3. 一行同时赋多个初始值
	var a,b,c,s = 3,4,true,"def"
	fmt.Println(a,b,c,s)
}

func variableShorter(){
    // 4.短变量声明
	a,b,c,s := 3,4,true,"def"
	b = 5    // 变量的作用域只在函数体内
	fmt.Println(a,b,c,s)
}
        
// 常量的定义
// 常量的关键字是const
func consts(){
	const (
		filename = "abc.txt"
		a, b = 3, 4  //  系统会自动当做float类型计算
	)
	var c int
	c = int(math.Sqrt(a*a + b*b))
	fmt.Println(filename, c)
}

//枚举类型
// const第一次遇见iota的时候，iota是0，从此递增加1
func enums()  {
	const(
		b = 1 << (10 *iota)
		kb
		mb
		gb
		tb
		pb
		)
	fmt.Println(b,kb,mb,gb,tb,pb)
}

func main(){
	fmt.Println("hello world")
	variableZeroValue()
	variableInitialValue()
	variableTypeDeduction()
	variableShorter()
	fmt.Println(aa,bb,ss)
    consts()
    enums()
}
```

