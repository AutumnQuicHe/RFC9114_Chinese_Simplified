---
title: "6. 流的映射与用法"
anchor: "6_Stream_Mapping_and_Usage"
weight: 60000
rank: "h1"
---

QUIC流为数据提供了可靠且有序的交付，但是它并不保证不同流之间的数据交付顺序。
在QUIC版本1中，包含HTTP帧的流数据是使用QUIC**流帧**来传递的，但是这种封装对于HTTP的分帧层是透明的。
传输层缓存并整理接收到的流数据，从而向应用提供可靠且有序的字节流。
尽管QUIC允许在单条流之中的数据乱序抵达，但是HTTP/3并没有利用该特性。

QUIC流可以是单向的，从发送方向接收方传递数据，或双向的，在两个方向上都能传递数据。
流可以由客户端或服务器中的任一一方发起。
有关QUIC流的更多细节，详见《[QUIC传输](../RFC9000_Chinese_Simplified)》的[第2章](../RFC9000_Chinese_Simplified/#2_Streams)。

通过QUIC发送HTTP字段和数据时，QUIC层解决了绝大多数的流管理问题。
只要使用了QUIC，HTTP就不需要再实现任何多路复用机制：通过QUIC流发送的任何数据都存在其归属的某个HTTP事务，或者整条HTTP/3连接的上下文。
