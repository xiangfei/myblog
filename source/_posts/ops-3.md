---
title: mariadb galera 集群安装
date: 2019-04-19 17:00:46
tags:
- ops
author: 相飞
comments:
- true
categories:
- openstack
- mariadb

---


#### 准备

+  192.168.224.181    controller1 
+  192.168.224.182   controller2 
+  192.168.224.183   controller3 

#### yum 源

```
# 163源测试,找不到wsrep
vim Mariadb.repo

[mariadb]

name = MariaDB

baseurl = http://yum.mariadb.org/10.1/centos7-amd64

enabled = 1

gpgcheck = 0

```

#### 安装
+ 在所有机器执行
```
 yum install mariadb-server mariadb-client galera xinetd rsync -y
systemctl start mariadb
systemctl status mariadb
 mysql_secure_installation
```

#### 配置
+ 192.168.224.181

```
vim /etc/my.cnf.d/server.cnf
 vim /etc/my.cnf.d/server.cnf

# Galera Cluster Configuration

[galera]

# Mysql Configuration

binlog_format = ROW

max_connections = 10000

bind-address = 192.168.224.181

# Galera Cluster Configuration

wsrep_provider = /usr/lib64/galera/libgalera_smm.so

wsrep_cluster_name = "OpenStack"

wsrep_cluster_address = "gcomm://192.168.224.182,192.168.224.181,192.168.224.183"

wsrep_node_name = controller1

wsrep_node_address = 192.168.224.181

wsrep_sst_method = rsync

wsrep_on = ON

wsrep_slave_threads = 1

# InnoDB Configuration

default_storage_engine = InnoDB

innodb_autoinc_lock_mode = 2

innodb_flush_log_at_trx_commit = 0

innodb_buffer_pool_size = 122M

# systemctl 参数
vim /usr/lib/systemd/system/mariadb.service

# add to service
LimitNOFILE=10000

LimitNPROC=10000

systemctl daemon-reload

启动

 /usr/sbin/mysqld --wsrep-new-cluster --user=root &
```


+ 其他机器配置

```
拷贝机器  server.cnf文件 ,修改机器ip

启动
# service mariadb start 

/usr/sbin/mysqld  --user=root

查看
show status like 'wsrep%';

#集群size是不是3
# | wsrep_incoming_addresses     | 192.168.17.132:3306,192.168.17.138:3306,192.168.17.151:3306

```
