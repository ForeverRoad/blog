---
title: wrk
date: 2021-12-28 19:05:54
tags: util
---

### 压测工具wrk

> 一个简单的 http benchmark 工具, 能做很多基本的 http 性能测试.
wrk 的一个很好的特性就是能用很少的线程压出很大的并发量.
原因是它使用了一些操作系统特定的高性能 io 机制, 比如 select, epoll, kqueue 等.


<!-- more -->


### 安装

1. 下载源码：git clone https://github.com/wg/wrk.git
2. 进入wrk目录并执行make（make失败的话请注意报错进行相关内容安装，如gcc，OpenSSL）


### 示例


1. wrk -t8 -c200 -d30s --timeout 2s --latency --script=business.lua  http://www.baidu.com
2. wrk是make后生成的可执行文件，有的需要./进行执行
3. -t 表示多少个进程，一般是核心线程的2-4倍
4. -c 表示模拟多少个连接，通过网络异步io达到并发效果，类似redis的异步事件驱动框架.
5. -d 表示持续时间
6. --timeout 表示超时时间
7. --script 调用lua表达式，可以放http请求的header，body，method等
8. --latency 在结果中查看响应时间分布



### 结果

```
 Thread Stats   Avg      Stdev     Max   +/- Stdev
    Latency   438.72ms  152.19ms   1.19s    64.74%
    Req/Sec    57.39     29.41   181.00     72.54%
  Latency Distribution
     50%  431.62ms
     75%  522.57ms
     90%  614.66ms
     99%  817.77ms
  13652 requests in 30.04s, 5.16MB read 
  Non-2xx or 3xx responses: 13277
Requests/sec:    454.53
Transfer/sec:    176.03KB
```

