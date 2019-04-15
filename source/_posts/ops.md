---
title: 小幺鸡安装
date: 2019-04-15 11:33:20
tags: ops
author: 相飞
comments:
- true

---


### 安装

+ 下载war包(https://github.com/zhoujingjie/xiaoyaoji/releases)
+ 安装JDK7, tomcat7

```
git clone https://github.com/zhoujingjie/xiaoyaoji.git
yum -y install mariadb mariadb-server 
yum -y instal java maven
cd xiaoyaoji
mvn clean package

#创建数据库
CREATE DATABASE `xiaoyaoji` CHARACTER SET utf8;
use xiaoyaoji;
source xiaoyaoji.sql
#修改数据库配置文件
/project/apache-tomcat-7.0.65/webapps/ROOT/WEB-INF/classes/config.properties 



```


### 问题解决
+ 启动pending

```
用JDK7编译, tomcat7 部署
```

+ api测试  csrf

```
手动修改xhr 认证
[root@autotestwebagent WEB-INF]# find ./ -name view.js
./plugins/cn.xiaoyaoji.plugin/web/websocket/view.js
./plugins/cn.xiaoyaoji.plugin/web/http/view.js
157             dataType: this.content.contentType,
158             crossDomain: true,
159             xhrFields: {
160                 withCredentials: false
161             },


```

+ 导出pdf乱码

```
系统自动的ttl, otf 文件不支持在,找一个支持中文的ttl

/service/apache-tomcat-7.0.90/webapps/ROOT/WEB-INF/plugins/cn.xiaoyaoji.pdf/classes

FZLTCXHJW.TTF
NotoSansCJKsc-Regular.otf
![FZLTCXHJW.TTF](/medias/ops/xiaoyaoji/FZLTCXHJW.TTF)
![NotoSansCJKsc-Regular.otf](/medias/ops/xiaoyaoji/NotoSansCJKsc-Regular.otf)

```

