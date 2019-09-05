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

ls /mnt/gentoo/var/cache/distfiles/ # emerge fetch download path

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
/swapfile	        none	        swap	        sw,loop	        0 0

swapon -a
```

硬盘只分一个区
```
parted /dev/sda --script -- mkpart primary 4MB -1
```

每次修改了USE后，更新world, 因为有的包会有新的依赖或者需要重新编译
```
emerge --ask --verbose --update --deep --newuse @world
```

emerge-webrsync 说明
```
下载地址
http://distfiles.gentoo.org/snapshots/portage-20190901.tar.xz

portage-20190901.tar.xz里面是可以安装的包的信息，相当于包管理器的db

portage下载路径，目录后缀随机
/mnt/gentoo/var/tmp/portage/webrsync-iXsqVl/portage-20190901.tar.xz

/var/db/repos/gentoo/ 是portage解压路径，可以看看包内文件目录是一致的

看看有什么选项
emerge-webrsync -h

保留下载的portage包到/etc/portage/make.conf的DISTDIR目录/var/cache/distfiles，也是emerge时，下载的源码包路径
emerge-webrsync -k
```

查看所有网卡
```
如果这样都没有就是驱动问题，可能需要重新配置编译内核
ifconfig -a
```

安装systemd与[gnome](https://wiki.gentoo.org/wiki/GNOME/Guide),[VIDEO_CARDS和INPUT_DEVICES](https://wiki.gentoo.org/wiki/Xorg/Guide#make.conf)
先[Xorg](https://wiki.gentoo.org/wiki/Xorg/Guide)
```
nano /etc/portage/make.conf
USE="-qt5 -kde X gtk gnome systemd"
# synaptics for touchpad
INPUT_DEVICES="libinput keyboard mouse synaptics"
VIDEO_CARDS="vmware"

# llvm for Enable LLVM backend for Gallium3D, but llvm compile so slow and easy oom
/etc/portage/package.use 或者/etc/portage/package.use下的这个文件 添加一行
media-libs/mesa -llvm

# xorg
emerge --pretend --verbose x11-base/xorg-drivers  #显示要安装的包，与所有VIDEO_CARDS INPUT_DEVICES 变量，以便按需设置
emerge --ask x11-base/xorg-server
env-update && source /etc/profile

# 包很多，根据配置大约有350-380个包，很慢
emerge --ask gnome-base/gnome

env-update && source /etc/profile
getent group plugdev # 输出：plugdev:x:104:

gpasswd -a <username> plugdev

systemctl start gdm
```

安装包时候会自动加入一些USE，设置功能，可以这样查看系统所有USE
```
emerge --info | grep ^USE
emerge --ask --autounmask-write package  # 自动合并包配置，USE等等
```

安装具体包
```
ls var/db/repos/gentoo/dev-java/oracle-jdk-bin/
oracle-jdk-bin-1.8.0.202.ebuild  oracle-jdk-bin-11.0.2.ebuild

emerge -av =oracle-jdk-bin-1.8.0.202
```
