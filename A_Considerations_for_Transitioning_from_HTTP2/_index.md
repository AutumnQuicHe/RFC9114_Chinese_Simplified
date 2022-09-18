---
title: "附录A. 从HTTP/2过渡时的考量"
anchor: "Appendix_A_Considerations_for_Transitioning_from_HTTP2"
weight: 130000
rank: "h1"
---

HTTP/3 is strongly informed by HTTP/2, and it bears many similarities. This section describes the approach taken to design HTTP/3, points out important differences from HTTP/2, and describes how to map HTTP/2 extensions into HTTP/3.

HTTP/3强烈地受到了HTTP/2的启发，它们间存在许多相似性。
本章介绍了设计HTTP/3时使用的方法，指出了它与HTTP/2间重要的区别，并描述了将HTTP/2扩展映射到HTTP/3的方式。

HTTP/3 begins from the premise that similarity to HTTP/2 is preferable, but not a hard requirement. HTTP/3 departs from HTTP/2 where QUIC differs from TCP, either to take advantage of QUIC features (like streams) or to accommodate important shortcomings (such as a lack of total ordering). While HTTP/3 is similar to HTTP/2 in key aspects, such as the relationship of requests and responses to streams, the details of the HTTP/3 design are substantially different from HTTP/2.

HTTP/3的设计有意与HTTP/2相似，但这不是一条硬性限制。
由于QUIC存在与TCP的不同之处，所以HTTP/3与HTTP/2也存在不同，这么做要么是为了利用QUIC的特性（比如流），要么是为了弥补一些关键的缺点（例如缺少全局的数据包排序）。
尽管HTTP/3在一些关键方面与HTTP/2类似，例如请求和响应与流的关系，但是HTTP/3设计的细节部分与HTTP/2大不相同。

Some important departures are noted in this section.

本章还记录了一些重要的差异。
