---
title: "Linux命令学习: cd"
toc: true
date: 2017-04-28 09:44:41
updated: 2017-04-28 09:44:41
categories: [Linux命令学习]
tags: [linux, shell]
---

cd 是一个shell内置的命令，用于切换当前的工作目录。

<!--more-->

### 命令格式

```shell
cd [-L | [-P ] [dir]

```

### 说明

dir如果没有设置，会从`$HOME`环境变量中取地址。所以直接输入cd 可以进入home目录。
如果 dir 写成 *-* 则会从 `$OLDPWD` 中获取地址。输入cd - 就可以后退到前一个目录。不过不能连续后退，因为 `$OLDPWD` 会被立即置为切换前的目录，这样导致只能在两个目录间跳转。想用更深层次的跳转，可以用 `pushd` 和 `popd` 
另外，还一个`$CDPATH`的环境变量，cd到某个相对路径时，是基于这个变量里的路径来搜索的，这个变量为 null 时，就用当前目录。。

### 常用选项

-L
对于符号链接，这个选项会cd到链接路径。这个选项是默认设置。

-P
对于符号链接，这个选项会cd到真实路径。

例如:

```
/# ll /tmp/test
lrwxr-xr-x 1 root cdroot 14 Mar 31 08:24  /tmp/test-> /data/test

# cd -L /tmp/test
/tmp/test#

# cd -P /tmp/test
/data/test#
```

### 小技巧 

1. 可以设置几个 alias 加速一下回退

```
alias "..."="../.."
alias "...."="../../.."
alias "....."="../../../.."
````

2. zsh提供的 cd 自带了路径栈功能，可以简单的用一下。感觉并不好用