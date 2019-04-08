---
title: opennebula 主机集群管理
date: 2019-04-04 19:53:21
author: 相飞
comments:
- true
tags:
- opennebula
categories:
- opennebula

---


### 创建,删除主机

```
onehost create host01 --im kvm --vm kvm
onehost delete host01

```

### Showing and Listing Hosts

```
onehost list
onehost show host01
```
### enable ,disable , offline

```
onehost disable host01
onehost enable host01
onehost offline host01
```

### custom tags

```
onehost update -a "TYPE=\"production\""

```

### update driver

```
onehost sync host01
```
