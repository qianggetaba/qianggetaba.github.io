---
title: 只启动x-server深入linux图形架构 后启动我们自己的小窗口
date: 2019-08-09 14:54:27
tags:
 - xorg
 - xorg-server
categories:
 - xorg
---

只启动xorg-server,显示我们自己写的程序(xorg-server的c语言入门)的窗口

[man xinit](https://www.x.org/archive/X11R6.7.0/doc/xinit.1.html)

``startx`` 是 ``xinit`` 的包装，xinit默认会启动xterm，一个窗口化的终端，方便那些不能直接在kernel启动init的时候，启动桌面，就先启动xterm，再启动桌面


安装xterm
```
sudo pacman -S xterm
xterm  #确保能启动
```

回到系统启动后的黑屏终端

```
xinit  #会报错，可能缺少一些环境变量
startx  会启动几个xterm与鼠标，图像窗口程序

./xorg  # 在其中一个xterm，运行我们编译的xlib写的简单窗口
```

可以看到程序启动，左上角有个小窗口，黑色背景，上面有条白色线


startx 会设置一些环境变量，启动xinit，启动xorg-server，启动xterm
我们就可以在xterm上启动我们的xlib程序了，但是由于没有窗口管理器，窗口不能移动，最大最小化


由此，可以看出，linux的图像架构，一般是启动xserver，xserver找到一个图像程序
一般是窗口管理器，然后这个程序启动桌面环境，就是我们看到的桌面了

如果xserver找不到一个图形程序，或者运行的那个图形程序退出了，xserver也会退出

