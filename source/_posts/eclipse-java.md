---
title: eclipse-java
date: 2019-11-01 11:15:58
tags:
---


[Eclipse Packages](https://www.eclipse.org/downloads/packages/)

[lombok](https://projectlombok.org/downloads/lombok.jar)
lombok.jar to eclipse.exe folder
```
//eclipse.ini tail add
-javaagent:D:\java\eclipse\lombok.jar
```

make eclipse activities desktop icon
```
touch eclipse.desktop
nano eclipse.desktop
[Desktop Entry]
Version=Neon
Name=Eclipse
Comment=Eclipse is an IDE
Exec=/home/li/eclipse/eclipse
Path=/home/li/eclipse/
Icon=/home/li/eclipse/icon.xpm
Terminal=false
Type=Application
Categories=Utility;Application;Development;
mv eclipse.desktop .local/share/applications/
```