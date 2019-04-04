---
title: opennebula 安装准备
date: 2019-04-03 15:10:02
author: 相飞
comments:
- true
tags:
- opennebula
categories:
- opennebula
---


### 准备

#### 版本选择
> 1. lxd 支持
> 2. 自动网卡选择
> 3. 可扩展性改进
> 4. os支持

如果需要lxd功能,需要5.8或者使用 [addon-lxdone](https://github.com/OpenNebula/addon-lxdone) ,如果使用centos安装,建议使用4.x版本

#### 虚拟化选择
> 1. kvm
> 2. lxd
> 3. docker
> 4. xen
> 5. vcenter

kvm 兼容性最好, lxd 只能在ubuntu 1604 或者更高的版本支持, xen有插件,经过测试,不兼容5.4.x版本,不清楚最新的兼容性是否支持 ,vcenter 未测试,建议使用kvm 或者lxd

#### 网络选择
> 1. openvswitch bridge
> 2. vxlan
> 3. dummy bridge
> 4. bridge ebtables
> 5. Bridged with Security Groups
> 6. 802.1q vlan network
> 7. Open vSwitch on VXLAN Networks

vxlan , bridge , 8021q 不支持多租户网络隔离,如果需要多租户网络隔离,使用openvswitch 

#### 存储选择

> 1. ceph
> 2. iscsi
> 3. raw device mapping 
> 4. filesystem
> 5. lvm
> 6. The Kernels & Files Datastore

filesystem 最简单 , iscsi 只能存放image , ceph 可以存放 image和instance

