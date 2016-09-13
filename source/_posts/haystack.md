---
title: haystack
date: 2016-03-22 23:51:38
tags:
---

### 场景
每个数据只会写入一次、读操作频繁、从不修改、很少删除

### 背景&现状
现有的缺点：
目录 导致 磁盘操作次数多 费时
文件元数据导致 存取效率 导致费时
现有的架构：
browser 请求 server 返回 cdn 或者 直接请求 cdn 
cdn 后边是 stroage 


### 系统特点
1. 分享照片而设计的对象存储技术
2. 高吞吐量和低延迟 减少磁盘操作 
3. 高容错 异地备份 
4. 高性价比 
5. 简单 容易开发和部署 （简约而不简单）


### 初期设计
改动：cdn后边的stroage 变成 stroage server + nas（nas: 以数据为中心，将存储设备与服务器彻底分离，集中管理数据，从而释放带宽、提高性能、降低总拥有成本、保护投资的设备，其实是一个存储服务器。其成本远远低于使用服务器存储，而效率却远远高于后者）
优化：限制目录下文件数量（减少磁盘操作）
      文件打开 使用缓存（memcache）fd的方式 但是依旧不能解决冷数据的问题
小结：冷图片的问题需要单独拿出来解决 不能用钱解决ram/disk的比率而是要提高内存使用效率

### haystack设计
cdn尽管为底层存储挡住了大部分请求 但是还是不够用（因为冷图很难进cdn，所以long tail请求是导致磁盘操作的原因）
所以需要解决底层io的瓶颈 bottleneck 通过减少无用元数据 同时打包图片成一个大文件 维护这个大文件
三个模块：Store 持久化保存原始数据 每个volume 100G 多个physic对应一个logic
Directory 负责维护 volume信息和关系 image的一些更有用的元信息 Cache 在cdn后边的一个部署一个 内部cdn 为Store挡请求 减小外部cdn的依赖
url的设计 也体现了这个架构的特点上层失效才去下层拿数据，直到底层：
cdn/cache/store/volume/offset

接下来是更细致的介绍

1. Directory 
四个作用：
保存logic到physic的关系
对写logic和读physic两种操作做均衡 
分发photo处理对象cdn or cache 路由的功能
指定volume状态 例如reaonly 粒度是machine

2. Cache 
直接返回request里的所需的图片 没有命中就可以从cdn里取 大致上就是一个分布式hash表 

3. Store 
一个很简单的存储系统 给一些信息 然后成功返回图片 否则给错误提示
结构是每个机器好多个volunme 每个volume好多个file 给定volume id 和 offset 就可以定位文件（Haystack design: retrieving the filename, offset, and size for a particular photo without needing disk operations） 内存中会有voluem的fd 和有一个image id to meta数据的map
每个needle就是一个file 
刚刚说的那个map 就是 key，alternative-key 到 offset，size，flag的一个映射
用来快速检索needle 机器crash后可以全盘扫描后生成这个map
三种基本操作：
读 cookie 随上传时间给定 可以保证安全性 防止伪造url攻击 map里取出value后要验证cookie 和 flag才返回给cache 然后cache在网上返回
写 先从Directory里获取volume位置 然后提供cookie key alternativekey 和原数据 往logic写入的同时 physic也写入 准确的说是append。map也要更新。对于修改的操作 要用相同的key 和 alternativekey 添加一个新的neddle 如果新needle在这个volume，更大的offset意味着更新数据 如果添加啊到别的volume 就在directory上进行改动
删 更新 map（内存中） 超级块中的flag 标记删除

4. index
为了重启后快速恢复内存里的map 每一个volume有一个index 这个index布局与volume中的needle **类似** 顺序与volume中的needle **相同** 内容就map的key+value
写文件时map是同步更新的 而index是异步更新的 这意味着index不是最新的 
删文件时 也不会去更新index 这意味着index不能反映图片是否删除
没有index记录的needle是一个orphan 重启时可以检查（从index末尾也就是volume末尾开始处理orphan）然后才恢复map
删除的问题 在检索的时候 可以通过取出文件后检查原数据中的flag来更新map

5. 故障恢复
这部分主要有两个工作 探测 和 修复 
探测叫pitch-fork 定期远程测试machine 测试连接 avail size 等 如果有问题就直接标记为readonly 线下人工修复
修复一般很快 只有非常严重的情况 需要大量同步 这种修复很耗时 因为nic有上限

6. 优化
这里有几个很有意义的优化：
**压缩** 回收重复 已删除文件的空间 就是从现有volume复制到新volume跳过不需要的 needle Directory也需要做改动
**内存效率** 之前好奇index的地方为什么检索的时候不查map的flag 原因是map里其实不存flag和cookie offset=0表示删掉了 取出来才检查cookie（问题不但没解决反而多了 1.那为什么检索的时候不看map的offset呢？2.忘掉了 囧... 实在是不能想起来了）
还有一点就是减小了每一个image inode需要使用内存
**批量上传** 大型连续写时的性能要比随机写好





