---
title: python-IO操作
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-19 19:26:28
password:
summary:
tags:
  - python
  - IO操作
copyright: true
categories:
  - python
  - 进阶
---

> 在编程工作中，时常需要对各种文件进行操作。读写文件是最常见的IO编程，Python中内置了读写文件的函数。读写文件是请求系统打开一个文件对象，通常称为文件描述符；然后通过操作系统提供的接口从这个文件对象中读取数据，或者将数据写入文件对象。
> [菜鸟课程](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.runoob.com%2Fpython%2Ffile-methods.html)
> [廖雪峰官方课程—IO编程](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.liaoxuefeng.com%2Fwiki%2F1016959663602400%2F1017606916795776)

<!--MORE-->

### 打开文件

打开文件使用`open()`函数，用读的模式打开返回的是文件对象，它是可迭代的；如果不存在就会报错`IOError`，标准的语法为：

```
open(name[,mode[,buffering]])
```

`open`函数的文件名是必须的，模式和缓冲参数是可选：

```python
f = open('c:\text\a.txt'，'r')  # 用读的模式打开
```

### 关闭文件

文件使用完毕必须关闭，因为文件对象会占用操作系统的资源，调用close()函数关闭文件

```python
f.close()
```

如果读写文件出错，close()函数就不会被调用执行。为了保证文件能够正确关闭文件，使用`try...finally`实现：

```python
try:
    f = open('/path/to/file', 'r')
    print(f.read())
finally:
    if f:
        f.close()
```

### 不同模式

文件打开模式`r、w、a`对应基本的`只读、只写、追加模式`；`b、t、+、U`对应二进制模式、文本模式、读写模式、通用换行符。

| 英文 | 中文      | 文件是否存在 | 是否清空 | 打开位置 |
| ---- | --------- | ------------ | -------- | -------- |
| r    | 只读      | 报错         | 不       | 文件开头 |
| r+   | 读+写     | 报错         | 不       | 文件开头 |
| w    | 只写      | 创建         | 清空     | 文件开头 |
| w+   | 写+读     | 创建         | 清空     | 文件开头 |
| a    | 追加写    | 创建         | 不       | 文件结尾 |
| a+   | 追加写+读 | 创建         | 不       | 文件结尾 |

### 读取文件

读取文件对象有3种方式：`read`、 `readline`、`readlines`，每种方法接受一个变量以限制每次读取的数据量。3种方法都会把每行末尾的`\n`读进来，通过`strip()`进行去除空格。

```python
file = open('/home/ubuntu/PycharmProjects/test.txt', 'w')
print(file)   
file.close()

# 结果显示为文件对象，用于读操作
<_io.TextIOWrapper name='/home/ubuntu/PycharmProjects/test.txt' mode='w' encoding='UTF-8'>
```

#### 以test.txt文件为例

```txt
Hello python
this is a test file

count = 0 
sum = 0
while count < 3:
    sum += 1
    print("hello linux)"
    count += 1
print("hello python")
```

**1、read**
每次读取整个文件，将文件内容放入一个`字符串`中。如果文件较大，用`read(size)`，指定每次最多读取size个字节。

```python
file = open('/home/ubuntu/PycharmProjects/test.txt', 'r')
res1 = file.read()
file.close()
```

**结果**：

```python
Hello python
this is a test file

count = 0 
sum = 0
while count < 3:
    sum += 1
    print("hello linux)"
    count += 1
print("hello python")          # str形式
```

**2、readline**
每次只读取一行数据，相比较于`readlines`慢，读取时候占用内存小，适合于大文件，返回的是`字符串`对象。如果对同一个文件进行多次读取，将会在上次的基础上再进行读取下一行。

```bash
file = open('/home/ubuntu/PycharmProjects/test.txt', 'r')

res2 = file.readline()
res2 = file.readline()
res2 = file.readline()   # 空行也算
res2 = file.readline()
print(res2)
file.close()
```

**结果**

```bash
count = 0    # 结果为第四行的数据，具体看test文件   str形式
```

**3、readlines**
读取全部文件内容，自动将文件分成一个行的列表，可用于`for...in...`迭代获取里面的每个数据。`适合读取配置文件`

```python
file = open('/home/ubuntu/PycharmProjects/test.txt', 'r')
res3 = file.readlines()
print(res3)
file.close()
```

**结果**：

```python
['Hello python\n', 'this is a test file\n', '\n', 'count = 0 \n', 'sum = 0\n', 'while count < 3:\n', '    sum += 1\n', '    print("hello linux)"\n', '    count += 1\n', 'print("hello python")\n']    # list形式
```

去掉空行，并用`for...in...`进行遍历

```python
file = open('/home/ubuntu/PycharmProjects/test.txt', 'r')
res3 = file.readlines()
for i in res3:
    print(i.strip())    # 去掉空格
file.close()
```

**结果**：

```python
Hello python
this is a test file

count = 0
sum = 0
while count < 3:
sum += 1
print("hello linux")
count += 1
print("hello python")
```

### with open(...) as f

Python中引入了`with`语句来自动调用`close()`方法；传入`encoding`和`errors`参数处理编码问题

```python
with open(path,'r',encoding='gbk',errors='ignore') as f: # 写入特定编码的文件，传入encoding和error参数
    print(f.read())    # 不必再调用close方法
```

**题目**：

两个文件中，每个有多行的IP地址，找出两个文件中相同的IP地址

```python
import bisect

with open('test1.txt', 'r') as f1:
    list1 = f1.readlines()
for i in range(0, len(list1)):
    list1[i] = list1[i].strip('\n')
with open('test2.txt', 'r') as f2:
    list2 = f2.readlines()
for i in range(0, len(list2)):
    list2[i] = list2[i].strip('\n')

list2.sort()
length_2 = len(list2)
same_data = []
for i in list1:
    pos = bisect.bisect_left(list2, i)
    if pos < len(list2) and list2[pos] == i:
        same_data.append(i)
same_data = list(set(same_data))
print(same_data)
```