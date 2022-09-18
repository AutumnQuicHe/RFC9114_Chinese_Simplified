---
title: "A.1. 流"
anchor: "A.1_Streams"
weight: 130100
rank: "h2"
---

HTTP/3 permits use of a larger number of streams (262-1) than HTTP/2. The same considerations about exhaustion of stream identifier space apply, though the space is significantly larger such that it is likely that other limits in QUIC are reached first, such as the limit on the connection flow-control window.

比起HTTP/2，HTTP/3允许使用大量的流（<code>2<sup>62</sup>-1</code>）。关于流标识符耗尽的考量仍然适用，尽管该空间已经大到很有可能是QUIC的其他限制先被触及，例如对连接的流量控制窗口的限制。

In contrast to HTTP/2, stream concurrency in HTTP/3 is managed by QUIC. QUIC considers a stream closed when all data has been received and sent data has been acknowledged by the peer. HTTP/2 considers a stream closed when the frame containing the END_STREAM bit has been committed to the transport. As a result, the stream for an equivalent exchange could remain "active" for a longer period of time. HTTP/3 servers might choose to permit a larger number of concurrent client-initiated bidirectional streams to achieve equivalent concurrency to HTTP/2, depending on the expected usage patterns.

与HTTP/2不同，HTTP/3中的流并发是由QUIC管理的。当所有数据已经被接收完毕，且所有已发送的数据都得到了对端的确认时，QUIC才会将一条流认定为关闭状态。而当包含着“结束流（`END_STREAM`）”比特位的帧被交付到传输层时HTTP/2就会将一条流认定为关闭状态。因此，即便对于两份一致的通信内容，流也会在更长的一段时间内保持“活跃”状态。根据预想的并发场景模型，HTTP/3服务器可以在同一时间下允许更多由客户端发起的双向流来达到与HTTP/2相等的并发性能。

In HTTP/2, only request and response bodies (the frame payload of DATA frames) are subject to flow control. All HTTP/3 frames are sent on QUIC streams, so all frames on all streams are flow controlled in HTTP/3.

在HTTP/2中，只有请求体和响应体（**数据帧**的载荷）受限于流量控制。而所有HTTP/3帧都是在QUIC流上发送的，所以在HTTP/3中，任何流上发送的任何帧均受到流量控制。

Due to the presence of other unidirectional stream types, HTTP/3 does not rely exclusively on the number of concurrent unidirectional streams to control the number of concurrent in-flight pushes. Instead, HTTP/3 clients use the MAX_PUSH_ID frame to control the number of pushes received from an HTTP/3 server.

由于其他类型的单向流的存在，HTTP/3并不完全依赖当前的活跃单向流数量来控制在途推送的并发数量。相反，HTTP/3客户端使用**最大推送帧**来控制从HTTP/3服务器接收到的推送数量。