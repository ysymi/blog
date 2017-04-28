---
title: "Linux命令学习: mkdir & rmdir"
toc: true
date: 2017-04-28 09:44:41
updated: 2017-04-28 09:44:41
tags: [linux, shell]
---

mkdir 和 rmdir 是一对相反的命令，一个用来创建目录，一个用来删除目录。

<!--more-->

## mkdir 


### 说明

mkdir 是创建目录的命令。这个操作要求用户有父目录的写权限。新建的目录名不能和已有的目录重名。默认情况下新建目录的权限由777和当前umask决定。

### 命令格式

```
mkdir [-pvm <mode>] dir [dir ..]
```

### 常见选项

-m, --mode=<mode>
创建特定mode的目录，<mode> 可以是`[ugoa]*([-+=]([rwxXst]*|[ugo]))+|[-+=][0-7]+`。通常目录是644。

-p,  --parent 
如果中途某一级目录不存在，会自动创建缺少的目录。没有这个选项的时候，会创建失败

-v, --verbose
打印出创建每一个目录时的详情 

## rmdir

### 说明

rmdir 是删除目录的命令。这个操作同样要求用户有父目录的写权限。只能用于删除空目录

### 命令格式

```
rmdir [-pv] dir 
```

### 常见选项：

-p  --parent 
默认只删除指定的目录，加入这个选项会删除每一级目录。比如`rmdir -p a/b/c` 就相当于 `rmdir a/b/c a/b a`

-v --verbose
打印出创建每一个目录时的详情 