---
title: 冒泡排序
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-19 20:52:08
password:
summary:
copyright: true
tags: 
  - 算法
  - 排序
categories:  
  - 算法
  - 十大经典排序
---

### 冒泡排序

#### 思想
- 每次将相邻的两个元素进行比较，大的元素放在右边
- 每次比较完毕，最大的元素一定回到最右边
- 下次比较的时候，已经排好序的数不需要再进行排序
- 算法稳定

内外两层的控制：
- 外层控制总共比较多少轮
- 内层控制每次比较的次数
#### 时间复杂度
- 最优时间复杂度：$O(n)$
- 最坏时间复杂度：$O(n^2)$

![](https://s2.ax1x.com/2019/09/30/ut1t1g.gif)

<!-- MORE -->



### python

```python
def bubble_sort(alist):
    # 冒泡排序
    n = len(alist)
    # 外层循环控制走多少趟：最后一个数到达末尾，无需再进行比较
    for j in range(n-1):
        # 内层循环控制每趟比较多少次：已经排好序的数不需再进行比较（j个）
        for i in range(0, n-j-1):
            # 升序排列
            if alist[i] > alist[i+1]:
                alist[i], alist[i+1] = alist[i+1], alist[i]
    return alist
```

```python
def bubble_sort1(alist):
    n = len(alist)
    for j in range(len(alist)-1, 0 ,-1):
        for i in range(j):
             # 升序排列
            if alist[i] > alist[i+1]:
                alist[i], alist[i+1] = alist[i+1], alist[i]
    return alist 
```



### Golang

```go
package main

import "fmt"

//冒泡算法
func bubble(numArray []int) []int{    //参数和返回值都要声明类型
	n := len(numArray)
	for i := 0; i < n; i++{
		for j :=0; j<n-i-1;j++{   //比较i轮后，i个数固定，不需要再进行比较
			if numArray[j] > numArray[j+1]{  //升序排列
				numArray[j], numArray[j+1] = numArray[j+1], numArray[j]  //相邻元素互换
			}
		}
	}
	return numArray
}


func main(){
	//短声明和var声明不能同时存在
	//var numArray = [...]int{1, 8, 9, 7, 3, 5, 2, 6, 4}
	bubbleArray := []int{1,8,9,7,3,5,2,6,4}
	fmt.Println("Before：", bubbleArray)
	bubble(bubbleArray)
	fmt.Println("After：", bubbleArray)
}
```

