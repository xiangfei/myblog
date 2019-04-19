---
title: opentsdb api使用常见问题
date: 2019-04-19 14:20:23
tags: ops
author: 相飞
comments:
- true
categories:
- opentsdb

---

###  openstsdb使用问题
#### opentsdb api 查找lastdata 返回空

```

解决方法

opentsdb.conf

tsd.core.auto_create_metrics = true
tsd.core.meta.enable_realtime_ts = true
tsd.core.meta.enable_tsuid_tracking = true
tsd.core.meta.enable_realtime_uid = true
tsd.core.meta.enable_tsuid_incrementing = true	

同步数据
tsdb  uid  metasync

```

#### opentsdb api 查找提示用重复数据


```
fsck --full-scan --threads=8 --fix --resolve-duplicates --compact

查询api  


    data = {
      metric: "device.heart.data.new",
      timestamp: timestamp,
      value: currentstatuscode,
      tags: {
        mac: mac,
        signal: signal,
        key: key,
        keyvalue: value,  #如果字段是value ,会产生 
      },
    }


```
