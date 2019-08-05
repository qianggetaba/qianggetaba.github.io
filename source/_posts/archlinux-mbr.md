---
title: mbr方式，简单快速安装archlinux
date: 2019-08-05 10:20:52
tags:
---

mbr方式，就是比较传统古老的启动方式，一块硬盘只能有4个主分区，或者3个主分区，一个扩展分区，扩展分区内，才能建立多个小分区。这个由于mbr的协议的限制的。
[Master boot record--mbr](https://en.wikipedia.org/wiki/Master_boot_record)

下面的shell安装命令，不是很复杂，第一次尝鲜archlinux还是用有线网络，或者虚拟机的nat网络，因为安装过程需要联网下载系统包。

下面的安装是在一块硬盘上安装，熟悉过程后，可以自己适当修改
```
# 用iso或者 rufus把iso写入的启动upan，进入live环境

# test internet
ping www.baidu.com

# delete all partition in /dev/sda
wipefs -a /dev/sda

# partition, use whole disk
fdisk /dev/sda
n
p
enter
enter
enter
w

# format partition and mount
mkfs.ext4 /dev/sda1
mount /dev/sda1 /mnt

# add china local mirror url to line 6 of mirrorlist
sed -i '6iServer = http://mirrors.163.com/archlinux/$repo/os/$arch' /etc/pacman.d/mirrorlist

# download package db, so pacman can know package and its dependency
pacman -Sy

# install base package to disk, like kernel, glibc, coreutils .etc
pacstrap -i /mnt base base-devel

# partition table to fstab, when system boot can auto mount partition 
genfstab -U /mnt >> /mnt/etc/fstab

# go into disk system enviroment
arch-chroot /mnt

# timezone
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

# system support lang
sed -i -e 's/#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
sed -i -e 's/#zh_CN.UTF-8 UTF-8/zh_CN.UTF-8 UTF-8/' /etc/locale.gen
locale-gen

# misc 杂项配置
echo "LANG=en_US.UTF-8" >> /etc/locale.conf
echo "myarch" >> /etc/hostname
echo "127.0.0.1  localhost" >> /etc/hosts
echo "::1 localhost" >> /etc/hosts
echo "127.0.1.1	 myarch.localdomain	 myarch" >> /etc/hosts

# initramfs
mkinitcpio -p linux

# root password
echo "root:ergdgfd!#" | chpasswd

# install grub bootloader, i386-pc is also for x86_64 arch
pacman -S grub
grub-install --target=i386-pc /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg

# quit to live system
exit

reboot

# after reboot, log in and playaround
```

Enjoy!
