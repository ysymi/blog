---
title: "Linux命令学习: ls"
toc: true
date: 2017-04-28 09:44:41
updated: 2017-04-28 09:44:41
categories: [Linux命令学习]
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
列出目录下的所有目录项，不列出`.`(表示当前目录)和`..`(表示当前目录的父目录)。

-d，--directory
显示目录本身的信息，而不是显示目录内容

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

--sort=<type>
可以指定排序的键:
类型 | 简写选项 | 说明
--sort=extension | -X | 根据拓展名进行排序，字典序
--sort=size | -S |根据文件大小排序,大的在前面
--sort=time | -t|按照文件的修改时间进行排序, 最新修改过的在上面
--sort=none | -U | 不排序，使用原始文件系统的顺序
--sort=version | -v | 版本数？

-u
显示access time

-c
显示file status info modification time

-t
按照显示的时间排序，默认显示的时间是文件内容修改时间

-r, –reverse 
反次序排列


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


### 补充

1. 文件的各种时间

- atime: Time when file data was last accessed. 最后一次访问的时间
- mtime: Time when data was last modified. 最后一次修改文件内容的时间
- ctime: Time when file status was last changed. 最后一次修改文件状态信息的时间

ls(1) 命令可用来列出文件的 atime、ctime 和 mtime。
```shell
ls -lc filename    #     列出文件的 ctime
ls -lu filename    #     列出文件的 atime
ls -l filename     #     列出文件的 mtime
```

2. 长格式
```shell
# ll /tmp
total 8
drwxrwxrwt  2 root root 4096 Apr 28 14:42 ./
drwxr-xr-x 23 root root 4096 Feb 16 20:43 ../
```

一般情况long格式的内容：

首行是512byte为单位的块占用数量,
接下来每一行是一个目录项，每个目录项有这几列

- 文件类型: b 块文件，c 字符文件，d 目录， l 符号链 s 套接字 p 管道 - 普通文件
- 文件权限: rwx- 分别表示读、写、执行、无，目录的执行权限表示目录能不能搜索。如果文件有一些拓展属性 还会加一个@。如果文件还有其他的可选打开方法，还会加一个`+`q
- 硬连接数:
- 所属用户:
- 所属组:
- 文件大小: byte为单位
- 时间戳：会被格式化，一般是文件内容修改时间
- 文件名