---
title: Golang之旅26_switch
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-07 19:33:35
password:
summary:
copyright: true
tags:
  - go
  - switch
  - 流程控制
categories:
  - go
---

`switch`语句的特点：

- 拥有多个分支
- switch后面可以不跟语句，在case的每个分支中直接添加
- 需要有`default`语句

<!--MORE-->

```go
package main

import (
	"fmt"
)

func eval(a, b int, op string) int{
	var result int
	switch op {
	case "+":
		result = a + b
	case "-":
		result = a - b
	case "*":
		result = a * b
	case "/":
		result = a / b
	default:
		panic("unsupport operator:" + op)  // panic 自动报错机制
	}
	return result
}


func grade(score int ) string{
	g := ""
	switch {   // switch 后面可以不跟表达式，直接在case中添加
	case score < 0 || score > 100 :
		panic(fmt.Sprintf("Wrong score: %d",score))
	case score < 60:
		g = "D"
	case score < 80:
		g = "C"
	case score < 90:
		g = "B"
	case score < 100:
		g = "A"
	// default:
	// panic(fmt.Sprintf("Wrong score: %d",score))
	}
	return g

}

func main(){
	//猜手指
	//finger := 3
	//switch finger {
	//case 1:
	//	fmt.Println("大拇指")
	//case 2:
	//	fmt.Println("食指")
	//case 3:
	//	fmt.Println("中指")
	//case 4:
	//	fmt.Println("无名指")
	//case 5:
	//	fmt.Println("小指")
	//default:
	//	fmt.Println("无效输入")

	////分支后面跟多个值
	//number := 5
	//switch  number {
	//case 1,3,5,7,9:
	//	fmt.Println("奇数")
	//case 2,4,6,8,10:
	//	fmt.Println("偶数")
	//default:
	//	fmt.Println("无效输入")
	//}

	result := eval(3, 5 ,"+")
	fmt.Println(result)

	fmt.Println(grade(87))

	//分支后面跟表达式
	number := 5
	switch   {
	case number % 2 == 0 :
		fmt.Println("hello python")
	case number % 2 == 1:
		fmt.Println("hello Go")
	default:
		fmt.Println("无效输入")
	}
}
```

