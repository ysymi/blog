---
title: Python之禅
toc: true
date: 2017-03-23 21:44:34
updated: 2017-03-23 21:44:34
tags:
---

[Guido van Rossum](https://zh.wikipedia.org/wiki/%E5%90%89%E5%A4%9A%C2%B7%E8%8C%83%E7%BD%97%E8%8B%8F%E5%A7%86)发明了Python，并确立了Python的指导原则，他被社区称为仁慈的独裁者（Benevolent Dictator For Life，BDFL）。 Python开发者Tim Peters将这些指导原则写成了非常简洁的20句格言，但是其中只有19句被记下来了。

<!--more-->

### 原文

> The Zen of Python, by Tim Peters
>  
> Beautiful is better than ugly.
> Explicit is better than implicit.
> Simple is better than complex.
> Complex is better than complicated.
> Flat is better than nested.
> Sparse is better than dense.
> Readability counts.
> Special cases aren't special enough to break the rules.
> Although practicality beats purity.
> Errors should never pass silently.
> Unless explicitly silenced.
> In the face of ambiguity, refuse the temptation to guess.
> There should be one-- and preferably only one --obvious way to do it.
> Although that way may not be obvious at first unless you're Dutch.
> Now is better than never.
> Although never is often better than **right** now.
> If the implementation is hard to explain, it's a bad idea.
> If the implementation is easy to explain, it may be a good idea.
> Namespaces are one honking great idea -- let's do more of those!

### 翻译
不想解释，只想翻译，自己感受其中的禅意:

> 美观优于丑陋
> 明确胜过隐晦
> 简单优于复杂
> 复杂胜过混乱
> 扁平优于嵌套
> 间隔胜于紧凑
> 可读性很重要
> 特例不应该破坏规则
> 即使特例会更实用
> 不要包容所有错误
> 除非你确定需要这样做
> 存在多种可能时，不要尝试去猜测
> 最好有且只有一种明确的解决方案
> 即使那个方案刚开始并不明确，除非你是Guido
> 做好过不做
> 但不假思索就动手不如不做
> 如果你的方案很难描述，那肯定不是个好方案
> 如果你的方案很容易描述，那可能是个好方案
> 命名空间是一种绝妙的理念，我们要多加利用

### 说明
14行需要说明:

python刚开始并不流行的时候，只是写脚本的人用来代替bash，有一些特性或者功能被不加考虑的加入到到python当中。比如 'backtick' 操作符实际上是 repr() 的语法糖。 上一行说了，应该有且只有一种好的方案，但是创建python的时候因为历史原因，没有遵循这一原则。因为，你不是Guido，2333。但是事实上，Guido一直在努力平衡实用性和极简性上，比如到现在python也没有一个三元操作符，因为已经有一个解决方案了。

简单说，这句话其实是劝大家，python里已经有的好轮子，大家就不要再造了。

[参考](http://stackoverflow.com/questions/2470761/what-does-this-sentence-mean-in-the-zen-of-python)
### 彩蛋

在python终端里执行 `import this` 会打印出这段Python之禅。有意思的是，这段Python之禅并不是明文存储的，而是用ROT13算法加密的，看了代码会感受到一种莫名的幽默。

最后，有一个遵循pep20的[例子](http://artifex.org/~hblanks/talks/2011/pep20_by_example.pdf)
