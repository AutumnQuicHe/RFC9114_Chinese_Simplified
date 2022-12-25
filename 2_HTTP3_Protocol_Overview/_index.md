---
title: "2. HTTP/3协议概览"
anchor: "2_HTTP3_Protocol_Overview"
weight: 20000
rank: "h1"
---

HTTP/3提供了一种使用QUIC传输协议的HTTP语义传输方式，以及与HTTP/2类似的内部分帧层。

一旦客户端了解到某个网络终端处存在HTTP/3服务器，它就会打开一条QUIC连接。
QUIC提供了协议协商、基于流的多路复用和流量控制机制。
[第3.1章](#3.1_Discovering_an_HTTP_3_Endpoint)描述了如何发现HTTP/3终端。

在每条流内，HTTP/3通信的基本单元是帧（详见[第7.2章](#7.2_Frame_Definitions)）。
每种帧类型都具有其特定用途。
例如，**标头帧**和**数据帧**构成了HTTP请求和响应的基础（详见[第4.1章](#4.1_HTTP_Message_Framing)）。
会对整条连接产生影响的帧是在一条单独的控制流中传递的。

请求的多路复用是使用QUIC的流抽象来做到的，详见《[QUIC传输](../RFC9000_Chinese_Simplified)》的[第2章](../RFC9000_Chinese_Simplified/#2_Streams)。
每一对请求与响应都会消耗掉一条QUIC流。
流与流彼此独立，所以一条流的阻塞或丢包事件不会干涉其他流的传输进度。

服务器推送是一种由HTTP/2（详见《[HTTP/2](https://www.rfc-editor.org/info/rfc9113)》）引入的交互模式，它允许服务器在预料到客户端会发送某个请求时向客户端主动推送一次请求与响应的通信。
这会增加网络负载，但是有可能在延迟表现上得到提升。
多种HTTP/3帧被用于管理服务器推送，例如**推送承诺帧**、**最大推送ID帧**和**取消推送帧**。

就像在HTTP/2中的那样，请求和响应的字段在传输时是经过压缩的。
由于HPACK（详见《[HPACK](https://www.rfc-editor.org/info/rfc7541)》）要求经压缩字段组的传输是有序的（而QUIC无法提供这种保证），因此HTTP/3用QPACK（详见《[QPACK](../RFC9204_Chinese_Simplified)》）代替了HPACK。
QPACK使用独立的单向流来修改和跟踪字段查找表的状态，同时经压缩的字段组会引用查找表的状态而不会对它作出修改。
