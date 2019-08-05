---
title: archlinux的autostart开机启动脚本
date: 2019-08-05 19:34:33
tags:
 - autostart
 - gnome#autostart
categories:
 - ArchLinux
---

在进入gnome桌面环境时，需要自动启动一些脚本，现在arch使用systemd，以前的自启动方式不能用


把以下内容存为init.desktop，放入～/.config/autostart/目录
```
[Desktop Entry]
Name=Ainit
GenericName=Input Method
Exec=/home/qianggetaba/init.sh
Terminal=false
Type=Application
StartupNotify=false
NoDisplay=true
X-GNOME-Autostart-Phase=Applications
X-GNOME-AutoRestart=false
X-GNOME-Autostart-Notify=false
```
上面的Exec，改为启动脚本【绝对】路径