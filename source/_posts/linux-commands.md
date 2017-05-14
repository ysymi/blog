---
title: Linux命令与工具学习
toc: true
date: 2017-04-28 18:14:46
updated: 2017-04-28 18:14:46
categories: [Linux命令学习]
tags: [Linux]
---

这是一个学习Linux命令和工具的list，我会不断学习和总结自己遇到过的命令。

<!--more-->

## shell 内置
- ** [echo](/posts/linux-echo/): 打印内容到屏幕上 **
- ** alias: 给一个程序起别名，通常被用来做快捷方式 **
- ** read: 从终端读入一个值 **
- ** cd: 更改当前工作目录(change directory) **

### 暂时分不出来

- ** ssh: (security shell) 实现ssh协议的软件，一般是指openssh **
- ** dd: (data duplicator) 从一个文件拷贝数据到另一个文件 **
- ** info **
- ** man ** 打开命令的手册页
- ** setterm ** 对终端进行设定


### 其他
- ** xargs 将文本处理后作为某个命令的参数 ** 
- ** md5sum 使用md5算法计算文件校验和 **
- ** sha1sum 使用sha1算法计算文件校验和 **
- ** sort 对指定文本进行排序**
- ** uniq 过滤指定文本中的重复行**
- ** mktemp 在/tmp目录下简历一个临时文件或目录**
- ** expect **

### 文件管理
- ** ls list 列出目录中的内容**
- ** mv move 移动文件**
- ** rm remove 删除文件**
- ** touch 摸一下文件，更新access time**
- ** mkdir make directory 新建目录 **
- ** rmdir remove directory 删除目录**
- ** find 查找满足条件的文件，并做处理  **
- ** chmod 修改文件模式位和ACL权限控制 **
- ** chown 修改文件归属 **

### 文件系统
- ** ln link 建立链接文件 **
- ** file 查看文件类型**
- ** readlink **
- ** stat 查看文件各项信息**
- ** mount & umount 把某个目录挂载到某个挂载点 **
- ** popd & pushd 利用栈管理目录 **
- ** tree 以树的形式直观的展示目录结构 **

### 文本处理
- ** awk 一个基于行的文本的编辑器**
- ** sed 一个将文本看做是流的编辑器**
- ** heada 显示一个文件的起始部分 **
- ** tail 显示一个文件的末尾部分 **
- ** cut 摘取某些列的文本**
- ** cat & tac 将多个文件中的内容拼接起来 **
- ** tee 拷贝标准输出的内容并输出到一个文件中 **
- ** tr 将文本中的某一写字符替换成对应的字符 **
- ** grep 搜索出文本匹配模式的行 **
- ** diff & patch  对比两个文件的内容**
- ** wc 统计某个文件中单词数、行数等信息**
- ** split 基于行的文本切分**

### web
- ** wget web getter 一个非交互式的web文件下载器 **
- ** curl see URL 支持多种协议的命令行数据传输工具**

### 网络
- ** ping 测试数据包能否通过IP协议到达 某个主机**
- ** nc net cat 显示网络中的数据?**
- ** ip 管理网络网络的工具 **
- ** ifconfig 配置网络设备的工具 **
- ** router 管理路由器的工具 **
- ** dig 查询域名服务器的工具**
- ** host 查询域名的工具**
- ** lsof 列出打开的文件 **
- ** iptables ip规则管理，防火墙**

### 系统信息 
- ** uptime 显示系统运行时间和负载信息 **
- ** top 显示占CPU资源的进程信息**
- ** iotop 显示占IO资源的进程信息
- ** iftop 按key显示网络流量情况 **
- ** iostat 显示IO资源的占用情况**
- ** netstat 显示网络情况**
- ** du 显示磁盘的使用情况**
- ** df 显示可用的磁盘空间**
- ** fdisk 管理磁盘分区的工具**
- ** hostname 管理主机名 **
- ** uname unix name 显示操作系统信息 **
- ** w 显示登录的用户和他们执行的命令**
- ** who 显示登录的用户**


### 进程
- ** ps 进程信息统计**
- ** pgrep 按照名字查找进程 **
- ** pkill 给对应名字的进程传信号 ** 
- ** kill & killall 按照pid或者name给进程发送信号 **

### 备份

- ** zip 打包和压缩工具**
- ** tar 打包和压缩工具**
- ** gzip & ungzip 压缩和解压工具 **
- ** rsync 同步工具 **
- ** scp 基于ssh的拷贝工具**

### 系统
- ** crontab 系统自带的周期性任务管理工具 **
- ** which 查找某个命令的全路径 **
- ** whereis 显示一个命令和手册页的路径 **
- ** whatis 显示一个命令的路径和一句话描述 **
- ** uptime  显示系统运行时间与负载**
- ** wall 给所有登录用户发送内容 **

### 用户管理
- ** useradd 添加用户 **
- ** deluser 删除用户 **
- ** chsh 修改某用户的默认shell**
- ** chage 修改某个用户过期时间 **
- ** passwd 修改某个用户的密码**
- ** addgroup 添加用户组**
- ** delgroup 删除用户组**


### 小工具

- ** cal 显示日历的小工具 **
- ** date 日期和时间的工具**
- ** bc 一个整数计算器 **
- ** od 显示用不同格式输出文本内容**
- ** seq 生成一个序列**
- ** time 统计一个命令执行时间 **
- ** watch 监视一个命令的输出 **
