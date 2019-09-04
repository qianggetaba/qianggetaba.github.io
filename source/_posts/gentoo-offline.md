---
title: gentoo完全离线安装
date: 2019-09-04 17:54:29
tags:
categories:
 - gentoo
---

gentoo完全离线安装，避免下载慢，也可以在其他电脑直接不联网安装


gentoo离线安装，要解决两个问题，一个是``emerge-webrsync``联网下载portage，一个是``emerge`` 安装包时的下载源码，与依赖的包源码

``emerge-webrsync`` 可以看这个[gentoo的portage与emerge-webrsync](/2019/09/04/gentoo-portage/)
下载portage-20190901.tar.xz后，解压到``/var/db/repos/gentoo``

``emerge`` 会先下载包，如果已下载对应版本的包与依赖包，就会开始解压安装。下载目录``/var/cache/distfiles``,就是``/etc/portage/make.conf``中定义的``DISTDIR``路径

下面是说明怎么下载emerge安装时的包，也可以安装完，把``/var/cache/distfiles``的包保存起来


使用vmware进入live cd下载， 有些步骤看 [快速安装最小gentoo系统](/2019/09/02/gentoo-quick/)

- boot live cd

- 开启ssh

- 分区准备

- 解压stage3

- 复制dns

- 配置准备 与chroot

- emerge-webrsync

- 编译配置 与 其他配置

可以在这里加开机快照，方便有问题直接回来，或者多试试几次不同配置要下载的包

恢复快照回来，连接ssh后的必要准备
```
cd /mnt/gentoo
chroot . /bin/bash
source /etc/profile
```

可以设置profile
```
eselect profile set default/linux/amd64/17.1
```

minimal, quick 需要下载的包，约360M，就是默认的profile ``eselect profile set default/linux/amd64/17.1``
```
emerge --autounmask-write --update --newuse --deep --fetchonly @world
emerge --ask --autounmask-write --fetchonly sys-kernel/gentoo-sources sys-kernel/linux-firmware
emerge --ask --autounmask-write --fetchonly sys-boot/grub sys-boot/os-prober \
sys-apps/iproute2 net-misc/dhcpcd net-wireless/wireless-tools net-wireless/iw net-wireless/wpa_supplicant \
app-portage/gentoolkit
```

gnome-systemd 需要下载的包，约1.5G
```
eselect profile set default/linux/amd64/17.1/desktop/gnome/systemd

emerge --ask --autounmask-write --update --newuse --deep --fetchonly @world
emerge --ask --autounmask-write --fetchonly sys-kernel/gentoo-sources sys-kernel/linux-firmware
emerge --ask --autounmask-write --fetchonly sys-boot/grub sys-boot/os-prober \
sys-apps/iproute2 net-misc/dhcpcd net-wireless/wireless-tools net-wireless/iw net-wireless/wpa_supplicant \
app-portage/gentoolkit
emerge --ask --autounmask-write --fetchonly x11-base/xorg-drivers
emerge --ask --autounmask-write --fetchonly x11-base/xorg-server \
x11-terms/xterm x11-wm/twm \
gnome-base/gnome  gnome-extra/gnome-shell-extensions gnome-extra/chrome-gnome-shell \
media-sound/alsa-utils \
app-admin/conky
```

其他需要下载的包，可以在上面加


最后通过 scp 把下载的源码包，导出来，``/var/cache/distfiles``

```
ls /mnt/gentoo/var/cache/distfiles
du -sh /mnt/gentoo/var/cache/distfiles  # 目录大小
```

可以在vmware，新建虚拟机，host only网络，测试，但是编译时间太长了。
最小安装后，如果没有安装dhcpcd，下面手动配置ip

host only net set, no route
vmware edit--network editor,on top list, click vmnet1, click right bottom dhch setting
find the ip range,netmask,broadcast
```
ifconfig eth0 192.168.31.128 netmask 255.255.255.0 broadcast 192.168.31.255
ssh root@192.168.31.128
```
