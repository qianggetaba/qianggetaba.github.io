---
title: arch安装sogou搜狗输入法
date: 2019-08-05 11:12:59
tags:
---

很多时候觉得linux难用，一是没有好用的中文输入法，二是没有熟悉的办公软件（office全家桶），三是没有qq微信（腾讯以前有过，用的人少就停了，现在wine的不好用，只能在xp虚拟机（内存占用少）使用），四是没有好玩的大型网络游戏

```
#sogoupinyin

# 确保package的mirror地址
sudo vim /etc/pacman.d/mirrorlist
Server = http://mirrors.163.com/archlinux/$repo/os/$arch 
sudo vim /etc/pacman.conf
[archlinuxcn]
Server = https://cdn.repo.archlinuxcn.org/$arch

# 下载输入法与依赖
sudo pacman -Sy
sudo pacman -S archlinuxcn-keyring
sudo pacman -S fcitx-im fcitx-configtool fcitx-gtk2 fcitx-gtk3 fcitx-qt4 fcitx-qt5 libidn fcitx-sogoupinyin fcitx-googlepinyin

# 配置
mkdir ~/.config/autostart
cp /etc/xdg/autostart/fcitx-autostart.desktop ~/.config/autostart/
  
echo "GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx" > .pam_environment

echo "export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx" > .xprofile

reboot

# 重启后的检查
ps aux |grep fcitx
ps aux |grep sogou   ;sogou-qimpanel, 搜狗输入法窗口
fcitx-configtool  ;add sogou
ctrl + space 切换输入法试试
```
