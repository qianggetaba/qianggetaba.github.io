---
title: 根据mac的系统app制作vmware可启动镜像
date: 2019-10-24 14:29:05
tags:
 -mac
---

可以下载各个版本完整系统的软件[macOS Patcher](http://dosdude1.com/software.html)


有了系统.app后，制作可启动镜像，第一个的``create``的size根据镜像大小设置，向上取整，可以设置稍微大一点，最后的``convert``会生成一个实际大小的镜像
```
hdiutil create -o /tmp/Catalina -size 8000m -layout SPUD -fs HFS+J

hdiutil attach /tmp/Catalina.dmg -noverify -mountpoint /Volumes/install_build

sudo /Users/li/Downloads/Install\ macOS\ Catalina\ Beta.app/Contents/Resources/createinstallmedia --volume /Volumes/install_build

hdiutil detach /Volumes/Install\ macOS\ Catalina\ Beta/

hdiutil convert /tmp/Catalina.dmg -format UDTO -o ~/Downloads/Catalina

mv ~/Downloads/Catalina.cdr ~/Downloads/Catalina.iso

```
制作可直接拖到application的dmg镜像installer
```
hdiutil create -o /tmp/Catalina -size 7000m -layout SPUD -fs HFS+J
hdiutil attach /tmp/Catalina.dmg -noverify -mountpoint /Volumes/Catalina_Installer
Copy Catalina.app to /Volumes/Catalina_Installer in finder
cd /Volumes/Catalina_Installer
ln -s /Applications Applications
cd ~
hdiutil detach /Volumes/Catalina_Installer
hdiutil convert -format UDZO -o ~/Downloads/CatalinaInstaller.dmg /tmp/Catalina.dmg
```