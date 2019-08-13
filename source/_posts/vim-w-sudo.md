---
title: vim的w时没有权限怎么sudo保存
date: 2019-08-13 15:59:27
tags:
 - linux命令
 - vim
categories:
 - linux命令
---


用vi或vim编辑文件，当要w保存时提示文件只读，没有权限，可以使用下面的命令来sudo保存文件，不然改了很多，从头再改很累

```
:w !sudo tee %
```

w 会把文件内容管道输入给 tee

% 是当前文件名
