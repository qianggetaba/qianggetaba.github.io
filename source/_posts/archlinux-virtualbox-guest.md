---
title: virtualbox内的archlinux安装Guest Additions iso
date: 2019-08-28 10:45:56
tags:
---

在virtual box内的archlinux，命令行下，安装Guest Additions 的iso

需要安装``base-devel``,``linux-headers``注意版本
```
pacman -Sy
pacman -S base-devel linux-headers
```
查找对应版本，下载安装
```
https://archive.archlinux.org/packages/l/linux-headers/
scp Downloads/linux-headers-5.1.15.arch1-1-x86_64.pkg.tar.xz root@192.168.1.43:/root
pacman -U linux-headers-5.1.15.arch1-1-x86_64.pkg.tar.xz
``

挂载镜像后``lsblk``查看
```
sr0 11:0 1  73.6M 0 rom
```

mount
```
mkdir /mnt/iso
mount /dev/sr0 /mnt/iso
ls /mnt/iso
```
开始安装
```
/mnt/iso/VBoxLinuxAdditions.run
```

重启
```
systemctl reboot
```