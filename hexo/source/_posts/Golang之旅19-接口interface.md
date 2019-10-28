---
title: Golang之旅19_接口interface
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-07 15:43:07
password:
summary:
copyright: true
tags:
  - go
  - interface
categories:
  - go
---

> 在Go语言中接口`interface`是一种类型，一种抽象的类型。

- 如何定义接口`interface`
- 接口如何实现和使用
- 值接收者和指针接收者的区别
  - 值接收者实现接口：类型的值和类型的指针接收者都可以保存在类型变量中
  - 指针接收者实现接口：只能是指针接收者
- 空接口的应用
- 空接口的类型断言`x.(type)`

<!--MORE-->

### 接口定义

Go语言提倡面向接口编程，每个接口由数个方法组成，接口的格式如下：

```go
type interfaceName interface{
    方法名1( 参数列表1 ) 返回值列表1
    方法名2( 参数列表2 ) 返回值列表2
    …
} 
```

- 接口名：使用`type`关键字将接口定义为自定义的类型名。
  - 接口名字一般是`er`结尾	
  - 接口名字最好能够体现该接口的含义，比如写操作的接口Writer，字符串功能的接口Stringer

- 方法名：只要当方法名和接口类型名字是大写的时候，接口所在的包之外的代码能够进行访问。

  记住：**在go语言中首字母带包的类型才能够被外部访问**

- 参数和返回值列表：可以省略不写

```go
// 栗子
type writer interface{
    Write([]byte) error
}
```

###  接口实现

下面的栗子中`dog、cat 、person`都实现了say()方法，因此在接口`sayer`中只要是实现了`say()`方法的都可以使用。

```go
package main

import "fmt"

// 接口

type dog struct {}

// 实现方法say()
func (d dog) say(){
	fmt.Println("wangwangwang")
}

type cat struct {}

// 实现方法say()
func (c cat) say(){
	fmt.Println("miaomiaomiao")
}

type person struct {
	name string
}

// 实现方法say()
func (p person) say(){
	fmt.Println("aaaa")
}

// 实现接口，关键字interface
// 定义一个抽象的类型，只要实现say()方法，都可以传入接口中,当做sayer类型
type sayer interface {
	say()
}

// da 函数，执行接口sayer中的方法say方法
func da(arg sayer) {
	arg.say()

}

func main(){
	//c1 := cat{} //实例化
	//da(c1)
	//d1 := dog{}
	//da(d1)
	//p1 := person{
	//	name:"xiaoming",
	//}
	//da(p1)

	// 声明sayer接口类型的变量s
	var s sayer
	c2 := cat{}
	s = c2   // 实例化的结果来赋值给s
	p2 := person{name:"xiaowangzi"}
	s = p2
	fmt.Println(s)
}
```

### 值接收者和指针接收者

```go
package main
 
import "fmt"

type mover interface{
    move()
}

type person struct {
    name string 
    age int
}

// 使用值接收者实现接口：类型的值和类型的指针接收者都可以保存在类型变量中
//func (p person) move(){
//    fmt.Printf("%s在跑..", p.name)
//}

// 使用指针接收者实现接口
func (p *person) move(){
    fmt.Printf("%s在跑..", p.name)
}

func main(){
    var m mover
    p1 := person{   //person类型的值
        name: "小王子",
        age: 18,   // 逗号不能忘记
    }
    p2 := &person{
        name: "哪吒",
        age: 18,
    }
    //m = p1   使用指针接收者报错：p1是person类型的值
    m = p2
    m.move()
    fmt.Println(m)
}
```

### 类型和接口的关系

一个类型可以实现多个接口

多个类型也可以实现同一个接口

 ```go
package main

import "fmt"

// 接口的嵌套：直接使用接口的名字；或者使用接口实现的方法名字
type animal interface {
    mover  
    sayer
    //move()
    //say()
    
}

type mover interface{
    move()
}

type sayer {
    say()
}

type person struct {
    name string 
    age int
}

func (p *person) move(){
    fmt.Printf("%s在跑...\n", p.name)
}

func (p *person) say(){
    fmt.Printf("%s在叫...\n", p.name)
}

func main(){
    var m mover   // 定义mover类型的变量
    //p1 := person{
    //    name: "小王子",
    //    age: 18,
    //}
    p2 := &person{
        name: "哪吒", 
        age: 20,
    }
    m = p2
    m.move()
    fmt.Println(m)
    
    var s sayer   // 定义一个sayer类型的变量
    s = p2
    s.say()
    fmt.Println(s)
    
    var g  animal  // 定义animal类型的变量
    g = p2
    g.move()
    g.say()
    fmt.Println(a)
}

 ```

### 空接口

空接口是指没有定义任何方法的接口，任何类型都可以实现空接口。

- 空接口类型的变量可以存储任意类型的变量
- 空接口实现接收任意类型的函数参数
- 空接口一般是不需要提前定义，使用的时候直接定义

```go
package main

import "fmt"

// 空接口：接口中没有定义任何需要实现的方法
// 空接口变量可以存储任意值
// 空接口可以作为map的value；可以当做函数的参数

// 一般是不需要提前定义
type do interface {

}

func main(){
	var x interface{}   // 定义一个空接口
    
	x = "hello"  // 存储多种类型的值
	fmt.Println(x)
	x = 100
	fmt.Println(x)
	
    // 空接口作为map的值value
	var m = make(map[string] interface{}, 16)
	m["name"] = "xiaoming"
	m["age"] = 18
	m["hobby"] = []string{"篮球", "足球"}  // 切片类型
	fmt.Println(m)
}

```

### 接口值

接口值是由一个`具体类型`和`具体类型的值`两部分组成，分别对应为接口的`动态类型`和`动态类型的值`。

![](https://s2.ax1x.com/2019/10/07/uREDeK.png)

**如何判断空接口中类型的值：使用接口断言**

```go
x.(T)
```

- x：表示类型为interface{}的变量
- T：表示可能的类型

返回的结果中有两个值：

- x转化为类型T后的变量

- 判断的结果，`bool`值：判断成功是`T`，失败是`F`

  

  ```go
  package main
  
  import (
  	"fmt"
  )
  
  func main(){
  	var x interface{}
  	x = "xiaoming"
  
  	// 类型断言
  	ret, ok := x.(string)  // 猜的类型不对，ok=false,ret=string类型的零值
  	if  !ok{
  		fmt.Println("不是字符串类型")
  	}else{
  		fmt.Println("是字符串类型", ret)
  	}
  
  	// 使用switch进行类型断言
  	switch v:= x.(type) {    
  	case string :
  		fmt.Printf("是字符串类型，value:%v\n", v)
  	case int:
  		fmt.Printf("是int类型，value:%v\n", v)
  	case bool:
  		fmt.Printf("是bool类型，value:%v\n", v)
  	default:   // 最后必须要有默认的结果值
  		fmt.Printf("找不到合适类型")	
  	}
  }
  ```

  