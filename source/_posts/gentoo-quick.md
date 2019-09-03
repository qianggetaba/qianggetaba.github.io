---
title: 快速安装最小gentoo系统
date: 2019-09-02 17:56:43
tags:
 - gentoo-quick
categories:
 - gentoo
---

[gentoo-Quick Installation](https://wiki.gentoo.org/wiki/Quick_Installation_Checklist)

在vmware上测试安装成功，下载 [install-amd64-minimal.iso](http://distfiles.gentoo.org/releases/amd64/autobuilds/20190901T214501Z/install-amd64-minimal-20190901T214501Z.iso) 和 [stage3-amd64.tar.xz](
http://distfiles.gentoo.org/releases/amd64/autobuilds/20190901T214501Z/stage3-amd64-20190901T214501Z.tar.xz)

boot minimal live cd，需要按【两次】回车

开启ssh, 连接ssh方便粘贴命令
```
passwd
rc-service sshd start
ip -4 addr
```
准备分区
```
wipefs -a /dev/sda
parted /dev/sda --script mklabel msdos 
parted /dev/sda --script -- mkpart primary 4MB -1
lsblk

mkfs.ext4 /dev/sda1

mount /dev/sda1 /mnt/gentoo
```
展开stage3
```
cd /mnt/gentoo
# scp stage3 to /mnt/gentoo 或者 wget
tar xvf stage3-amd64-20190901T214501Z.tar.xz
```
dns
```
cp /etc/resolv.conf etc
```
配置准备,chroot
```
mount -t proc none proc
mount --rbind /sys sys
mount --make-rslave sys
mount --rbind /dev dev
mount --make-rslave dev

chroot . /bin/bash
source /etc/profile
```
下载安装ebuild与portage
```
emerge-webrsync
```
编译配置 与 其他配置
```
nano /etc/portage/make.conf # 修改第一行，其他行添加
COMMON_FLAGS="-O2 -march=native -pipe"
MAKEOPTS="-j2"
GENTOO_MIRRORS="https://mirrors.163.com/gentoo/"
CHOST="x86_64-pc-linux-gnu"

passwd

useradd -g users -G wheel,portage,audio,video,usb,cdrom -m qianggetaba
passwd qianggetaba

nano /etc/fstab # 添加下面
/dev/sda1		/		ext4		noatime		0 1

echo 'LANG="en_US.utf8"
LC_COLLATE="C"' > /etc/env.d/02locale

nano /etc/conf.d/hostname # 安装后电脑名，终端前缀符会显示

ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
```
内核
```
emerge -av sys-kernel/gentoo-sources sys-kernel/linux-firmware

cd /usr/src/linux
make localyesconfig
make -j2

make modules_install
make install
```
grub
```
emerge --ask sys-boot/grub
grub-install --target=i386-pc /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg # 输出：Found vmlinuz-3.14.4-gento
```
其他软件
```
# 网络管理，尤其dhcpcd
emerge --ask sys-apps/iproute2 net-misc/dhcpcd net-wireless/wireless-tools net-wireless/iw net-wireless/wpa_supplicant

# equery 等等包管理工具
emerge --ask app-portage/gentoolkit
```

安装完事
```
exit
cd ~
umount -R /mnt/gentoo

reboot
```

重启等待进入新系统，试试能 ping联网就行，其他可以慢慢来，再试试上面的sshd能不能启动,可能需要启动``dhcpcd``

