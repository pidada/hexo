title: 栈、队列和双端队列
date: 2019-09-09 18:44:41
tags: #数据结构 #Python #算法
categories: 数据结构
copyright: true

-----



### 栈

>栈`Stack`是一种线性的数据结构，`FILO`（先进后出）的操作，可以用顺序表实现，也可以用链表来实现。
#### 操作
**栈的基本操作包含：**:arrow_down:

- stack()：创建空的栈
- push()：入栈
- pop()：出栈
- peek()：返回栈顶元素
- is_empty()：判断是否为空栈
- size()：返回栈的元素个数

<!-- MORE -->

#### 实现


```python
# coding: utf-8
# 栈的实现

class Stack(object):
    """Stack"""
    def __init__(self):
        self.__list = []
    
    def push(self, item):
        # 添加元素到栈顶
        self.__list.append(item)
    
    def pop(self):
        # 弹出栈顶元素
        return self.__list.pop()
    
    def peek(self):
        # 返回栈顶元素
        # 需要判断是否为空列表
        if self.__list:
            return self.__list[-1]
        else:
            return None
    
    def size(self):
        # 返回栈的元素个数
        return len(self.__list)
    
    def is_empty(self):
        # 判断是否为空
        # 空列表返回真
        return self.__list == []
        # 如果是空列表，布尔值为F，取反变成T
        # return not self.__list
    
if __name__ == "__main__":
    s = Stack()
    s.push(1)
    s.push(2)
    s.push(3)
    s.push(4)
    s.push(5)
    
    print(s.pop())
    print(s.pop())
    print(s.pop())
    print(s.pop())
    print(s.pop())
```

    5
    4
    3
    2
    1


### 队列
#### 概念
队列`queue`也是一种线性结构，方式是先进先出`FIFO`， 想象成一支队伍。
- 允许插入数据的一端：队尾
- 允许删除的一端：队头
假设队列$q=(a_1, a_2 ,..., a_n)$，则$a_1$是队头元素，$a_n$是队尾元素。删除从$a_1$开始，添加从$a_n$开始

#### 操作
几个重要的操作
- enqueue()：插入元素
- dequeue()：删除元素
- is_empty()：判断是否为空
- size()：返回元素个数

#### 实现


```python
# coding: utf-8

class Queue(object):
    # Queue
    def __init__(self):
        self.__list = []
    
    def enqueue(self, item):
        # 添加元素：append默认是添加到末尾
        self.__list.append(item)
        # self.__list.append(0, item)
    
    def dequeue(self):
        # 从队列头部删除元素：pop默认是末尾
        return  self.__list.pop(0)
        # return  self.__list.pop()
    
    def is_empty(self):
        # 判断是否为空
        return self._list == []
    
    def size(self):
        # 返回个数
        return len(self.__list)
    
if __name__ == "__main__":
    # 创建类的实例
    q = Queue()  
    
    # 调用实例的方法
    q.enqueue(1)  
    q.enqueue(2)
    q.enqueue(3)
    q.enqueue(4)
    q.enqueue(5)
    
    print(q.dequeue())
    print(q.dequeue())
    print(q.dequeue())
    print(q.dequeue())
    print(q.dequeue())
```

    1
    2
    3
    4
    5

### 双端队列

#### 概念

能够在队头和队尾<span class="burk">同时进行插入和删除</span>操作的队列

#### 实现


```python
# coding: utf-8
# 双端队列

class Dueue(object):
    # Doublequeue
    # 构造函数，用来定义私有化属性
    def __init__(self):
        self.__list = []
    
    def add_front(self, item):
        # 添加元素：append默认是添加到末尾；也可以指定位置
        # 双端队列中哪里添加就在哪里删除
        self.__list.insert(0, item)
        # self.__list.append(0, item)
    
    def add_rear(self, item):
        # 从队列头部删除元素：pop默认是末尾
        self.__list.append(item)
    
    def pop_front(self):
        # 头部删除
        return self.__list.pop(0)
    
    def pop_rear(self):
        # 尾部删除
        return self.__list.pop()
    
    def is_empty(self):
        # 判断是否为空
        return self._list == []
    
    def size(self):
        # 返回个数
        return len(self.__list)
    
if __name__ == "__main__":
    q = Dueue()
    
    # 头部插入 
    q.add_front(1)
    q.add_front(2)
    q.add_front(3)
    # 尾部删除
    q.add_rear(4)
    q.add_rear(5)
    q.add_rear(6)
    
    # 同步删除
    print(q.pop_front())
    print(q.pop_front())
    print(q.pop_front())
    # 尾部删除
    print(q.pop_rear())
    print(q.pop_rear())
    print(q.pop_rear())
```

    3
    2
    1
    6
    5
    4



<CENTER><span style='font-size:2.5rem'>Stay Foolish Stay Hungry</span></CENTER>