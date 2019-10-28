---
title: Golang之旅21-time模块使用
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-16 23:47:22
password:
summary:
copyright: true
tags:
  - go
  - time
categories: go
---

### time包

> time包提供了时间的显示和测量用的函数。日历的计算采用的是公历。 

#### 时间类型

 `time.Time`类型表示时间。我们可以通过`time.Now()`函数获取当前的时间对象，然后获取时间对象的年、月、日、时、分、秒等信息。 

<!--MORE-->

```go
package main

import (
	"fmt"
	"time"
)

// time包：时间的测量和显示

func main(){
	 // Now()首字母大写才能外部访问；
	 now := time.Now()   // 输出当前时间，得到时间对象实例
	 year := now.Year()
	 month := now.Month()
	 day := now.Day()
	 hour := now.Hour()
	 minute := now.Minute()
	 second := now.Second()
	 fmt.Println(year, month, day, hour, minute, second)
}
```

#### 时间戳

 时间戳是自1970年1月1日（08:00:00GMT）至当前时间的总毫秒数。它也被称为Unix时间戳（UnixTimestamp）

 使用`time.Unix()`函数可以将时间戳转为时间格式

```go
package main

import (
	"fmt"
	"time"
)

// time包：时间的测量和显示

func main(){
	 // 时间戳：1970年1月1日到现在经过的时间，用秒数表示
	 timeStamp1 := now.Unix()
	 timeStamp2 := now.UnixNano()  // 得到纳秒形式
	 fmt.Println(timeStamp1, timeStamp2)

	 // 将时间戳转成具体时间格式
	 t := time.Unix(1470997973, 0)   // 传入参数是时间戳
	 fmt.Println(t)
}
```

#### 时间间隔

 `time.Duration`是`time`包定义的一个类型，它代表两个时间点之间经过的时间，以纳秒为单位 

> time包中定义的时间间隔类型的常量如下： 

```go
const (
    Nanosecond  Duration = 1
    Microsecond          = 1000 * Nanosecond
    Millisecond          = 1000 * Microsecond
    Second               = 1000 * Millisecond
    Minute               = 60 * Second
    Hour                 = 60 * Minute
)
```



```go
package main

import (
	"fmt"
	"time"
)

// time包：时间的测量和显示

func main(){
	 // 时间间隔：单位纳秒
	 //time.Duration()，强制转成时间间隔形式
	 //time.Sleep(5*time.Second)   同下两句
	 n := 5  
	 time.Sleep(time.Duration(n) * time.Second)  // time.Duration(n)  将整型转成时间间隔的形式
	 fmt.Println("over") 
}
```

#### 几个函数

```go
package main

import (
	"fmt"
	"time"
)

// 几个函数
func main(){
    
	 // add
	 fmt.Println(now)
	 // 传入的时间间隔类型time.Hour
	 t2 := now.Add(time.Hour)  // 加上1个小时
	 fmt.Println(t2)
	 // sub：t2 - now
	 fmt.Println(t2.Sub(now))
	 // equal before after
}
```



### 定时器

使用`time.Tick`来设置定时器，本质上返回的是一个通道（channel）。

```go
func timeTick(){
    ticker := time.Tick(time.Second * 2)   // 间隔2秒钟
    for i := range ticker{  // 返回值是通道
        fmt.Println(i)  
    }
}
```

### 时间格式化与解析

时间类型自带的方法`Format`进行格式化，格式化采用的是`Go`语言诞生的时间：2006年1月2号15点04分（记忆：2006 1234）

> 如果想格式为`12`小时制需要指定`PM`

```GO
package main

import (
	"fmt"
	"time"
)

// 时间格式化和解析

func main(){
    
     // 待解析字符串格式的时间
	 timeStr := "2019/10/17 08:09:36"
	 
     // 1. 拿到时区
	 loc, err := time.LoadLocation("Asia/Shanghai")  // 获取当前时区
	 if err != nil{
	 	fmt.Println(err)
		 return
	 }
	 
    // 2. 根据时区去解析得到的一个字符串格式化的时间
	 timeObj, err := time.Parse("2016/01/02 15:04:05", timeStr)
	 if err != nil{
	 	fmt.Printf("parse timeStr failed, err:%v\n", err)
		 return
	 }
	 fmt.Println(timeObj)
    
	// 解析到具体的某个时区：loc当地时区
	timeObj2, err := time.ParseInLocation("2016/01/02 15:45:05", timeStr, loc)
	if err != nil{
		fmt.Printf("parse timeStr failed, err:%v\n", err)
		return
	}
	fmt.Println(timeObj2)
    
    // 按照指定时区和指定格式解析字符串时间
	timeObj3, err := time.ParseInLocation("2006/01/02 15:04:05", "2019/10/04 14:15:20", loc)
	if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(timeObj3)
}
```

