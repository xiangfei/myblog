---
title: golang 学习
date: 2020-11-18 14:54:00
tags: golang
author: 相飞
comments:
- true
categories:
- golang

---

### go mod

 - 管理第三包

```bash  
[root@beta-devops-web--bak transfer]# go mod init main
go: creating new go.mod: module main
[root@beta-devops-web--bak transfer]# go get github.com/Shopify/sarama
go: github.com/Shopify/sarama upgrade => v1.27.2
[root@beta-devops-web--bak transfer]# cat go.sum 
github.com/Shopify/sarama v1.27.2 h1:1EyY1dsxNDUQEv0O/4TsjosHI2CgB1uo9H/v56xzTxc=
github.com/Shopify/sarama v1.27.2/go.mod h1:g5s5osgELxgM+Md9Qni9rzo7Rbt+vvFQI4bt/Mc93II=
github.com/Shopify/toxiproxy v2.1.4+incompatible/go.mod h1:OXgGpZ6Cli1/URJOF1DMxUHB2q5Ap20/P/eIdh4G0pI=
 


```


