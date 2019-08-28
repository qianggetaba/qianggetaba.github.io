---
title: 使用archiso镜像快速安装--mbr方式
date: 2019-08-28 10:10:24
tags:
 - archiso
 - offline
categories:
 - ArchLinux
---

下载的``archlinux``镜像``archiso``有600m多，但是安装的时候都是``pacstrap``去联网下载，安装时间长，慢。可以先使用镜像安装系统，之后启动安装好的系统可以升级和安装其他软件，很方便

官方文档教程[ArchLinux--Offline installation](https://wiki.archlinux.org/index.php/Offline_installation)

以下是我整理的，在virtual box内安装。virtual box的网卡需要设置为Bridge Adapter,网卡名称类似enp2s0，这样才能在外面用ssh连接到里面的虚拟机，方便输入命令

```
# arch install only iso without download

# boot archiso

ping www.baidu.com
ip -4 addr
passwd
systemctl start sshd

# connect to vm
ssh root@192.168.1.43

# delete all partition
wipefs -a /dev/sda

# use whole sda
sed -e 's/\s*\([\+0-9a-zA-Z]*\).*/\1/' << EOF | fdisk /dev/sda
  o # clear the in memory partition table
  n # new partition
  p # primary partition
    # partition number 1
    # default - start at beginning of disk 
    # end of disk
  w
EOF

mkfs.ext4 /dev/sda1
mount /dev/sda1 /mnt

# copy iso and modify, same as archlinux wiki
cp -ax / /mnt

cp -vaT /run/archiso/bootmnt/arch/boot/$(uname -m)/vmlinuz /mnt/boot/vmlinuz-linux

arch-chroot /mnt /bin/bash

sed -i 's/Storage=volatile/#Storage=auto/' /etc/systemd/journald.conf

rm /etc/udev/rules.d/81-dhcpcd.rules

systemctl disable pacman-init.service choose-mirror.service
rm -r /etc/systemd/system/{choose-mirror.service,pacman-init.service,etc-pacman.d-gnupg.mount,getty@tty1.service.d}
rm /etc/systemd/scripts/choose-mirror

rm /etc/systemd/system/getty@tty1.service.d/autologin.conf
rm /root/{.automated_script.sh,.zlogin}
rm /etc/mkinitcpio-archiso.conf
rm -r /etc/initcpio

pacman-key --init
pacman-key --populate archlinux

# end of offline install

# start configuration
exit
genfstab -U /mnt >> /mnt/etc/fstab
arch-chroot /mnt /bin/bash

ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

sed -i -e 's/#en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen
sed -i -e 's/#zh_CN.UTF-8 UTF-8/zh_CN.UTF-8 UTF-8/' /etc/locale.gen
locale-gen

echo "LANG=en_US.UTF-8" >> /etc/locale.conf
echo "myarch" > /etc/hostname
echo "127.0.0.1  localhost" >> /etc/hosts
echo "::1 localhost" >> /etc/hosts
echo "127.0.1.1	 myarch.localdomain	 myarch" >> /etc/hosts

mkinitcpio -p linux

echo "root:toor" | chpasswd

grub-install --target=i386-pc /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg

exit

systemctl reboot
```

重启后没有网
```
dhcpcd
ip link # up mode
ip -4 addr # ipv4 addr
```

重启后不能连接ssh
```
systemctl start sshd
```