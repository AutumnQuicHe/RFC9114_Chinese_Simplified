---
title: "6.2. 单向流"
anchor: "6.2_Unidirectional_Streams"
weight: 60200
rank: "h2"
---

Unidirectional streams, in either direction, are used for a range of purposes. The purpose is indicated by a stream type, which is sent as a variable-length integer at the start of the stream. The format and structure of data that follows this integer is determined by the stream type.

单向流，无论哪个方向，具有多种用途。
单向流的具体的用途是由流类型决定的，它以可变长度整型的形式出现在流的起始处。
跟在该整型值后方的数据的格式是由流类型决定的。

Unidirectional Stream Header {
Stream Type (i),
}
Figure 1: Unidirectional Stream Header

{{% block_ref
indx="Figure_1_Unidirectional_Stream_Header"
title="图1：单向流的头部" %}}

```
单向流的头部 {
  流类型 (i),
}
```

{{% /block_ref %}}

Two stream types are defined in this document: control streams (Section 6.2.1) and push streams (Section 6.2.2). [QPACK] defines two additional stream types. Other stream types can be defined by extensions to HTTP/3; see Section 9 for more details. Some stream types are reserved (Section 6.2.3).

本文档定义了两种流类型：控制流（详见[第6.2.1章]()）和推送流（详见[6.2.2章]()）。
《[QPACK]()》定义了两种额外的帧类型。
HTTP/3的扩展还可以定义其他流类型；详见[第9章]()。
某些流类型是被保留使用的（详见[第6.2.3章]()）。

The performance of HTTP/3 connections in the early phase of their lifetime is sensitive to the creation and exchange of data on unidirectional streams. Endpoints that excessively restrict the number of streams or the flow-control window of these streams will increase the chance that the remote peer reaches the limit early and becomes blocked. In particular, implementations should consider that remote peers may wish to exercise reserved stream behavior (Section 6.2.3) with some of the unidirectional streams they are permitted to use.

在一条HTTP/3连接的生命周期的早期阶段，单向流上的数据创建与通信很容易影响到连接的性能。
过度限制流的最大数量或这些流的流量控制窗口的终端将增大对端迅速抵达这些上限值并被阻塞的可能性。
特别是，实现应该考虑到对端可能想要通过某些允许使用的单向流来利用被保留使用的流的行为（详见[第6.2.3章]()）。

Each endpoint needs to create at least one unidirectional stream for the HTTP control stream. QPACK requires two additional unidirectional streams, and other extensions might require further streams. Therefore, the transport parameters sent by both clients and servers MUST allow the peer to create at least three unidirectional streams. These transport parameters SHOULD also provide at least 1,024 bytes of flow-control credit to each unidirectional stream.

每个终端都需要创建至少一条作为HTTP控制流的单向流。
除此之外，QPACK需要两条单向流，并且其他扩展还可能需要更多流。
因此，客户端和服务器发送的传输参数都**必须**允许对端创建至少三条单向流。
这些传输参数还**应该**为每条单向流提供至少1024字节的流量控制额度。

Note that an endpoint is not required to grant additional credits to create more unidirectional streams if its peer consumes all the initial credits before creating the critical unidirectional streams. Endpoints SHOULD create the HTTP control stream as well as the unidirectional streams required by mandatory extensions (such as the QPACK encoder and decoder streams) first, and then create additional streams as allowed by their peer.

注意，如果对端在创建关键的单向流前就用完了所有的初始额度，那么终端没有必要额外提供用于创建更多单向流的额度。
终端**应该**首先创建HTTP控制流和强制使用的扩展（例如QPACK的编码器流和解码器流）所需的单向流，然后再在对端允许下创建额外的流。

If the stream header indicates a stream type that is not supported by the recipient, the remainder of the stream cannot be consumed as the semantics are unknown. Recipients of unknown stream types MUST either abort reading of the stream or discard incoming data without further processing. If reading is aborted, the recipient SHOULD use the H3_STREAM_CREATION_ERROR error code or a reserved error code (Section 8.1). The recipient MUST NOT consider unknown stream types to be a connection error of any kind.

如果流的头部位置是一种不被接收方支持的流类型，那么就不能用已知的语义来使用流的剩余部分。
未知类型的流的接收方**必须**要么中止流的读取，要么丢弃传入数据且不进行任何处理。
如果选择中止读取，那么接收方**应该**使用错误码`H3_STREAM_CREATION_ERROR`（流创建错误）或某被保留使用的错误码（详见[第8.1章]()）。
接收方**必须不**将接收到未知类型的流的情况视作为任何类型的连接错误。

As certain stream types can affect connection state, a recipient SHOULD NOT discard data from incoming unidirectional streams prior to reading the stream type.

由于特定的流类型可以影响连接状态，因此接收方**不应该**在读取单向流的流类型前就丢弃传入的数据。

Implementations MAY send stream types before knowing whether the peer supports them. However, stream types that could modify the state or semantics of existing protocol components, including QPACK or other extensions, MUST NOT be sent until the peer is known to support them.

实现**可以**在了解到对端是否支持某些流类型前就将它们发送出去。
然而，在了解到对端支持与否之前，**必须不**发送会修改现存协议组件的状态或语义的流类型，包括QPACK或其他扩展。

A sender can close or reset a unidirectional stream unless otherwise specified. A receiver MUST tolerate unidirectional streams being closed or reset prior to the reception of the unidirectional stream header.

除非特别规定，否则发送方可以关闭或重置一条单向流。
接收方**必须**容忍在接收到单向流的头部前单向流就被关闭或重置的情况。
