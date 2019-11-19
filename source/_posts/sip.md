---
title: sip
date: 2019-11-19 20:25:00
tags:
---

sip,类似http的文本协议，也有状态码，异步，一个 SIP请求可能导致生成一个或多个请求
SIP包有三个主要部分：Request-Line(表示是请求信息)，Message Header(SIP包的消息头哉)，Message Body(SIP包的消息主体)
sip消息两种，request，respond

请求头--Request:INVITE sip: 015B0081587310007000D0000C578@192.168.2.40
Status: 100 Trying /Status: 180 Ring
Status:200 OK
Request:BYE sip: 015B0081587310007000D0000C578@192.168.2.40

sip协议，交互时许
![sip](sip协议.png)