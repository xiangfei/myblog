---
title: zookeeper 集群安装
date: 2019-04-18 14:35:28
tags: ops
author: 相飞
comments:
- true
categories:
- zookeeper

---



### 准备

+ server1  192.168.213.58
+ server2  192.168.213.59
+ server3  192.168.213.65

```
1. github download https://github.com/apache/zookeeper/releases/tag/release-3.4.14/zookeeper*.zip
2. unzip
3. 安装 java ant #用oracle java, openjdk 不行
4. ant 
5. 修改配置文件 zoo.cfg


# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
dataDir=/opt/data/zookeeper
# the port at which the clients will connect
clientPort=2181
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the 
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1
#多个ip需要修改
quorumListenOnAllIPs=true
server.1=192.168.213.58:2888:3888
server.2=192.168.213.59:2888:3888
server.3=192.168.213.65:2888:3888

3888 选举端口, 2888业务端口

```



