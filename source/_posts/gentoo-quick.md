---
title: gentoo-quick
date: 2019-09-02 17:56:43
tags:
---

boot minimal live cd

https://wiki.gentoo.org/wiki/Quick_Installation_Checklist


ssh

wipefs -a /dev/sda
parted /dev/sda --script mklabel msdos 
parted /dev/sda --script -- mkpart primary 4MB -1
lsblk

mkfs.ext4 /dev/sda1

mount /dev/sda1 /mnt/gentoo

scp stage3 to /mnt/gentoo
cd /mnt/gentoo
tar xvf stage3-amd64-20190901T214501Z.tar.xz

cp /etc/resolv.conf etc

mount -t proc none proc
mount --rbind /sys sys
mount --make-rslave sys
mount --rbind /dev dev
mount --make-rslave dev

chroot . /bin/bash
source /etc/profile

nano /etc/portage/make.conf
COMMON_FLAGS="-O2 -march=native -pipe"
MAKEOPTS="-j2"
GENTOO_MIRRORS="https://mirrors.163.com/gentoo/"

# download portage
emerge-webrsync  # /mnt/gentoo/var/tmp/portage/webrsync-iXsqVl/portage-20190901.tar.xz

http://distfiles.gentoo.org/releases/amd64/autobuilds/20190901T214501Z/install-amd64-minimal-20190901T214501Z.iso
http://distfiles.gentoo.org/releases/amd64/autobuilds/20190901T214501Z/stage3-amd64-20190901T214501Z.tar.xz
http://distfiles.gentoo.org/snapshots/portage-20190901.tar.xz

emerge-webrsync 会下载portage-20190901.tar.xz解压到/var/db/repos/gentoo/下，里面是可以安装的包的信息，相当于包管理器的db

https://github.com/gentoo/portage/releases
# manual install portage<<<EOF
wget https://github.com/gentoo/portage/archive/v2.2.22.tar.gz -O portage-2.2.22.tar.gz
tar --extract --gz --verbose --file portage-2.2.22.tar.gz
cd portage-2.2.22



echo "portage:x:250:250:portage:/var/tmp/portage:/bin/false" >> /etc/passwd
echo "portage::250:portage" >> /etc/group
mkdir /var/db/repos/gentoo

python setup.py install

<<<EOF

passwd

useradd -g users -G wheel,portage,audio,video,usb,cdrom -m qgta
passwd qgta

nano /etc/fstab
/dev/sda1		/		ext4		noatime		0 1

nano /etc/portage/make.conf
CHOST="x86_64-pc-linux-gnu"

echo 'LANG="en_US.utf8"
LC_COLLATE="C"' > /etc/env.d/02locale

nano /etc/conf.d/hostname

ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

emerge -av sys-kernel/gentoo-sources sys-kernel/linux-firmware

cd /usr/src/linux
make localyesconfig
make -j2

make modules_install
make install

emerge --ask sys-boot/grub
grub-install --target=i386-pc /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg

exit
cd ~
umount -R /mnt/gentoo

reboot

ip route show # ip r, route

ifconfig eno16777736 up
ifconfig eno16777736 192.168.245.128 netmask 255.255.255.0 broadcast 192.168.245.255 up

route add default gw 192.168.1.1

emerge --ask net-misc/dhcpcd

# minimal gnome
# before add USE to /etc/portage/make.conf and  /etc/portage/package.use
emerge -UD --autounmask n @world # may need install some package

USE="gtk"
emerge --ask app-crypt/gcr


# make.conf
USE="-qt5 -kde X gtk gnome systemd"

emerge --ask gnome-base/gnome-light
env-update && source /etc/profile
getent group plugdev # output:plugdev:x:104:

gpasswd -a username plugdev

systemctl start gdm



app-portage/gentoolkit # equery

# package.use, nano /etc/portage/package.use/mesa
media-libs/mesa -llvm   # llvm for Enable LLVM backend for Gallium3D, but llvm compile so slow and easy oom
emerge --update --deep --newuse -av @world 
emerge --depclean

nano /etc/portage/make.conf
VIDEO_CARDS="vmware"

# after change USE
emerge --ask --verbose --update --deep --newuse @world
