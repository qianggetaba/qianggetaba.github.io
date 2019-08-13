---
title: archlinux开启ssh远程连接服务
date: 2019-08-05 11:03:46
tags:
 - ssh
 - sshd
categories:
 - ArchLinux
---

在启动到archiso的live安装环境，也可以打开ssh，远程连接后进行安装，方便

```
# 远程连接必须设置密码，root登录需要配置 /etc/ssh/sshd_config PermitRootLogin yes
sed -i -e 's/#PermitRootLogin .*$/PermitRootLogin yes/' /etc/ssh/sshd_config
# live的sshd已经配置好，直接连接

```
```
passwd
systemctl start sshd

# show ip addr
ip -4 addr
```
