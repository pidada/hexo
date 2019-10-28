---
title: Golang之旅23-通道channel
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-17 10:06:13
password:
summary:
copyright: true
tags:
  - go
  - channel
categories:
  - go
---

### channel

在goroutine并发执行的时候，需要在函数和函数之间进行通信。Go语言并发模式CSP(communicating Sequents Processes)，通过通信共享内存。 

> Go 语言中的通道（channel）是一种特殊的类型。通道像一个传送带或者队列，总是遵循先入先出（First In First Out）的规则，保证收发数据的顺序。 

- `goroutine`是`go`语言并发的执行体，`channel`是它们之间的通道
- `channel`是将一个`goroutine`发送到另一个`goroutine`的通信机制

![](https://s2.ax1x.com/2019/10/17/Kk2g9P.png)

<!--MORE-->

#### channel类型

channel是一种类型，是一种引用类型，需要`make`进行初始化才能够使用，空值是`nil`，关键字是`chan` 。

```go
var 变量名 chan 元素类型

// demo
var ch1 chan int
var ch2 chan bool
var ch3 chan []int   // 切片类型
```

#### 初始化

**通道是一种类型。**声明通道之后必须进行初始化， 缓冲大小是可选的。`Go`语言中需要进行初始化的类型

- 切片`slice`
- 映射`map`
- 通道`channel`

```go
make (chan 元素类型, [缓冲大小])

ch4 := make(chan int)
ch5 := make(chan bool)
```

### channel 操作

#### 三种操作

发送和接收使用`<-`符号

- 发送send
- 接收receive
- 关闭close

```go
ch <- 10   // 将10发送到通道中

x := <- 10  // 从通道中取值，进行接收
<- 10

close(ch)  // 关闭
```

**关于通道的关闭：**

- 只有接收方`goroutine`所有的数据都发送完毕后才会关闭
- 通道是种类型，是可以被垃圾回收机制回收的；通道的关闭不是必须的
- 对一个关闭的通道再发送值就会导致`panic`
- 对一个关闭的通道进行接收会一直获取值直到通道为空。
- 对一个关闭的并且没有值的通道执行接收操作，会得到对应类型的零值。
- 关闭一个已经关闭的通道会导致panic。

#### demo

```go
package main

import (
	"fmt"
)

/*
两个goroutine和两个channel
1. 生成0-100的数字发送到ch1
2. 从ch1中取出数求平方，结果发送到ch2中
*/

//生成0-100的数字发送到ch1
func f1(ch chan int){  // 函数的参数是通道类型
	for i:= 0; i<100;i++{
		ch <- i   // 将i传递给通道ch
	}
	close(ch)
}

// 2. 从ch1中取出数求平方，结果发送到ch2中
func f2(ch1 chan int, ch2 chan int){
	for {
		// 从通道取值方式1
		tmp, ok := <- ch1  // 取出ch1的值
		if !ok{
			break
		}
		ch2 <- tmp*tmp
	}
	close(ch2)
}

func main(){
	ch1 := make(chan int, 100)
	ch2 := make(chan int, 100)

	// 后台的goroutine
	go f1(ch1)
	go f2(ch1, ch2)

	// 从通道取值方式2
	for ret := range ch2{   // 主程序做的事情，取出ch2中的内容进行输出
		fmt.Println(ret)
	}
}
```

### 单向通道

```go
package main

import "fmt"

// 单向通道：多用于函数的参数中使用
func f1(ch chan <- int){  // 只能往ch中发送
	for i:= 0; i<100;i++{
		ch <- i
	}
	close(ch)
}

func f2(ch1 <-chan int, ch2 chan <- int){  // ch1只能取值，ch2只能发送
	for {

		// 从通道取值方式1
		tmp, ok := <- ch1
		if !ok{
			break
		}
		ch2 <- tmp*tmp
	}
	close(ch2)
}

func main(){
	ch1 := make(chan int, 100)
	ch2 := make(chan int, 100)

	go f1(ch1)
	go f2(ch1, ch2)

	// 从通道取值方式2
	for ret := range ch2{   // 主程序做的事情，取出ch2中的内容进行输出
		fmt.Println(ret)
	}
}
```

### channel异常总结

| channel | nil   | 非空（有值）               | 空                 | 满                         | 未满                       |
| ------- | ----- | -------------------------- | ------------------ | -------------------------- | -------------------------- |
| 接收    | 阻塞  | 接收值                     | 阻塞               | 接收值                     | 接收值                     |
| 发送    | 阻塞  | 发送值                     | 发送值             | 阻塞                       | 发送值                     |
| 关闭    | panic | 关闭成功，读完数据返回零值 | 关闭成功，返回零值 | 关闭成功，读完数据返回零值 | 关闭成功，读完数据返回零值 |

关闭已经关闭的`channel`会引起`panic`

------

### work pool机制（goroutine池）

```go
package main

import (
	"fmt"
	"time"
)

// worker pool 机制

func worker(id int, jobs<- chan int, results chan<- int){
	for job := range jobs{
		fmt.Printf("worker:%d start job:%d\n", id, job)
		results <- job*2
		time.Sleep(time.Millisecond * 500)
		fmt.Printf("worker: %d stop job :%d\n", id, job)
	}
}

func main(){
	jobs := make(chan int, 100)
	results := make(chan int, 100)

	// 开启3个goroutine
	for j:=0;j<3;j++{
		go worker(j, jobs, results)
	}

	// 发送5个任务
	for i:=0; i<5; i++{
		jobs <- i
	}
	close(jobs)

	// 输出结果
	for i := 0; i<5; i++{  // 显式地读取三次
		ret := <-results
		fmt.Println(ret)
	}
}
```

### select 多路复用

同时从多个通道中接收数据，可以通过for遍历来实现，但是运行性能差，Go内置select关键字来实现。类似switch语句

- 有多个case分支和一个默认分支
- 每个case对应一个通道（接收或者发送），select等待某个case的通信操作完成，执行该语句

```go
for {
    data, ok := <-ch1
    data, ok := <-ch2
}
```

```go
// demo1
package  main

import "fmt"

func main()  {
	ch := make(chan int, 1)
	for i:=0;i<10;i++{
		select {  // 选择具有随机性
		case x := <-ch:  // 接收
			fmt.Println(x)
		case ch <- i:   // 取值
		default:
			fmt.Println("Do Nothing!")
		}
	}
}

// result
D:\go\src\code\select>select.exe
0
2
4
6
8

---------------
// demo2
package  main

import "fmt"

func main()  {
	ch := make(chan int, 10)  // 容量改成10
	for i:=0;i<10;i++{
		select {  // 选择具有随机性
		case x := <-ch:  // 接收
			fmt.Println(x)
		case ch <- i:   // 取值
		default:
			fmt.Println("Do Nothing!")
		}
	}
}

// result：每次结果不同
D:\go\src\code\select>select.exe
0
2

D:\go\src\code\select>select.exe
0
2
3
4

D:\go\src\code\select>select.exe
0
2
3

D:\go\src\code\select>select.exe
0
2
3
5
8
```

### 资源竞争问题

下面的栗子：表示多个协程同时向`map`空间中写入数据，没有对全局便量加锁，造成了资源竞争的问题`concurrent map writes?`

> 确定是否存在资源竞争问题：在编译程序的时候，加上`-race`参数`go build -race main.go`

![](https://s2.ax1x.com/2019/10/18/KZcU2t.md.png)



```go
package main

import (
	"fmt"
	"time"
)

/*
1. 计算1-200每个数的阶乘
2. 启动多个协程，将结果放入map中
3. map设置成全局变量
*/

var (
	myMap = make(map[int]int, 10)
)

// 计算阶乘n! 将结果放入myMap
func test(n  int){

	res := 1
	for i:=1; i<=n; i++ {
		res *= i
	}

	myMap[n] = res   // concurrent map writes?
}

func main(){

	// 开启协程
	for i := 1; i <= 200; i++{
		go test(i)
	}

	// 休眠10秒钟
	time.Sleep(time.Second * 10)

	// 输出变量结果
	for i, v := range myMap{
		fmt.Printf("map[%d] = %d\n", i, v)
	}
}
```

#### 解决1

> 全局加锁：**全局变量进行加入互斥锁**。当某个协程在进程操作的时候，其他的协程排队等待。执行完毕，解锁，下个协程开始执行。使用`sync`包

![](https://s2.ax1x.com/2019/10/18/KZhEVK.png)

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

/*
1. 计算1-200每个数的阶乘
2. 启动多个协程，将结果放入map中
3. map设置成全局变量
*/

var (
	myMap = make(map[int]int, 10)
	// lock 是一个全局互斥锁
	// sync：synchornized 同步
	// Mutex：互斥（排他锁），是一个结构体，包含lock和unlock两个方法
	lock sync.Mutex
)

// 计算阶乘n! 将结果放入myMap
func test(n  int){

	res := 1
	for i:=1; i<=n; i++ {
		res *= i
	}

	lock.Lock()  // 加锁
	myMap[n] = res   // concurrent map writes?
	lock.Unlock()  // 解锁
}

func main(){

	// 开启协程
	for i := 1; i <= 200; i++{
		go test(i)
	}

	// 休眠10秒钟：执行完所有的协程
	time.Sleep(time.Second * 10)

	// 输出变量结果
	// 这里加锁的原因：10秒所有的协程执行完毕；在实际的执行过程中for语句中可能出现资源竞争问题
	// 10 秒钟执行完所有协程，但是主线程并不知道，底层仍可能出现资源竞争问题，因此也需要加入互斥锁
	lock.Lock()
	for i, v := range myMap{
		fmt.Printf("map[%d] = %d\n", i, v)
	}
	lock.Unlock()
}
```

#### 解决2

