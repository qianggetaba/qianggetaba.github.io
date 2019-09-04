---
title: linux手动配置网络
date: 2019-09-04 18:22:55
tags:
 - linux命令
 - ifconfig
 - route
categories:
 - linux命令
---


```
/etc/resolv.conf  # dns nameserver 192.168.245.2

ifconfig eth0 up
ifconfig eth0 192.168.245.128 netmask 255.255.255.0 broadcast 192.168.245.255
ifconfig eth0 #check ip

route add default gw 192.168.245.2 eth0
route -n # check route

host only net set, no route
vmware edit--network editor,on top list, click vmnet1, click right bottom dhch setting
find the ip range,netmask,broadcast
ifconfig eth0 192.168.31.128 netmask 255.255.255.0 broadcast 192.168.31.255
```