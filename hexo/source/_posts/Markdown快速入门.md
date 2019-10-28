---
title: Markdown快速入门
date: 2019-09-07 09:06:39
tags: 
  - Markdown
  - 神器
categories: Tool
copyright: true
---



### What`s Markdown 

从3月份接触到`Markdown`，到现在差不多半年的时间。从当初的自学语法，到现在熟练地使用`Markdown`进行日常文档的书写和笔记，自己也是迷上了`Markdown`。What`s Markdown ?

> Markdown 是一种纯文本、轻量级的文本标记语言，它不是一款软件，通过简单的标记语法，它以纯文本的形式编写，基本上所有的文本编辑器都能够对其进行编辑。

![](https://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Markdown-mark.svg/250px-Markdown-mark.svg.png)



### Advantages  of  Markdown

相比较于其他的文本编辑工具，比如`word、notepad++`、记事本等而言，`Markdown`有着独特的自身优势：

- 轻量级：结构简单，语法非常容易记忆，上手快
- 标记语言：通过一些特定的符号`- + *`来实现特定的功能
- 用户能够专注于书写，而非排版格式
- 纯文本：`Markdown`支持的纯文本内容，兼容绝大部分的文本编辑器
- 时效性：`Markdown`文件在通过不同的工具能够随时修改，容易版本控制
- 流行性：很多大型的博客网站，例如`Wordpress`、`CSDN`都是支持`Markdown`，甚至`Ghost`、`Typecho`等平台只支持`Markdown`格式的`.md`文件



<!-- MORE -->

### Markdown Tools

在不同的平台上，能够使用Markdown进行编辑的略有不同，常见的`Tools`有：

- `Mou,MacDown,Ulysses,Typora,MarkdownPlus`,`Cmd Markdown`。笔者绝大部分时间使用的是[Typora](https://www.typora.io/)，纯字符界面。
- 印象笔记，有道云笔记等软件中也只支持`Markdown`编辑功能，笔者现在使用的是印象笔记，有工具栏可供使用。
- 手机上有`马克文档`，`MWeb`，`Markdown`等`APP`可供选择



### How  to use Markdown

Markdown的常见语法主要包含下面几种：

- 标题
- 引用
- 分割线
- 链接
- 图片
- 列表
- `Todo`列表
- 代码
- 加粗
- 斜体
- 删除线
- 表格
- 脚注
- 邮箱



#### 标题

我们知道，在写文章或者论文的时候，标题是有等级的。在Markdown中，标题的等级是通过`#`来实现，目前最多支持六级

```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

效果如下：

![nNEVn1.png](https://s2.ax1x.com/2019/09/10/nNEVn1.png)



#### 引用

当你在写文章的时候，想引用其他人的观点或者语句，通过`>`来实现，书写完毕后显示在界面上，文字前面会有一条竖线。下面通过百度上的一段对`Markdown`的解释来进行说明

```
>Markdown是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。
>> 本文中介绍Markdown的相关知识、常用工具和使用语法，希望对大家有所帮助。
```

>`Markdown`是一种可以使用普通文本编辑器编写的标记语言，通过简单的标记语法，它可以使普通文本内容具有一定的格式。
>
>> 本文中介绍Markdown的相关知识、常用工具和使用语法，希望对大家有所帮助。

引用也是有层级关系的，通过`>`的个数 来实现嵌套。

---



#### 分割线

若我们想把上下文分开，需要使用华丽丽的分割线，使用的是`---`或者`***`，比如笔者已经使用分割线和上面的引用与下面的链接部分进行了分割。

```markdown
---
***
-------
***********
```

温馨提示：短横线或者星号的个数至少要3个

***



#### 链接

写文章的时候，如果想实现跳转，需要使用链接。笔者最近在学习[Python](https://www.python.org/)编程语言，这里`Python`正是使用了链接的功能；当你点击`Python`，就会自动跳转到指定的`Python`官网。

```
笔者最近在学习[Python](https://www.python.org/)编程语言

语法规则：[点击内容](链接)
```



#### 段内链接

段内链接实际上就是生成目录列表`Table of Contents（TOC）`，实现方法是输入`[toc]`，按下回车自动生成

![](https://s2.ax1x.com/2019/09/24/ukMSYR.png)



#### 图片

图片的引用和链接比较类似，只是前面多了个`！`，比如我们给上面的栗子加上`Python`语言的图标

![python](https://www.python.org/static/img/python-logo.png)

```
![](https://www.python.org/static/img/python-logo.png)
```



#### 列表

列表分为两种，有序列表和无序列表，通过`*`或`+`或`-`均可实现

##### 无序列表

- Python基础
- 基本语法
- 循环控制
  + if
  + while
  + break
  + continue
- 函数思想
- 面向对象
- 常用库

```
- Python基础
- 基本语法
- 循环控制
	+ if
	+ while
	+ break
	+ continue
* 函数思想
* 面向对象
* 常用库

短横线、星号或者加号与后面的内容之间，必须有至少1个空格，也是具有嵌套层级关系，通过空格或者Tab键来进行缩进
```



##### 有序列表

1. Python基础
2. 基本语法
3. 循环控制
   + if
   + while
   + break
   + continue
4. 函数思想
5. 面向对象
6. 常用库

```
1. Python基础
2. 基本语法
3. 循环控制
	+ if
	+ while
	+ break
	+ continue
4. 函数思想
5. 面向对象
6. 常用库

数字和英文的点与后面的内容之间，必须有至少一个空格；有序和无序列表可以同时使用
```

#### `Todo`列表

- [ ] 支持以 `PDF` 格式导出文稿
- [ ] 改进` Cmd `渲染算法，使用局部渲染技术提高渲染效率
- [x] 新增` Todo` 列表功能
- [x] 修复` LaTex `公式渲染问题
- [x] 新增` LaTex` 公式编号功能

```
- [ ] 支持以 PDF 格式导出文稿
- [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
- [x] 新增 Todo 列表功能
- [x] 修复 LaTex 公式渲染问题
- [x] 新增 LaTex 公式编号功能
```



#### 代码

在写文档的时候经常会需要用到代码，代码的实现分为段内代码和代码块两种

##### 段内

每个人学习编程语言都是从打印`print("hello world")`开始的，就是段内引用

```python
`print("hello world")`
```

在你的代码前后加上反引号，`Tab`键上面的！

不是单引号！不是单引号！不是单引号！

##### 代码块

使用方法是用一对三引号将你的代码块包起来

```python
# 高斯求和，还可以加上注释部分
sum = 0 
for i in range(1, 101):  # range函数包含头不含尾
    sum += i
print(sum)
```

---



#### 加粗

如果你想强调某部分内容，可以用加粗来实现，比如上面的栗子，强调不是单引号!，只需要前后加上一对双星号即可：

```
**不是单引号！**
```

加粗效果为：**不是单引号！**



#### 斜体

还是上面的栗子，给不是单引号！实现斜体，只需要加一对单星号：

```
*不是单引号！*
```

斜体效果为：*不是单引号！*

---



#### 删除线

看个栗子：~~不要999~~，**只要99**，栗子中删除线通过一对双波浪线（Tab键上面）来实现，~~这里是删除线~~

```
~~不要999~~，**只要99**
```

加粗、斜体和删除线可以同时使用

这里是*斜体***加粗**的~~删除线~~

```
这里是*斜体***加粗**的~~删除线~~
```



#### 表格

在Markdown中也可以实现基本表格的插入，语法稍微麻烦，`but`很好记忆，表格内部的换行通过`<br>`来实现

| 学号 | 学生 | 性别 | 成绩                 |
| ---- | ---- | ---- | -------------------- |
| 1    | 张三 | 男   | 语文：88<br>数学：90 |
| 2    | 李四 | 男   | 语文：85<br>数学：96 |



具体写法为

```markdown
学号|学生|性别|成绩
---|---|---|---
1|张三|男|语文：88<br>数学：90
2|李四|男|语文：85<br>数学：96
```



#### 流程图

```flow
st=>start: Start:>https://www.zybuluo.com
io=>inputoutput: verification
op=>operation: Your Operation
cond=>condition: Yes or No?
sub=>subroutine: Your Subroutine
e=>end

st->io->op->cond
cond(yes)->e
cond(no)->sub->io
```



```fl
​```flow
st=>start: Start:>https://www.zybuluo.com
io=>inputoutput: verification
op=>operation: Your Operation
cond=>condition: Yes or No?
sub=>subroutine: Your Subroutine
e=>end

st->io->op->cond
cond(yes)->e
cond(no)->sub->io
​```
```



#### 脚注

```markdcsharp
Markdown[^1]
[^1]: Markdown是一种纯文本标记语言，易于书写        // 在文章最后面显示脚注的内容
```

Markdown[^1]



#### 邮箱

使用尖括号`<>`将你的邮箱括起来，我的`QQ`邮箱是<756803877@qq.com>

```
<756803877@qq.com>
```



### Conclusion

- Markdown真的很好
- Markdown真的很好
- Markdown真的很好

[^1]: Markdown是一中纯文本标记型语言，易于书写。



<CENTER><span style='font-size:2.5rem'>Stay Foolish Stay Hungry</span></CENTER>



