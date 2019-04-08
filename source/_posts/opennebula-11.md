---
title: opennebula 虚拟机管理
date: 2019-04-04 20:25:48
author: 相飞
comments:
- true
tags:
- opennebula
categories:
- opennebula
---


### image

```
需要安装one-context
```
### Set Up the Virtual Machine Template
```
CONTEXT = [
    TOKEN = "YES",
    NETWORK = "YES",
    SSH_PUBLIC_KEY = "$USER[SSH_PUBLIC_KEY]",
    START_SCRIPT = "yum install -y ntpdate"
]
```
>+ Set OneGate token and onegate information in the context
+ Add network configuration to the Virtual Machine
+ Enable login into the Virtual Machine using ssh with the value of the user’s parameter SSH_PUBLIC_KEY
+ On Virtual Machine boot execute the command yum install -y ntpdate


### OneGate Token

+ Token: vm rest 接口认证
> 需要在oned.conf 配置endpoint

### Network Configuration

```
CONTEXT = [
  NIC = [ NETWORK = "Network", IP = 10.0.0.153 ]
]
```

### User Credentials

```
ssh-keygen 
手动在template copy公钥 or
CONTEXT = [
  SSH_PUBLIC_KEY="ssh-rsa MYPUBLICKEY..."
]

```

### Execute Scripts on Boot

```
CONTEXT = [
    START_SCRIPT = "#!/bin/bash
yum update
yum install -y ntpdate
ntpdate 0.pool.ntp.org"
]

```



