---
title: 希尔排序
top: false
cover: false
toc: true
mathjax: true
date: 2019-09-30 10:38:19
password:
summary:
copyright: true
tags:
  - 算法
  - 希尔排序
  - 数据结构
categories:
   - 算法
   - 十大经典排序
---

### 希尔排序
#### 思想
希尔排序是插入排序的一种，也称之为`缩小增量排序`。[希尔排序算法](https://baike.baidu.com/item/希尔排序算法)是直接插入排序算法的一种改进，减少了其复制的次数，速度要快很多。希尔排序是非稳定排序算法，实现过程：
- 将记录按照下标的一定增量进行分组，`对每个组进行插入排序`
- 增量逐渐减少：当减少至1时，算法终止

#### 栗子
假设步长为step，对[1, 8, 2, 9, 3, 7, 4, 6, 5]进行排序，步长一般是按照`折半`进行选取
- 步长取4：对[1, 3, 5],[8, 7],[2, 4],[9, 6]三个序列，分别进行插入排序
- 步长取2：对上述排序的序列，步长取一半，再按照类似的方法进行排序
- 步长取1：重复上述操作

[数据结构和算法](https://www.cnblogs.com/skywang12345/p/3603935.html)

<!-- MORE -->

#### 时间复杂度
- 最优时间复杂度：根据步长序列的不同而不同
- 最坏时间复杂度：$O(n^2)$
- 稳定性：不稳定

![](https://s2.ax1x.com/2019/09/30/ut8GQg.gif)

### Python实现

```python 
def shell_sort(alist):
    # 希尔排序：核心是插入排序
    n = len(alist)
    # 折半：取整数解，防止小数:n=9,step=4
    step = n // 2
    i = 1
    
    # 步长变化到0之前即1，插入算法执行的次数 
    while step > 0:
        # 插入算法：与普通插入算法的区别就是步长step
        for j in range(step, n):
            # j = gap, gap+1, ..., n-1
            i = j
            while i > 0:
                if alist[i] < alist[i-step]:
                    # 比较的两个数下标相差为步长step
                    alist[i], alist[i-step] = alist[i-step], alist[i]
                    i -= step
                else:
                    break
        # 一个for循环结束则缩短步长step
        step //= 2
        
    return alist
```

### Golang实现

```go
package main

import "fmt"

// shell排序
func shellSort(numberArray []int) []int{   // 定义函数、参数和返回值
	n := len(numberArray)
	step := n / 2   // 设定步长step

	for step > 0{   //   进行排序的条件：步长大于0
		for j := step; j < n; j++{  // 控制外层循环：从中间的step位置开始，往后进行比较
			// j = [step, step+1, ..., n-1]
			i := j
			for i > 0{
				if numberArray[i] > numberArray[i-step]{   // 进行比较和交换的两个数之的索引相差为step
					numberArray[i], numberArray[i-step] = numberArray[i-step], numberArray[i]
					i -= step   // 每交换一次，下标左移step；直接插入排序中是左移一个位置
				}else {
					break
				}
			}
		}
		step = step / 2   // 执行一次，步长要缩短一半
	}
	return numberArray
}


func main(){
	shellArray := []int{1,8,9,7,3,5,2,6,4}
	fmt.Println("Before:", shellArray)
	shellSort(shellArray)
	fmt.Println("After:", shellArray)
}
```

