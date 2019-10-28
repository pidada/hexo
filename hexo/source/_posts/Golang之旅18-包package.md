---
title: Golang之旅18—-package
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-24 09:36:22
password:
summary:
copyright: true
tags:
  - go
  - 包
  - 闭包
  - package
categories:
  - go
---

### 包

包是多个`Go`源码即`.go`文件的集合，可以理解为包是一个存放源码文件的文件夹，如内置的`fmt`、`os`、`io`。

```go
package 包名  // 每个.go文件夹下都要声明文件所属的包 
```

- 一个文件夹下只能有一个包
- 同样一个包的文件不能在多个文件夹下
- 包名不能和文件夹重名
- 包名为`main`包的应用程序入口

<!--MORE-->

### 可见性

将标识符的首写字母大写，就可以对外可见。结构体中字段名的首写字母如果大写，外部包也可以访问这些字段。

```go
package pkg2

import "fmt"

// 包变量可见性

var a = 100 // 首字母小写，外部包不可见，只能在当前包内使用

// 首字母大写外部包可见，可在其他包中使用
const Mode = 1

type person struct { // 首字母小写，外部包不可见，只能在当前包内使用
	name string
}

// 首字母大写，外部包可见，可在其他包中使用
func Add(x, y int) int {
	return x + y
}

func age() { // 首字母小写，外部包不可见，只能在当前包内使用
	var Age = 18 // 函数局部变量，外部包不可见，只能在当前函数内使用
	fmt.Println(Age)
}
```



### 包的导入

- 单行或者多行导入
- 包的别名
- 匿名导入包`_`

```go
import "package_path"
```

例如：

```go
package packagedemo

//单行导入
import "code/package/snow"
import	"fmt"

//匿名导入
import _ "package_path"

//多行导入
import (
	"code/package/snow"
	"fmt"
    neza "github.com/Q1mi/studygo/pkg_test"  //给包起别名neza
)
```

- 包的导入放在文件开头包声明语句下面
- 不能循环导入，用双引号包裹起来
- 包名是从`$GOPATH/src/`后开始计算的，使用`/`进行路径分隔



### init函数

在Go语言程序执行时导入包语句会自动触发包内部`init()`函数的调用。需要注意的是： `init()`函数没有参数也没有返回值。 

- 没有返回值和参数
- 在程序运行时候自动被调用

![](https://s2.ax1x.com/2019/09/26/umcHk6.png)

![](https://s2.ax1x.com/2019/09/26/umRBOH.png)

