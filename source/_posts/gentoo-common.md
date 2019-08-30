---
title: gentoo的一些总结
date: 2019-08-30 17:33:06
tags:
categories:
 - gentoo
---

live cd 的chroot后，emerge 会下载源码并编译安装，下载路径
```
# 查找压缩包路径
find /mnt/gentoo \( -name "*.tar.bz2" -o -name "*.tar.xz" -o -name "*.tar.gz" \)

ls /mnt/gentoo/var/cache/distfiles/ # emerge fetch

# 查看目录大小
du -sh /mnt/gentoo/var/cache/distfiles/
```

live cd安装时，看看硬盘占用大小
```
df -h |grep /mnt/gentoo
```
已经安装的包
```
ls /var/db/pkg/* # installed package
```

在live cd安装完后，进入系统，有问题，回到live cd 并正常启动后，开始进入chroot的步骤
```
mount /dev/sda1 /mnt/gentoo

mount --types proc /proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --make-rslave /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
mount --make-rslave /mnt/gentoo/dev

chroot /mnt/gentoo /bin/bash
source /etc/profile
export PS1="(chroot) ${PS1}"

# 执行其他操作，安装软件等等
```

按照教程安装了``sys-kernel/genkernel``，想安装systemd, 按教程需要安装``sys-kernel/genkernel-next``，提示冲突
```
# 卸载 sys-kernel/genkernel
emerge -cav sys-kernel/genkernel # remove
```

设置swap虚拟内存
```
# 4g, 1m是512m
dd if=/dev/zero of=/swapfile count=8M
mkswap /swapfile

nano /etc/fstab 
/swapfile   none    swap    sw,loop 0 0

swapon -a
```

硬盘只分一个区
```
parted /dev/sdf --script -- mkpart primary 0 -1
```


