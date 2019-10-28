---
title: Golang之旅12-指针
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-19 10:47:51
password:
summary:
copyright: true
tags: 
  - 指针
  - go
categories:
  - go
---



### 指针

变量是一种使用方便的占位符，用于引用计算机内存地址。`Go` 语言的取地址符是 &，放到一个变量前使用就会返回相应变量的内存地址。另外一个符号是`*`，表示的是根据地址取值。`Go`语言中指针不能进行偏移和计算，是安全的指针。 指针是引用类型，初始化之后才能够使用。

- &：取地址
- *：根据地址取值

### 指针地址和指针类型

每个变量在运行时都拥有一个地址，这个地址代表变量在内存中的位置。`Go`语言中使用`&`字符放在变量前面对变量进行“取地址”操作。 

<!-- MORE -->

`Go`语言中的值类型`int、float、bool、string、array、struct`都有对应的指针类型，如：`*int`、`*int64`、`*string`等。取变量指针：

```go
func main() {
	a := 10
	b := &a    //b存放的是变量a的指针地址，同时内存也给b开辟了新的内存地址，用于存放变量b的指针地址
	fmt.Printf("a:%d ptr:%p\n", a, &a) // a:10 ptr:0xc00001a078
	fmt.Printf("b:%p type:%T\n", b, b) // b:0xc00001a078 type:*int；变量b是指针类型，存放的是a的指针地址
	fmt.Println(&b)                    // 0xc00000e018
}
```

### 如何使用指针

- 定义指针变量
- 为指针变量赋值
- 访问指针变量中指向地址的值
- &和*实际上是一对取反操作：`值------->&---->指针地址------->*----->值`

```go
package main

import "fmt"

func main(){
	var a int = 10   //声明实际变量
	var ip *int   //声明指针变量

	ip = &a  //取出a的指针

	//取指针地址，%x表示转义
	fmt.Printf("变量地址：%x\n", &a)
	fmt.Printf("变量地址：%x\n", ip)

	//访问指针的值
	fmt.Printf("*ip变量的值：%d\n", *ip)   //*ip根据指针来取值
}
```

### 指针取值

```go
package main

import "fmt"

//指针传值
func modify1(x int){   //定义变量
	x = 100
}

func modify2(x *int) {  //定义指针类型变量，通过*T实现
	*x = 100
}


func main(){
	a := 10

	modify1(a)
	fmt.Println(a)   //10
    modify2(&a)   //modify2参数类型是指针类型，所以需要先对a取指针：&a
	fmt.Println(a)   //100
}
```



### new和make

- 引用类型的变量，在使用的时候不仅要进行声明，还要分配内存空间。

- 值类型的声明不需要分配内存空间，声明的时候默认分配了内存空间。
- new和make是内建函数，主要是用来分配内存

#### new

```
func new(Type) *Type
```

- Type表示类型，new函数只接收一个参数
- *Type表示指针类型，new函数返回一个值指向该类型地址的指针
- new函数返回指针，并且该指针对应的值为该类型的零值

```go
func main(){
    a := new(int)
    b := new(bool)
    fmt.Printf("%T\n", a)  //int
    fmt.Printf("%T\n", b)  //bool
    fmt.Println(*a)   //0  该类型的零值
    fmt.Println(*b)  //false
}
```



#### make

- 用于内存分配

- 只用于`slice`、`map`和`chanel`的内存创建，常用于初始化

- 返回的类型是三种类型本身，而不是他们的指针类型

  ```go
  funmake(t Type, size ...InterType) Type
  ```

  

```go
package main

import "fmt"

func main(){
	var b map[string]int   //map类型没有通过make初始化
	b = make(map[string]int, 10)   // 初始化
	b["沙河娜扎"] = 100
	fmt.Println(b)
}
```



#### new和make区别

- 二者都是用于分配内存的
- make只用于slice、map和chanel的初始化，返回的是引用类型本身
- new用于类型的内存分配，并且内存对应的值为类型零值，返回的是指向类型的指针



### 指向指针的指针

若一个`指针变量`指向的是另一个`指针变量的地址`，称这个指针变量为指向指针的`指针变量`。`一个指针变量------>另一个指针变量的地址`

![](https://www.runoob.com/wp-content/uploads/2015/06/pointer_to_pointer.jpg)

访问指向指针的指针变量需要使用两个`**`。

```
var ptr **int
```

#### 栗子

```go
package main

import "fmt"

func main(){
	//var a int = 20   //变量定义

	var a int = 20   //定义变量a
	var ptr *int     //定义指针变量ptr
	var pptr **int   //定义指向指针的指针变量

	a = 20

	ptr = &a   //取出a的指针地址
	pptr = &ptr  //取出指针的指针地址

	fmt.Printf("变量 a= %d\n", a)
	fmt.Printf("指针变量 *ptr = %d\n", *ptr)   //*是&的取反操作，得到指针的值
	fmt.Printf("指向指针的指针变量 **pptr = %d\n", **pptr)   //得到指向指针地址的指针的值
}
```