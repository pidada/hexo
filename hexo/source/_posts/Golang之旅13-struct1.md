---
title: Golang之旅13-struct1
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-20 08:34:20
password:
summary:
copyright: true
tags:
  - go
  - 结构体
categories:
  - go
---

### 结构体

> `Go`中没有类的概念，不支持面向对象`OOP`。`Go`语言中通过结构体的内嵌再配合接口比面向对象具有更高的扩展性和灵活性。`Go`语言中通过`struct`来实现面向对象。
>
> 结构体是复合类型，由一系列属性组成，每个属性都有自己的类型和值，结构体通过属性把数据聚集在一起。
>
> 结构体是值类型，可以通过`new`函数进行创建。



- Go 语言中数组可以存储同一类型的数据，在结构体中可以为不同项定义不同的数据类型。

- 结构体是由一系列具有相同类型或不同类型的数据构成的数据集合

- 结构体表示一项纪录，比如书籍的各种属性

  ```
  Title: 标题
  Author: 作者
  ID: 书籍ID号
  Subject: 学科
  ```

  [菜鸟课程](https://www.runoob.com/go/go-structures.html)

[Go语言之结构体](https://www.liwenzhou.com/posts/Go/10_struct/)

### 类型自定义

Go中的基本数据类型有`string、整型、浮点型、布尔型`等，类型可以自定义，通过`type`关键字实现：

```go
type MyInt int    //将MyInt 定义为int类型，它具有int类型的特性
```



<!-- MORE -->

### 类型别名

#### 别名定义

类型别名规定`TypeAlias`是`Type`的别名。比如`byte`是`uint8 `的别名，`rune`是`int32`的别名，别名的定义

```go
type TypeAlias = Type

//栗子
type byte = uint8
type rune = int32
```

#### 类型别名和类型定义的区别

在定义上二者只是差在一个等号上面

```go
type NewInt int    //类型定义

type MyInt = int   //类型别名

func main() {
	var a NewInt
	var b MyInt
	
	fmt.Printf("type of a:%T\n", a) //type of a:main.NewInt
	fmt.Printf("type of b:%T\n", b) //type of b:int
}
```



### 结构体

定义结构体使用的是`type`和`struct`语句。

- `struct` 语句定义一个新的数据类型，结构体有中有一个或多个成员
- `type `语句设定了结构体的名称，用来自定义一个全新的类型
- 结构体内部占用连续的一块内存，格式如下：

```go
type 类型名 struct {
    field1 type1   //字段名 字段类型
    field2 type2
    _ type3   //用不到的字段用_表示  
    field2 type4  
    …
}

//空结构体创建
struct {}
```

- 类型名：标识自定义结构体的名称，在同一个包内不能重复
- 字段类型：表示结构体字段的具体类型
- 字段名：结构体的字段名，结构体中的字段名不能重复，必须唯一
- 字段名称可以显式或者隐式指定，没有显式字段名称的字段称为`匿名字段`
- 用不到的字段，用`_`表示

看一个具体的栗子：关于`Person`的结构体

```go
type person struct {
	name string
	city string
	age  int8
}

// 同样类型的字段可以在同一行
type person1 struct {
	name, city string
	age        int8
}
```

- 结构体中的一个字段用来描述一个值或者某个属性

- 结构体是用来描述一组值的，是一种聚合型的数据类型。



### 初始化和实例化结构体

#### 初始化结构体

没有初始化的结构体，其成员变量都是对应其类型的零值。

- 键值对初始化
- 值的列表进行初始化
- 最后一个字段后面的逗号一定要带上

```go
package main

import "fmt"

type Books struct {
    //title, author, subject string   类型相同可以写到一行
	title string
	author string
	subject string
	book_id int8
}

func main(){
	//1、直接写出对应的字段的值values
    //必须初始化结构体的所有字段
    //填充顺序和字段在结构体中的顺序相同
    //不能和键值对的方式混合使用
	fmt.Println(Books{
		"go 语言",
		"小明",
		"go 教程",
		1})

	//2、通过keys-values进行创建
	fmt.Println(Books{
		title: "go 语言",
		author: "小明",
		subject: "go 教程",
		book_id: 1})

	// 3、忽略的字段为 0 或 空
	fmt.Println(Books{
		title: "Go 语言",
		author: "小明"})
    
    p := &Books{
		title: "go 编程",
		author: "Peter",    //逗号一定要带上
	}
	fmt.Printf("%#v\n", p)
}
```



#### 实例化和匿名结构体

只有当结构体实例化时，才会真正地分配内存。也就是必须实例化后才能使用结构体的字段。结构体本身也是一种类型，可以通过关键字`var`来声明结构体类型。**类比`Python`的中类和类的实例化。**

```go
package main

import "fmt"

//定义结构体
type person struct {
	name string
	city string
	email string
	age int8
}

func main(){
	var p1 person   //实例化结构体
	p1.name = "小明" //访问结构体中的字段，通过.来实现
	p1.city = "深圳"
	p1.email = "123456@qq.com"
	p1.age = 20

	fmt.Printf("p1=%v\n", p1)   //%v打印的数据更清晰
	fmt.Printf("p1=%#v\n", p1)
    fmt.Println(p1.name)   //直接通过结构体的属性
	fmt.Println(p1.age)
    
    
    //匿名结构体：临时使用
	var user struct{
		name string
		married bool
	}
	user.name = "张三"
	user.married = false
	fmt.Printf("%#v\n", user)
}
```



