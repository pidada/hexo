---
title: Golang之旅0—安装、配置和命令
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-15 20:39:30
password:
summary:
copyright: true
tags: 
  - go
  - 分布式
  - 高性能
  - 高并发
categories: 
  - go
---

### GO简介

> 开始跟着七米老师学习`Go`语言啦！`Go`代表的是一种趋势，一种未来！`Go`于`2009`年发布，当时多核处理器已经上市。`Go`语言在多核并发上拥有原生的设计优势，`Go`语言从底层原生支持并发，无须第三方库、开发者的编程技巧和开发经验。

![](https://www.runoob.com/wp-content/uploads/2015/06/go128.png)

`Go`语言的并发是基于 `goroutine` 的，`goroutine` 类似于线程，但并非线程。可以将 `goroutine` 理解为一种虚拟线程。Go 语言运行时会参与调度 `goroutine`，并将 `goroutine` 合理地分配到每个 CPU 中，最大限度地使用CPU性能。开启一个`goroutine`的消耗非常小（大约2KB的内存），你可以轻松创建数百万个`goroutine`。goroutine`的特点：

1. `goroutine`具有可增长的分段堆栈。这意味着它们只在需要时才会使用更多内存。
2. `goroutine`的启动时间比线程快。
3. `goroutine`原生支持利用channel安全地进行通信。
4. `goroutine`共享数据结构时无需使用互斥锁。

[GO1](https://www.mauisportsclub.com/posts/Go/install_go_dev/)



<!-- MORE -->

### 安装

- Windows系统下，直接`next`安装
- Linux系统下
  - 下载：wget https://dl.google.com/go/go1.13.linux-amd64.tar.gz
  - 创建目录：mkdir /usr/local/go
  - 解压安装：sudo tar -C /usr/local/go .gz
  - 配置环境变量：`vim /etc/profile`，添加如下内容：
    - `export GOROOT=/usr/local/go`
    - `export PATH=\$PATH:\$GOROOT/bin`
  - 上述过程重启生效
  - 如果是修改：`vim $HOME/.profile`，进行`source $HOME/.profile`
  - 检查版本：`go version`
  
  

### 配置GOPATH

> `GOPATH`是一个环境变量，用来表示GO语言代码保存的位置。win10为例，在高级系统设置的环境变量中

- 用户变量下，设置`GOPATH`，添加代码存放位置，比如：`d:\go`

- 添加`PATH`，`GO`语言安装的`bin`路径

- 系统环境变量的`PATH`中也要添加`GO`的`bin`安装路径。
- 重启`cmd`，检测命令：`go env`

**必须将`GOROOT`和`GOPATH`下的`bin`目录都添加到环境变量中**



### GO项目架构

#### 个人开发

> GO源码都在`GOPATH`的`src`目录下：

- bin：存放编译后的二进制文件
- pkg：存放编译后的库文件
- src：存放源码文件，各种项目文件

![](https://www.mauisportsclub.com/images/Go/install_go_dev/1550805203054.png)

#### 流行的项目结构

> Go语言也是通过包组织代码文件，通过别人的包来发布自己的包，防止包名字的冲突，采用`顶级域名`的方式，作为包的前缀。流行的方式是通过`Github`用户来区分不同的包名

![](https://www.mauisportsclub.com/images/Go/install_go_dev/1550805044488.png)

#### 公司级架构

![](https://www.mauisportsclub.com/images/Go/install_go_dev/1550806101915.png)



### GO语言编辑器

使用最多的是`VS code`和`Goland`。七米老师推荐的是`VS code`，我使用的是`Goland`

### Hello word

#### 代码

```go
package main
//声明当前库文件是可执行程序,非普通库

import "fmt"
//内置的fmt

func main(){
	//声明入口函数
	fmt.Println("Hello  World!")
}
```

#### 编译和执行

`Goland`界面在终端中进行编译和执行：

![](https://s2.ax1x.com/2019/09/16/nR3TDf.png)

- 执行`go build`，在`hello`目录下生成了`hello.exe`可执行文件
- 执行`hello.exe`，运行第一个代码
- 执行`go build -o demo.exe`，生成指定名字`demo`的可执行文件，同样运行

![](https://s2.ax1x.com/2019/09/16/nRGMyq.png)

- 通过`go install`，在`bin`目录下生成可执行文件，在整个系统的任何目录均可执行。

![](https://s2.ax1x.com/2019/09/16/nRYia8.png)

#### 通过`github`自命名运行

![](https://s2.ax1x.com/2019/09/16/nRUmMd.png)

![](https://s2.ax1x.com/2019/09/16/nRUnsA.png)

#### 跨平台编译

```go
SET CGO_ENABLED=0   //终端执行命令，禁用CGO；CGO默认是不允许跨平台
SET GOOS=linux     //目标操作平台是linux
SET GOARCH=amd64  //目标处理架构
go build  //编译成二进制文件
SET GOOS=windows     //操作系统还回去
```

![](https://s2.ax1x.com/2019/09/16/nRUXwt.png)



### `GO`常用命令

```go
go build  //编译
go build -o demo.exe  //生成指定的编译文件
demo.exe  //运行可执行文件
go install  //在bin目录下生成可执行文件，之后在系统的任何目录下均可运行可执行文件
go run main.go  //运行go的脚本文件

//关于跨平台编译
SET CGO_ENABLED=0   
SET GOOS=linux     
SET GOARCH=amd64 
go build 
SET GOOS=windows     
```

> 人生苦短，Let`s GO!

