---
title: What is WSGI
date: 2016-03-09 16:52:39
tags: [python, wsgi]
---

WSGI（Web Server Gateway Interface，Web服务器网关接口，读成'wiz-gee'？）是Python语言定义的server或gateway与client或framework之间的一种简单而通用的接口，是一种标准，规范

<!--more-->
之前，Web应用框架的选择将限制可用的Web服务器的选择，反之亦然。而统一了两者交互的接口之后，只要符合这个规范，就能更自由的选择框架和服务器的组合了，同时应用可移植性提高了。

WSGI是基于现存的CGI标准而设计的。
什么是CGI？
一个网站的动态内容肯定是程序生成的，光是静态的html页面无法达到这个效果。那么，这个程序就需要接受客户端的请求，然后进行相应处理，再返回给客户端。然后我们会发现，这个程序在处理客户端请求的时候，大部分时候会进行很多重复的工作，比如说HTTP请求的解析。也就是说，你的程序需要解析HTTP请求，我的程序也需要解析。

于是为了DRY原则，Web服务器诞生了。（以下所说的都是CGI的工作模式）

Web服务器可以解析这个HTTP请求，然后把这个请求的各种参数写进进程的环境变量，比如REQUEST_METHOD，PATH_INFO之类的。之后呢，服务器会调用相应的程序来处理这个请求，它会负责生成动态内容，然后返回给服务器，再由服务器转交给客户端。服务器和CGI程序之间通信，一般是通过进程的环境变量和管道。

这样做虽然很清晰，但缺点就是每次有请求，服务器都会fork and exec，每次都会有一个新的进程产生，开销还是比较大的。

而WSGI的规范是，有三个部分server，application，middleware。

client发出request给server，
server将这个reuqest交给application处理，这个过程有一个middleware掺进来。middleware拦截（或者由server传给middleware）request，并做一些包装（或者预处理）以后传给application，application处理完给出response，同样经过middleware包装传给server，然后server将这个response返回给client。

```python
def application(environ,start_response):
    do_something
    start_response
```
environ是一个dict，app 执行所需的数据，
start_response是一个callback，供上层调用，比如返回时包装用

可见 middleware在server和application之间起调解作用：从WSGI服务器的角度来说，中间件扮演应用程序，而从应用程序的角度来说，中间件扮演服务器。

所以middleware要具备一些功能，比如route（负载均衡），包装req和rsp，render（内容处理）
从设计模式的角度看 middleware 就是装饰器模式
middleware的好处 大概就是会细化工作，解耦server上的工作，可移植性

可以说别的语言中的容器 就是一种middleware 

所以看现在的bottle 到底是个什么
Bottle is a fast, simple and lightweight WSGI micro web-framework for Python.

 - Routing
 - Template
 - Utilities
 - Server
 
这下清晰一点了


参考
https://www.zhihu.com/question/19998865/answer/29395327
https://zh.wikipedia.org/wiki/Web服务器网关接口
https://segmentfault.com/a/1190000003069785


