---
title: i3-wm
date: 2019-08-09 15:15:36
tags:
---


[i3](https://wiki.archlinux.org/index.php/I3)

i3是没有窗口重叠的，窗口管理器，窗口可以最大，最小，全屏，但是都是连在一起的，不像一般的层叠窗口，类似vim的多窗口模式，有鼠标，可以打开浏览器，但是窗口模式与我们场景的不一样

```
sudo pacman -S i3-wm i3status
```
启动到黑屏终端

```
startx /usr/bin/i3
```

不会用的话，就会不会操作了，推荐先 ``startx`` 启动xterm，再 ``i3`` 启动i3窗口管理器

[i3 User’s Guide](https://i3wm.org/docs/userguide.html) 看看（Default keybindings）熟悉快捷键