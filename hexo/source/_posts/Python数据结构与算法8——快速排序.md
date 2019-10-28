---
title: 快速排序
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-30 18:49:30
password:
summary:
copyright: true
tags:
  - 算法
  - 快速排序
  - Python
  - 数据结构
categories:
  - 算法
  - 十大经典排序
---



### 快速排序



#### 算法思想
>快速排序算法首先会在序列中随机选择一个基准值（pivot），然后将除了基准值以外的数分为“比基准值小的数”和“比基准值大的数”这两个类别，再将其排列成以下形式:

>[ 比基准值小] 基准值 [比基准值大]

>接着，对两个“[ ]”中的数据进行排序之后，整体的排序便完成了。对“[ ]”里面的数据进行排序时同样也会使用快速排序，即使用递归的思想。


#### 时间复杂度
时间复杂度$nlog_2(n)$

不稳定



![](https://s2.ax1x.com/2019/09/30/ut8Uwn.gif)

<!--MORE-->

-----

### Python代码实现

```python
def quick_sort(alist, first ,last):
    # 快速排序 
    if first >= last:
        return
    
    # 先选择基准值，即第一个元素
    mid_value = alist[first]
    # 通过两个游标的从头部和尾部的移动来确定基准值的位置
    # 满足左边的比基准值小，右边的比基准值大
    low = first
    high = last
    
    while low < high:
        # 下面两个循环交替执行，不断的左右移动
        
        # 如果high指向的元素比基准值大，说明该值应该留在右边
        while low < high and alist[high] >= mid_value:
            # 同时high的指针往左移动
            high -= 1
        # 如果不满足循环条件，则说明high指向的值比基准值小，和前面low指向的值交换位置
        alist[low] = alist[high]

        # 对于左边的low指向的值小于基准值，放在左边
        while low < high and alist[low] < mid_value:
            low += 1
        # 不满足while循环则交换位置
        alist[high] = alist[low]
    # 从循环退出，low == high
    alist[low] = mid_value
    
    # 递归调用函数自身：传入下标的起始值不同，还是原来的列表
    
    # 基准值左边的列表进行快排
    quick_sort(alist,first, low-1)
    
    # 基准值右边的列表进行快排
    quick_sort(alist,low+1, last)
    
if __name__ == "__main__":
    li = [1, 9, 6, 3, 8, 4, 7, 5, 2]
    print(li)
    quick_sort(li, 0, len(li)-1)
    print(li)
```

### 算法图解



```python
def quicksort(array):
    # 判断只有一个元素的情况
    if len(array) < 2:
        return array
    else:
        # 指定基数
        pivot = array[0]
        # 基准数左边：从原有列表中选择出 <= 基准值的元素
        less = [i for i in array[1:] if i <= pivot]
        # 基准数右边
        greater = [i for i in array[1:] if i > pivot]
        # 调用函数自身，递归思想
        return quicksort(less) + [pivot] + quicksort(greater)
print (quicksort([10, 5, 2, 9, 3, 8, 5]))
```