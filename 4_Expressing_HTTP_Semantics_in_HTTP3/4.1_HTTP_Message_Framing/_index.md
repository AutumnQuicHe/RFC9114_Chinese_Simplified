---
title: "4.1. HTTP消息分帧"
anchor: "4.1_HTTP_Message_Framing"
weight: 40100
rank: "h2"
---

A client sends an HTTP request on a request stream, which is a client-initiated bidirectional QUIC stream; see Section 6.1. A client MUST send only a single request on a given stream. A server sends zero or more interim HTTP responses on the same stream as the request, followed by a single final HTTP response, as detailed below. See Section 15 of [HTTP] for a description of interim and final HTTP responses.

客户端在请求流上发送HTTP请求，请求流是一条由客户端发起的QUIC双向流，详见[第6.1章]()。在一条给定的流上，客户端**必须**只能发送一次请求。在请求所在的流上，服务器可以发送任意数量的（或不发送）临时HTTP响应，接着再发送一个最终的HTTP响应，如下文所述。有关临时和最终HTTP响应的描述，详见《[HTTP]()》的[第15章]()。

Pushed responses are sent on a server-initiated unidirectional QUIC stream; see Section 6.2.2. A server sends zero or more interim HTTP responses, followed by a single final HTTP response, in the same manner as a standard response. Push is described in more detail in Section 4.6.

要推送的响应的是在一条由服务器发起的QUIC单向流上被发送的，详见[第6.2.2章]()。就像标准的响应过程一样，服务器可以发送任意数量的（或不发送）临时HTTP响应，接着再发送一个最终的HTTP响应。[第4.6章]()详细描述了服务器推送。

On a given stream, receipt of multiple requests or receipt of an additional HTTP response following a final HTTP response MUST be treated as malformed.

在某条给定的流上，如果接收到多个请求，或者在最终的HTTP响应后又接收到了额外的HTTP响应，那么**必须**将这类请求或响应视作为畸形的。

An HTTP message (request or response) consists of:

一条HTTP消息（请求或响应）由以下部分构成：

1. the header section, including message control data, sent as a single HEADERS frame,

1. 头部，其中包括消息控制数据，它是用单个**标头帧**来发送的；

2. optionally, the content, if present, sent as a series of DATA frames, and

2. 可选的内容，它是用一系列**数据帧**来发送的（如果存在内容的话）；和

3. optionally, the trailer section, if present, sent as a single HEADERS frame.

3. 可选的尾部，它是用单个**标头帧**来发送的（如果存在尾部的话）。

Header and trailer sections are described in Sections 6.3 and 6.5 of [HTTP]; the content is described in Section 6.4 of [HTTP].

《[HTTP]()》的[第6.3章]()和[第6.5章]()描述了头部和尾部；《[HTTP]()》的[第6.4章]()描述了内容。

Receipt of an invalid sequence of frames MUST be treated as a connection error of type H3_FRAME_UNEXPECTED. In particular, a DATA frame before any HEADERS frame, or a HEADERS or DATA frame after the trailing HEADERS frame, is considered invalid. Other frame types, especially unknown frame types, might be permitted subject to their own rules; see Section 9.

如果接收到的帧序列并非按照此顺序，那么**必须**将这种情况视作类型为`H3_FRAME_UNEXPECTED`（意料外的帧）的连接错误。尤其是，在任何**标头帧**之前的**数据帧**，或在末尾的**标头帧**之后出现的**标头帧**或**数据帧**，均被视作非法。其他类型的帧，尤其是未知类型的帧，可能会按照它们自己的规则被批准出现在某处；详见[第9章]()。

A server MAY send one or more PUSH_PROMISE frames before, after, or interleaved with the frames of a response message. These PUSH_PROMISE frames are not part of the response; see Section 4.6 for more details. PUSH_PROMISE frames are not permitted on push streams; a pushed response that includes PUSH_PROMISE frames MUST be treated as a connection error of type H3_FRAME_UNEXPECTED.

服务器**可以**在响应消息的帧之前、之后、甚至是以穿插其中的方式，发送一个或多个**推送承诺帧**。这些**推送承诺帧**不是响应的一部分；有关更多细节详见[第4.6章]()。**推送承诺帧**不被允许出现在推送流中；如果被推送的响应中包含着**推送承诺帧**，那么**必须**将该情况视作类型为`H3_FRAME_UNEXPECTED`的连接错误。

Frames of unknown types (Section 9), including reserved frames (Section 7.2.8) MAY be sent on a request or push stream before, after, or interleaved with other frames described in this section.

未知类型的帧（详见[第9章]()），包括类型值被保留使用的帧（详见[第7.2.8章]()），可以在本章描述的其他类型的帧之前、之后、甚至是以穿插其中的方式，被发送。

The HEADERS and PUSH_PROMISE frames might reference updates to the QPACK dynamic table. While these updates are not directly part of the message exchange, they must be received and processed before the message can be consumed. See Section 4.2 for more details.

**标头帧**和**推送承诺帧**可能引用的是更新后的QPACK动态查找表。尽管这些对查找表的更新并不直接属于本次消息通信，但是只有接收并处理了这些更新，才能处理消息。有关更多细节，详见[第4.2章]()。

Transfer codings (see Section 7 of [HTTP/1.1]) are not defined for HTTP/3; the Transfer-Encoding header field MUST NOT be used.

传输编码（详见《[HTTP/1.1]()》的[第7章]()）不是为HTTP/3定义的；**必须不**使用标头字段`Transfer-Encoding`。

A response MAY consist of multiple messages when and only when one or more interim responses (1xx; see Section 15.2 of [HTTP]) precede a final response to the same request. Interim responses do not contain content or trailer sections.

当且仅当在某个请求的最终响应前存在一个或多个临时响应（状态码为`1xx`；详见《[HTTP]()》的[第15.2章]()）时，此次响应才**可以**由多条消息组成。临时响应并不包含内容和尾部。

An HTTP request/response exchange fully consumes a client-initiated bidirectional QUIC stream. After sending a request, a client MUST close the stream for sending. Unless using the CONNECT method (see Section 4.4), clients MUST NOT make stream closure dependent on receiving a response to their request. After sending a final response, the server MUST close the stream for sending. At this point, the QUIC stream is fully closed.

一轮完整的HTTP请求与响应的通信正好消耗掉一条由客户端发起的QUIC双向流。在发送完请求后，客户端**必须**关闭流的发送部分。除非使用的是`CONNECT`方法（详见[第4.4章]()），否则客户端**必须不**将接收到请求的响应作为关闭流的前置条件。在发送完最终的响应后，服务器**必须**关闭流的发送部分。此时，一条QUIC流就被完全关闭了。

When a stream is closed, this indicates the end of the final HTTP message. Because some messages are large or unbounded, endpoints SHOULD begin processing partial HTTP messages once enough of the message has been received to make progress. If a client-initiated stream terminates without enough of the HTTP message to provide a complete response, the server SHOULD abort its response stream with the error code H3_REQUEST_INCOMPLETE.

一条流的关闭意味着最后那条HTTP消息的结束。由于一些消息非常巨大，或没有设置边界，所以终端**应该**在消息接收到足以进行处理时就开始消耗部分HTTP消息。如果在由客户端发起的流上还没有接收到足以创建完整响应的HTTP消息，这条流就被终止了，那么服务器**应该**使用错误码`H3_REQUEST_INCOMPLETE`（不完整的请求）来中止其响应流。

A server can send a complete response prior to the client sending an entire request if the response does not depend on any portion of the request that has not been sent and received. When the server does not need to receive the remainder of the request, it MAY abort reading the request stream, send a complete response, and cleanly close the sending part of the stream. The error code H3_NO_ERROR SHOULD be used when requesting that the client stop sending on the request stream. Clients MUST NOT discard complete responses as a result of having their request terminated abruptly, though clients can always discard responses at their discretion for other reasons. If the server sends a partial or complete response but does not abort reading the request, clients SHOULD continue sending the content of the request and close the stream normally.

如果要创建的响应并不依赖于请求中尚未被发送或接收的部分，那么服务器可以在客户端发送出完整的请求前就发送回完整的响应。当服务器不需要接收请求的剩余部分时，它**可以**中止读取请求流，将完整的响应发送回去，然后再干净地关闭流的发送部分。当要求客户端停止在请求流上发送数据时，**应该**使用错误码`H3_NO_ERROR`（无错误）。尽管客户端总是可以出于其他理由自行决定要不要忽略响应，但是客户端**必须不**因为请求的发送过程被突然中止而将完整的响应忽略。如果服务器发送的响应并不完整，或发送了完整的响应但是没有中止读取请求，那么客户端**应该**继续发送请求的内容，然后将流正常关闭。
