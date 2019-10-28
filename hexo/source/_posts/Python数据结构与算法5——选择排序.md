---
title: 选择排序
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-23 00:49:31
password:
summary:
tags:
  - 算法
  - 选择排序
  - 数据结构
categories:
   - 算法
   - 十大经典排序
---



### 选择排序



#### 思想
- 将数据分成两个部分：前面排好序和后面待排序的
- 从没有排序的数据选择出一个最小的数据，放在前面排好序的后面
- 不稳定

#### 时间复杂度
- 最坏时间复杂度：$O(n^2)$

- 最优时间复杂度：$O(n^2)$

![](https://s2.ax1x.com/2019/09/30/ut8aoq.gif)

<!-- MORE -->

### Python实现

```python
def select_sort(alist):
    # 选择排序
    n = len(alist)
    for j in range(0, n-1):
        # 记录最小位置
        min_index = j
        # 内层for循环找到了后面未排序数据的最小值
        for i in range(j+1, n):
            if alist[min_index] > alist[i]:
                # 找出真正最小值的索引，再进行交换
                min_index = i
        alist[j], alist[min_index] = alist[min_index], alist[j]
    return alist
```

### Golang实现

```go
package main

// 算法思想
// 将数据分成两个部分：前面排好序和后面待排序的
// 指定一个基准元素，将基准元素和后面的每个元素进行比较
// 从没有排序的数据（后面未排序）选择出一个最小的数据，放在前面排好序的后面

import "fmt"

func selectSort(numberArray []int) []int{      // 函数的输入是数组，返回值也是数组
	n := len(numberArray)     // 指定数组的长度
	for j := 0; j < n - 1; j++{    // 外层循环控制整个比较轮数
		min_index := j        // 假定最小数的索引的位置
		for i := j+1; i < n; i++{     // 将索引为j的元素同它后面的元素进行比较[j+1, n]
			if numberArray[min_index] > numberArray[i]{    // 如果 指定的最小元素比其后面的元素大，说明较小的元素仍在后面
				min_index = i   // 比较完后，找出最小元素的索引，并且将索引赋值给变量min_index
			}
		}
		// 内层比较完后进行元素互换
		numberArray[j], numberArray[min_index] = numberArray[min_index], numberArray[j]
	}
	return numberArray
}

func main()  {
	selectArray := []int{1,8,9,7,3,5,2,6,4}   // 数组的定义
	fmt.Println("Before：", selectArray)
	selectSort(selectArray)
	fmt.Println("After：", selectArray)
}
```

