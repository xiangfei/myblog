---
title: opennebula marketplace
date: 2019-04-04 20:25:51
author: 相飞
comments:
- true
tags:
- opennebula
categories:
- opennebula
---

### public
+ 默认安装

### private

#### Systems marketplace

```
cat market.conf
NAME = PrivateMarket
MARKET_MAD = one
ENDPOINT = "http://privatemarket.opennebula.org"
```

####  http marketplace

```
cat market.conf
NAME = PrivateMarket
MARKET_MAD = http
BASE_URL = "http://frontend.opennebula.org/"
PUBLIC_DIR = "/var/loca/market-http"
BRIDGE_LIST = "web-server.opennebula.org"
onemarket create market.conf

```
