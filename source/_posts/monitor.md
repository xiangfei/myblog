---
title: mysql 监控
date: 2020-07-16 09:47:17
author: 相飞
comments:
- true
tags:
- monitor
- mysql
categories:
- monitor
- mysql
---

## 常见的mysql监控指标

- tps 
 ``` mysql
 MariaDB [(none)]> show global status where variable_name in ('com_insert' , 'com_delete' , 'com_update', 'uptime');
 ```
 > 事务数TC ≈'com_insert' , 'com_delete' , 'com_update'
 > TPS  ≈ (TC2 -TC1) / (uptime2 - uptime1)
 > uptime 数据库运行时间
- qps
  ```
  MariaDB [(none)]> show global status where variable_name in ('Queries', 'uptime');
  ```
  > QPS = (Queries2 -Queries1) / (uptime2 - uptime1)

- max_connection ,connection

  ```mysql
   MariaDB [(none)]> show global status like 'Threads_connected'; --当前连接数
   MariaDB [(none)]> show global status like 'max_connections';  --最大连接数

  ```
 > 可以做一个百分比告警
 

- 并发数

  ```mysql
  MariaDB [(none)]> show global status like 'Threads_running';
  ```

- 缓存命中率
 ```mysql 
  MariaDB [(none)]> show global status like 'innodb_buffer_pool_read_requests';  -- innodb缓冲池查询总数：
  MariaDB [(none)]> show global status like 'innodb_buffer_pool_reads';  --  innodb从磁盘查询数：

  ```
  > 生产中配置报警阈值：(innodb_buffer_pool_read_requests - innodb_buffer_pool_reads) / innodb_buffer_pool_read_requests > 0.95 

- 慢查询Slow_queries

  ```mysql  
  MariaDB [(none)]>  show status like 'Slow_queries';
  ```
  > 超过该值（--long-query-time）的查询数量，或没有使用索引查询数量。对于全部查询会有小的冲突。如果该值增长，表明系统有性能问题

