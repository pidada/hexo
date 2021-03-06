---
title: Golang之旅24-socket编程
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-21 15:16:54
password:
summary:
tags:
  - go
  - socket
  - 网络编程
copyright: true
categories:
  - go
---

### TCP/IP协议

> 通常使用的网络是在`TCP/IP`协议的基础上进行运作的。`HTTP`协议只是属于`TCP/IP`协议内部的一个子集。
>
> `TCP/IP`协议是`IP`协议的通信过程中，使用到的协议族的总称。

![](https://s2.ax1x.com/2019/10/21/KlcoQg.png)

`TCP/IP`协议族按层次分别表示为：

- 应用层
- 传输层
- 网络层
- 链路层

**越往上越靠近用户，越往下越接近底层**

![](https://s2.ax1x.com/2019/10/22/K3H6gJ.md.png)

<!--MORE-->

------

#### 链路层（网络接口层）

- **物理层**：处理网络连接的硬件部分，包含：控制操作系统、设备驱动、网卡和光纤等，把电脑连接起来的物理手段
- **数据链路层**： 它在**物理层**的上方，确定了物理层传输的0和1的分组方式及代表的意义。以太网协议统一了电信号分组方式。
- **广播机制**： 向本网络内所有计算机都发送，让每台计算机读取这个包的`标头`，找到接收方的`MAC`地址，然后与自身的`MAC`地址相比较；如果两者相同，就接受这个包，做进一步处理，否则丢弃。这种发送方式就叫做`广播broadcasting` 



#### 网络层

主要是用来处理网络上流动的数据包。该层规定了通过怎么样的传输路径到达对方，选择合适的传输路径。主要是包含`IP`协议用来处理各种数据包给对方。保证传输准确性的是`IP`协议和`Mac`地址。

链路层中的`广播机制`有一定的限制：必须保证双方在同一个子网络中，否则只能通过`路由`的方式发送数据。

 网络层的作用是引进一套新的地址，使得我们能够区分不同的计算机是否属于同一个子网络。这套地址就叫做”网络地址”，简称”网址”。 那么，每台计算机出现了两个地址：Mac地址和网络地址。

- Mac地址是绑定在网卡上：将数据包发送到子网络的目标网卡
- 网络地址在网络管理员分配的：确定计算机所在的子网络

#### 传输层

提供处于网络连接中的两台计算机之间的**数据传输**。主要是有两个重要协议：传输控制协议`TCP`和用户数据报协议`UDP`

##### 产生

> 有了`MAC`地址和`IP`地址，我们已经可以在互联网上任意两台主机上建立通信。但问题是同一台主机上会有许多程序都需要用网络收发数据，比如`QQ`和浏览器这两个程序都需要连接互联网并收发数据，我们如何区分某个数据包到底是归哪个程序的呢？

我们还需要一个参数，表示这个数据包到底供哪个程序（进程）使用。这个参数就叫做”端口”（port），它其实是每一个使用网卡的程序的编号。每个数据包都发到主机的特定端口，所以不同的程序就能取到自己所需要的数据。

-  0~65535，16个二进制组成
- 系统端口：0-1023；用户使用的端口1024开始

##### UDP协议

我们必须在数据包中加入端口信息，于是产生了`UDP`协议：在数据前面加上端口号。包含`标头`和`数据`两个部分，总长度不超多`65535`个字节。UDP协议比较简单，实现容易，但是可靠性差，一旦数据发出，无法知道对方是否收到。

- 标头：发出端口号和接收端口号，8个字节
- 数据：具体的数据内容

##### TCP协议

> 为了克服`UDP`协议的缺点，`TCP`协议诞生了。` TCP`协议能够确保数据不会遗失。它的缺点是过程复杂、实现困难、消耗较多的资源。

`TCP`数据包没有长度限制，理论上可以无限长，但是为了保证网络的效率，通常`TCP`数据包的长度不会超过`IP`数据包的长度，以确保单个`TCP`数据包不必再分割。 

#### 应用层

应用层接收到传输层传来的数据，接下来需要对数据进行解包。 应用层的作用就是规定应用程序使用的数据格式，例如`TCP`协议之上常见的`Email、HTTP、FTP`等协议，这些协议就组成了互联网协议的应用层。 它决定了向用户提供应用服务时的**通信活动**，比如：`FTP`（文件传输协议）、`DNS`（域名系统）服务，`HTTP`也处于该层



![](https://s2.ax1x.com/2019/10/22/K3UR0K.md.png)

#### 术语

- **帧Frame：**一组电信号构成一个数据包，每个帧包含标头`head`和数据`data`两个部分，长度在`64~1518`字节之间；如果数据太多，可以多帧发送。

  - **标头head**：包含数据的说明项，例如发送者、接收者、数据类型等，**固定为18个字节**
  - **数据data**：数据包的具体内容，长度在`46-1500`字节之间

- **MAC地址**：连接网络的所有接口都必须有网卡接口。数据包从一块网卡传送到另一块网卡。MAC地址（网卡的地址）：数据包的发送地址和接收地址。每块网卡具有唯一的`MAC`地址

  - 长度是`48`位二进制位
  - 通常是`12`个十六进制数表示，`6位厂商编号+6位网卡流水号`

  > 由于实际通信中，双方在同一个`LAN`的情况很少，经过多台计算机和网络设备进行中转才能到达对方。
  >
  > 采用`ARP(address resolution protocol)`协议来解析`IP`地址，根据通信方的`IP`地址就能反查出对方的`MAC`地址。

  ![](https://s2.ax1x.com/2019/10/22/K30vbq.png)

- IP地址：规定网络地址的协议叫做`IP`协议，`IP`协议所定义的地址叫做`IP`地址。现在使用的是IPV4：32位二进制组成，范围是`0.0.0.0~255.255.255.255`

  > 根据`IP`协议发送的数据，就叫做`IP`数据包。`IP`数据包也分为”标头”和”数据”两个部分：”标头”部分主要包括版本、长度、`IP`地址等信息，”数据”部分则是`IP`数据包的具体内容。
  >
  > 
  >
  > `IP`数据包的”标头”部分的长度为20到60字节，整个数据包的总长度最大为65535字节。 



### socket

> Socket是BSD UNIX的进程通信机制，通常也称作”套接字”，用于描述IP地址和端口，是一个通信链的句柄。Socket可以理解为TCP/IP网络的API，它定义了许多函数或例程，程序员可以用它们来开发TCP/IP网络上的应用程序 

#### socket图解

 `Socket`是应用层与` TCP/IP(Transmission Control Protocol/Internet Protocol)  `协议族通信的中间软件抽象层。在设计模式中，`Socket`把复杂的`TCP/IP`协议族隐藏在`Socket`后面，对用户来说只需要调用`Socket`规定的相关函数，让`Socket`去组织符合指定的协议数据然后进行通信。 

![](https://s2.ax1x.com/2019/10/22/K3qBpF.md.png)

#### socket编程

 `socket`是基于`C/S`架构的，也就是说进行`socket`网络编程，通常需要编写两个`go`文件，一个服务端，一个客户端。 

![](https://s2.ax1x.com/2019/10/22/K3bRzQ.png)

#### TCP服务端

 `TCP`协议是一种面向连接（连接导向）的、可靠的、基于字节流的传输层（Transport layer）通信协议，因为是面向连接的协议，数据像水流一样传输，会存在`黏包`问题。 服务端处理流程：

- 监听端口
- 接收客户端请求建立连接
- 创建goroutine处理连接

```go
package main

import (
	"bufio"
	"fmt"
	"net"
)

// TCP server端

// 处理函数
func process(conn net.Conn) {
	defer conn.Close() // 关闭连接
	for {
		reader := bufio.NewReader(conn)
		var buf [128]byte
		n, err := reader.Read(buf[:]) // 读取数据
		if err != nil {
			fmt.Printf("read from client failed, err:", err)
			break
		}
		recvStr := string(buf[:n])
		fmt.Println("收到client端发来的数据：", recvStr)
		conn.Write([]byte(recvStr)) // 发送数据
	}
}

func main() {
	listen, err := net.Listen("tcp", "127.0.0.1:20000")   // 指定协议和ip地址及端口
	if err != nil {
		fmt.Println("listen failed, err:", err)
		return
	}
	for {
		conn, err := listen.Accept() // 建立连接
		if err != nil {
			fmt.Println("accept failed, err:", err)
			continue
		}
		go process(conn) // 启动一个goroutine处理连接
	}
}
```



#### TCP客户端

 一个TCP客户端进行TCP通信的流程如下： 

- 建立与服务端的连接
- 进行数据的收发
- 关闭连接

```go
package main

import (
	"bufio"
	"fmt"
	"net"
)

// 客户端
func main() {
    // 与服务端建立连接
	conn, err := net.Dial("tcp", "127.0.0.1:20000")
	if err != nil {
		fmt.Println("err :", err)
		return
	}
	defer conn.Close() // 关闭连接
    
    // 利用该连接进行数据的发送和接收
	inputReader := bufio.NewReader(os.Stdin)
	for {
		input, _ := inputReader.ReadString('\n') // 读取用户输入
		inputInfo := strings.Trim(input, "\r\n")
		if strings.ToUpper(inputInfo) == "Q" { // 如果输入q就退出
			return
		}
		_, err = conn.Write([]byte(inputInfo)) // 发送数据
		if err != nil {
			return
		}
		buf := [512]byte{}
		n, err := conn.Read(buf[:])
		if err != nil {
			fmt.Println("recv failed, err:", err)
			return
		}
		fmt.Println(string(buf[:n]))
	}
}
```

### 粘包

#### 为什么产生粘包

> `TCP`数据传递模式是流模式，在保持长连接的时候可以进行多次的收和发 。粘包可以发生在发送端和接收端

- 由`Nagle`算法造成发送端的粘包：`Nagle`算法是一种改善网络传输效率的算法。当我们提交一段数据给TCP发送时，**TCP并不立刻发送此段数据**，而是等待一小段时间，看看在等待期间是否还有要发送的数据；若有则会一次把这两段数据发送出去。
- 接收端接收不及时造成的接收端粘包：TCP会把接收到的数据存在自己的缓冲区中，然后通知应用层取数据。当应用层由于某些原因不能及时的把TCP的数据取出来，就会造成TCP缓冲区中存放了几段数据。



#### 解决办法

通过接收方对数据包进行**封包和解包**的操作。

- 封包：给一段数据加上包头，数据包就包含包头和包体两个部分。
- 包头的长度是固定的， 并且它存储了包体的长度。根据包头长度固定以及包头中含有包体长度的变量就能正确的拆分出一个完整的数据包。 

