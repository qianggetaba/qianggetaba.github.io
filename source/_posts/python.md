---
title: python
date: 2019-10-08 10:03:46
tags:
categories:
 - python
---

[python-3.7.4-amd64.exe](https://www.python.org/ftp/python/3.7.4/python-3.7.4-amd64.exe)

默认安装,路径``C:\Users\winuser\AppData\Local\Programs\Python\Python37``

cmd， 试试``py``或者``python``，进入python交互式命令>>>,说明安装正常，``exit()`` 退出


新建文件``test.py``，测试功能
```
print("hello world!")
```

注释
单行``#``
多行``'''``,``"""``

行缩进表示代码块(就是``{}``)，同一代码块的缩进【必须】一致

``\``连接多行语句

``"""``多行字符串

import 与 from...import

变量类型【自动】，直接定义``name="runoob"``

基本数据结构，list, tuple(元组，不可变list),set,dic(就是map)

元组使用小括号，列表使用方括号

{ } 或者 set()

dict = {'Alice': '2341', 'Beth': '9102', 'Cecil': '3258'}

基本逻辑处理，if,else,while 最后是分号

for in 循环遍历元组，列表，字符串，数字

```
if __name__ == '__main__':
   print('程序自身在运行')
else:
   print('被引用为模块')
```

目录包含一个 ``__init__.py``文件，是一个包

``__init__.py``内的``__all__``的列表变量，作为导入包的名称

class,

``def __init__(self,n,a,w):``类的构造函数
``class Chinese(Person):``类继承，支持多继承
单下划线开头，protected
双下划线开头，private，私有方法一样加

可以直接定义函数，``def network():``


模块安装``py -m pip install psutil``

windows系统模块，``pywin32``,``wmi``