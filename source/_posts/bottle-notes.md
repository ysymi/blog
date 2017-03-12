---
title: bottle框架源码学习
date: 2016-03-30 15:48:04
tags: [框架]
---

为了接下来能快速的写那个反馈后台，需要从原理上理解bottle. 之前看过文档，今天准备再进一步，阅读源码.

<!--more-->

这次看得是bottle 0.13-dev的代码， 4000多行


### 介绍
1-21行：
简单的介绍bottle，然后有作者的大名

### 准备工作
23-59行：
主要是为了给server是gevent和eventlet的情况 导入他们必须要用的monkey-patch库 ，monkey-patch就是需要执行的时候动态修改代码的一个patch，因为gevent等异步相关的库会用自己的方法替换掉python标准库中的thread/socket
这里还写了bottle 的 arg_parse 供main使用

70-120行：
导入一些标准库
其中getargspec是标准库inspect里，用来获取方法声明的参数名和默认参数值，
和处理json的函数需要安装simplejson或者标准库里没有

122-219行：
作者尝试着处理py2和py3的不兼容的用法：
python3 才有 as 所以作者调取了sys.exc_info
print在2中是一个statement 而在3中是一个funciton 作者统一到了_stdout和_stderr
然后导入了python3和python2的库 并统一了一些方法
统一了tonat来处理字符串 tob和touni都想转成utf8
这里插播一点知识：当程序有了问题，找出问题然后解决，这叫Fix;而问题没法解决，就像个办法忽略这个错误，使其不能影响程序，这叫 Workaround
有些bug 作者就通过pass 这个exception，
或者写了个不能close的NCTextIOWrapper（never close，用一个空方法覆盖close）
然后写了三个property 特点分别是dict-like，cache-able， lazy ！！！这里没看懂

### 异常
302-304行：
定义了一个BottleException 啥也没写

### 路由
311-330行：
定义了各种route的exception，因为只是集成关系，所以都没有实现

333-340行：
定义了一个替换函数 ！！！但是没看懂替换掉了什么样的字符串

342-545行：
定义了Router类
Router是一个route->target的pair组成的有序集合，target可以是stirng，ID或者callable。route可以是静态或者动态的
！！！没太看懂
主要是有用正则匹配规则然后添加到集合里

548-650行：
定了了Route类
route包括一个callback，运行在指定的一些配置下，
主要是一个_make_callback 函数 也就是call的函数主体 另外一个方法是get_undecorated_callback
可见make_callback 就是在decorate 要跑一遍plugin的操作

### Application
657-1140行：
定了Bottle类 
每一个bottle对象就是一个web 应用 
init里可以看出
routes 是 Route的实例
router 是 Router的实例
config 是一个ConfigDict init只指定了 fatchall和autojson
plugins init的时候可能会装JSONPlugin和TemplatePlugin 

还有新加的hook机制
在init里指定了一些触发时机
随后也有一些添加删除的方法

然后是一个mount方法 将一个以prefix开头的path 这样可以实现命名空间的分离 
比如 parent_app.mount('/prefix/', child_app)      mount的对象可以是一个bottle的实例上也可以是其他的wsgi app上
mount的bottle自己的操作：先检查是否重复挂载，没有以/结尾等问题，如果么问题 就添加app里的数据给parent_app

mount到其他app的操作：
设置了path-depth 然后给option设置了一些值 关键是设定了 app的callback方法 然后将所有prefix下的path都route到option设定的

merge操作 可以合并routes 调用 add_route

install 就是可以加入pulgin 在route的时候 pulgin有一定要求要callable或者有.apply方法
对应的有uninstall

reset方法 清空缓存 重置路由 还要触发一个hook
close方法 关闭plugins
match方法 就是route的match

geturl 算是一个utill 可以获取当前route的所在的url

route方法 最常见的方法 就是字面意思 只不过是一个decorator
会覆盖之前的同一个path的router 主要docoratort的是实例化了一个Route然后append到bottle的route-list里
接下来 get post put delete patch 这5个方法 只是换了不一样的method

_handle
比较重要的方法 先是设置了environ的一些参数 path handle route args等信息
然后调用了route的call 这里主要是有好多try 请求发送出去之前bind envrion 然后bind response 得到route call以后的结构以后 还要这个结果放到 response 还要在发送之前和之后触发对应时机的hook

_cast 给call返回的结果设定一些header 或者进行一些包装 使得能够兼容WSGI
但是没有看到究竟是在哪里调用的

wsgi
这个是bottle的application方法
第一句就是 out = _cast(_handle(environ))
然后是一些特殊情况要直接关闭out
然后start_response 是设定status_line 和 response_header
这里有一个try except 如果fetchall是false 就是可以给 debug用 之前init的地方有写 如果不是 就构造出一个error的response

最后就是一个对内的解释 __call__ 

### Response&Request
1148-1591行 :
BaseRequest
包含了一堆property 其中哦有一部分就是environ 
就是http request的head的一些对应 最后有一个get set del

1597-1612行：
HeaderProperty：
为header而写的一个decorator

1615-1859行：
BaseResponse：
一个存储用的class 存储了 response的header body cookies
这两个类可以看得出来写一个web 应用框架还是很需要http的知识的 中途作者还遵守了几个rfc的要求 
这个类里设定了header 还有 cookie 还是需要看的

1862-1901行：
这里有一个local_property的方法是基于当前线程的local()的方法
然后有两个Local 一个LocalResponse一个LocalRequest 
里边的有两个变量 bind 是 对应的 init 然 body就是local_property 目测用法就是可以随时get set del 当前处理的对象


1905-1930行：
Response就是BaseResponse
Request就是BaseRequest
然后定了一个HttpResponse 除了init方法还有一个apply方法但是给一个叫other的对象强行绑response

### Plugins
1937-1990行：
这里主要是两个内置的Plugin：
JsonPlugin：用之前定义的json_dumps 写一个decoretor 写好content_type 然后return
TemplatePlugin：从route里取出template的配置 然后调用view（一个在后边实现的decoretor）

1995-2024行：
一个叫_ImportRedirect的类 看起来是导入module的但是没看到调用

### Utilities

2031-2131行：
MultiDict 一个key可以有多个value对应的的dict 不用all得到最新的 用all得到全部的

2134-2279行：
一个FormDict 一个HeaderDict 一个WSGIHeaderDict 抢两个基于MultiDict的特定的Dict要有一些特定场合的实现 最后的这个需要处理字符问题 都是要实现一个 get set del 或者len items之类的操作

2282-2416行：
一个ConfigDict
存储configuration的一个class 
可以从module file dict中读取key/value的配置 
具有setdefault 和 update方法 好像还有on_change的方法 就是调一遍listener 
还有meta的get set fallback

2419-2438行：
Appstack 
list模拟的stack 每次返回list末尾的bottle实例

2441-2552行：
WSGIFileWrapper
一个模拟文件的对象 iter 是一个不断读取数据的函数 yield使这一个函数非常简单

2455-2468行：
_closeiter
一个用来给没有close方法的object加上这个方法 

2471-2554行：
ResourceManager
管理resource感觉就是文件用的 可能要配合下面的FileUpload用
除了有个基本的path-list 有add-path lookup open 函数可以用 之外 还有cache的作用

2557-2617行：
FileUpload  表示了一个上传的文件
两个核心的方法 获取filename save到一个地方

### Application Helper
2624-2727行：
主要是3个方法
abort 直接rasie一个异常
redirect 设置一个重定向的response然后返回
static_file 相对比较复杂 要涉及到403 404 minetype if-modified-since range 作者一块一块处理的 最后返回了一个HTTPResponse

### HTTP Utilities
2734-2978行：
这里主要是一些parse的函数 处理date auth range http-header qsl
还有一些对cookie的decode 和 encode html的转义处理
返回一个函数对应的路由 这里可以返回多级匹配到的路由
还有一个path_shift 可以将script 和 path 两部分的目录shift给对方
还有一个auth_basic 是一个decoretor
最后有一个 简单的对外调用的接口 算是一种快捷方式吧


### Server Adapter
2985-3338行 :
有一个ServerAdapter
然后有十几个不同的server继承这个ServerAdapter 都重写了run方法

### Application Control
3345-3486行：
这里写了 load 负责加载module load_app 负责从appstack里去一个实例
然后有一个run 方法 负责跑起来实例 这个方法会一直block到server terminate
先是利用popen检测进程是不是terminate 接下来是正式的处理 获取bottle实例 安装插件 加载server模块 设置quite模式 reload的配置 需要加载下一个FileCheckerThread 除了继承Thread 就是 有一个每隔一个interval 比较一下 mtime 然后中断主进程 但是主进程是怎么重启的呢？

### Template Adapter
3535-3954行：
实现了一个BaseTemplate 有search prepare render config几个方法 然后和 还有作者实现的template adpater 都继承这个BaseTemplate ：moko jinja2 cheetah
作者自己还写了个simpletemplate 然后自己 写了个parseSyntax的类 如果是其他的模板 至少我见过的方法是实现一个插件的形式来用别的模板 

3957-4021行：
这里又做了进一步的封装 
template是已经做了一次整合 template_adapter作为参数拿进来用，然后 写了个view的decoretor 这次主要是为了使用方便吧 

### Constant&Global
4027-4098行：
设置了一些全局的参数的默认值 比如 DEBUG  HTTP_CODE对应的REASON ERROR_PAGE 的内容
然后有几个比较有意思的语句
request response local 都是thread-safe的 分别是当前正在处理的 request response namespace 不过namespace 不是bottle的 
apps app default_app 都是Appstack 
bottle还支持导入 ext的包 如果以后发展开来 这个会用的多吧

4102-4153行：
main函数
取出配置的参数 
什么host port 还有其他的config 一并读取没有问题以后 run 



参考：

http://blog.csdn.net/handsomekang/article/details/40297775
http://my.oschina.net/taisha/blog/77413?fromerr=5koyI0Vc


