---
title: linux-common
date: 2019-09-30 16:22:56
tags:
---

```
curl www.google.com
export http_proxy=http://192.168.1.37:1080
curl www.google.com
```

sudo权限，与代理,需要安装``sudo``软件
```
/etc/sudoers
在下面添加一行对应用户名，替换root
root    ALL=(ALL:ALL) ALL
sudo命令时，保留代理配置，在env_reset下面加一行
Defaults        env_reset
Defaults        env_keep = "http_proxy https_proxy ftp_proxy DISPLAY XAUTHORITY"
```


```
查找日志文件,显示文件大小,按从大到小排序
find ~/ -name *.log.* | xargs du -sh | sort -r -h
```