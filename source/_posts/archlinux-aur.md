---
title: archlinux的用户自建的包安装Aur
date: 2019-08-05 20:12:57
tags:
 - Aur
 - Package
categories:
 - ArchLinux
---

Arch User Repository (Aur),就是用户创建上传的包，里面是构建包的一个脚本文件```PKGBUILD```，当这个包比较受欢迎，就会进入community库的里面

下面是怎么安装aur的包，比如 [https://aur.archlinux.org/packages/jdk8/](https://aur.archlinux.org/packages/jdk8/)

```
# 前提，已安装base-devel，一般在安装系统时，与base一起安装
pacman -S base-devel

# 下载里面的git库地址，也有打包下载的，右侧Download snapshot
git clone https://aur.archlinux.org/jdk8.git

cd jdk8

# 开始下载依赖等等，安装，就好了
makepkg -si
```

由于是用户上传的包，不一定都好用，心里准备，但有的软件核心库没有，自己下载需要很多配置，就可以试试aur


