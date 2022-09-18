---
title: "A.2. HTTP帧类型"
anchor: "A.2_HTTP_Frame_Types"
weight: 130200
rank: "h2"
---

Many framing concepts from HTTP/2 can be elided on QUIC, because the transport deals with them. Because frames are already on a stream, they can omit the stream number. Because frames do not block multiplexing (QUIC's multiplexing occurs below this layer), the support for variable-maximum-length packets can be removed. Because stream termination is handled by QUIC, an END_STREAM flag is not required. This permits the removal of the Flags field from the generic frame layout.

许多来自HTTP/2的帧概念都可以被舍弃了，因为QUIC会处理它们。
帧都是在流上发送的，所以在帧中可以省略流编号。
因为帧并不会阻塞多路复用（QUIC的多路复用运行于比分帧更低的层），还可以移除对可变最大长度数据包的支持。
由于流的终止是由QUIC处理的，“结束流”标志位也不再被需要，从而可以将“标志（Flags）“字段从通用的帧结构中移除。

Frame payloads are largely drawn from [HTTP/2]. However, QUIC includes many features (e.g., flow control) that are also present in HTTP/2. In these cases, the HTTP mapping does not re-implement them. As a result, several HTTP/2 frame types are not required in HTTP/3. Where an HTTP/2-defined frame is no longer used, the frame ID has been reserved in order to maximize portability between HTTP/2 and HTTP/3 implementations. However, even frame types that appear in both mappings do not have identical semantics.

帧的载荷结构与《[HTTP/2]()》大致相似。
不过，QUIC包含了许多出现在HTTP/2中的特性（例如，流量控制）。
在这些情况下，HTTP映射没有对它们进行重新实现。
因此，HTTP/3中不再需要一些HTTP/2的帧类型。
尽管不再需要一些为HTTP/2定义的帧类型，其类型值仍被保留使用，为的是最大化HTTP/2和HTTP/3实现间的可移植性。
然而，即便某些帧类型在两种映射中同时出现，它们的语义也并不一致，

Many of the differences arise from the fact that HTTP/2 provides an absolute ordering between frames across all streams, while QUIC provides this guarantee on each stream only. As a result, if a frame type makes assumptions that frames from different streams will still be received in the order sent, HTTP/3 will break them.

许多差异源自HTTP/2在多条流间提供帧的绝对顺序的事实，而QUIC仅在单条流上提供这种保证。
因此，如果某种帧类型假定了不同流上的帧在被接受到时会保持其发送顺序，那么实际上HTTP/3会违背这种假设。

Some examples of feature adaptations are described below, as well as general guidance to extension frame implementors converting an HTTP/2 extension to HTTP/3.

下文介绍了一些将来的可能用得上的适配样例，以及一份通用的关于帧类型扩展的实现方如何将HTTP/2扩展转换为HTTP/3扩展的指引。
