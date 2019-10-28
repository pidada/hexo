---
title: 顺序表和链表
date: 2019-09-09 18:44:41
tags: #数据结构 #Python #算法
categories: 
  - 数据结构
copyright: true
---

### 引言

> 题目：a\*\*2 + b\*\*2 = c\**2，a+b+c=1000，求解a，b，c

### 关于`jupyter`中的魔法命令

- `timeit`：自动运行多次取均值
  
  - `%timeit` 运行单条语句
  - `%%timeit` 运行多条语句
- `time`：只运行一次
  - `%time` 运行单条语句
  
  - `%%time` 运行多条语句
  
    

```python
%%time
import time 

start = time.time()
for a in range(0, 1001):
    for b in range(0, 1001):
        for c in range(0, 1001):
            if a + b + c == 1000 and a**2 + b**2 == c**2:
                print("a:{0}，b:{1}, c:{2}".format(a, b, c))
end = time.time()
print(end - start)
```



<!-- MORE -->

------

### 算法五大特征

- 输入

- 输出

- 有穷性

- 确定性

- 可行性

  

```python
%%time
import time 

start = time.time()
for a in range(0, 1001):
    for b in range(0, 1001):
        # 优化之处
        c = 1000 - a - b
        if  a**2 + b**2 == c**2:
            print("a:{0}，b:{1}, c:{2}".format(a, b, c))
end = time.time()
print(end - start)
```



### 时间复杂度O(n)

#### 基础

通过运算的时间来进行度量，用`n`来进行表示。有几种情况：

- 最优时间复杂度
- 最坏时间复杂度
- 平均复杂度

> 常见的计算规则：

- 顺序：累加结构计算
- 循环：乘法结构计算
- 分支：取最大值

一般取的是最坏时间复杂度，考虑最坏的情况，同时忽略常数项

#### 常见的时间复杂度

消耗的时间大小关系：$O(1)< O(logn) < O(n) < O(nlogn) < O(n^2) < O(n^3) < O(2^n)<O(n!)<O(n^n)$

#### `timeit`模块

主要功能是进行测试

```python
# 生成列表
li1 = [1, 2]
li2 = [3, 4]
li = li1 + li2
```

```python
li
```

```python
li = [i for i in range(10)]
li
```

```python
li = list(range(10))
li
```

```python
li = []
for i in range(10):
    li.append(i)
li
```

```python
# 比较几种列表生成式
from timeit import Timer

def t1():
    li = []
    for i in range(10000):
        li.append(i)
        
def t2():
    li = []
    for i in range(10000):
        # li = li + [i]
        li += [i]
        
def t3():
    li = [i for i in range(10000)]
    
def t4():
    li = []
    for i in range(10000):
        li.insert(0, i)
    
time1 = Timer("t1()", "from __main__ import t1")
print("append:", time1.timeit(10000))

time2 = Timer("t2()", "from __main__ import t2")
print("+:", time2.timeit(10000))

time3 = Timer("t3()", "from __main__ import t3")
print("[]:", time3.timeit(10000))

time4 = Timer("t4()", "from __main__ import t4")
print("insert:", time4.timeit(10000))
```

### 列表和字典的时间复杂度

#### 列表

| 操作     | 复杂度   |
| -------- | -------- |
| index [] | 0(1)     |
| append   | 0(1)     |
| pop()    | 0(1)     |
| pop(i)   | O(n)     |
| insert() | O(n)     |
| sort()   | O(nlogn) |



#### 字典

| 操作                              | 复杂度 |
| --------------------------------- | ------ |
| copy()                            | O(n)   |
| get,set,delete,contains,iteration | O(1)   |

### 数据结构

数据结构是指数据对象中数据元素之间的关系，在`Python`中内置的数据结构，比如列表、字典、元组等，扩展结构：栈、队列等。
`程序 = 数据结构 + 算法`



常见的操作：

- 插入
- 删除
- 修改
- 查找
- 排序

### 顺序表

#### 存储方式

数据本身是连续存储，每个元素占据固定大小的单元，元素下标是逻辑地址，包含：数据区+表头信息，两种存储方式：

- 一体式结构：表头信息和数据区连在一起，表头区包含容量和元素个数
- 分离式结构：表头信息和数据区分开存放，通过表头区的地址单元去指向数据区

#### 扩充策略

- 每次固定的扩充数目：线性扩充，节省空间。扩充频繁，操作频繁
- 每次扩充容量加倍：减少扩充执行次数，浪费资源。以空间换取时间



### 链表

#### 链表由来

> 顺序表的构建需要预先知道数据大小来申请连续的存储空间；再进行扩充的时候需要进行数据的迁移，很不方便。链表能够充分地利用计算机的存储空间，实现灵活的内存动态管理。

线性表包含顺序表和链表。在链表中，元素与元素之间通过链接构造起来的一系列存储结构中，**每个节点（存储单元）中存放下一个节点的位置信息。**。节点中包含：`数据取 + 链接区（指针区）`。最后一个没有指针区

#### 单向链表

> 单向链表包含数据区和链接区。链接指向下一个链接表中的节点。最后一个节点指向空值(一竖一横表示)。

主要的操作是：

- is_empty()
- length()
- travel()
- add(item)
- append() 
- insert()
- remove()
- search()

在Python的$a=10$中，a是个地址，保存的不是10，地址指向10。

### 顺序表和链表对比

#### 顺序表

- 随机读取数据
- 查找很快，耗时主要是在拷贝和覆盖
- 存储空间必须是连续的

#### 链表

- 增加了节点地指针区域，空间开销大，对存储空间的使用更加灵活
- 耗时主要是体现在：遍历查找
- 只记录头结点，如果想找到其他节点，必须通过遍历的方式去寻找
- 存储空间不是连续的：数据区+指针区，对离散空间能够充分利用

#### 时间复杂度对比

| 操作     | 链表 | 顺序表 |
| -------- | ---- | ------ |
| 访问元素 | O(n) | O(1)   |
| 头部     | O(1) | O(n)   |
| 尾部     | O(n) | O(1)   |
| 中间     | O(n) | O(n)   |

**为什么链表的时间复杂度增加，还要使用链表？**

- 链表存储数据时不使用连续空间，如果内存中没有连续的空间用来存储数据，那么不能用顺序表只能用链表；链表对离散空间利用率高

```python
# 单向链表

# 定义节点类
class Node(object):
    def __init__(self, elem):
        self.elem = elem
        self.next = None
        
class SingleLinkList(object):
    def __init__(self, node=None):
        self.__head = node
    
    def is_empty(self):
        return self.__head == None
    
    def length(self):
        # 遍历整个链表
        # cur游标，用来移动遍历节点；初始值设为头部的值
        cur = self.__head
        # count记录数量 
        # 特殊情形：若链表是空的，cur = self._head = None，不进入while循环，count=0直接返回
        count = 0
        while cur != None:
            count += 1
            # 移动
            cur = cur.next
        return count
    
    def travel(self):
        # 遍历：打印每个节点的数据
        cur = self.__head
        while cur != None:
            # 游标所指的当前节点的数据
            # end属性：使用空格不换行，默认是换行\n
            print(cur.elem, end=" ")
            # 移动
            cur = cur.next
        print("")
        
    def add(self,item):
        # 头部插入元素
        # 实例构造节点
        node = Node(item)
        # 将节点的next值指向最初__head的值
        node.next = self.__head
        # 将创建的节点的值node赋给self.__head
        self.__head = node
    
    def append(self, item): 
        # 尾部插入元素     
        # item是传入的具体元素，通过实例存入节点中
        node = Node(item)
        # 考虑空列表的情况
        if self.is_empty():
            # 如果为空，直接指向节点node
            self.__head = node
        else:
            cur = self.__head
            # 只要cur的next区域不是None，则一直进行
            while cur.next != None:
                cur = cur.next
            # 添加节点
            cur.next = node
    
    def insert(self, pos, item):
        """
        指定位置添加：指定位置pos的前一个位置添加
        pos：从0开始
        
        """
        # 如果pos <= 0，相当于是pos=0，看做是在头部插入add方法
        if pos <= 0:
            self.add(item)
        # 如果pos比链表最后一个元素的位置还大，默认看做是在尾部插入
        elif pos > (self.length()-1):
            self.append(item)
        else:
            pre = self.__head
            count = 0
            while count < (pos-1):
                count += 1
                pre = pre.next
            # 当循环退出，pre指向pos-1位置
            node = Node(item)
            node.next = pre.next
            pre.next = node
        
        
    def remove(self, item):
        """
        删除节点
        使用两个游标：pre和cur；最开始pre指向None，cur指向head
        """
        cur = self.__head
        pre = None
        # 终止条件：走到最后，直至每个元素都有被遍历
        # 空链表为None，不执行任何操作
        while cur != None:
            if cur.elem == item:
                # 先判断此节点是否是头节点
                # 如果是头节点：pre == None或者cur == self.__head
                if cur == self.__head:
                    self.__head = cur.next
                else:
                    pre.next = cur.next
                break
            else:
                pre = cur
                cur = cur.next
        
    def search(self, item):
        #  判断是否存在某个数据item：存在返回T，否则F
        # 如果直接是空链表，也可以进行处理，直接返回F
        cur = self.__head
        # while cur.next != None：如果这样写，最后一个元素的next为空，那么最后的元素将不能进入while循环，导致最后的元素没有进行遍历
        while cur != None:
            if cur.elem == item:
                return True
            else:
                cur = cur.next
        return False
    
if __name__ == "__main__":
    li = SingleLinkList()
    print(li.is_empty())
    print(li.length())
    
    li.append(1)
    print(li.is_empty())
    print(li.length())
    
    li.append(2)
    li.append(3)
    li.append(4)
    # 插入在头部
    li.add(8)
    li.append(5)
    li.append(6)
    li.insert(-1,9)
    li.travel()
    li.insert(4,11)
    li.travel()
    li.insert(15,12)
    li.travel()
    li.remove(9)
    li.travel()
    li.remove(4)
    li.travel()
    li.remove(12)
    li.travel()
    
```

```
True
0
False
1
9 8 1 2 3 4 5 6 
9 8 1 2 11 3 4 5 6 
9 8 1 2 11 3 4 5 6 12 
8 1 2 11 3 4 5 6 12 
8 1 2 11 3 5 6 12 
8 1 2 11 3 5 6 
```



```python
# 单向循环链表

# 定义节点类
class Node(object):
    def __init__(self, elem):
        self.elem = elem
        self.next = None
        
class SingleCycleList(object):
    def __init__(self, node=None):
        self.__head = node
    
    def is_empty(self):
        return self.__head == None
    
    # 单向循环链表
    # 尾节点的指针指向头部head
    def length(self):
        # 空链表直接返回0
        if self.is_empty():
            return 0
        cur = self.__head
        # 循环链表中的count初始值为1
        count = 1
        # 判断cur的下个指向是否为head
        while cur.next != self.__head:
            count += 1
            cur = cur.next
        return count
    
    def travel(self):
        # 遍历：打印每个节点的数据
        if self.is_empty():
            return 
        cur = self.__head
        while cur.next != self.__head:
            # 游标所指的当前节点的数据
            # end属性：使用空格不换行，默认是换行\n
            print(cur.elem, end=" ")
            cur = cur.next
        # 退出循环，cur指向尾结点，但尾结点的元素未被打印，单独处理
        print(cur.elem)
      
    # 循环链表中添加元素
    def add(self,item):
        # 头部插入元素
        node = Node(item)
        if self.is_empty():
            self.__head = node
            node.next = node
        else:
            cur = self.__head
            while cur.next != self.__head:
                cur = cur.next
            # 退出循环，cur指向尾结点
            node.next = self.__head
            self.__head = node
            # cur.next = node
            cur.next = self.__head

    def append(self, item): 
        # 尾部插入
        node = Node(item)
        # 考虑空链表的情况
        if self.is_empty():
            self.__head = node
            node.next = node
        else:
            cur = self.__head
            while cur.next != self.__head:
                cur = cur.next
            # node.next = cur.next
            node.next = self.__head
            cur.next = node
    
    def insert(self, pos, item):
        """
        指定位置添加：指定位置pos的前一个位置添加
        pos：从0开始
        
        """
        # 如果pos <= 0，相当于是pos=0，看做是在头部插入add方法
        if pos <= 0:
            self.add(item)
        # 如果pos比链表最后一个元素的位置还大，默认看做是在尾部插入
        elif pos > (self.length()-1):
            self.append(item)
        else:
            pre = self.__head
            count = 0
            while count < (pos-1):
                count += 1
                pre = pre.next
            # 当循环退出，pre指向pos-1位置
            node = Node(item)
            node.next = pre.next
            pre.next = node
          
    def remove(self, item):
        """
        删除节点
        使用两个游标：pre和cur；最开始pre指向None，cur指向head
        """
        if self.is_empty():
            return 
        cur = self.__head
        pre = None
        # 终止条件：走到最后，直至每个元素都有被遍历
        # 空链表为None，不执行任何操作
        while cur.next != self.__head:
            if cur.elem == item:
                # 先判断此节点是否是头节点
                # 如果是头节点：pre == None或者cur == self.__head
                if cur == self.__head:
                    # 头结点
                    # self.__head = cur.next
                    rear = self.__head
                    while rear.next != self.__head:
                        rear = rear.next
                    self.__head = cur.next
                    rear.next = self.__head
                else:
                    # 中间结点
                    pre.next = cur.next
                return
            else:
                pre = cur
                cur = cur.next
        # 退出循环，cur指向尾结点
        if cur.elem == item:
            if cur == self.__head:
                # 链表只有一个节点
                self.__head = None
            else:
                pre.next = cur.next      
        
        
    def search(self, item):
        #  判断是否存在某个数据item：存在返回T，否则F
        # 如果直接是空链表，也可以进行处理，直接返回F
        if self.is_empty():
            return False
        cur = self.__head
        while cur.next != self.__head:
            if cur.elem == item:
                return True
            else:
                cur = cur.next
        # 退出循环，cur指向尾节点
        if cur.elem == item:
            return True
        return False
    
if __name__ == "__main__":
    li = SingleCycleList()
    print(li.is_empty())
    print(li.length())
    
    li.append(1)
    print(li.is_empty())
    print(li.length())
    
    li.append(2)
    li.append(4)
    # 插入在头部
    li.add(8)
    li.append(5)
    li.insert(-1,9)
    li.travel()
    li.insert(4,11)
    li.travel()
    li.insert(15,12)
    li.travel()
    li.remove(9)
    li.travel()
    li.remove(4)
    li.travel()
    li.remove(12)
    li.travel()
```

```
True
0
False
1
9 8 1 2 4 5
9 8 1 2 11 4 5
9 8 1 2 11 4 5 12
8 1 2 11 4 5 12
8 1 2 11 5 12
8 1 2 11 5
```

<CENTER><span style='font-size:2.5rem'>Stay Foolish Stay Hungry</span></CENTER>
