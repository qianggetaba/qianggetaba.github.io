---
title: 安装最小gentoo系统
date: 2019-08-29 15:52:57
tags:
 - gentoo
categories:
 - gentoo
---

gentoo是通过编译源码安装系统与软件的linux发行版，安装时间比较长，配置比较复杂。

64位gentoo安装官方教程[gentoo-x86_64 wiki](https://wiki.gentoo.org/wiki/Handbook:AMD64)

从 [Gentoo-Downloads](https://www.gentoo.org/downloads/) 下载 [minimal install cd](http://distfiles.gentoo.org/releases/amd64/autobuilds/20190828T214505Z/install-amd64-minimal-20190828T214505Z.iso) 与 [stage3](http://distfiles.gentoo.org/releases/amd64/autobuilds/20190828T214505Z/stage3-amd64-20190828T214505Z.tar.xz)

开始安装前的一些关于``gentoo``的说明，方便理解gentoo系统，对于安装过程的命令和问题等等有所了解，避免困惑

- minimal install cd 就是一个live cd的启动进入安装环境的系统镜像。

- stage 3是【准】系统文件，把要安装gentoo的分区格式化好后，解压 stage 3 到这个分区，但是并不包括kernel和软件

- emerge是gentoo的包管理器，会根据你要安装的软件，查找依赖的软件，下载相关【源码】后，编译安装，每次执行都是重新编译安装
- emerge运行时，偶尔会提示需要更新配置文件

    ```
    IMPORTANT: 2 config files in '/etc/portage' need updating
    ```


    原因：安装软件后，软件可能需要修改配置，但是不是直接修改了，而是在同一目录生成一个._c00*这样类似的文件，emerge发现后，会提示你update。

    解决：运行 ``etc-update``,输入 ``-3``表示自动 merge 合并,然后会提示你是否覆盖，输入``y``。就是自动把新生成的配置._c00*文件 mv 到为要修改的配置文件

- profile的选择，gentoo是从源码编译安装，安装时，需要选择系统的类型
    ```
    root #eselect profile list
    Available profile symlink targets:
    [1]   default/linux/amd64/17.0 *
    [2]   default/linux/amd64/17.0/desktop
    [3]   default/linux/amd64/17.0/desktop/gnome
    [4]   default/linux/amd64/17.0/desktop/kde
    ```
    可以理解为，确定系统类型，然后再用emerge下载时会根据你选择的类型，找到合适版本的软件，但是profile选择的越多，编译越麻烦，越容易出错。第一次就选择命令行的 default/linux/amd64/17.0，安装完，进入系统，等熟悉了，可以安装其他软件，并不是固定的

- emerge编译安装出错，先dmesg看看有没有oom (out of memory)，kill cc1plus等等信息，说明编译过程中内存溢出导致编译错误。系统安装过程会设置gcc编译的并发数 ``-j``,可能数太多，内存不够用，并不是``-j``为cpu核数+1就万事大吉，j是job的意思，每个job需要的内存也不一定。尤其是编译profile为desktop时，软件太多，很有可能这样出错。
- nano编辑器的使用。gentoo的live cd默认是nano编辑器

    ```
    nano /path/to/file
    ```
    打开文件后直接就像普通文本编辑器一样，直接可以编辑，修改删除等等，编辑完成后，按 ``ctrl + x`` 退出，会提示是否保存，输入``y``，然后会显示文件路径，提示是否写入，按``enter``,就好了，没有修改按 ``ctrl + x`` 直接退出


所以我建议先安装最小gentoo系统，熟悉一次流程，能够正常安装启动进入了，再试试其他的配置。由于是编译安装，耗时是肯定的。

下面是我总结的安装过程，对照上面提到的官方wiki教程，对比着安装

在virtual box内安装，硬盘设置大一些，20-50g，因为源码，编译生成的中间文件，安装软件会占用很大空间。如果是minimal的8g硬盘可以。

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
展开stage3到挂载分区，1种是按照教程下载，地址就是上面的stage3地址，2种是通过scp把下载的stages3复制过去
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

# profile 的序号是会变的，重启后，再查询就不一样了
eselect profile set 2
# 或者
eselect profile set default/linux/amd64/17.0
```
@world，可以理解为Windows注册表一样的全局配置，根据你的profile去下载与配置
```
# 会下载与编译安装
emerge --ask --verbose --update --deep --newuse @world
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

# 提示update configuration，可能需要etc-update
emerge --ask sys-kernel/genkernel

# 生成kernel initramfs, 很慢
genkernel all
```
加入fstab
```
nano /etc/fstab
/dev/sda1           	/         	ext4      rw,relatime	0  1
```

hostname会显示在登录系统后的，终端提示符
```
echo 'hostname="mygentoo"' > /etc/conf.d/hostname

nano /etc/hosts
127.0.0.1     mygentoo.homenetwork mygentoo localhost

# root password
passwd
```

其他一些软件
```
emerge --ask sys-fs/e2fsprogs
emerge --ask net-misc/dhcpcd
emerge --ask net-wireless/iw
emerge --ask net-wireless/wpa_supplicant
emerge --ask sys-boot/grub:2


grub-install --target=i386-pc /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg


exit
poweroff
```

正常安装完，去掉virtual box连接的iso后再启动，否则可能出现 ``FATAL: INT18: BOOT FAILURE ``

如果都安装都正常，开启虚拟机，看见grub选项，正常进入系统

minimal正常安装后，可以试试profile为``eselect profile set default/linux/amd64/17.0/desktop/gnome/systemd``的系统配置，安装的软件很多，编译时间很长，很容易出现oom导致的编译错误，不要把``-j``设置太大


gentoo有别于其他linux发行版，就是他是下载软件源码后编译安装，比较费时，编译过程很容易出错。还有其他什么USE等等的设置，我还不会用。