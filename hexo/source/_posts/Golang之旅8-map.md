---
title: Golang之旅8-map
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-17 20:11:02
password:
summary:
copyright: true
tags:
  - go
  - map
categories:
  - go
---



### 映射map

`map`是基于`key-value`的数据结构，是引用类型，默认值是`nil`。`map`中的数据是成对出现的，必须初始化才能够使用，定义为

```go
map[keytype]valueType
```

- `KeyType`:表示键的类型。
- `ValueType`:表示键对应的值的类型。

`map`类型的变量默认初始值为`nil`，需要使用`make()`函数来分配内存。语法为：

```go
make(map[keytype]valueType, [cap])  //容量cap不是必须的，但是最好一个容量
```

### 基本使用

- map类型必须进行初始化操作；不初始化则为`nil`



#### 创建`map`

```go
package main

import (
	"fmt"
)

func main(){
	//声明map类型，没有初始化，a的值就是nil
	var a map[string]int
	fmt.Println(a == nil)  //true
	//map的初始化
	a = make(map[string]int, 8)
	fmt.Println(a == nil)

	//a中添加键值对
	a["深圳"] = 1
	a["北京"] = 2
	//%#v显示出来字符串中的引号
	fmt.Printf("a:%#v\n", a)
	fmt.Printf("type:%T\n", a)

	//声明map的同时并初始化
	b := map[int]bool{
		1:true,
		2:false,
	}
	fmt.Printf("b:%#v\n", b)
	fmt.Printf("type:%T\n", b)

	//判断某个键是否存在
	var scoreMap = make(map[string]int, 8)
	scoreMap["张三"] = 98
	scoreMap["李四"] = 90

	value, ok :=scoreMap["王五"]
	fmt.Println(value, ok)
	if ok{
		fmt.Println("李四在scoreMap中",value)
	} else {
		fmt.Println("查无此人")
	}
}
```



#### 遍历和删除

```go
package main

import "fmt"

func main(){
	var scoreMap = make(map[string]int, 8)
	scoreMap["张三"] = 98
	scoreMap["李四"] = 90
	scoreMap["王五"] = 97
	scoreMap["小明"] = 89

	//同时遍历键值
	for k, v := range scoreMap{
		fmt.Println(k,v)
	}

	//遍历键k
	for key := range scoreMap{
		fmt.Println(key)
	}

	//遍历value；其中_表示匿名变量
	for _, value := range scoreMap{
		fmt.Println(value)
	}

	//删除指定的键值对
	delete(scoreMap, "小明")
	fmt.Println(scoreMap)
}
```



#### 元素类型为`map`的切片

```go
package main

import "fmt"

//元素类型为：map
func main(){
	//元素类型为map的切片
	var mapSlice = make([]map[string]int, 8, 8)  //完成切片的初始化：map[string]int定义为map类型

	fmt.Println(mapSlice[0] == nil)
	//内部map的初始化
	mapSlice[0] = make(map[string]int, 0)  //map的初始化
	mapSlice[1] = make(map[string]int, 0)  //切片中的每个map都需要进行初始化才能使用
	mapSlice[0]["张三"] = 1000
	mapSlice[1]["李四"] = 100
	fmt.Println(mapSlice)
}

//result
true
[map[张三:1000] map[李四:100] map[] map[] map[] map[] map[] map[]]

```

#### 值为切片的map

```go
package main

import "fmt"

func main(){
	//值为切片的map：首先定义map，并且初始化
	var sliceMap = make(map[string][]int, 8)  //完成对map的初始化
	v, ok := sliceMap["中国"]   //查看map中是否存在某个键
	if ok{
		fmt.Println(v)
	}else{
		//sliceMap中没有“中国”这个键
        sliceMap["中国"] = make([]int, 8)  //完成对切片的初始化：长度和容量都是8
		sliceMap["中国"][0] = 100
		sliceMap["中国"][2] = 200
		sliceMap["中国"][4] = 400
	}
	//遍历sliceMap
	for k,v := range sliceMap{
		fmt.Println(k,v)
	}
}
```

### 练习题

统计字符串中每个单词出现的元素

```go
package main

import (
	"fmt"
	"strings"
)

//统计字符串中每个单词出现的次数
//"how do  you do"func main(){
	var s = "how do you do"
	var wordCount = make(map[string]int, 10) //切片的长度和容量都是10

	//1. 字符串中有哪些单词：用字符串的Split方法
	words := strings.Split(s, " ")
	//2. 遍历单词做统计
	for _, word := range words{
		v,ok := wordCount[word]
		if ok {
			//map中有这个单词，次数加1
			wordCount[word] = v + 1
		}else {
			//map中没有这个单词，次数初始化为1
			wordCount[word] = 1
		}
	}
	for k,v := range wordCount{
		fmt.Println(k,v)
	}
}
```

