---
title: gentoo的portage与emerge-webrsync
date: 2019-09-04 17:40:12
tags:
 - webrsync
categories:
 - gentoo
---


gentoo安装时的 ``emerge-webrsync`` 命令就是去 ``http://distfiles.gentoo.org/snapshots/portage-20190901.tar.xz`` 下载最新的，好像是每日一个
然后解压到 ``/var/db/repos/gentoo``目录

``/var/db/repos/gentoo``目录，简单理解为gentoo包管理器的数据库，可以安装的包，和包的依赖关系都在这个目录里的ebuild文件

``emerge-webrsync`` 与 ``emerge --sync`` 功能一样，webrsync是下载压缩包后解压，sync是直接使用http来rsync文件，比较慢

所以可以手动安装``emerge-webrsync``

```
# scp portage-20190901.tar.xz /mnt/gentoo
wget http://distfiles.gentoo.org/snapshots/portage-20190901.tar.xz

tar xf portage-20190901.tar.xz
rm -rf /var/db/repos/gentoo
mv portage /var/db/repos/gentoo
```

然后就可以配置与安装其他软件了