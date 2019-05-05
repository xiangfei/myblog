---
title:  sse events in nginx
date: 2019-05-05 16:27:20
tags:
- ops
author: 相飞
comments:
- true
categories:
- nginx
- rails
- django

---


### nginx 配置

+ proxy_set_header Connection '';
+ proxy_buffering off;
+ proxy_cache off;
+ proxy_http_version 1.1;

