---
title: gentoo安装gnome桌面与systemd
date: 2019-08-30 17:10:05
tags:
 - gentoo桌面
categories:
 - gentoo
---

安装有桌面环境的gentoo系统，gentoo默认是openrc的启动init

官方教程：
Installing Gentoo部分直到``Installing the Gentoo base system``之前 [Gentoo AMD64 Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64)
接着上面的``Installing the Gentoo base system`` [systemd/Installing Gnome3 from scratch](https://wiki.gentoo.org/wiki/Systemd/Installing_Gnome3_from_scratch)

我安装过程：

在virtual box内安装，硬盘设置大一些，20-50g，因为源码，编译生成的中间文件，安装软件会占用很大空间。

新建虚拟机，virtual box网卡选bridge，enp2s0，不然不能在虚拟机外面ssh，vmware就用默认nat

加载 minimal install cd 启动

启动后需要 按一次 enter 回车，启动进入cd

再按一次 enter 回车，选择默认键盘keymap

出现命令提示符
```
livecd ~ #
```
设置root密码，开启ssh连接，你在虚拟机直接输后面的命令也行
```
passwd
rc-service sshd start # 或者 /etc/init.d/sshd start
ip -4 addr
```
在外面终端连接
```
# 每次重启连接，需要先移除先前的记录的ssh fingerprint
# 重启后就变了,会拒绝ssh连接
ssh-keygen -R 192.168.1.49
ssh root@192.168.1.49
```
准备分区，使用整个磁盘，或者就一个分区，太多分区，对于第一次安装会造成混乱
```
fdisk /dev/sda
输入n 新建分区
输入p 主分区
输入1 分区号1
enter 默认开始扇区
enter 默认结束扇区
输入w 写入磁盘分区表

格式化为ext4
mkfs.ext4 /dev/sda1
mount /dev/sda1 /mnt/gentoo
```
展开stage3到挂载分区，1种是按照教程下载，2种是通过scp把下载的stages3复制过去
```
cd /mnt/gentoo
wget http://distfiles.gentoo.org/releases/amd64/autobuilds/20190825T214502Z/stage3-amd64-20190825T214502Z.tar.xz

或者
scp Downloads/stage3-amd64-20190825T214502Z.tar.xz root@192.168.1.49:/mnt/gentoo
```
展开，【一定】要进入到挂载点再执行tar解压
```
cd /mnt/gentoo
tar xpvf stage3-amd64-20190825T214502Z.tar.xz --xattrs-include='*.*' --numeric-owner

解压完，回到live cd的根目录/root,开始配置与安装
cd ~
```

修改编译选项与软件包镜像地址
```
nano /mnt/gentoo/etc/portage/make.conf

COMMON_FLAGS="-O2 -march=native -pipe" # 添加-march=native，就是live cd的位宽，x86_64

MAKEOPTS="-j2" #末尾加

GENTOO_MIRRORS="https://mirrors.163.com/gentoo/" #末尾加，与mirrorselect效果一样
```
其他准备与进入chroot环境的配置，具体可以看看官方文档

```
# repo
mkdir --parents /mnt/gentoo/etc/portage/repos.conf
cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf

# dns
cp --dereference /etc/resolv.conf /mnt/gentoo/etc/

mount --types proc /proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --make-rslave /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
mount --make-rslave /mnt/gentoo/dev

chroot /mnt/gentoo /bin/bash
source /etc/profile
export PS1="(chroot) ${PS1}"
```
ebuild，应该是编译环境和软件
```
# 会下载
emerge-webrsync
```
如果是虚拟机可以在此时，建立快照，方便测试不同profile

选择profile
```
eselect profile list
eselect profile set default/linux/amd64/17.0/desktop/gnome/systemd
```

@world，可以理解为Windows注册表一样的全局配置，根据你的profile去下载与配置,最好配置swap，防止oom导致编译错误
```

# 会下载与编译安装，就相当于系统全局更新
emerge --ask --verbose --update --deep --newuse @world

emerge --depclean
emerge --update --newuse --deep --with-bdeps=y @world
```

```
echo 'Asia/Shanghai' > /etc/timezone
emerge --config sys-libs/timezone-data

echo 'en_US.UTF-8 UTF-8' >>/etc/locale.gen
echo 'zh_CN.UTF-8 UTF-8' >>/etc/locale.gen
locale-gen

# 与locale set相同
echo 'LANG="en_US.utf8"
LC_COLLATE="C"' > /etc/env.d/02locale
```

```
env-update && source /etc/profile && export PS1="(chroot) ${PS1}"
```

内核
```
emerge --ask sys-kernel/gentoo-sources
emerge --ask sys-kernel/genkernel-next # 可能需要etc-update
emerge --ask sys-kernel/linux-firmware
```
内核编译
```
# nano /etc/genkernel.conf , MAKEOPTS="j2"
genkernel --menuconfig all

# 在内核编译菜单最下面选中systemd与openrc, 按空格，显示星号，选择，然后save，exit，exit，保存退出后会自动开始编译
Gentoo Linux --->
        Support for init systems, system and service managers --->
                [*]Openrc 
                [*] systemd
```

加入fstab
```
nano /etc/fstab
/dev/sda1               /               ext4            rw,relatime     0 1
```

hostname会显示在登录系统后的，终端提示符
```
echo 'hostname="mygentoo"' > /etc/conf.d/hostname

nano /etc/hosts , make sure first line
127.0.0.1     mygentoo.homenetwork mygentoo localhost

# root password
passwd
```

```
ln -sf /proc/self/mounts /etc/mtab
```
```
emerge --ask sys-apps/systemd
emerge --ask sys-boot/grub:2
```

安装grub引导
```
nano /etc/default/grub
GRUB_CMDLINE_LINUX="rootfstype=ext4 real_init=/usr/lib/systemd/systemd"  # 添加

grub-install --target=i386-pc /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

exit
poweroff

正常安装完，去掉virtual box连接的iso后再启动，否则可能出现 ``FATAL: INT18: BOOT FAILURE ``

开机后可能需要开启ssh
```
ping www.baidu.com
dhcpcd

# 会提示/sbin/openrc-run bad interpreter
# 因为/etc/init.d/sshd是脚本，开始行设置的解释器就是/sbin/openrc-run，这是gentoo系统默认openrc的启动脚本
# /etc/init.d/sshd start 

systemctl start sshd
```

安装xorg与gnome

```
lspci | grep -i VGA
nano /etc/portage/make.conf
VIDEO_CARDS="vmware"  # intel ("intel"),amd ("radeon","amdgpu"), nvidia ("nouveau","nvidia")
```

```
emerge --ask x11-base/xorg-server
```
下面是安装xorg提供的xterm与twm，相当于demo，可以用来测试xorg是否安装正常
```
emerge --ask x11-terms/xterm x11-wm/twm

# 执行后，会出现图形化的终端，可以移动鼠标，说明安装xorg正常，每个终端输入exit，
startx
# 卸载
emerge --ask --depclean --verbose x11-terms/xterm x11-wm/twm
```
安装gnome，包很多，慢慢等待编译完成
```
emerge --ask gnome

# 启动gnome桌面
systemctl start gdm
```

```
# 启动并设置为开机启动
systemctl enable gdm --now
```

在系统未稳定执行，还是开机到命令行终端后，在执行start gdm启动桌面，安全
