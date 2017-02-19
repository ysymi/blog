---
title: terminal和Shell
date: 2016-09-15 10:31:29
tags:
---
终端、Shell、控制台，这几个词确实带着久远历史的气息，天天用，但是却不知道他们的故事，多少还是有点不应该的哈。

# 什么是终端 

计算机核心部分是主机，包括内存，处理器，磁盘等，外围设备是为了与人交互，以便机器能够接受指令和显示结果。所以说终端实际上指的是硬件，是进行人机交互的接口。

>计算机 = 主机 + 终端<br>
>终端 = 输入设备 + 输出设备

早期的计算机，显示器和键盘都集成在主机上，只能同时有一个用户进行操作。
[Ken Thompson](https://zh.wikipedia.org/zh/%E8%82%AF%C2%B7%E6%B1%A4%E6%99%AE%E9%80%8A) 和 [Dennis Ritchie](https://zh.wikipedia.org/wiki/%E4%B8%B9%E5%B0%BC%E6%96%AF%C2%B7%E9%87%8C%E5%A5%87)使用了一种叫做电传打字机的廉价设备作为输入设备，输出结果打印在纸上。可以使用多组输入输出设备，就能实现多用户的同时操作。<br>
电传打字机，很别扭的名字，英文是Teletypes，缩写tty，输入who命令的时候能看到这个缩写。

# 什么是控制台

刚刚输入who命令时，第一行tty的名字是console，那这个控制台又是什么呢。通常终端都是通过线路连接到主机。有一个终端却与众不同，它与主机是一体的，不需要连线。这个特殊的终端就是console。console与terminal的角色大概类似于皇后和妃子。当系统启动出现错误时，错误信息会显示在console的显示器屏幕上，而不会显示在一般的终端上。这是因为系统还没有成功启动，用户是不能在一般的终端登录系统的。另外，当主机需要维护或修复问题时，Unix以单用户模式启动(single-user mode)。在单用户模式下，只有console才能连接到主机，其他终端没有权限访问主机。

以前，console的用户是机器的管理员，terminal的用户是普通用户。随着个人计算机的普及，每个人都是自己机器的管理员，console和terminal也就没什么区别了。

# 什么是终端模拟器

现在我们应该知道真实的终端是指输入输出用的硬件，而我们日常使用的Mac下的Iterm2，终端应用，ubuntu下的XTerm，UXTerm，Windows下的命令提示符，这些都是终端模拟器（Terminal Emulator），主机认为他们就是真正的终端。

终端分字符终端(Character Terminal)和图形终端(Graphics Terminal)，字符终端就是我们常看到的终端，图形终端没见过，不知道是不是示波器那种。终端模拟器也有两种，一种是终端窗口，另一种是虚拟控制台。我们看到的命令行是模拟字符终端的终端窗口。虚拟控制台的话，比如Linux会同时启动七个不同的虚拟控制台。第一个到第六个虚拟控制台是全屏的字符终端，第七个虚拟控制台是图形终端，用来运行GUI程序。从图形终端切换到字符终端，我们只需按快捷键Ctrl+Alt+F1，或 Ctrl+Alt+F2…….Ctrl+Alt+F6。要切换回图形终端，只需按快捷键Ctrl+Alt+F7。当图形终端崩溃时，我们可以按快捷键切换到这六个字符终端的其中一个，然后输入命令修复问题或重启系统。

为什么会有终端模拟器呢？可能是为了远程登录别人的计算机，也可能是为了让新的输入输出设备兼容以前的计算机。或者是为了并行工作，人比机器慢，软件实现的终端可以复用硬件终端！还能比物理终端更好用，更强大。

# 什么是Shell

怎么强大呢？这里要说的是Shell。Shell是什么呢？脚本语言？脚本引擎？<br>

Shell是一种命令解释器，它接受用户输入的指令，将其转换为一系列系统调用并创建进程执行。
比如，当你执行`rm *.py`时，删除当前路径下的所有python文件，实际上是执行 `rm demo1.py demo2.py demo3.py`，谁去匹配`*.py`呢？肯定不是rm命令，不符合unix的哲学。答案是，Shell。<br>
它之所以被称作Shell是因为它隐藏了操作系统低层的细节,就像一个外壳一样包着里边的系统内核。Shell实际上是人和操作系统间交互的接口。类似编译器是将高级语言翻译成机器语言，那Shell就是把命令翻译成系统调用。<br>

图形化的界面也是一种Shell，右键单击新建文件夹的时候，跟你输入mkdir没什么区别，对吧？
只不过这个命令解释器支持一些复杂的逻辑，就把命令上升到的了脚本的层次。也就有了所谓的Shell编程，只不过大家讨论Shell脚本更多一些，就把Shell和Shell脚本当一回事了。和python一样，既支持交互式也支持脚本模式，其实应该是python借鉴Shell的运行方式。
Shell有各种实现，其中bash是众多Shell的集大成者。

Shell和终端模拟器，一个是负责软件层面用户和操作系统的交互，一个是负责硬件层面用户和机器的交互。

# 小结 

>万物之源是Unix，万变不离其宗。

一个计算机系统需要与人交互，就要有外围的输入输出设备，即终端。管理员的终端叫控制台，root角色的终端。在一个多用户的系统中，多终端显然是硬件上的一个必要条件。发展出虚拟终端以后，用户在终端窗口内输入命令，传给连接着主机的进程，这个进程就是Shell。Shell以为自己跟真实的终端相连，其实他是跟终端模拟器相连。

总的来看，人跟机器的交互有很了很大变化，但是底层的机制还是一样的。

PS. Uinx系统一开始就设计为一个单主机多终端的多用户系统。比如说，我们可以用多套显示器和键盘鼠标连接同一个机箱（有线或者无线，直连或者远程，前提是有权限），来构建一个可多人同时使用的计算机系统。windows系统虽然号称是多任务多用户系统，但windows对多用户的定义是可以创建多个账号。在windows系统上，同一时间只能登录一个用户账号。Unix-like系统允许多个账号同时登录，是真正的多用户系统.

参考：

1. [https://www.linuxdashen.com/你真的知道什么是终端吗？](https://www.linuxdashen.com/你真的知道什么是终端吗？)
2. [https://zh.wikipedia.org/wiki/虚拟终端](https://zh.wikipedia.org/wiki/虚拟终端)
3. [https://www.zhihu.com/question/21711307](https://www.zhihu.com/question/21711307)
4. [http://blog.csdn.net/on_1y/article/details/20203963](http://blog.csdn.net/on_1y/article/details/20203963)