---
title: opennebula 用户权限
date: 2019-04-04 19:53:24
author: 相飞
comments:
- true
tags:
- opennebula
categories:
- opennebula

---


## User

### Adding and Deleting Users

```
oneuser create otheradmin password
oneuser chgrp otheradmin oneadmin

oneuser create publicuser password
oneuser chauth publicuser public

oneuser create serveruser password
oneuser chauth serveruser server_cipher

oneuser delete server_cipher

```

### token

```
oneuser token-create
oneuser token-set --token b6
export ONE_AUTH=/var/lib/one/.one/5ad20d96-964a-4e09-b550-9c29855e6457.token; export ONE_EGID=-1
oneuser token-delete b6
```
## Group

###  add
```
onegroup create "new group"

```
### add user

```
oneuser chgrp -v regularuser "new group"

```

### manage Virtual Machine Templates ,images, service, instance 

```
在sunstone 页面直接操作
```

## permission

###

```
onetemplate chmod 0 607 -v
```

## ACL

### 创建

```
oneacl create "@106 IMAGE/#31 USE"
```

## quota

###  创建

```
oneuser batchquota userA,userB,35
onegroup batchquota 100..104
oneuser defaultquota
```


## account

```
oneacct -s 05/01 -e 06/01
```
