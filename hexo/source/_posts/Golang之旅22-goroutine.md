---
title: Golang之旅22-goroutine
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-17 00:24:43
password:
copyright: true
summary:
tags:
  - go
  - goroutine
  - 并发
  - 并行
categories:
  - go
---

### 进程和线程

> 进程是程序在操作系统中的一次执行过程，是系统进行资源分配和调度的基本单位。

> 线程是进程的最小单位，是程序执行的最小单元，是进程的一个执行实例。

- 一个进程可以创建和销毁多个线程
- 同一个进程中的多个线程可以并发执行
- 一个程序至少有一个进程，一个进程至少有一个线程

<!--MORE-->

### Go并发编程

#### go协程

> 主线程是一个物理线程，直接作用在CPU上，是重量级的，非常耗费资源。
>
> 其他编程语言的并发机制是基于线程的，开启过多的线程，耗费资源。
>
> go主线程（有程序员称之为线程，甚至是进程）：在一个Go线程上开起多个协程，**协程是轻量级的线程**，是逻辑态的，对资源的耗费小。 

- 有独立的占空间
- 共享程序堆空间
- 调度由用户控制
- 轻松开启上个协程
- 协程是轻量级的线程

**并发和并行**

- 并发：同一个时间段内执行多个任务（一个人同时和多个人聊天）：`1---多`
- 并行：同一时刻执行多个任务（多个人同时和一个人聊天）`多---1`

Go语言并发通过`goroutine`实现，类似线程，用户轻量级的线程，但不是线程

- `goroutine`是Go语言运行时候`runtime`调度实现，用户自己开的线程，属于轻量级的线程，更加地高效 。线程是操作系统层面实现的

- Go提供通道channel在多个`goroutine`进行通信。

- `channel`和`goroutine`是实现`Go`语言并发模式的基础

  

  ![](https://s2.ax1x.com/2019/10/17/Kk2Yfx.md.png)

  
  
  ![](https://s2.ax1x.com/2019/10/17/Kk2g9P.png)

#### MPG模式

`Goroutine`的调度模型是`MPG`模式，解释为

- M：操作系统的主线程，是物理线程；可以在一个CPU或者多个上执行 

- P：协程执行需要的上下文环境

- G：协程



---------

### goroutine实现

- `goroutine`的使用是在调用函数的前面加上关键字`go`
- 一个`goroutine`必定对应一个函数

```go
// demo
package main

import (
	"fmt"
	"strconv"
	"time"
)

// 每隔1秒输出hello world
func test()  {
	for i :=1 ; i < 10; i++{
		fmt.Println("test () hello world" + strconv.Itoa(i))
		time.Sleep(time.Second)
	}
}


func main(){
	go test()  // 开启协程

	for i := 1;i < 10; i++{
		fmt.Println("main() hello golang" + strconv.Itoa(i))
		time.Sleep(time.Second)
	}
}
```

![](https://s2.ax1x.com/2019/10/18/KZYHdf.png)

```go
// 开启单个goroutine

package main

import (
	"fmt"
	"sync"
)

// goroutine demo
var wg sync.WaitGroup

func hello(){
	fmt.Println("hello xiaoming")
	wg.Done()  // 通知wg把计数牌减1， 直至到0结束
}

func main(){    // 开启主goroutine去执行main函数

	wg.Add(1)  // 计数牌+1
	go hello()  // 开启一个goroutine去执行hello函数；go关键字
	fmt.Println("hello main")   // 主goroutine做的事情
	// time.Sleep(time.Second)
	wg.Wait()  // 主goroutine结束，小弟可能没有结束；等待上面的所有执行结束
}
```

```go
// 开启多个goroutine

package main

import (
	"fmt"
	"sync"
)

// goroutine demo
var wg sync.WaitGroup

func hello(i int){
	fmt.Println("hello 小明", i)
	wg.Done()  // 通知wg把计数牌减1
}

func main(){    // 开启主goroutine去执行main函数

	wg.Add(10000)  // 计数牌+1
	for i := 0; i < 10000; i++{
		go hello(i)  // 开启多个goroutine去执行hello函数；go关键字
	}

	fmt.Println("hello main")   // 主goroutine做的事情
	// time.Sleep(time.Second)
	wg.Wait()  // 等待上面的所有执行结束
}
```

```go
// 匿名函数

package main

import (
	"fmt"
	"sync"
)

var wg sync.WaitGroup
func main(){
    wg.Add(10000)
    for i := 0; i<10000;i++{
        // goroutine 中存在闭包的解决
        go func(i int){  // 传递参数i
            fmt.Println("hello", i)
        	wg.Done()
        }(i)   // i显式地传递匿名函数
    }
    fmt.Println("hello main")
    wg.Wait()    // 阻塞，等待所有的小弟干完活之后才结束
}
```



### GOMAXPROCS

```go
package main

import (
	"fmt"
	"runtime"
)

func main()  {
	// 获取当前成CPU的数量
	num := runtime.NumCPU()
	fmt.Println(num)

	// 自行设置使用多个CPU
	runtime.GOMAXPROCS(num - 1)
	fmt.Println("ok")

}
```



`GOMAXPROCS`参数需要使用多少个OS线程来同时执行Go代码，可设置同时执行的最大CPU数，默认是机器上的CPU核心数

- `GO`通过`runtime.GOMAXPROCS()`函数来设置并发执行时占用的CPU逻辑核心数
- `GO1.5`之后默认使用全部的CPU逻辑核心数

```go
// 先执行完a，再执行b
package main

import (
	"fmt"
	"runtime"
	"sync"
)

var wg sync.WaitGroup

func a(){
	for i := 1; i<10; i++{
		fmt.Println("A", i)
	}
	wg.Done()
}

func b(){
	for i := 1; i<10; i++{
		fmt.Println("B", i)
	}
	wg.Done()
}

func main(){
	runtime.GOMAXPROCS(1)  // 只占用一个CPU核心
	wg.Add(2)  // 开启两个goroutine：a b
	go a()
	go b()
	wg.Wait()
	//time.Sleep(time.Second)
    
    // ab同时执行，混合在一起
    runtime.GOMAXPROCS(4)  // 只占用4个CPU核心
	wg.Add(2)  // 开启两个goroutine：a b
	go a()
	go b()
	wg.Wait()
}
```

### 操作系统线程和goroutine关系
- 一个操作系统线程对应用户多个的`goroutine`
- `go`程序可以同时使用多个操作系统线程
- `goroutine`和`OS`线程是多对多的关系，即`m:n`：m个goroutine分配到n个操作系统线程中，比如1000个goroutine分配到8个系统线程中