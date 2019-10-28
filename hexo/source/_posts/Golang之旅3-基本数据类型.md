---
title: Golang之旅3-基本数据类型
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-16 13:46:36
password:
summary:
copyright: true
tags: 
  - go
  - 数据类型
  - 高性能
  - 高并发
categories: 
  - go
---

### 基本数据类型

`GO`语言中的数据类型十分丰富，包含：整型、浮点型、布尔型，还有数组、切片、结构体、函数、`map`、通道`chanel`等

[go基本数据类型](https://www.liwenzhou.com/posts/Go/02_datatype/)

#### 整型

1. 无符号整型
   	- uint8：$0——2^8-1$，0到255
    - uint16：$0——2^16-1$,0到65535
    - uint32
    - uint64

2. 有符号整型
      - int8：-128到127
      - int16：-32768到32767
      - int32
      - int64 

3. 其他数字类型

   - uint：32/64位系统上就是uint32/uint64
   - int：32/64位系统上就是int32/int64
   - uintptr：无符号整型，用于存放一个指针

   
   
   <!-- MORE -->
   
   > **注意：** 
   >
   > - 在使用`int`和 `uint`类型时，不能假定它是32位或64位的整型，而是考虑`int`和`uint`可能在不同平台上的差异。
   > - 获取对象的长度的内建`len()`函数返回的长度可以根据不同平台的字节长度进行变化。实际使用中，切片或 map 的元素数量等都可以用`int`来表示。在涉及到二进制传输、读写文件的结构描述时，为了保持文件的结构不会受到不同编译目标平台字节长度的影响，不要使用`int`和 `uint`。

#### 浮点型

两种浮点类型：`float32`和`float64`，需要导入包`math`。

- `float32` 的浮点数的最大范围约为 `3.4e38`，可以使用常量定义：`math.MaxFloat32`

-  `float64` 的浮点数的最大范围约为 `1.8e308`，可以使用一个常量定义：`math.MaxFloat64`

  ```go
  package main
  import (
      "fmt"
      "math"    // 导入math包
  )
  	//浮点数
  	fmt.Printf("%f\n", math.Pi)
  	fmt.Printf("%f\n", math.Phi)
  	fmt.Printf("%.2f\n", math.Pi)
  
  	// 查看系统默认最大的浮点数
  	fmt.Println(math.MaxFloat32)
  	fmt.Println(math.MaxFloat64)
  }
  ```

  

  #### 复数

  有`complex64`（实部和虚部都是`32`位）和`complex128`（实部和虚部都是`64`位）两种

  ```go
  var c1 complex64
  c1 = 1 + 5i
  
  var c2 complex64
  c2 = 10 + 10i
  ```

  

  

  #### 布尔值

  - 只有`False`和`True`，默认是`Fasle`

  - `Go`语言中不允许将数值类型强制转成布尔型
  - 布尔类型不参与运算，无法和其他类型进行转换

  #### 字符串

  - `Go`语言中以原生数据类型出现

  - 使用的的是`utf-8`编码，默认是支持中文的

  - 字符串为双引号（""）里面的内容

  - 可以在`Go`语言的源码中直接添加非`ASCII`码字符

    ```go
    s1 := "hello beijing" 
    s2 := "你好，北京"
    fmt.Println(s1, s2)转义字符
    ```

  #### 转义字符

  
  
  | 转义字符 | 作用             |
  | -------- | ---------------- |
  | ---      | ---              |
  | \r       | 回车符，回到行首 |
  | \n       | 换行符           |
  | \t       | 制表符           |
  | \‘       | 单引号           |
  | \"       | 双引号           |
  | \\\      | 反斜杠           |

```go
//转义字符：打印路径
fmt.Println("c:\\code\\go.exe")	//第一个反斜杠是转义字符，第二个表示原样输出反斜杠

```

#### 多行字符串

通过反引号来实现，类似于`Python`中的`r''`，里面的内容是原样输出，不进行任何转义

```go
//多行字符串输出：里面的内容是原样输出，不进行任何转义
s3 := `go语言是谷歌开发的\n
go语言有多种数据类型
多行字符串通过反引号来实现
`
fmt.Println(s3)
```



####  字符串操作

- 求长度
- 拼接
- 分割
- 是否包含
- 前后缀判断
- 子串出现的位置
- `jion`操作

![](https://s2.ax1x.com/2019/09/16/nW0fsg.png)

```go
package main

import (
	"fmt"
	"strings"
)

func main()  {
	s1 := "hello"   //一个英文字母占据一个字节
	s2 := "go语言"  //一个汉字占3个字节

	//求长度
	fmt.Println(len(s1), len(s2))

	//拼接
	fmt.Println(s1 + s2)
	s3 := fmt.Sprintf("%s - %s", s1, s2)  //通过Sprintf方法
	fmt.Println(s3)

	//分割
	s4 := "how do you do"
	fmt.Println(strings.Split(s4, " "))  //两个参数：待分割字符和指定分割的符号
	fmt.Printf("%T\n", strings.Split(s4, " "))  //判断字符串类型

	//包含与否
	fmt.Println(strings.Contains(s4, "do"))

	//判断前后缀
	fmt.Println(strings.HasPrefix(s4, "how"))
	fmt.Println(strings.HasSuffix(s4, "how"))

	//子字符串的位置，通过strings.Index
	fmt.Println(strings.Index(s4, "do"))
	fmt.Println(strings.LastIndex(s4, "do"))

	//join操作
	s5 := []string{"how", "do", "you", "do"}  //如何创建列表
	fmt.Println(s5)
	fmt.Println(strings.Join(s5, "+"))
}

//result
D:\go\src\code\str>go build
D:\go\src\code\str>str.exe
5 8
hellogo语言
hello - go语言
[how do you do]
[]string
true
true
false
4
11
[how do you do]
how+do+you+do
```

#### byte和rune类型

组成每个字符串的元素叫做“字符”，通过遍历的方式获取字符串中单个字符。字符用`单引号`括起来

```go
var a := "zhong"
var b := "中"
```

`Go`语言的字符有两种：

- `uint8`类型，或者叫做`byte`类型，代表`ASCII`码的一个字符，处理默认字符串类型，不能处理中日韩等文字
- `rune`类型，代表的是`utf-8`字符，实际上是一个`int32`，用来处理Unicode类型

```go
package main

import "fmt"

func main(){
	//byte处理ASCII码；rune处理Unicode编码
	var c1 byte = 'c'
	var c2 rune = 'c'
	fmt.Println(c1, c2)
	fmt.Printf("c1:%T  c2:%T\n", c1, c2)

	s := "hello中国"
	for i := 0; i < len(s); i++{   //遍历方式
		fmt.Printf("%c\n", s[i])
	}
	fmt.Println()  //打印空行分割作用
	for _, r := range s{   // range循环实现
		fmt.Printf("%c\n", r)
	}
	fmt.Println()
}
```

- `byte`类型不能处理中文，出现乱码
- `rune`处理中日韩等文字，根据字符来遍历

### 修改字符串

要修改字符串需要先将其转成`[]rune`或者`[]byte`类型，完成后再转成`string`。转换会重新分配内存，并且赋值字节数组。`字符串--->[]rune/[]byte--->string`

```go
package main

import "fmt"

func main(){
	s1 := "big"
	//强制类型转换
	byteS1 := []byte(s1)
	byteS1[0] = 'p'
	fmt.Println(string(byteS1))

	s2 := "白萝卜"
	byteS2 := []rune(s2)
	byteS2[0] = '红'   //赋值语句
	fmt.Println(string(byteS2))
}
```

**Go语言中只有强制类型转换，没有隐式类型转换**