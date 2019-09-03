---
title: gentoo的USE变量
date: 2019-09-03 17:11:31
tags:
 - USE
categories:
 - gentoo
---

gentoo的USE变量，就是控制系统和包编译安装时的一些功能，避免编译不需要的功能，或者少编译功能

``/etc/portage/make.conf``是全局设定USE，不能针对包设置,比如下面，就是系统需要的一些功能
```
USE="-qt5 -kde X gtk gnome systemd"
```
``/etc/portage/package.use``或者下的那个文件，【添加一行】，时针对某个包设定功能，比如下面，就是编译``media-libs/mesa``时不要编译``llvm``功能或包
```
media-libs/mesa -llvm
```

USE修改后需要更新，因为有的包会有新的依赖或者需要重新编译
```
emerge --ask --verbose --update --deep --newuse @world
```

清理,只有所有包的依赖关系理清楚了，才会执行清理
```
emerge --depclean
emerge --update --newuse --deep --with-bdeps=y @world
```
