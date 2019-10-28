---
title: Golang之旅14-struct2
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-20 10:16:10
password:
summary:
copyright: true
tags:
  - go
  - 结构体
  - 指针
categories:
  - go
---



### 指针类型的结构体

通过`new`关键字进行结构体的实例化，得到是结构体的地址。指向结构体指针的类似于其他指针变量，格式

``` go
var struct_pointer *Books   //定义指针变量，前面加上*号
```

定义解释：

- 指针变量存储结构体变量的地址
- 查看结构体变量地址，可以将`&`符号置于结构体变量前面
- 访问结构体中的成员通过`.`的方式

<!--MORE-->

``` go
struct_pointer = &Books
```

```go
package main

//指针类型结构体

import "fmt"

type person struct {
	name, city string
	age int
}

func main(){
	var p2 = new(person)   //通过new创建指针类型的结构体
	fmt.Printf("%T\n", p2)
	//(*p2).name = "小明"
	//(*p2).city = "深圳"
	//(*p2).age = 20

	p2.city = "深圳"
	p2.name = "小明"   //(*p2).name = "小明"，二者等价
	p2.age = 20
	fmt.Printf("%#v\n", p2)

	//取结构体的地址进行实例化
	p3 := &person{}   //实例化指针类型的person结构体
	fmt.Printf("%T\n", p3)
	fmt.Printf("%#v\n", p3)
	p3.name = "小红"
	p3.city = "北京"
	p3.age = 18
	fmt.Printf("%#v\n", p3)
}

```



### 构造函数

> 构造一个结构体实例的函数，构造函数通常在前面加上new

```go
package main

import "fmt"

//构造函数：构造一个结构体实例的函数
//结构体是值类型
type person struct {
	name, city string
	age        int8
}

//构造函数：通常是new开头
//如果字段多，返回结构体的体积大，开销大，采用返回的是结构体指针
func newPerson(name,city string, age int8) *person{   //*person返回结构体指针
	return &person{  //
        name: name,  //初始化的字段名: 传入的参数
		city: city,
		age:  age,
	}
}

func main(){
    //调用构造函数
	p1 := newPerson("小王子", "北京", int8(18))
	fmt.Printf("type:%T  value:%#v\n",p1,p1)
}
```



![](https://s2.ax1x.com/2019/09/20/njk1W8.png)