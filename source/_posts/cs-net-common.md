---
title: 计算机网络通识
date: 2019-09-12 16:31:01
tags:
categories:
 - 计算机专业
---

![三种协议](3-protocol.png)
osi七层协议，是制定的标准
tcp/ip四层，是实际使用的标准
五层协议，是书为了教知识，自己对应的，不是标准，也不存在

![tcp/ip](tcp-protocol.jpg)
tcp/ip, 常见协议工作层

【数据链路层】, 网线直连的两个网卡接口传输数据,mac地址

ARP协议（Address Resolve Protocol，地址解析协议）和RARP协议（ReverseAddress Resolve Protocol，逆地址解析协议）

【网络层】, 与目标之间，传输经过多个【路由】（想想这个名字“路由”，就是怎么找路，然后邮递数据），解决如何选择这些路由路线

IP协议（Internet Protocol，因特网协议）和ICMP协议（Internet Control Message Protocol，因特网控制报文协议）

    ip, 根据数据包的目的IP地址来决定如何投递它,为它寻找一个合适的下一跳（next hop）路由器, 直到到达目的

    icmp,检测网络连接

【传输层】, 两台主机上的应用程序提供端到端（end to end）的通信，忽略中间的传输过程。通信数据的起始和结束

TCP协议（Transmission Control Protocol，传输控制协议）和UDP协议（User Datagram Protocol，用户数据报协议）

【应用层】，程序收到数据后的处理

各层数据单元，数据是一层层添加包头等等，上下依次处理
应用层——消息
传输层——数据段(segment)
网络层——分组、数据包（packet）
链路层——帧（frame），mtu(帧最大传输单元)
物理层——P-PDU（bit）

![网络数据封装](protocol-up-down.gif)

常见协议工作层

![tcp/ip](protocol-common.png)

tcp不能进行广播和多播，因为tcp是面向连接的

计算机程序的端口是为了区分计算机上的程序，或者对面连接的计算机上的程序
