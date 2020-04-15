---
title: opennebula win7 img制作
date: 2020-04-15 09:17:06
author: 相飞
comments:
- true
tags:
- opennebula
categories:
- opennebula


---



# 需要安装virtio-win-0.1.141.iso #

1. win7.img 20G , qcow2 , virtio
2. virtio-win raw cdrom
3. win7.iso  raw cdrom

4. 启动顺序 win7.iso , virtio-win.iso win7.img

5. 执行onecontext 脚本

> 可以通过webvirtmgr安装 image

