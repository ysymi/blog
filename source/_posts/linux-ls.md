---
title: "Linux命令学习: ls"
toc: true
date: 2017-04-28 09:44:41
updated: 2017-04-28 09:44:41
tags: [linux, shell]
---

ls命令是list的缩写，用于列出某目录下的所有目录项，默认会显示目录项的名称，当然也可以显示其他的信息，比如文件所属、文件大小、修改时间等。

<!--more-->

### 命令格式

```shell
ls [options] [dir]
```

### 说明


### 常用选项

-a, -–all
列出目录下的所有目录项，包括以 . 开头的

-A, --almost-all
列出目录下的所有目录项，不列出“.”(表示当前目录)和“..”(表示当前目录的父目录)。

-d，--directory
显示目录的自己的信息，而不是显示目录内容

-c 
与-lt连用的时候：根据 ctime 排序并显示ctime （文件状态信息的最后修改时间）
与 -l连用的时候：显示 ctime 但根据名称排序
其余情况：根据 ctime 排序


-L, –dereference
当显示符号链接的文件信息时，显示符号链接所指示的对象而并非符号链接本身的信息

-R, –recursive 
递归的，同时列出所有子目录层

-h, –human-readable 
以容易理解的格式列出文件大小 (例如 1K 234M 2G)

--color 
对不同类型的文件，显示不同的颜色

--format <type>
输出格式如下: 

选项  | 说明
-----|-----
-x, --format across ,--format horizontal | 按照文件名水平排列
-C, --format vertical | 按照文件名垂直排列
-l, --format long, --format verbose | 显示详情
-1, --format single-column | 每行只有一个名字
-m, --format commas | 逗号隔开
-i, --inode | 显示每个文件的 inode 

-t 
按照文件的修改时间进行排序, 最新修改过的在上面

-X
根据拓展名进行排序

-r, –reverse 
反次序排列

-S 
根据文件大小排序,大的在前面

### 小技巧

1. ls 支持的 dir 参数可以使用通配符， 比如列出所有python版本

```shell
# ll /usr/bin/python*
lrwxrwxrwx 1 root root    9 Dec 21  2013 /usr/bin/python -> python2.7*
lrwxrwxrwx 1 root root    9 Dec 21  2013 /usr/bin/python2 -> python2.7*
-rwxr-xr-x 1 root root 3.2M Jun 23  2015 /usr/bin/python2.7*
```

2. 查看满足条件的文件的详细信息

```shell
# find . -type f -size 500k | xargs ls -l
```
