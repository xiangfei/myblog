---
title: opennebula 第三方接口,rest api
date: 2019-04-04 20:25:58
author: 相飞
comments:
- true
tags:
- opennebula
categories:
- opennebula

---
###   ruby xml prc 
```
require 'xmlrpc/client'
server = XMLRPC::Client.new("opennebulaserver", "/RPC2", 2633)
server.call("one.system.version", "oneadmin:oneadmin")
```

> java ruby sdk  ,command line 需要在master机器运行 


