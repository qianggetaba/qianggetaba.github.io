---
title: efi方式，安装archlinux，需要主板支持，目前新的主板都可以
date: 2019-08-05 10:42:47
tags:
 - efi
 - 安装
 - efi启动
categories:
 - ArchLinux
---

efi是新一代的bios标准与磁盘标准，是未来的趋势，硬盘使用gpt管理分区，避免mbr的分区数量，分区大小限制

efi启动方式简介，开机按钮，bios后，bios会在gpt分区的硬盘上找esp（EFI system partition）分区，分区格式fat32，格式简单方便处理。bios启动esp分区下的\efi\boot\bootx64.efi文件，开始系统引导。比mbr的磁盘分区字节搜索方式更高级，智能。

efi最少需要两个分区，一个esp启动分区，一个系统分区，【需要主板支持】，新的主板的【csm模式】就是为了兼容老的mbr legacy启动方式
有些命令可以看[mbr方式，简单快速安装archlinux](https://qianggetaba.com/2019/08/05/archlinux-mbr/)

```
# 启动进入live 环境

ping www.baidu.com

wipefs -a /dev/sda

# partition, 256M for esp partition, g make disk to gpt
fdisk /dev/sda
g
n
enter
enter
+256M
n
enter
enter
+100G
w

# sda1 is esp
mkfs.fat -F32 /dev/sda1  ;format esp
mkfs.ext4 /dev/sda2

# mount order
mount /dev/sda2 /mnt 
mkdir -p /mnt/boot/EFI
mount /dev/sda1 /mnt/boot/EFI

sed -i '6iServer = http://mirrors.163.com/archlinux/$repo/os/$arch' /etc/pacman.d/mirrorlist 

pacman -Sy
pacstrap -i /mnt base base-devel
genfstab -U /mnt >> /mnt/etc/fstab

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

# os-prober for windows dual boot search
pacman -S grub efibootmgr dosfstools os-prober mtools
grub-install --target=x86_64-efi  --bootloader-id=ArchLinux --efi-directory=/boot/EFI --recheck
os-prober
grub-mkconfig -o /boot/grub/grub.cfg

exit

reboot

```

Done!
