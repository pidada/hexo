---
title: linuxNote2_软硬链接
date: 2019-09-11 10:25:54
tags: 
  - linux
  - 链接 
categories: 
  - linux
  - 基础
copyright: true
---

>`Linux`中链接分为两种，一种是硬链接 `Hard link`，一种是软链接 `Symbolic link`。默认情况下，ln命令产生硬链接。链接为 `Linux `系统解决了文件的共享使用，还带来了隐藏文件路径、增加权限安全及节省存储等好处。
>[Linux软硬链接](https://www.runoob.com/linux/linux-file-content-manage.html)
>[理解Linux的硬链接和软链接](https://www.ibm.com/developerworks/cn/linux/l-cn-hardandsymb-links/index.html)

<!-- MORE -->



---
### 硬链接

+ 在` Linux` 的文件系统中，保存在磁盘分区中的文件都被分配一个编号，称为索引节点号(Inode Index)。硬连接指通过**索引节点**来进行连接。

+ 硬链接是有相同的`inode`，仅文件名不同的文件。

+ 删除一个硬链接文件不影响其他具有相同`inode`的文件。文件真正删除的条件是与之相关的所有硬连接文件均被删除。

---
### 软连接
+ 另一种连接称之为符号连接`（Symbolic Link）`，也叫软连接。

+ 软链接文件有类似于 Windows 的快捷方式。它实际上是一个特殊的文件。

+ 软链接有自己的文件属性及权限等；

+ 可对不存在的文件或目录创建软链接；

+ 软链接可交叉文件系统；

+ 若A 是 B 的软链接（A 和 B 都是文件名），A 与 B 的目录项中` inode` 节点号不相同，A 和 B 指向的是两个不同的`inode`，继而指向两块不同的数据块。如果B 被删除了，A仍存在（因为两个是不同的文件），但指向的是一个无效的链接。
---
**举例说明**
```shell
root@peter:~# touch f1          # 创建文件f1
root@peter:~# ln f1 f2          # 创建硬链接f2
root@peter:~# ln -s f1 f3       # 创建软链接f3 
root@peter:~# ls -li            # 节点号1，2相同，3不同
total 0
1190998 -rw-r--r-- 2 root root 0 May 27 00:34 f1
1190998 -rw-r--r-- 2 root root 0 May 27 00:34 f2
1191109 lrwxrwxrwx 1 root root 2 May 27 00:34 f3 -> f1
root@peter:~# echo "I am f1 file" >>f1
root@peter:~# cat f1
I am f1 file
root@peter:~# cat f2
I am f1 file
root@peter:~# cat f3     # 1,2,3的内容相同 
I am f1 file
root@peter:~# rm -rf f1
root@peter:~# cat f2     # 删除f1，f2不受影响
I am f1 file
root@peter:~# cat f3
cat: f3: No such file or directory  # f3随着f1同时删除
```
**结论**
- 删除3，对1、2没有影响
- 删除2，对1、3没有影响
- 删除1，对2没有影响，3失效
- 同时删除1和2，整个文件被真正地删除



<CENTER><span style='font-size:2.5rem'>Stay Foolish Stay Hungry</span></CENTER>