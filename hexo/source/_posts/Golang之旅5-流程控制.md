---
title: Golang之旅5-流程控制
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-17 00:33:35
password:
summary:
copyright: true
tags:
  - go
  - 流程控制
  - 高并发
categories:
  - go
---



### Go语言的流程控制

`Go`语言中的流程控制主要有`if`和`for`，还有简化代码和降低重复性的`switch`和`goto`。

### if

```go
package main

import (
	"fmt"
)

func main(){
	// 基本写法
	var score = 65
	if score >= 90{   //左花括号和if必须在一行中
		fmt.Println("A")
	} else if score > 75{  //左花括号必须和else放在一行代码中
		fmt.Println("B")
	} else {
		fmt.Println("C")
	}

	//2.特殊写法
	if score := 65;  score >= 90{   //score变量只在if语句内部生效
		fmt.Println("A")
	} else if score > 75{
		fmt.Println("B")
	} else {
		fmt.Println("C")
	}
}
```

<!-- MORE -->

### for

`Go `语言中的所有循环类型均可以使用`for`关键字来完成。`Go`语言中没有`while`

- 基本格式
- 特殊格式
- `break`和`continue`

```
for 初始语句;条件表达式;结束语句{
    循环体语句
}
```

```go
package main

import "fmt"

//for循环基本写法
func main(){
	//基本写法
	for i := 0; i < 10; i++{
		fmt.Println(i)
	}
	//for循环省略初始语句，但是必须保留分号
	var j = 0
	for ; j < 10; j++{
		fmt.Println(j)
	}

	//省略初始语句和结束语句
	var k = 10
	for k > 0{
		fmt.Println(k)
		k--
	}

	//无限循环
	//for {
	//	fmt.Println("hello Go!")
	//}

	//break跳出循环
	for i := 0; i < 5; i++{
		fmt.Println(i)
		if i == 3{   //i等于3，跳出循环体，结束for循环
			break
		}
	}
	//continue跳出循环
	for i := 0; i < 5; i++{
		if i == 3{
			continue  //跳过本次for循环，不执行打印语句，直接继续下次循环
		}
		fmt.Println(i)
	}
}
```

#### 转二进制

```go
// 将数字转成二进制
// 13/2 = 6...1(第四位)；6/2=3...0(第三位)；
// 3/2=1...1(第二位)；1/2=0...1(第一位)；直到商为0结束
func convertToBin(n int) string{
	result := ""
	for ; n > 0;n /= 2{
		lsb := n % 2   // 取模
		result = strconv.Itoa(lsb) + result   // strconv.Itoa 转字符串
	}
	return result
}
```

#### 读取文件

```go
//读取文件
func printFile(filename string){
	file,err := os.Open(filename)
	if err != nil{
		panic(err)
	}

	scanner := bufio.NewScanner(file)  // 创建读取文件实例

	// 省略初始条件
	for scanner.Scan(){
		fmt.Println(scanner.Text())
	}
}

// 死循环
func forever(){
	for {
		fmt.Println("ABC")
	}
}
```

#### 高斯求和

```go
sum := 0
for i:= 1; i<=100; i++{
    sum += i
}

```



### switch case

Go语言规定每个`switch`只能有一个`default`分支。

```go
package main

import "fmt"

func main(){
    //基本写法
	finger := 3
	switch finger {
	case 1:
		fmt.Println("大拇指")
	case 2:
		fmt.Println("食指")
	case 3:
		fmt.Println("中指")
	case 4:
		fmt.Println("无名指")
	case 5:
		fmt.Println("小指")
	default:
		fmt.Println("无效输入")   //只能有一个default分支
	}
    
    //case后面跟多个值
	number := 5
	switch  number {
	case 1,3,5,7,9:  // 将number和case后面的值依次进行比较
		fmt.Println("奇数")
	case 2,4,6,8,10:
		fmt.Println("偶数")
	default:
		fmt.Println("无效输入")
    }
    
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



###  99乘法表

```go
package main

import "fmt"

func main(){
	for i := 1; i < 10; i++{
		for j := 1; j < i+1; j++{
			fmt.Printf("%v*%v=%v\t",j,i,i*j)  //j在前面，每次j都是从1开始
		}
		fmt.Println()  // 每次执行一个循环进行一次换行
	}
}
```

