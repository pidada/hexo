---
title: Golang之旅11-匿名函数及闭包
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-18 23:55:30
password:
copyright: true
summary:
tags: 
  - go
  - 匿名函数
  - 闭包
categories:
  - go
---

### 匿名函数

匿名函数就是没有函数名的函数，匿名函数的定义格式如下：

```go
func(参数)(返回值){
    函数体
}
```

- 没有函数名，无法进行调用
- 匿名函数需要保存到某个函数或者立即执行，即：函数体后面直接加上`()`，如果有参数带上参数
- 多用于实现回调函数和闭包

<!-- MORE -->

```go
func main() {
	// 将匿名函数保存到变量
	add := func(x, y int) {
		fmt.Println(x + y)
	}
	add(10, 20) // 通过变量调用匿名函数

	//自执行函数：匿名函数定义完加()直接执行
	func(x, y int) {
		fmt.Println(x + y)
	}(10, 20)
}
```

### 闭包

闭包指的是一个函数和与其相关的引用环境组合而成的实体。简单来说，`闭包=函数+外层变量的引用环境`

```go
package main

import "fmt"

func a() func(){  // 定义a函数，返回值的类型是func
	name := "GO语言"
	return func(){  //返回的是匿名函数
		fmt.Println("hello", name)  //先在匿名函数中找name，没有再到匿名函数外部查找
	}
}

func main(){
	r := a()   //r就是闭包
	r()   //相当于是执行了a函数里面的匿名函数
}
```



```go
package main

import "fmt"

func a(name string) func(){  // 定义a函数，返回值的类型是func
	return func(){  //返回的是匿名函数
		fmt.Println("hello", name)  //先在匿名函数中找name，没有再到匿名函数外部查找
	}
}

func main(){
	r := a("Go语言")   //将函数赋值给变量r
	r()   //相当于是执行了a函数里面的匿名函数
}
```



**文件名判断**

```go
package main

import (
	"fmt"
	"strings"
)

//makeSuffixFunc函数：参数是suffix，string类型；返回值是func;
//func的参数是string，返回值还是string
//第一个return的返回值是func

func makeSuffixFunc(suffix string) func(string) string {
	return func(name string) string {
		if !strings.HasSuffix(name, suffix) {   //判断name的后缀是否是suffix
			return name + suffix  //不是则进行拼接
		}
		return name
	}
}

func main() {
	jpgFunc := makeSuffixFunc(".jpg")   //makeSuffixFunc函数传入string类型的参数
	txtFunc := makeSuffixFunc(".txt")
	fmt.Println(jpgFunc("test")) //test.jpg
	fmt.Println(txtFunc("test")) //test.txt
}
```



```go
package main

import "fmt"

func calc(base int) (func(int) int, func(int) int) {   //定义calc函数，返回值是两个函数
	add := func(i int) int {   //返回其中一个函数，赋值给add变量
		base += i   //匿名函数有base变量，是外层函数中，闭包
		return base
	}

	sub := func(i int) int {
		base -= i
		return base
	}
	return add, sub
}

func main() {
	f1, f2 := calc(10)  //两个变量分别接受两个返回值  base=10
    //add函数中返回的base值会带入sub函数的计算中
	fmt.Println(f1(1), f2(2)) //i=1；11 9
	fmt.Println(f1(3), f2(4)) //12 8
	fmt.Println(f1(5), f2(6)) //13 7
```

### 内置函数

| 内置函数       | 介绍                                                         |
| :------------- | :----------------------------------------------------------- |
| close          | 主要用来关闭`channel`                                        |
| len            | 用来求长度，比如`string、array、slice、map、channel`         |
| new            | 用来分配内存，主要用来分配值类型，比如`int`、`struct`。返回的是指针 |
| make           | 用来分配内存，主要用来分配引用类型，比如`chan、map、slice`   |
| append         | 用来追加元素到数组、`slice`中                                |
| panic和recover | 用来做错误处理                                               |



**发生panic通过recover来处理**

```go
package main

import "fmt"

func a(){   //定义匿名函数
	fmt.Println("func a")
}

func b(){
	//recover一定要在panic之前，defer和recover联合使用
	defer func(){
		//如果发生了panic错误，通过recover()来恢复
		err := recover()
		if err != nil{   //如果err不是空的，说明err发生错误
			fmt.Println("func b error")
		}
	}()   //()表示对匿名函数的调用
	panic("panic in b")    //panic函数表示错误
}

func c(){
	fmt.Println("func c")
}

func main(){
	a()
	b()
	c()
}
```

- `defer`语句一定要在`panic`之前
- `panic`和`recover`要联合使用

### 练习题

```go
package main

import (
	"fmt"
)

/*
你有50枚金币，需要分配给以下几个人：Matthew,Sarah,Augustus,Heidi,Emilie,Peter,Giana,Adriano,Aaron,Elizabeth。
分配规则如下：
a. 名字中每包含1个'e'或'E'分1枚金币
b. 名字中每包含1个'i'或'I'分2枚金币
c. 名字中每包含1个'o'或'O'分3枚金币
d: 名字中每包含1个'u'或'U'分4枚金币
写一个程序，计算每个用户分到多少金币，以及最后剩余多少金币？程序结构如下，请实现 ‘dispatchCoin’ 函数
*/

var (
	coins = 50
	users = []string{
		"Matthew", "Sarah", "Augustus", "Heidi", "Emilie",
		"Peter", "Giana", "Adriano", "Aaron", "Elizabeth",
	}
	distribution = make(map[string]int, len(users))   //定义空map，用于存储数据
)


func dispatchCoin() int{   
	for _, v := range users {
		// distribution中的每个键的值初始化为0，用于将来按照要求进行相应的增加操作
		distribution[v] = 0
		//对于单个的users遍历
		for i := 0; i < len(v); i++ {
			//将每个user的字母转成字符类型string，并赋值给word
			word := string(v[i])
			if word == "e" || word == "E" {
				distribution[v] += 1
			}
			if word == "i" || word == "I" {
				distribution[v] += 2
			}
			if word == "o" || word == "O" {
				distribution[v] += 3
			}
			if word == "u" || word == "U" {
				distribution[v] += 4
			}
		}
	}
	sum := 0
	//distribution是map类型，存储的是键值对形式，key是单个user，value是上面的number
	for _, v := range distribution {
		sum += v
	}
	left := coins - sum
	return left
}

func main() {
	left := dispatchCoin()
	fmt.Println("剩下：", left)
}

```



- 先遍历`users`数组，取出其中的每个`user`
- 通过`for range`遍历出`user`中的每个字母，通过`string`函数转换成字符类型，再和字母进行比较
- `map`类型的`distribution`初始化为0；比较完毕，对`distribution`中的每个`value`值进行增加操作
- 得到`distribution`：键为单个`user`，值为增加的数值
- 通过`for range` 循环遍历`distribution`中的每个`value`，所有的`value`相加赋值给`sum`
- 得到`left = coins - sum`
  