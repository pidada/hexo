title: 双向链表
date: 2019-09-09 18:44:41
tags: #数据结构 #Python #算法
categories: 数据结构
copyright: true

---------------



### 双向链表

#### 概念

> 双向链表是普通链表的扩展，它的特点是具有两个节点。

- 后继节点：指向下一个节点

- 前驱节点：指向前一个节点

- 头节点没有前驱节点，尾节点没有后继节点

  

<!-- MORE -->

#### 实现


```python
# coding: utf-8

# 定义节点
class Node(object):
    def __init__(self, item):
        self.elem = item
        self.next = None
        self.prev = None
        
class DoubleLinkList(object):
    def __init__(self, node=None):
        # head的内部属性指向头节点
        self.__head = node
    
    def is_empty(self):
        return self.__head is None
    
    def length(self):
        cur = self.__head
        count = 0
        while cur != None:
            count += 1
            cur = cur.next
        return count
    
    def travel(self):
        cur = self.__head
        while cur != None:
            print(cur.elem, end=" ")
            cur = cur.next
        print("")
        
    def add(self, item):
        # 头部插入
        node = Node(item)
        node.next = self.__head
        self.__head = node
        node.next.prev = node
        # self.__head.prev = node
        # self.__head = node
        
    def append(self, item):
        # 尾部插入
        node = Node(item)
        if self.is_empty():
            self.__head = node
        else:
            # 游标从头开始走
            cur = self.__head
            while cur.next != None:
                cur = cur.next
            # 退出循环的操作
            cur.next = node
            node.prev = cur
            
    def insert(self, pos, item):
        # 指定位置插入
        # 如果pos <= 0，相当于是pos=0，看做是在头部插入add方法
        if pos <= 0:
            self.add(item)
        # 如果pos比链表最后一个元素的位置还大，默认看做是在尾部插入
        elif pos > (self.length()-1):
            self.append(item)
        else:
            cur = self.__head
            count = 0
            while count < pos:
                count += 1
                cur = cur.next
            # 退出循环，cur指向pos位置
            node = Node(item)
            node.next = cur
            node.prev = cur.prev
            cur.prev.next = node
            cur.prev = node
            # 上面两条等价于下两条
            # cur.prev = node
            # node.prev.next = node
            
    def remove(self, item):
        cur = self.__head
        while cur != None:
            if cur.elem == item:
                # 判断是否为头节点
                if cur == self.__head:
                    self.__head = cur.next
                    if cur.next:
                        # 判断链表是否只有一个节点
                        cur.next.prev = None
                else:
                    cur.prev.next = cur.next
                    if cur.next:
                        cur.next.prev = cur.prev
                break
            else:
                cur = cur.next
            
            
    def search(self, item):
        # 查找节点是否存在
        cur = self.__head
        while cur != None:
            if cur.elem == item:
                return True
            else:
                cur = cur.next
        return False

if __name__ == "__main__":
    li = DoubleLinkList()
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

```python
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