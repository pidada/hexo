---
title: Golang之旅6-数组
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-17 10:11:33
password:
summary:
copyright: true
tags:
  - go
  - 数组
  - 高并发
categories:
  - go
---

### 数组

数组是一种数据类型的集合，`数组类型包含：数组元素和数组长度`。`Go`语言中数组的特点

- 从声明数组的时候确定数组类型

- 使用的时候可以修改数组的一部分

- 数组一旦被定义，长度不能改变

- 数组可以通过下标进行访问，下标从`0`开始，最后一个是`len-1`

- 若访问越界，触发`panic`

- 关于如何查看类型`type`

  ```go
  fmt.Printf("type of data:%T\n", data)
  ```

  

```go
var a [3]int  //定义一个长度为3，类型为int的数组

var a [5]int 和 var a [10]int  //二者是不同的数组，因为长度不同
```

### 输出初始化

- 定义时候使用初始值列表的方式进行初始化
- [...]编译器自动推导数组的长度
- 通过索引值的方式来初始化

```go
package main

import "fmt"

//数组相关内容
func main(){
	var a [3]int   //int默认是0值
	fmt.Println(a)

	//数组初始化
	//var testArray = [3]int{0,0,0}  带上等号后面需要显式地将数组的0元素写出来
	var testArray = [3]int{}
	var numArray = [3]int{1, 2}
	var cityArray = [3]string{"深圳", "长沙", "广州"}  //使用指定的初始值来完成初始化工作
	fmt.Println(testArray)
	fmt.Println(numArray)
	fmt.Println(cityArray)

	//不带上数组的长度，编译器自动判断数组长度
	var numArray1 = [...]int{1, 2}
	var cityArray1 = [...]string{"北京", "上海", "深圳"}
	fmt.Println(numArray1)   //[1 2]，直接打印元素
	fmt.Printf("type of numArray1:%T\n", numArray1)   //type of numArray:[2]int；%T：表示打印元素类型
	fmt.Println(cityArray1)  //[北京 上海 深圳]
	fmt.Printf("type of cityArray1:%T\n", cityArray1) //type of cityArray:[3]string

	//通过指定索引的值来初始化数组
	//指定索引为1和3的元素值
	b := [...]int{1: 1, 3: 5}
	fmt.Println(b)
	fmt.Printf("type of b:%T\n", b)

	var langArray = [...]string{1:"golang", 3:"python", 5:"java", 7:"c"}
	fmt.Println(langArray)
	fmt.Printf("type of langArray：%T\n", langArray)
}
```



### 遍历数组

两种遍历方式

- `for`＋`下标`进行遍历
- `for range`进行遍历

```go
package main

import "fmt"

//数组遍历
func main(){
	//for 下标进行遍历
	for i :=0; i < len(langArray); i++{
		fmt.Println(langArray[i])
	}

	//for range进行遍历
	for index, value := range langArray{
		fmt.Println(index, value)
	}
    
    //for _, value := range langArray{   //通过下划线只取到value值，而不要索引
	//	fmt.Println(value)
	//}
}
```



### 二维数组

- 创建的时候只有最外层能够使用...
- 创建的内部元素之间用`{}`分开
- 访问所有元素：通过多层`for range`实现

```go
package main

import (
	"fmt"
)

//二维数组
func main() {
	//cityArray := [...][2]string{   //只有外层可以使用...
	cityArray := [4][2]string{   //总长度是4，里面的长度是2
		{"北京","西安"},
		{"上海","杭州"},
		{"重庆","成都"},
		{"深圳","广州"},
	}
	fmt.Println(cityArray)
	fmt.Println(cityArray[2][0])  //访问某个元素

	//通过for range来遍历所有的元素
	for _, value1 := range cityArray{
		for _, value2 := range value1{
			fmt.Println(value2)
		}
	}
}
```



###   数组是值类型

数组是值类型，赋值和传参会复制整个数组。因此改变副本的值，不会改变本身的值。

```go
package main

import "fmt"

func main(){
	x := [3][2]int{    //如何创建二维数组
		{1,2},
		{3,4},
		{5,6},
	}
	fmt.Println(x)
	f1(x)  //调用f1函数，比较前后x：无变化
	fmt.Println(x)
	y := x
	y[0][0] = 1000
	fmt.Println(x)
}

func f1(a [3][2]int){   //接收长度为3的int类型的数组
	a[0][0] = 100
}
```



### 练习题

```go
package  main

import "fmt"

//求两数之和
func main(){
	fmt.Println("hello world")
	var numArray = [5]int{1, 3, 5, 7, 8}
	for i := 0; i < len(numArray); i++{
		for j := i+1; j <len(numArray);  j++{    //j的起始值为i+1，避免重复
			if numArray[i] + numArray[j] == 8{
				fmt.Println(i,j)
			}
		}
	}
}
```

