---
title: pacemaker haxproxy loadbalance
date: 2019-04-22 16:24:05

tags:
- ops
author: 相飞
comments:
- true
categories:
- openstack
- haxproxy
- pacemaker


---

### hosts

+ 192.168.224.181 controller1  terminaldevice-1
+ 192.168.224.182 controller2  terminaldevice-2
+ 192.168.224.183 controller3  terminaldevice-3



### 安装

```
all node

 yum install -y pacemaker pcs psmisc policycoreutils-python
systemctl start pcsd.service
systemctl enable pcsd.service
echo hacluster | passwd hacluster --stdin

192.168.224.181

 pcs cluster auth 192.168.224.181 192.168.224.182 192.168.224.183

#输入用户名密码hacluster

pcs cluster setup --name lb_cluster  192.168.224.181 192.168.224.182 192.168.224.183

# 在所有lb节点，启动corosync & pacemaker服务
systemctl start corosync.service
systemctl enable corosync.service
systemctl start pacemaker.service
systemctl enable pacemaker.service


# 3个节点   ssh-keygen  ssh-copy-id 
# 配置lb集群的特性
pcs property set stonith-enabled=false
pcs property set no-quorum-policy=ignore
pcs property set start-failure-is-fatal=false
pcs resource defaults resource-stickiness=10


# 创建vip与haproxy资源，并添加限制，确保p_vip运行在haproxy服务正常的节点
pcs resource create p_vip ocf:heartbeat:IPaddr2 ip=192.168.224.189 cidr_netmask=24 op monitor interval=2s
pcs resource create p_haproxy systemd:haproxy op monitor interval=2s --clone
pcs constraint colocation add p_vip with p_haproxy-clone --force
pcs resource meta p_vip migration-threshold=3 failure-timeout=60s











```


