---
title: rabbitmq集群安装
date: 2019-04-22 14:40:33
tags:
- ops
author: 相飞
comments:
- true
categories:
- openstack
- rabbitmq

---

### 机器准备 (centos7)

+ 192.168.224.181 controller1  terminaldevice-1
+ 192.168.224.182 controller2  terminaldevice-2
+ 192.168.224.183 controller3  terminaldevice-3


```
yum -y install epel-release
yum install -y erlang rabbitmq-server

# in  controller1

rabbitmqctl add_user openstack openstack

rabbitmqctl set_permissions openstack ".*" ".*" ".*"

 /usr/lib/rabbitmq/bin/rabbitmq-plugins enable rabbitmq_management mochiweb webmachine rabbitmq_web_dispatch amqp_client rabbitmq_management_agent


scp /var/lib/rabbitmq/.erlang.cookie controller2:/var/lib/rabbitmq/.erlang.cookie
scp /var/lib/rabbitmq/.erlang.cookie controller3:/var/lib/rabbitmq/.erlang.cookie

# controller2, controller3
chown -R rabbitmq:rabbitmq /var/lib/rabbitmq/.erlang.cookie 
 systemctl restart rabbitmq-server.service

rabbitmqctl stop_app

rabbitmqctl join_cluster --ram rabbit@terminaldevice-1
rabbitmqctl start_app

#controller 1


[root@terminaldevice-1 mysql]#  rabbitmqctl cluster_status
Cluster status of node 'rabbit@terminaldevice-1' ...
[{nodes,[{disc,['rabbit@terminaldevice-1']}]},
 {running_nodes,['rabbit@terminaldevice-1']},
 {cluster_name,<<"rabbit@controller1">>},
 {partitions,[]}]
...done.


```

### RABBITMQ退出集群

#### rabbit node

+ rabbitmqctl stop_app

+ rabbitmqctl reset

+ rabbitmqctl start_app

 
#### master node

+ rabbitmqctl forget_cluster_node rabbit@rabbitmq2



###  RABBITMQ集群重启

#### 先在一个节点上执行

+ rabbitmqctl force_boot

+ service rabbitmq-server start

#### 在其他节点上执行

+ service rabbitmq-server start

#查看cluster状态是否正常（要在所有节点上查询）。

+ rabbitmqctl cluster_status
