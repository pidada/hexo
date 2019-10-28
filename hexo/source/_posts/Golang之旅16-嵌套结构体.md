---
title: Golang之旅16-嵌套结构体
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-21 14:20:09
password:
summary:
copyright: true
tags:
  - go
  - 嵌套
  - 后端开发
categories: 
  - go
---

### 内容

- 嵌套结构体
- 不同的结构体之间的命名冲突
- 结构体的继承实现

### 嵌套结构体

> 一个结构体中可以嵌套包含另一个结构体或结构体指针

<!-- MORE -->

```go
package main

import "fmt"

type Address struct {
	Provice string  //字段名称  字段类型
	City string
}

type User struct {
	Name string
	Gender string
	Address Address   //类型是上面的Address类型
}

// 结构体的匿名字段
type Person struct{
    string 
    int
}

func main(){
	user1 := User{   //先定义User类型
		Name:    "皮皮虾",  
		Gender:  "name",
		Address: Address{   //再定义Address类型
			Provice: "广东",
			City: "深圳",
		},
	}
	fmt.Printf("user=%#v\n", user1)
}
```



### 字段名冲突

```go
package main

import "fmt"

//字段名冲突问题

type Address struct {
	Province string
	City string
	CreateTime string
}

type Email struct{
	Account string
	CreateTime string
}


type Person struct {
	Name string
	Gender string
	Age int
	Address
	Email
}

func main(){
	p1 := Person{
		Name: "小明",
		Gender: "男",
		Age: 18,
		Address: Address{
			Province: "广东",
			City: "深圳",
			CreateTime: "2019-09-21",
		},
		Email: Email{
			Account:    "pipixia",
			CreateTime: "2018-08-09",
		},
	}
	fmt.Printf("%#v\n", p1)
	fmt.Println(p1.Address.CreateTime)   // 冲突的字段名，必须指明全部的类型名称进行访问
	fmt.Println(p1.Email.CreateTime)
}
```





### 结构体的继承

使用结构体能够实现继承功能

```go
package main

import "fmt"

//结构体的继承

// Animal 动物：自定义一个类
type Animal struct{
	name string
}

// 定义一个方法move 传给Animal
// 接收者类型为*Animal，a是接收者变量
func (a *Animal) move(){
	fmt.Printf("%s can move\n", a.name)
}

// cat 类型
type Cat struct{
	Feet int8
	*Animal   // 通过嵌套匿名结构体实现继承，嵌套的是指针
}

func (d *Cat) miao() {
	fmt.Printf("%s can miao miao\n", d.name)
}

func main(){
	d1 := &Cat{  // 不要忘记取址符号
		Feet: 4,
		Animal: &Animal{
			name: "尤尔",
		},
	}
	d1.miao()
	d1.move()
}
```

