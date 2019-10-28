---
title: Golang之旅25-reflect反射
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-22 14:59:18
password:
summary:
tags:
  - go
  - reflect
copyright: true
categories:
  - go
---

### 变量的内在机制

Go语言中的变量分成两个部分

- 类型信息：预先定义的元信息
- 值信息：程序运行过程中可动态变化的部分

### 反射reflect

> 反射是指程序在运行期间对程序本身进行访问和修改的能力，实现通过程序运行时反射，让程序能够操作任意类型的变量。

<!--MORE-->

### 结构体反射

#### 与结构体相关的方法

通过`reflect.TypeOf()`获得反射对象信息后，若其类型是结构体，可以通过反射值对象（`reflect.Type`）的`NumField()`和`Field()`方法获得结构体成员的详细信息 。

`reflect.Type`中与获取结构体成员相关方法：



| 方法                                                        | 说明                                                         |
| ----------------------------------------------------------- | ------------------------------------------------------------ |
| Field(i int) StructField                                    | 根据索引，返回索引对应的结构体字段的信息。                   |
| NumField() int                                              | 返回结构体成员字段数量。                                     |
| FieldByName(name string) (StructField, bool)                | 根据给定字符串返回字符串对应的结构体字段的信息。             |
| FieldByIndex(index []int) StructField                       | 多层成员访问时，根据 []int 提供的每个结构体的字段索引，返回字段的信息。 |
| FieldByNameFunc(match func(string) bool) (StructField,bool) | 根据传入的匹配函数匹配需要的字段。                           |
| NumMethod() int                                             | 返回该类型的方法集中方法的数目                               |
| Method(int) Method                                          | 返回该类型方法集中的第i个方法                                |
| MethodByName(string)(Method, bool)                          | 根据方法名返回该类型方法集中的方法                           |



#### StructField类型

`StructField`类型用来描述结构体中的一个字段的信息。`StructField`的定义如下：

```go
type StructField() struct{
    Name string
    Type Type
    Tag StructTag
    Index []int
    Anonymous bool
}
```



```go
// struct-reflect demo

package main

import (
	"fmt"
	"reflect"
)

type student struct {
	Name string `json:"name" ini:"s_name"`
	Score int `json:"score" ini:"s_score"`
}

func main(){
	stu1 := student{
		Name:  "小王子",
		Score: 20,
	}

	// 通过反射去获取结构体中的字段信息
	t := reflect.TypeOf(stu1)   //
	fmt.Printf("name:%v   kind:%v\n", t.Name(), t.Kind())

	for i := 0; i<t.NumField();i++{
		// 根据结构体字段的索引去取字段
		fileObj := t.Field(i)
		fmt.Printf("name:%v type:%v tag:%v\n", fileObj.Name, fileObj.Type, fileObj.Tag)
		fmt.Println(fileObj.Tag.Get("json"), fileObj.Tag.Get("ini"))
	}

	// 根据名字去取结构体中的字段
	fileObj2, ok := t.FieldByName("Score")
	if ok{
		fmt.Printf("name:%v type:%v tag:%v\n", fileObj2.Name, fileObj2.Type, fileObj2.Tag)
	}
}

// result
name:student   kind:struct
name:Name type:string tag:json:"name" ini:"s_name"
name s_name
name:Score type:int tag:json:"score" ini:"s_score"
score s_score
name:Score type:int tag:json:"score" ini:"s_score"
```

