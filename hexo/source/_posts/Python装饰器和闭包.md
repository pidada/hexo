---
title: Python装饰器和闭包
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-15 09:55:40
password:
summary:
tags:
  - python
  - 装饰器
  - 闭包
copyright: true
categories:
  - python
  - 进阶
---

### 闭包

> `python`是一种面向对象的编程语言，在`Python`中一切皆是对象。`函数也是对象`。变量拥有的属性，函数同样拥有。因此在函数内部创建一个函数的行为是完全合法的。这种函数称为`嵌套函数`或者`内嵌函数`。**闭包称为词法闭包或者函数闭包**，是引用了自由变量的函数 ，两个特点

- 定义在另一个函数里面，嵌套作用
-  内部函数对外部函数**作用域**里面**变量**的引用 
- 函数内部的变量或者函数，只有函数执行期间有生命周期

<!--MORE-->

```python
def func():   # 外部函数
    a = 1   # 外部函数作用域里的变量
    print("this is func()")
    def bar(num):   # 闭包函数
        print("this is bar()")
        print(num + a)
    return bar   # 返回函数

var = func()   # 运行外部函数，则内部函数同时被创建
# var == bar  func返回的就是bar函数，直接用var接收
var(3)  # 4
# print(a)  直接报错，在执行函数之后，变量已经结束了生命周期
```

![](https://s2.ax1x.com/2019/10/15/K9W23F.png)

### 装饰器@

- 写了一段代码，想查看运行了多长时间。

- 可能需要修改源代码，给它加上新的计时功能`time()`函数。

- 加上计时功能之前，你至少需要阅读源代码，理清它的逻辑，才敢加上新的功能。

- 如果代码比较少的话，还是`ok`；如果代码很庞大，通读下来岂不是很费时间呢？

- 有没有一种方法：无论源代码多长，不修改源代码，就可以实现加上其他功能的这个需求呢？`The answer is :YES!`**装饰器由此而来**

  

  [[译] Python装饰器Part I：装饰器简介]( https://www.jianshu.com/p/d03e4dfc5a78 )

```python
# demo

def func1(func):
    def func2():
        print("hello python")
        return func()
    return func2

# 1. func1(bar)() : 接收被装饰的函数作为参数，并且执行一次调用
# 2. func1 执行完毕返回的就是func2（return语句）
# 3. 执行func2函数，返回了func也就是bar函数调用

@func1   # 执行bar之前先执行func1函数
def bar():
    print("hello world")
    
bar()
```

```python
def func1(func):  # 装饰器函数
    def func2(x,y):
        print(x,y)
        x += 5
        y += 5
        return func(x,y)
    return func2

@func1
def mysum(a,b):  # 被装饰的函数带参数
    print(a+b)   
mysum(1,2)

# 执行过程
1. 执行mysum()函数，首先mysum当做参数func传入func1中
2. func1返回了func2，接着执行func2
3. 执行func2过程：print(x,y)---> x +=5---> X=6,y=7--->返回func(x,y)，即mysum
```



> 装饰器本身就是个函数，将被装饰的类或者函数当做参数传递给装饰器函数。它的功能是用来扩展原来函数功能的一种函数。
>
> 它经常用于有切面需求的场景，比如：`插入日志、性能测试、事务处理、缓存、权限校验`等场景。装饰器的特殊之处在于：它的返回值也是一个函数。

- 装饰器本身是函数

- 返回值也是函数

- 装饰器是一种特殊的[闭包](https://www.jianshu.com/writer#/notebooks/10675701/notes/49631435)

  

一个栗子：

```python 
def foo(fun):   # foo函数的参数是个被装饰的函数对象fun
    def wrap():
        print("start")
        fun()  # 调用执行fun函数
        print("end")
        print(fun.__name__)
    return wrap

def bar():
    print("I am in bar()")
    
f = foo(bar)    # 给foo函数一个参数bar；向foo()传递了函数对象bar()
f()

# 结果
start
I am in bar()
end
bar
```

加上装饰器：

```python
def foo(fun):   # foo函数的参数是个函数fun
    def wrap():
        print("start")
        fun()
        print("end")
        print(fun.__name__)
    return wrap

@foo   # 增加的内容：称为语法糖；增减程序的可读性，减少代码出错的机会。
def bar():  # 在执行bar函数的时候，先执行foo函数的内容
    print("I am in bar()")
    
bar()    
```

### 判断质数

> 判断某个数是否是质数，并输出2-10000之间的所有质数 

```python
import time

# 定义装饰器函数display_time
def display_time(func):    # 找到display_time()装饰器下面的count_prime_nums()函数
    def wrapper(*args):      # 不确定参数的个数，通过*args以元组的方式统一收集；参数是count_prime_nums函数中的参数
        t1 = time.time()        # 第一步：执行获取时间t1 
        result = func(*args)  # 第二步：执行装饰器里面的函数，将count保存在result中；同时打印时间差
        t2 = time.time()    # 第三步：执行获取时间t2
        print("total time:{:.4} s".format(t2 - t1))
        return result     # 第四步：将result进行返回
    return wrapper

def is_prime(num):   # 判断一个数是否是质数
    if num < 2:   # 即num为1
        return False  #返回结果1不是质数
    elif num == 2:  # 判断num为2
        return True   # 判断2是质数
    else:
        for i in range(2, num):   # 判断2到num之间，是否还有num的因数
            if num % i == 0:   # 如果存在某个数i使的num除以i结果的余数为0，则i是num的因数，num就不是质数。
                return False  # 结果判断不是质数
        return True

# 装饰器函数需要执行的部分
@display_time
def count_prime_nums(maxnum):
    count = 0
    for i in range(2,  maxnum):
        if is_prime(i):  # 如果某个数是质数，默认条件为真True
            count += 1   
    return count     # 返回质数的个数
```

**加强理解**

- 整个代码中有三个函数，分别实现三个功能
  - 1、`display_time()` ：为装饰器函数
  - 2、`is_prime()`：为判断是否为质数的函数
  - 3、`count_prime_nums()`：为统计质数个数的函数
- 注意函数1中的执行顺序：先计算`time1`，再执行装饰器里面的函数，当遇到func，执行函数3，最后计算`time2`
- 当函数3中有多个参数，且不确定个数的时候，通过`*args`以元组的形式进行收集
- 在执行`count_prime_nums`函数之前，先执行`display_time()`函数