---
title: find查找是不要显示"Permission denied"
date: 2019-08-15 16:16:31
tags:
 - find
categories:
 - linux命令
---

```
find / -name art  2>&1 | grep -v "Permission denied"
```
