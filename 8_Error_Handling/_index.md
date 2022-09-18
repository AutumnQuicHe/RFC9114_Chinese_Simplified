---
title: "8. 错误处理"
anchor: "8_Error_Handling"
weight: 80000
rank: "h1"
---

When a stream cannot be completed successfully, QUIC allows the application to abruptly terminate (reset) that stream and communicate a reason; see Section 2.4 of [QUIC-TRANSPORT]. This is referred to as a "stream error". An HTTP/3 implementation can decide to close a QUIC stream and communicate the type of error. Wire encodings of error codes are defined in Section 8.1. Stream errors are distinct from HTTP status codes that indicate error conditions. Stream errors indicate that the sender did not transfer or consume the full request or response, while HTTP status codes indicate the result of a request that was successfully received.

在无法成功结束一条流时，QUIC允许应用对一条流发起中止（重置）并将原因传递给对端，详见《[QUIC传输]()》的[第2.4章]()。这被称为“流错误”。HTTP/3实现可以决定要不要关掉一条QUIC流并将原因告知对端。[第8.1章]()中定义了错误码的编码方式。同样是用于表达错误的HTTP状态码，流错误与之有一些区别。流错误表示发送方并没有传输或处理完整个请求或响应，而HTTP状态码表示的是对已成功接收到的请求的处理结果。

If an entire connection needs to be terminated, QUIC similarly provides mechanisms to communicate a reason; see Section 5.3 of [QUIC-TRANSPORT]. This is referred to as a "connection error". Similar to stream errors, an HTTP/3 implementation can terminate a QUIC connection and communicate the reason using an error code from Section 8.1.

如果需要终止一整条连接，QUIC也提供了一种传达关闭理由的机制，详见《[QUIC传输]()》的[第5.3章]()。这被称为“连接错误”。与流错误类似，HTTP/3实现可以决定要不要关闭一条QUIC连接并使用[第8.1章]()中的错误码将理由告知对端。

Although the reasons for closing streams and connections are called "errors", these actions do not necessarily indicate a problem with the connection or either implementation. For example, a stream can be reset if the requested resource is no longer needed.

尽管关闭流或连接的理由被称作为“错误”，但是这些操作并不一定表示在连接上或实现中存在问题。例如，一条流可能在请求的资源不再被需要时被重置。

An endpoint MAY choose to treat a stream error as a connection error under certain circumstances, closing the entire connection in response to a condition on a single stream. Implementations need to consider the impact on outstanding requests before making this choice.

在某些情况下，终端**可以**选择将流错误视作为连接错误，用关闭整条连接的方式来响应单条流上出现的流错误。实现在做出此决定前需要考虑这么做对于在途请求的影响。

Because new error codes can be defined without negotiation (see Section 9), use of an error code in an unexpected context or receipt of an unknown error code MUST be treated as equivalent to H3_NO_ERROR. However, closing a stream can have other effects regardless of the error code; for example, see Section 4.1.

由于无需协商即可定义新的错误码（详见[第9章]()），所以如果在意料外的上下文中使用了某错误码，或接收到了一个未知类型的错误码，那么**必须**将该情况视作类型为`H3_NO_ERROR`（无错误）的错误。然而无论哪个错误码，关闭一条流总会带来其他影响，[第4.1章]()中就有一个例子。
