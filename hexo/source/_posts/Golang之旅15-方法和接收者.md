---
title: Golang之旅15-方法和接收者
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-21 09:35:30
password:
summary:
copyright: true
tags: 
  - go
  - method
  - receiver
categories:
  - go
---

### 方法和接收者

方法`Method`是种作用域特殊类型变量的函数，特定类型的变量称为接收者`receiver`。接收者类似Python中的`self`。方法的定义格式

```go
func (接收者变量 接收者类型) 方法名(参数列表) (返回参数) {
    函数体
}
```

- 接受者变量：接收者参数变量命令，直接采用接收者类型名的第一个小写字母，`Person`类型的`p`，`Student`类型的`s`

- 接收者类型：接收者类型和参数类似，可以是指针类型和非指针类型
- 接收者的类型可以是任何类型，不仅是结构体，任何类型都可以拥有方法。
- 其他格式和普通函数相同



### 指针类型接收者

- 由一个结构体指针组成

- 调用方法时修改接收者指针的任意成员变量，在方法结束后，修改都是有效的。



<!-- MORE -->

- 什么时候使用指针接收者:question:
  - 需要修改接收者中的值
  - 接收者是拷贝代价比较大的对象
  - 保证一致性，若某个方法使用了指针接收者，其他也要使用

```go
package main

import (
	"fmt"
)

//结构体
type Person struct {
	name string
	age int8
}

// 构造函数，通常是new开头
// 构造函数通常返回的是指针地址，减小开销
func NewPerson(name string, age int8) *Person{   //返回值是指针
	return &Person{
		name: name,
		age: age,
	}
}

// 为Person类定义方法
func (p Person) Dream(){   //Dream()是给谁的，传给接收者
	fmt.Printf("%s的梦想是学好go和Python\n", p.name)
}

// 指针接收者：接收者的类型是指针
func (p *Person) setAge(newAge int8){
	p.age = newAge
}

// 使用值类型接收者
func (p Person) setAge2(newAge int8){
	p.age = newAge
}

func main(){
	p1 := NewPerson("Peter", 22)   //调用函数
	//(*p1).Dream()   语法糖，*p1地址：对应的变量
	p1.Dream()   //Dream()是属于Person类型的，指针类型转成变量
	fmt.Println(p1.age)   //22
	p1.setAge(int8(25))
	fmt.Println(p1.age)   //25 指针接收者能够改变  
	p1.setAge2(int8(30))
	fmt.Println(p1.age)    //25
}
```

### 值类型接收者

- 在值类型接收者的方法中可以获取接收者的成员值，但修改操作只是针对副本
- 无法修改接收者变量本身，只是将接收者的值复制了一份

### 任意类型添加方法

可以将内置的类型使用关键字type来进行自定义类型，然后为自定义的类型添加方法，下面通过一个栗子来表示如何自定义和使用自定义类型：16

```go
//将内置的int定义为myInt类型
type myInt int

//接收者类型为上面自定义的myInt类型
func (m myInt) sayHello(){
   fmt.Println("hello 我是int")
}

func main(){
   var m1 myInt
   m1.sayHello()
   m1 = 100
   fmt.Printf("%#v  %T\n", m1, m1)
}
```

