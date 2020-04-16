---
title: ruby  agent 内存增长
date: 2020-04-16 15:24:49
tags: ruby
author: 相飞
comments:
- true
categories:
- ruby

---



#### ruby collect 内存一直增加

reason ruby 申请的内存不会释放,需要自己去解决


- 代码启动过高的线程
- 代码可能导致内存泄露


解决方式

1.  god  killer 

2. 线程换成进程处理








