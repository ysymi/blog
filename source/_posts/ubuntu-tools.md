---
title: ubuntu-tools
date: 2016-04-01 13:54:01
tags: ubuntu
---

中午瞌睡的时候 不小心把 /etc目录给删掉了 没有办法拷别人的etc 只好重装
顺便记一下必备的工具

#### 1. chrome 
从[官网](https://www.google.com/chrome/browser/desktop/index.html)下载安装包
```bash
dpkg -i  google-chrome-stable_current_amd64.deb
```
#### 2. sogou pinyin 
从[官网](http://pinyin.sogou.com/linux/)下载安装包 如果没装fcitx 要先装fcitx
```bash
sudo apt-get install fcitx
dpkg -i sogoupinyin_2.0.0.0068_amd64.deb
sudo apt-get install -f -y
```
all setttings -> language support -> keyboard input method system
选择fcitx 然后 重启

#### 3. sublime-text
```bash
sudo add-apt-repository ppa:webupd8team/sublime-text-3
sudo apt-get update
sudo apt-get install sublime-text-installer
```
从[这里](https://packagecontrol.io/installation)可以找到Package Control的安装脚本
上次发现的超酷炫的[theme](https://github.com/kkga/spacegray/)

#### 4. pycharm 
一个很强大的py ide 下载地址在[这里](https://www.jetbrains.com/pycharm/download/)
``` bash
tar xfz pycharm-professional-2016.1.2.tar.gz
cd pycharm-professional-2016.1.2/bin
install.sh
```
然后发现个可以注册lience的[地方](http://idea.qinxi1992.cn/)

#### 5. vim
```bash
sudo apt-get install vim 
```
配置文件 ~/.vimrc 还好已经备份

#### 6. ssh
```bash
sudo apt-get install openssh-server
```
直接拷贝 .ssh 文件夹里的两个key
或者重新生成新的

#### 6. git 没什么可多说的 
```bash
sudo apt-get install git-core
```

#### 7. terminator 
还有个tmux大家都在用 有空也研究一下
```bash
sudo apt-get install terminator
sudo update-alternatives --config x-terminal-emulator
```

#### 7. java 
跟c/c++一样 有一些软件得有java环境才能运行
```bash
sudo add-apt-repository ppa:webupd8team/java 
sudo apt-get update 
sudo apt-get install oracle-java8-installer
java -version
```
还有JAVA_HOME这个环境变量
```bash 
 export JAVA_HOME=/usr/lib/jvm/java-8-oracle/ # ~/.bashrc 
 or
 JAVA_HOME="/usr/lib/jvm/java-8-oracle/"      # /etc/environment
```
 
 
 参考：
 
 http://blog.csdn.net/tao_627/article/details/45054057
 