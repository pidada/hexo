---
title: Golang之旅7-切片slice
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-17 14:31:52
password:
summary:
copyright: true
tags:
  - go
  - slice
categories:
  - go
---

### 切片slice

- 切片是引用类型切片初始化之后才能进行使用
- 切片不能直接比较
- `append`函数可以向切片中追加元素，同时还能利用`append`函数删除切片中的某个元素
- 切片是数组的底层封装
- `make([]T, len,map)`函数能够直接创建切片`make([]int, 5, 10)`

> 切片（`Slice`）是一个拥有`相同类型元素`的`可变长度`的序列。`切片是引用类型，必须初始化才能进行使用`。它是基于数组类型做的一层封装。它非常灵活，支持自动扩容。
>
> 切片是一个引用类型，一般用于快速地操作一块数据集合。切片的底层就是数组，三要素：
>
> - 地址：指针的第一个位置
> - 长度：`len()`函数获得
> - 容量：`cap()`函数获得
>
> 切片的基本语法：

```
var name []T
```

- `name`表示变量名
- `T`：表示切片中的元素类型，比如`string、int、bool`等
- 切片创建的时候不指定长度；`[]`里面不带数字

```go
func main() {
	// 声明切片类型
	var a []string              //声明一个空字符串切片
	var b = []int{}             //声明一个整型切片并初始化
	var c = []bool{false, true} //声明一个布尔切片并初始化
	var d = []bool{false, true} //声明一个布尔切片并初始化
	fmt.Println(a)              //[]
	fmt.Println(b)              //[]
	fmt.Println(c)              //[false true]
	fmt.Println(a == nil)       //true
	fmt.Println(b == nil)       //false
	fmt.Println(c == nil)       //false
	// fmt.Println(c == d)   //切片是引用类型，不支持直接比较，只能和nil比较
}
```

[GO切片](https://www.liwenzhou.com/posts/Go/06_slice/)



<!-- MORE -->



### make函数

通过`make()`函数构造动态的切片，基本语法为

```go
make([]T, size, cap)
```

- T:切片的元素类型
- size:切片中元素的数量
- cap:切片的容量

```go
//make函数构造切片
h := make([]int,5,10)  //5代表长度，10代表容量（可省略）
fmt.Println(h)
fmt.Printf("%T\n", h)
```



### 切片的本质

切片的本质就是对底层数组的封装，它包含了三个信息：底层数组的指针、切片的长度`（len）`和切片的容量`（cap）`。

- 指针
- 长度
- 容量

现在有个数组`a := [8]int{0, 1, 2, 3, 4, 5, 6, 7}`，切片`s1 := a[:5]`，相应示意图如下：

- 指针：第一个元素所在的位置
- 长度：元素总个数
- 容量：原有数组的容量减去指针所在的位置

![](https://www.liwenzhou.com/images/Go/slice/slice_01.png)

切片`s2 := a[3:6]`

![](https://www.liwenzhou.com/images/Go/slice/slice_02.png)



### 切片不能直接比较

- 切片之间不能通过`==`来进行比较和判断
- 唯一合法的操作是和`nil` 进行比较
  - 一个`nil`值的切片没有底层数组
  - `nil`值的切片的长度和容量都是0
  - 不能说长度和容量是0的切片是`nil`

- 判断切片是否为空，用`len(s) == 0`

  ```go
  var s1 []int         //len(s1)=0;cap(s1)=0;s1==nil
  s2 := []int{}        //len(s2)=0;cap(s2)=0;s2!=nil
  s3 := make([]int, 0) //len(s3)=0;cap(s3)=0;s3!=nil
  ```

### 空切片`nil`

一个切片在未初始化之前默认都是`nil`，长度是`0`：

```go
package main

import "fmt"

func main(){
	var a []int     //定义切片未初始化
 	var b = []int{}  // 定义并且初始化

 	printSlice(a)
	//判断a和nil的关系
	if (a== nil){
		fmt.Println("切片a是空的")
	}

	printSlice(b)
	//判断b和nil的关系
	if (b == nil){
		fmt.Println("切片b是空的")    //b已经初始化，索引不会打印本句
	}
}

//定义printSlice函数，输出长度、容量和切片
func printSlice(x []int){
	fmt.Printf("len=%d cap=%d slice=%v\n", len(x), cap(x), x)
}
```



----



### 切片赋值拷贝

当进行赋值拷贝之后，两个变量共享底层数组的数据，一个切片的变化会影响另一个切片。

```go
package main

import "fmt"

func main() {
	s1 := make([]int, 3) //[0 0 0]
	s2 := s1             //将s1直接赋值给s2，s1和s2共用一个底层数组
	s2[0] = 100
	fmt.Println(s1) //[100 0 0]
	fmt.Println(s2) //[100 0 0]
}
```

### 遍历

切片的遍历分为两种：索引遍历和`for  range`遍历

```go
package main

import "fmt"

//切片遍历
func main(){
	numArray := []int{1,2,3,4,5,6,7,8}
	
    //索引遍历
	for i :=0; i<len(numArray); i++{
		fmt.Println(i, numArray[i])
	}

	fmt.Println()
	
    //for range 遍历
	for index, value := range numArray{
		fmt.Println(index, value)
	}
}

```





### append

`append`函数可以为切片动态添加元素。每个切片指向一个底层数组，数组能够容纳一个数量的元素。当底层数组不能容纳新增的元素时，切片就会自动按照一定的策略进行“扩容”，此时该切片指向的底层数组就会更换。“扩容”操作往往发生在`append()`函数调用时。

```go
package main

import "fmt"

func main(){
	var a []int      //此时a没有申请内存，不能直接添加元素
	//a[0] = 100
	//fmt.Println(a)

	for i :=0; i<10; i++{
		a = append(a,i)  // append函数将元素追加到切片的最后，同时需要原来的切片进行接收
		fmt.Printf("%v len:%d cap:%d ptr:%p\n", a, len(a), cap(a), a)
	}

	//append追加多个元素
	var citySlice []string
	citySlice = append(citySlice, "北京")   // 追加一个元素
	citySlice = append(citySlice, "上海", "广州", "深圳")  	// 追加多个元素
	b := []string{"成都", "重庆"}
	citySlice = append(citySlice, b...)   	// 追加切片
	fmt.Println(citySlice) //[北京 上海 广州 深圳 成都 重庆]
}
```

```go
[0] len:1 cap:1 ptr:0xc00000e100
[0 1] len:2 cap:2 ptr:0xc00000e150
[0 1 2] len:3 cap:4 ptr:0xc00000a3a0
[0 1 2 3] len:4 cap:4 ptr:0xc00000a3a0
[0 1 2 3 4] len:5 cap:8 ptr:0xc00000c280
[0 1 2 3 4 5] len:6 cap:8 ptr:0xc00000c280
[0 1 2 3 4 5 6] len:7 cap:8 ptr:0xc00000c280
[0 1 2 3 4 5 6 7] len:8 cap:8 ptr:0xc00000c280
[0 1 2 3 4 5 6 7 8] len:9 cap:16 ptr:0xc00008c000
[0 1 2 3 4 5 6 7 8 9] len:10 cap:16 ptr:0xc00008c000
```

**小结**

- append函数将元素追加到切片的最后，同时需要原来的切片进行接收
- 容量按照1,2,4,8.16进行扩容，每次扩容后都是扩容前的2倍。
- append函数能够同时追加多个元素

### 切片的扩容策略

```go
newcap := old.cap
doublecap := newcap + newcap
if cap > doublecap {
	newcap = cap   //新申请的容量cap > 2倍旧容量，最终的容量newcap就是新申请的容量cap
} else {
	if old.len < 1024 {  
		newcap = doublecap   //如果旧切片的长度小于1024，最终容量就是旧容量的2倍
	} else {
		// Check 0 < newcap to detect overflow
		// and prevent an infinite loop.
		for 0 < newcap && newcap < cap {  //如果旧切片的长度大于1024，则新容量每次增加旧容量的四分之一，直到最终容量大于等于新申请的容量
			newcap += newcap / 4
		}
		// Set newcap to the requested cap when
		// the newcap calculation overflowed.
		if newcap <= 0 {
			newcap = cap  //如果最终容量溢出，则最终容量就是新申请的容量
		}
	}
}
```



### copy函数

`copy`函数实现将一个切片的数据迅速地复制到另一个切片空间中，切片要初始化之后才能使用。基本语法为：

```
copy(destSlice, srcSlice []T)
```

- `srcSlice`: 数据来源切片
- `destSlice`: 目标切片

```go
package main

import "fmt"

func main() {
	s1 := make([]int, 3) //[0 0 0]
	s2 := s1             //将s1直接赋值给s2，s1和s2共用一个底层数组
	s2[0] = 100
	fmt.Println(s1) //[100 0 0]
	fmt.Println(s2) //[100 0 0]

	a := []int{1,2,3,4,5}
	b := make([]int, 5, 5)
	c := b     //b直接赋值给c，二者共享数组中的数据，变化同时发生。
	copy(b,a)  //ab之间没有关系，独立
	b[0] = 20
	fmt.Println(a)
	fmt.Println(b)
	fmt.Println(c)

	//通过copy函数删除元素
	d := []string{"北京","深圳","广州","长沙"}
	d = append(d[0:2], d[3:]...)
	fmt.Println(d)
}
```

总结：删除切片中索引为index的元素，需要注意的是append函数中可以同时追加多个元素

```go
append(a[0:index], a[index+1:]...)
```



### 练习题

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	var a = make([]string, 5, 10)   //定义初始化[     ]  5个空格的空切片
	fmt.Println(a)
	for i := 0; i < 10; i++ {
		a = append(a, fmt.Sprintf("%v", i))
	}
	fmt.Println(a)
	fmt.Printf("%T\n", a)  //[]string类型的切片
	fmt.Println(len(a))  //15


	//请使用内置的sort包对数组var b = [...]int{3, 7, 8, 9, 1}进行排序
	var b = [...]int{4,8,2,9,1}   //定义数组
	sort.Ints(b[:])   //将数组变成了切片，直接排序
	fmt.Println("sort_b:",b)
}
```

