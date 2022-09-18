---
title: "2. HTTP/3协议概览"
anchor: "2_HTTP3_Protocol_Overview"
weight: 20000
rank: "h1"
---

HTTP/3 provides a transport for HTTP semantics using the QUIC transport protocol and an internal framing layer similar to HTTP/2.

HTTP/3提供了一种使用QUIC传输协议的HTTP语义传输方式，以及与HTTP/2类似的内部分帧层。

Once a client knows that an HTTP/3 server exists at a certain endpoint, it opens a QUIC connection. QUIC provides protocol negotiation, stream-based multiplexing, and flow control. Discovery of an HTTP/3 endpoint is described in Section 3.1.

一旦客户端了解到某个网络终端处存在HTTP/3服务器，它就会打开一条QUIC连接。
QUIC提供了协议协商、基于流的多路复用和流量控制机制。
[第3.1章]()描述了如何发现HTTP/3终端。

Within each stream, the basic unit of HTTP/3 communication is a frame (Section 7.2). Each frame type serves a different purpose. For example, HEADERS and DATA frames form the basis of HTTP requests and responses (Section 4.1). Frames that apply to the entire connection are conveyed on a dedicated control stream.

在每条流内，HTTP/3通信的基本单元是帧（详见[第7.2章]()）。
每种帧类型都具有其特定用途。
例如，**标头帧**和**数据帧**构成了HTTP请求和响应的基础（详见[第4.1章]()）。
会对整条连接产生影响的帧是在一条单独的控制流中传递的。

Multiplexing of requests is performed using the QUIC stream abstraction, which is described in Section 2 of [QUIC-TRANSPORT]. Each request-response pair consumes a single QUIC stream. Streams are independent of each other, so one stream that is blocked or suffers packet loss does not prevent progress on other streams.

请求的多路复用是使用QUIC的流抽象来做到的，详见《[QUIC传输]()》的[第2章]()。
每一对请求与响应都会消耗掉一条QUIC流。
流与流彼此独立，所以一条流的阻塞或丢包事件不会干涉其他流的传输进度。

Server push is an interaction mode introduced in HTTP/2 ([HTTP/2]) that permits a server to push a request-response exchange to a client in anticipation of the client making the indicated request. This trades off network usage against a potential latency gain. Several HTTP/3 frames are used to manage server push, such as PUSH_PROMISE, MAX_PUSH_ID, and CANCEL_PUSH.

服务器推送是一种由HTTP/2（详见《[HTTP/2]()》）引入的交互模式，它允许服务器在预料到客户端会发送某个请求时向客户端主动推送一次请求与响应的通信。
这会增加网络负载，但是有可能在延迟表现上得到提升。
多种HTTP/3帧被用于管理服务器推送，例如**推送承诺帧**、**最大推送ID帧**和**取消推送帧**。

As in HTTP/2, request and response fields are compressed for transmission. Because HPACK ([HPACK]) relies on in-order transmission of compressed field sections (a guarantee not provided by QUIC), HTTP/3 replaces HPACK with QPACK ([QPACK]). QPACK uses separate unidirectional streams to modify and track field table state, while encoded field sections refer to the state of the table without modifying it.

就像在HTTP/2中的那样，请求和响应的字段在传输时是经过压缩的。
由于HPACK（详见《[HPACK]()》）要求经压缩字段组的传输是有序的（而QUIC无法提供这种保证），因此HTTP/3用QPACK（详见《[QPACK]()》）代替了HPACK。
QPACK使用独立的单向流来修改和跟踪字段查找表的状态，同时经压缩的字段组会引用查找表的状态而不会对它作出修改。
