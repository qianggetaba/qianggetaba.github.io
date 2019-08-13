---
title: wpa_supplicant连接无线
date: 2019-08-13 14:54:29
tags:
 - linux命令
 - wpa_supplicant
 - wifi
categories:
 - linux命令
---

命令行连接已知ssid无线名称与密码的wifi无线网络

```
# 连接 名为mywifi，密码为123456789的无线网络
wpa_passphrase "mywifi" "123456789" >wpa.conf
wpa_supplicant -i wlp3s0 -c wpa.conf  &
dhcpcd wlp3s0  #启动dhcp分配ip，ip link 或者 ip addr查看ip与网卡名
ping www.baidu.com

ip -4 addr   # ip4 地址
```