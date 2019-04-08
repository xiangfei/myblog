---
title: opennebula 公有云扩展
date: 2019-04-04 20:25:58
author: 相飞
comments:
- true
tags:
- opennebula
categories:
- opennebula

---

#### Add provider
```
 onehost create remote_provider im_provider vmm_provider
```

#### Adding the Information Manager

```
vi /etc/one/oned.conf

IM_MAD = [
      name       = "im_provider",
      executable = "one_im_sh",
      arguments  = "-t 1 -r 0 provider_name" ]
```
#### Populating the Probes
```
/var/lib/one/remotes/im/<provider_name>.d

```


### Adding the Virtual Machine Manager
```
VM_MAD = [
    name       = "vmm_provider",
    executable = "one_vmm_sh",
    arguments  = "-t 15 -r 0 provider_name",
    type       = "xml" ]
```

###
```
/var/lib/one/remotes/vmm/provier_name implement

```
