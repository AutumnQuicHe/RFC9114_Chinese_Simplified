---
title: "6. 流的映射与用法"
anchor: "6_Stream_Mapping_and_Usage"
weight: 60000
rank: "h1"
---

A QUIC stream provides reliable in-order delivery of bytes, but makes no guarantees about order of delivery with regard to bytes on other streams. In version 1 of QUIC, the stream data containing HTTP frames is carried by QUIC STREAM frames, but this framing is invisible to the HTTP framing layer. The transport layer buffers and orders received stream data, exposing a reliable byte stream to the application. Although QUIC permits out-of-order delivery within a stream, HTTP/3 does not make use of this feature.

QUIC流为数据提供了可靠且有序的交付，但是它并不保证不同流之间的数据交付顺序。在QUIC版本1中，包含HTTP帧的流数据是使用QUIC**流帧**来传递的，但是这种封装对于HTTP的分帧层是透明的。传输层缓存并整理接收到的流数据，从而向应用提供可靠且有序的字节流。尽管QUIC允许在单条流之中的数据乱序抵达，但是HTTP/3并没有利用该特性。

QUIC streams can be either unidirectional, carrying data only from initiator to receiver, or bidirectional, carrying data in both directions. Streams can be initiated by either the client or the server. For more detail on QUIC streams, see Section 2 of [QUIC-TRANSPORT].

QUIC流可以是单向的，从发送方向接收方传递数据，或双向的，在两个方向上都能传递数据。流可以由客户端或服务器中的任一一方发起。有关QUIC流的更多细节，详见《[QUIC传输]()》的[第2章]()。

When HTTP fields and data are sent over QUIC, the QUIC layer handles most of the stream management. HTTP does not need to do any separate multiplexing when using QUIC: data sent over a QUIC stream always maps to a particular HTTP transaction or to the entire HTTP/3 connection context.

通过QUIC发送HTTP字段和数据时，QUIC层解决了绝大多数的流管理问题。只要使用了QUIC，HTTP就不需要再实现任何多路复用机制：通过QUIC流发送的任何数据都存在其归属的某个HTTP事务，或者整条HTTP/3连接的上下文。
