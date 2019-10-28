---
title: python内置函数_a系列
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-01 22:04:21
password:
summary:
copyright: true
tags:
  - python
  - 内置函数
categories:
  - python
  - 进阶
---

### abs()

`Python`中内置的绝对值函数

- 参数如果是整型或者浮点数，返回的是绝对值
- 参数如果是复数，返回的是模

> Return the absolute value of a number. The argument may be an integer or a floating point number. If the argument is a complex number, its magnitude is returned.

<!--MORE-->

```python
abs(-3)   // 3
abs(5.6)  // 5.6
abs(3+4j)  // 5.0
```

### all()

如果 `iterable` 的所有元素为真（或迭代器为空），返回 `True` 。等价于：

```python
def all(iterableElements):
    for element in iterableElements:   # 遍历所有的可迭代元素
        if not element:   # 如果有一个元素不在，则返回F
            return False
    return True  # 如果都在，返回T
```

### any()

如果 `iterable`的任一元素为真则返回 `True`。 如果迭代器为空，返回 `False`。 等价于

```python
def any(iterableElements):
    for element in iterableElements:
        if element:  # 如果有一个在里面则返回T
            return True
    return False   # 遍历完所有的元素，都不在则返回F
```

### ascii()

返回一个对象可打印的字符串。生成的字符串和`Python 2`的 返回的结果相似。但是 `repr`返回的字符串中非 `ASCII` 编码的字符，会使用 `\x`、`\u` 和 `\U` 来转义。

```python
ascii("hello python")  // "'hello python'"
ascii(4)   // '4'
```

