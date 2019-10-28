---
title: Golang之旅17-结构体可见性和JSON序列化
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-21 16:08:21
password:
summary:
copyright: true
tags:
  - go
  - json
  - 可见性
  - 高性能
categories:
  - go
---

### 结构体可见性

结构体中字段大写开头表示可公开访问，小写表示私有（仅在定义当前结构体的包中可访问）

- 不管是结构体还是结构体字段名，大写可以公开访问
- 小写则只能当前的包内访问



### JSON序列化

`JSON(JavaScript Object Notation)` 是一种轻量级的数据交换格式。易于人阅读和编写。同时也易于机器解析和生成。`JSON`键值对是用来保存`JS`对象的一种方式

- 键值对中的`键名`写在前面，用`双引号`包裹起来
- 键值之间使用冒号分开
- 多个键值对之间用英文逗号分开

<!-- MORE -->

### 如何将`json`数据和`go`语言格式数据之间进行转换:grey_question:

- `Go---->JSON：json. Marshal`

```go
data, err := json.Marshal(c1)
	if err != nil{  //报错提示
		fmt.Println("json error failed , err:", err)
		return
	}
```



- `JSON--->GO：json.Unmarshal`

  ```go
  var c class   // c2是解析出来存放数据的位置
  err = json.Unmarshal([]byte(jsonStr), &c)
  if err != nil{
      fmt.Println("json unmarshal failed, err:", err)
      return
  }
  ```

  

  

```go
package main

import (
	"fmt"
	"encoding/json"
	)

// 结构体字段可见性和JSON序列化

// Go语言中如果定义的标识符是首字母大写，则是对外可见的
// 结构体中字段名是大写，则是对外可见的

type student struct {
	ID int
	Name string
}

//student的构造函数
func newStudent(id int, name string) student{
	return student{
		ID: id,
		Name: name,
	}
}

type class struct {
	Title string
	Students []student   //slice 类型
}

func main(){
	//创建一个班级变量
	c1 := class{
		Title:    "三七班",
		Students: make([]student, 0, 20),
	}
	//往c1中添加学生
	for i := 0; i < 10;i++{
		//造10个学生
		temStu := newStudent(i, fmt.Sprintf("stu%02d", i))
		c1.Students = append(c1.Students, temStu)
	}
	fmt.Printf("%#v\n", c1)

	// JSON序列化和反序列化
	// Go数据---->json格式数据
	data, err := json.Marshal(c1)
	if err != nil{  //报错提示
		fmt.Println("json error failed , err:", err)
		return
	}
	fmt.Printf("%T\n", data)
	fmt.Printf("%s\n", data)

	//JSON反序列化：json---->go语言格式的数据
	jsonStr := `{"Title":"三七班","Students":[{"ID":0,"Name":"stu00"},{"ID":1,"Name":"stu01"},{"ID":2,"Name":"stu02"},{"ID":3,"Name":"stu03"},{"ID
		":4,"Name":"stu04"},{"ID":5,"Name":"stu05"},{"ID":6,"Name":"stu06"}]}`

	var c2 class   // c2是解析出来存放数据的位置
	err = json.Unmarshal([]byte(jsonStr), &c2)
	if err != nil{
		fmt.Println("json unmarshal failed, err:", err)
		return
	}
	fmt.Printf("%#v\n", c2)
}
```



### Tag标签

当后端语言是`Go`语言，如果前后端的命名出现冲突，可以使用`tag`来解决问题。`Tag`是结构体的元信息，可以在运行的时候通过反射的机制读取出来。 `Tag`在结构体字段的后方定义，由一对反引号包裹起来，具体的格式如下：

```p
`k1:"v1" k2:"v2"`   //键值对组成
```

标签的组成部分

- 由一个或者多个键值对组成
- 键和值之间使用冒号`:`分开
- 值用双引号`""`括起来
- 多个键值对之间用空格分开
- `key`和`value`之间不要有空格

`Attention：`为结构体编写`Tag`时，必须严格遵守键值对的规则。

```go
//Student 学生
type Student struct {
	ID     int    `json:"id"` //通过指定tag实现json序列化该字段时的key
	Gender string //json序列化是默认使用字段名作为key
	name   string //私有不能被json包访问
}

func main() {
	s1 := Student{
		ID:     1,
		Gender: "男",
		name:   "沙河娜扎",
	}
	data, err := json.Marshal(s1)
	if err != nil {
		fmt.Println("json marshal failed!")
		return
	}
	fmt.Printf("json str:%s\n", data) //json str:{"id":1,"Gender":"男"}
}
```

