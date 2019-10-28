---
title: Golang之旅20-文件操作
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-09 11:42:19
password:
summary:
copyright: true
tags:
  - go
  - 文件操作
categories:
  - go
---

文件操作的包是`os`，主要的方法是`Create、Open、OpenFile、Read、ReadAt`（定位读取）等

### 文件读取

- `read()`
- `bufio`读取，按照行读取
- `ioutil`读取，快速

### 文件写入

- `write()`
- `bufio`
- `ioutil`

<!--MORE-->

```go
// ioutil 读取和写入文件
package main

import (
    "fmt"
    "io/ioutil"
)

func main() {
      // 读取整个文件
    content, err := ioutil.ReadFile("file")
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(string(content))
    
    // 写入文件，参数：写入的文件名、写入的内容
    err:= ioutil.WriteFile("test.txt", []byte("My Name is GoLang"), 0755)    
    if err != nil {
        return
    }
}

```

### 栗子

```go
package main

import (
	"bufio"
	"fmt"
	"io"
	"io/ioutil"
	"os"
)

// 文件操作

// 单次读取文件
func readFromFile(){
	fileObj, err := os.Open("./test.txt")   // 参数是文件路径
	if err != nil{
		fmt.Println("open the file,err:%v\n",err)
		return
	}

	defer fileObj.Close()    // 关闭文件
	// 读取文件
	var tmp = make([]byte, 128)   // 读取字节数量128
	n, err := fileObj.Read(tmp)   // 读取文件用read
	if err != nil{
		fmt.Printf("read from file failed, err:%v\n", err)
		return   //返回的就是err
	}
	fmt.Printf("read %d bytes from file.\n", n)  // 读取字节数
	fmt.Println(string(tmp))   // tmp是字节，需要转成字符串格式
}

// 读取大文件，通过for循环实现
func readAll(){
	fileObj, err := os.Open("./test.txt")   //  文件路径
	if err!= nil{
		fmt.Println("open the file, err:%v\n",err)
		return
	}

	defer fileObj.Close()    // 关闭文件

	// 循环读取
	for {
		var tmp = make([]byte, 64)  // 每次读取128个字节
		n, err := fileObj.Read(tmp)
		// EOF: end of file ，读取到文件末尾打印出来
		if err == io.EOF{
			// 把当前读了多少个字节打印出来
			fmt.Println(string(tmp[:n]))
			return
		}

		if err != nil{
			fmt.Println("read from file failed, err%v\n", err)
			return
		}
		fmt.Printf("read %d bytes from file.\n", n)
		fmt.Println(string(tmp[:n]))
	}
}

// bufio 在file的基础上封装了一层API，支持更多的功能；一行行读取文件
func readByBufio(){
	fileObj, err := os.Open("./test.txt")
	if err != nil{
		fmt.Println("open file failed, err:%v\n", err)
		return
	}
	defer fileObj.Close()

	reader := bufio.NewReader(fileObj)  // 参数是文件，得到的是reader实例对象

	// 循环读取
	for{
		line, err := reader.ReadString('\n')  //  单引号：字符， 双引号：字符串
		if err == io.EOF{
			fmt.Print(line)
			return
		}
		if err != nil{
			fmt.Printf("read file by failed, err:%v\n",err)
			return
		}
		fmt.Print(line)
	}
}

// ioutil  快速读取大文件
func readByIoutil(){
	content, err := ioutil.ReadFile("./test.txt")
	if err != nil{
		fmt.Printf("read bu ioutil failed, err:%v\n", err)
		return
	}
	fmt.Println(string(content))
}


// write

// 文件写入操作 openfile
func write(){
	fileObj, err := os.OpenFile("./test.txt", os.O_CREATE|os.O_WRONLY|os.O_APPEND, 0644)   // 标志位 可以有多个
	if err != nil{
		fmt.Printf("open file failed,err:%v\n",err)
		return
	}

	defer fileObj.Close()

	str := "小明"
	fileObj.Write([]byte(str))   // 写入字节byte
	fileObj.WriteString("hello 沙河")  // 写入字符串string
}

func writeByBufio(){
	fileObj, err := os.OpenFile("testOne.txt",os.O_CREATE|os.O_APPEND|os.O_TRUNC,0644)
	if err != nil{
		fmt.Printf("open file failed,err:%v\n", err)
		return
	}
	defer fileObj.Close()
	
	writer := bufio.NewWriter(fileObj)
	writer.WriteString("小明和小红")   // 将内容写入缓冲区
	writer.Flush()     // 调用操作系统的接口，将缓冲区的内容写入磁盘中
}

func writeByIoutil(){
	str := "我命由我不由天"
	err := ioutil.WriteFile("testTwo.txt", []byte(str), 0644)
	if err != nil{
		fmt.Printf("write file failed,err:%v\n",err)
		return
	}
}

func main() {
	readFromFile()
	readAll()

	// 不循环的话，只读取第一行
	readByBufio()
	readByIoutil()

	write()
	writeByBufio()
	writeByIoutil()
}
```
