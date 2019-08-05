---
title: archlinux/google-chrome
date: 2019-08-05 11:06:45
tags:
---

安装好用又好看的google chrome谷歌浏览器，需要在已经安装了桌面环境，这常识应该都知道吧

```
# add official archlinuxcn mirror
echo "[archlinuxcn]
Server = https://cdn.repo.archlinuxcn.org/$arch" >> /etc/pacman.conf

# update package db, install key for check downloaded package
pacman -Sy
pacman -S archlinuxcn-keyring

pacman -S google-chrome
```

如果安装谷歌浏览器图标不正常，看看图标文件存在了，再执行下面命令

```
cp /usr/share/icons/hicolor/48x48/apps/google-chrome.png /usr/share/pixmaps/
```
