---
title: alias-sudo-to-please
date: 2019-08-13 14:36:50
tags:
 - linux命令
 - linux
categories:
 - linux命令
---


重试上次需要权限的命令
```
alias please='sudo $(fc -ln -1)'
```

```
[myarch ~]$ ls /root/
ls: cannot open directory '/root/': Permission denied
[myarch ~]$ plese

