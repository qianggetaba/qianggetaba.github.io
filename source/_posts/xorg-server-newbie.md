---
title: xorg-server的c语言入门
date: 2019-08-09 11:29:40
tags:
 - xorg
 - xorg-server
categories:
 - xorg
---

xorg官网[Xlib - C Language X Interface](https://www.x.org/releases/X11R7.7/doc/libX11/libX11/libX11.html#Introduction_to_Xlib)

初级教程[Xlib programming: a short tutorial](https://tronche.com/gui/x/xlib-tutorial/)

linux的桌面图形架构图，使用client与server模式，server使用x11（X Window System protocol）协议，client通过x11协议告诉server怎么显示，显示什么，就像http协议，xlib是封装协议的c库，使用xlib可以方便与xserver建立连接，显示图像，不用关注协议具体的字节格式等等

![xorg-server](xorg-server.png)

xlib是对x11显示服务协议的封装

x11定义了客户端到服务器端的显示模式

下面的小例子，演示怎么使用xlib库

```
测试库文件，有桌面环境的应该都有
sudo find / -name Xlib.h
sudo find / -name libX11.so
```

[prog-2.cc](https://tronche.com/gui/x/xlib-tutorial/prog-2.cc)
```
// Written by Ch. Tronche (http://tronche.lri.fr:8000/)
// Copyright by the author. This is unmaintained, no-warranty free software. 
// Please use freely. It is appreciated (but by no means mandatory) to
// acknowledge the author's contribution. Thank you.
// Started on Thu Jun 26 23:29:03 1997

//
// Xlib tutorial: 2nd program
// Make a window appear on the screen and draw a line inside.
// If you don't understand this program, go to
// http://tronche.lri.fr:8000/gui/x/xlib-tutorial/2nd-program-anatomy.html
//

#include <X11/Xlib.h> // Every Xlib program must include this
#include <assert.h>   // I include this to test return values the lazy way
#include <unistd.h>   // So we got the profile for 10 seconds

#define NIL (0)       // A name for the void pointer

main()
{
      // Open the display

      Display *dpy = XOpenDisplay(NIL);
      assert(dpy);

      // Get some colors

      int blackColor = BlackPixel(dpy, DefaultScreen(dpy));
      int whiteColor = WhitePixel(dpy, DefaultScreen(dpy));

      // Create the window

      Window w = XCreateSimpleWindow(dpy, DefaultRootWindow(dpy), 0, 0, 
				     200, 100, 0, blackColor, blackColor);

      // We want to get MapNotify events

      XSelectInput(dpy, w, StructureNotifyMask);

      // "Map" the window (that is, make it appear on the screen)

      XMapWindow(dpy, w);

      // Create a "Graphics Context"

      GC gc = XCreateGC(dpy, w, 0, NIL);

      // Tell the GC we draw using the white color

      XSetForeground(dpy, gc, whiteColor);

      // Wait for the MapNotify event

      for(;;) {
	    XEvent e;
	    XNextEvent(dpy, &e);
	    if (e.type == MapNotify)
		  break;
      }

      // Draw the line
      
      XDrawLine(dpy, w, gc, 10, 60, 180, 20);

      // Send the "DrawLine" request to the server

      XFlush(dpy);

      // Wait for 10 seconds

      sleep(10);
}
```

```
gcc -L /usr/lib/ -lX11 -o xorg prog-2.cc
./xorg

桌面出现一个小黑窗口，上面一条白线，10s后关闭，只关闭窗口程序没结束
```