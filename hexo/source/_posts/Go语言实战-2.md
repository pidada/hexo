---
title: Go语言实战-2
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-20 15:37:05
password:
summary:
copyright: true
tags:
  - go
categories:
  - go
---

### Go

Go语言解决的问题：C和C++执行速度快，Python擅长快速开发，Go兼具二者特点。

- 编译速度快
- 内置并发机制
- 自带垃圾回收机制
- 用户不用自己管理内存
- Go语言使用接口作为代码复用的基础模块



### goroutine

-  goroutine像线程，占用内存少；使用一个线程来执行多个goroutine
- Go语言会自动在配置的一组逻辑处理器上调度执行goroutine



<!-- MORE -->

### chanel

- 通道chanel内置的数据结构，让用户在不同的goroutine之间同步发送具有类型的消息
- 通道常用于几个运行的goroutine之间发送数据，保证goroutine之间进行安全通信

![](https://s2.ax1x.com/2019/09/20/njnMnO.png)

### 程序架构

主程序分成多个不同步骤，在多个不同过的goroutine中运行。

![](https://s2.ax1x.com/2019/09/20/njMrUe.png)

### main

- `main`函数保存在名为`main`的包里；Go`语言中每个代码文件都是一个包
- 一个包定义一组编译过的代码。包的命名类似命名空间
- Go 编译器不允许声明导入某个包却不使用。import 导入包的时候，如果有下划线`_`，表示不使用包里的标识符
- 程序中每个代码文件里的init 函数都会在main 函数执行前调用。
- 标识符：
  - 公开的标识符：大写字母开头，能够直接访问
  - 非公开的标识符：小写字母开头，不能直接访问

### 初始值问题

在Go语言中，所有变量都被初始化为其零值

- 数值类型：0

- 字符串类型：空字符串

- 布尔类型：false

- 指针：nil

- 引用类型：所引用的底层数据结构会被初始化为对应的零值。

  **被声明为其零值的引用类型的变量，会返回nil 作为其值。**