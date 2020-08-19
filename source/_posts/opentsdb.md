---
title: opentsdb 2.4.0 集群安装
date: 2020-08-14 08:40:51
author: 相飞
comments:
- true
tags:
- opentsdb
- bigdata
categories:
- opentsdb
- bigdata

---



###  准备

#### 机器
|  vip   | ip   |  host  | OS |
|  ----   | ----  | ---- |  ----  |
| 192.168.151.200 |  192.168.226.64   | hadoop-cluster-1   |  centos 7  |
| 192.168.151.200 |  192.168.226.65    | hadoop-cluster-2 |  centos 7  |
| 192.168.151.200 |  192.168.226.63    | hadoop-cluster-3 |  centos 7  |
|     |  192.168.226.66   |  hadoop-datanode01  |  centos 7  |
|     |  192.168.226.67   |  hadoop-datanode02  |  centos 7  |
|     |  192.168.226.68   |  hadoop-datanode03  |  centos 7  |
|     |  192.168.226.69   |  hadoop-datanode04  |  centos 7  |


#### cluster node

- config opentsdb

####  data node

- not used




### 依赖安装


```bash

[root@hadoop-cluster-3 bin]# yum install gnuplot.x86_64  gnuplot-common.x86_64  git -y
[root@hadoop-cluster-3 bin]# yum groupinstall 'Development Tools' -y

```



### 安装opentsb 

 - zk  hbase opentsdb  在同一台机器



#### git 下载 rpm文件
  - https://github.com/OpenTSDB/opentsdb/releases/tag/v2.4.0 

```
[root@hadoop-cluster-2 tools]# ls
apache-zookeeper-3.6.0-bin  hadoop-3.3.0  hbase-2.3.0  hbase-2.3.0-bin.tar.gz  opentsdb-2.4.0.noarch.rpm
[root@hadoop-cluster-2 tools]# yum install .
./  ../ 
[root@hadoop-cluster-2 tools]# yum install ./opentsdb-2.4.0.noarch.rpm  
Loaded plugins: fastestmirror
Examining ./opentsdb-2.4.0.noarch.rpm: opentsdb-2.4.0-1.noarch
Marking ./opentsdb-2.4.0.noarch.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package opentsdb.noarch 0:2.4.0-1 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

===========================================================================================================================================================================
 Package                               Arch                                Version                               Repository                                           Size
===========================================================================================================================================================================
Installing:
 opentsdb                              noarch                              2.4.0-1                               /opentsdb-2.4.0.noarch                               14 M

Transaction Summary
===========================================================================================================================================================================
Install  1 Package

Total size: 14 M
Installed size: 14 M
Is this ok [y/d/N]: y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : opentsdb-2.4.0-1.noarch                                                                                                                                 1/1 
  Verifying  : opentsdb-2.4.0-1.noarch                                                                                                                                 1/1 

Installed:
  opentsdb.noarch 0:2.4.0-1                                                                                                                                                

Complete!

```

#### 创建表


```
[root@hadoop-cluster-2 tools]# env COMPRESSION=NONE HBASE_HOME=/data/tools/hbase-2.3.0  /usr/share/opentsdb/tools/create_table.sh
2020-08-19 03:21:22,421 WARN  [main] util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
HBase Shell
Use "help" to get list of supported commands.
Use "exit" to quit this interactive shell.
For Reference, please visit: http://hbase.apache.org/2.0/book.html#shell
Version 2.3.0, re0e1382705c59d3fb3ad8f5bff720a9dc7120fb8, Mon Jul  6 22:27:43 UTC 2020
Took 0.0016 seconds                                                                                                                                                        
create 'tsdb-uid',
  {NAME => 'id', COMPRESSION => 'NONE', BLOOMFILTER => 'ROW', DATA_BLOCK_ENCODING => 'DIFF'},
  {NAME => 'name', COMPRESSION => 'NONE', BLOOMFILTER => 'ROW', DATA_BLOCK_ENCODING => 'DIFF'}
Created table tsdb-uid
Took 4.1026 seconds                                                                                                                                                        
Hbase::Table - tsdb-uid

create 'tsdb',
  {NAME => 't', VERSIONS => 1, COMPRESSION => 'NONE', BLOOMFILTER => 'ROW', DATA_BLOCK_ENCODING => 'DIFF', TTL => 'FOREVER'}
Created table tsdb
Took 2.1730 seconds                                                                                                                                                        
Hbase::Table - tsdb
  
create 'tsdb-tree',
  {NAME => 't', VERSIONS => 1, COMPRESSION => 'NONE', BLOOMFILTER => 'ROW', DATA_BLOCK_ENCODING => 'DIFF'}
Created table tsdb-tree
Took 2.1679 seconds                                                                                                                                                        
Hbase::Table - tsdb-tree
  
create 'tsdb-meta',
  {NAME => 'name', COMPRESSION => 'NONE', BLOOMFILTER => 'ROW', DATA_BLOCK_ENCODING => 'DIFF'}
Created table tsdb-meta
Took 2.1447 seconds                                                                                                                                                        
Hbase::Table - tsdb-meta


```


#### hbase table 校验

```bash
[root@hadoop-cluster-3 bin]# ./hbase shell

2020-08-19 03:22:45,560 WARN  [main] util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
HBase Shell
Use "help" to get list of supported commands.
Use "exit" to quit this interactive shell.
For Reference, please visit: http://hbase.apache.org/2.0/book.html#shell
Version 2.3.0, re0e1382705c59d3fb3ad8f5bff720a9dc7120fb8, Mon Jul  6 22:27:43 UTC 2020
Took 0.0022 seconds                                                                                                                                                        
hbase(main):008:0* list
TABLE                                                                                                                                                                      
hbase_1103                                                                                                                                                                 
hbase_1104                                                                                                                                                                 
hbase_machine                                                                                                                                                              
hbase_machine_log                                                                                                                                                          
hbase_machine_resource_usage                                                                                                                                               
tsdb                                                                                                                                                                       
tsdb-meta                                                                                                                                                                  
tsdb-tree                                                                                                                                                                  
tsdb-uid                                                                                                                                                                   
9 row(s)
Took 0.9993 seconds                                                                                                                                                        
=> ["hbase_1103", "hbase_1104", "hbase_machine", "hbase_machine_log", "hbase_machine_resource_usage", "tsdb", "tsdb-meta", "tsdb-tree", "tsdb-uid"]
hbase(main):009:0> 

# hadoop-cluster-1/2 都能看见opentsb table


```


#### 修改配置文件
 - /etc/opentsdb/opentsdb.conf

```
#tsd.http.cachedir - Path to write temporary files to
#tsd.http.staticroot - Path to the static GUI files found in ./build/staticroot
#在同一台机器,不需要配置
#tsd.storage.hbase.zk_quorum - If HBase and Zookeeper are not running on the same machine, specify the host and port here.

tsd.core.auto_create_metrics  = true
tsd.core.meta.enable_realtime_uid=true
tsd.core.meta.enable_realtime_ts=true
tsd.core.meta.enable_tsuid_tracking=true

```


#### 启动opentsdb (all cluster node)
 - service 启动找不到文件,手动启用

```
[root@hadoop-cluster-2 tools]# cat start_opentsdb.sh 
export LOG_FILE=/var/log/opentsdb/server.log
nohup tsdb  tsd  --config=/etc/opentsdb/opentsdb.conf    2>&1 >/dev/null &

```

#### yum 安装文件说明


```
/etc/opentsdb - Configuration files
/tmp/opentsdb - Temporary cache files
/usr/share/opentsdb - Application files
/usr/share/opentsdb/bin - The "tsdb" startup script that launches a TSD or command line tools
/usr/share/opentsdb/lib - Java JAR library files
/usr/share/opentsdb/plugins - Location for plugin files and dependencies
/usr/share/opentsdb/static - Static files for the GUI
/usr/share/opentsdb/tools - Scripts and other tools
/var/log/opentsdb - Logs

```

#### 配置文件说明

 - zk opentsb 在同一台机器,官方文档不需要配置


#### 测试
 - 任意一台机器插入, 其他2台机器都能获取到数据

>  缺少图片




### Error


```bash
04:34:23.346 WARN  [PutDataPointRpc.processDataPoint] - Unknown metric: metric=sys.cpu.nice ts=1597811662 value=22 host=xiangfeidc=dc1

```

```/etc/opentsdb/opentsdb.conf
tsd.core.auto_create_metrics  = true
```
