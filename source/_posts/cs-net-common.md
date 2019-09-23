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

物理层，两个连接的计算机怎么物理通信
传输媒体：
    导向传输媒体，双绞线，同轴电缆，光纤
    非导向传输媒体，无线电波
信道复用：频分复用，时分复用，统计时分复用，波分复用（光的频分复用），码分复用(同一频率，用户不同码型)

数据链路层，两个连接的计算机数据通信
链路，两个连接的物理线路
数据链路，链路与通信协议，
帧
  封装成帧，mtu，添加帧首尾，soh（start of head）,eot（end of transmission）
  透明传输，字节填充，帧中间出现eot前加esc转义，esc前也要加esc，
  错检测，crc
ppp,点对点协议，首尾为0x7e，
以太网，总线型，广播，csma/cd
网桥，链路层的mac子层，隔离开碰撞域

网络层，数据包，从源地址到目的地址，ip数据报
两种服务，虚电路服务(vc,需要建立连接)，数据报服务(ip数据报)
ip数据报中，网络层的源地址与目的地址在传输中不变，数据链路层的mac帧的源地址(mac物理地址)与目的地址会根据发往的节点改变
arp，通过ip找到对应的物理地址，局域网内，数据根据mac地址传输，arp会通过广播找到需要的ip的硬件地址
icmp，网际控制报文协议，基于ip数据报
    差错报告报文，（终点不可达，源点抑制（包因拥塞丢弃），超时，参数问题）
    询问报文，（回送，时间戳）
ping使用icmp的回送请求与回送回答报文
traceroute，每次ttl加1，收到icmp超时报文，数据报是无法交付的udp，最终达到会收到icmp终点不可达报文
路由选择协议
    icp内部网关协议，rip，ospf
    egp外部网关协议，bgp-4

运输层，tcp，udp