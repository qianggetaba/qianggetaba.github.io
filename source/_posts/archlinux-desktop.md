---
title: archlinux安装桌面环境系统，好好玩耍
date: 2019-08-05 11:33:53
tags:
 - 桌面环境
 - gnome
 - gdm
categories:
 - ArchLinux
---

在简单的从iso或者usb安装完成arch后，只有终端命令行，这样是简单方便，也可以安装桌面环境，不再是黑黑的命令

以下是安装gnome桌面环境，等熟悉了，自己可以安装其他桌面环境尝鲜，gnome是主流桌面环境， gnome是Ubuntu默认的桌面环境，想来应该还是不错的，
```

pacman -Sy
# wqy-microhei是chrome中文乱码
pacman -S xorg xorg-server gnome ttf-droid wqy-microhei

# 安装的包很多，需要耐心慢慢装

# 添加非root用户，root也可以登录桌面，但是一般推荐不是root用户登录桌面
useradd -m -g users -s /bin/bash qianggetaba
passwd qianggetaba
vi /etc/sudoers #root ALL=(ALL) ALL 下面加 qianggetaba ALL=(ALL) ALL  添加sudo执行命令权限

systemctl enable sshd
systemctl enable NetworkManager

```
[Driver installation](https://wiki.archlinux.org/index.php/Xorg#Driver_installation)
```

# 显卡驱动选择
lspci | grep -e VGA -e 3D       #查看显卡
pacman -S xf86-video-intel      #intel video driver
pacman -S xf86-video-amdgpu     #amd video driver
pacman -S xf86-video-nouveau	# nvidia
```

```
reboot

# 重启后还是进入命令行，是为了保险起见，因为不知道桌面环境是否能正常启动，执行下面命令，启动桌面
systemctl start gdm
```
