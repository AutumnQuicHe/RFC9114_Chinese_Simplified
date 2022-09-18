---
title: "A.2.3. 流量控制的差异"
anchor: "A.2.3_Flow_Control_Differences"
weight: 130230
rank: "h3"
---

HTTP/2 specifies a stream flow-control mechanism. Although all HTTP/2 frames are delivered on streams, only the DATA frame payload is subject to flow control. QUIC provides flow control for stream data and all HTTP/3 frame types defined in this document are sent on streams. Therefore, all frame headers and payload are subject to flow control.

HTTP/2指定了一种针对流的流量控制机制。尽管所有HTTP/2的帧都是在流上传递的，但是只有**数据帧**的载荷受限于流量控制。QUIC为流数据提供了流量控制，且本文档中定义的所有类型的HTTP/3帧都是在流上发送的。因此，所有帧的头部和载荷都受限于流量控制。
