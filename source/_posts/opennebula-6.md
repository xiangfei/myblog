---
title: opennebula sunstone
date: 2019-04-04 19:06:37
author: 相飞
comments:
- true
tags:
- opennebula
categories:
- opennebula
---

#### views

+ Admin view
+ Group Admin view
+ Cloud view
+ User view

5.8 定义 /etc/one/sunstone-views/{kvm,vcenter,mixed}
vi /etc/one/sunstone-server.conf
:mode: 'mixed'

5.6 /etc/one/sunstone-views/*.yaml

custom view

```
copy  /etc/one/sunstone-views/{kvm,vcenter,mixed}/admin.yaml 

```
