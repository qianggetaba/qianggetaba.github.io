---
title: archlinux制作离线安装软件包
date: 2019-09-03 18:39:58
tags:
---

archlinux一般安装需要联网，然后pacstrap下载安装，其实下载好包放入/var/cache/pacman/pkg，可以快速安装，适合网速慢的，尤其是安装gnome桌面的时候，包很多，下载慢

在虚拟机内下载包，然后使用scp导出，保存

boot archiso

开启ssh
```
passwd
systemctl start sshd
ip -4 addr
```
分区准备
```
wipefs -a /dev/sda
parted /dev/sda --script mklabel msdos 
parted /dev/sda --script -- mkpart primary 4MB -1
lsblk

mkfs.ext4 /dev/sda1
mount /dev/sda1 /mnt
```
repo地址准备
```
sed -i '6iServer = http://mirrors.163.com/archlinux/$repo/os/$arch' /etc/pacman.d/mirrorlist
pacman -Sy
```
简单初始化分区系统
```
pacstrap /mnt base # to init disk system, like /bin/bash, other arch-chroot error and cannot pacman -Sw download only
```
进入chroot，配置
```
arch-chroot /mnt

echo "[archlinuxcn]
Server = https://cdn.repo.archlinuxcn.org/\$arch" >> /etc/pacman.conf
pacman -Sy
pacman -S archlinuxcn-keyring # for archlinuxcn
```
下载需要安装的软件包
```
pacman -Sw base base-devel \
openssh \
vim \
grub efibootmgr dosfstools os-prober mtools \
xorg xorg-server gnome ttf-droid wqy-microhei git xf86-video-intel xf86-video-amdgpu xf86-video-nouveau gnome-tweaks chrome-gnome-shell \
google-chrome \
fcitx-im fcitx-configtool fcitx-gtk2 fcitx-gtk3 fcitx-qt4 fcitx-qt5 libidn fcitx-sogoupinyin fcitx-googlepinyin \
wireless_tools wpa_supplicant iw \
alsa-utils \
conky
```
配置locale，否则repo-add会报错  bsdtar: Failed to set default locale
```
sed -i -e 's/#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
locale-gen
```
制作离线包repo的db
```
cd /var/cache/pacman/pkg
md5sum * > md5sums   # check:cd /var/cache/pacman/pkg && md5sum -c md5sums | grep -v OK
mv md5sums ../
repo-add ./custom.db.tar.gz ./* # md5sums in pkg will failed:No packages modified, nothing to do
cd ..
tar czf pkg.tar.gz pkg
```

使用scp保存 pkg.tar.gz 与 md5sums


以后直接离线安装,rufus以iso方式写archiso到U盘，pkg.tar.gz复制到U盘根目录,启动后，直接解压包到新分区对应目录，然后配置repo
```
mkdir -p /mnt/var/cache/pacman/
tar xzf /run/archiso/bootmnt/pkg.tar.gz -C /mnt/var/cache/pacman/

echo "[custom]
Server = file:///mnt/var/cache/pacman/pkg" > /etc/pacman.conf

pacman -Sy
pacstrap /mnt base base-devel openssh vim grub xorg xorg-server gnome ttf-droid wqy-microhei git

arch-chroot /mnt
```