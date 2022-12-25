---
title: "4.1. HTTP消息分帧"
anchor: "4.1_HTTP_Message_Framing"
weight: 40100
rank: "h2"
---

客户端在请求流上发送HTTP请求，请求流是一条由客户端发起的QUIC双向流，详见[第6.1章](#6.1_Bidirectional_Streams)。
在一条给定的流上，客户端{{< req_level MUST >}}只能发送一次请求。
在请求所在的流上，服务器可以发送任意数量的（或不发送）临时HTTP响应，接着再发送一个最终的HTTP响应，如下文所述。
有关临时和最终HTTP响应的描述，详见《[HTTP](https://www.rfc-editor.org/info/rfc9110)》的[第15章](https://www.rfc-editor.org/rfc/rfc9110#section-15)。

要推送的响应的是在一条由服务器发起的QUIC单向流上被发送的，详见[第6.2.2章](#6.2.2_Push_Streams)。
就像标准的响应过程一样，服务器可以发送任意数量的（或不发送）临时HTTP响应，接着再发送一个最终的HTTP响应。
[第4.6章](#4.6_Server_Push)详细描述了服务器推送。

在某条给定的流上，如果接收到多个请求，或者在最终的HTTP响应后又接收到了额外的HTTP响应，那么{{< req_level MUST >}}将这类请求或响应视作为畸形的。

一条HTTP消息（请求或响应）由以下部分构成：

1. 头部，其中包括消息控制数据，它是用单个**标头帧**来发送的；

2. 可选的内容，它是用一系列**数据帧**来发送的（如果存在内容的话）；和

3. 可选的挂载，它是用单个**标头帧**来发送的（如果存在挂载的话）。

《[HTTP](https://www.rfc-editor.org/info/rfc9110)》的[第6.3章](https://www.rfc-editor.org/rfc/rfc9110#section-6.3)和[第6.5章](https://www.rfc-editor.org/rfc/rfc9110#section-6.5)描述了头部和挂载；《[HTTP](https://www.rfc-editor.org/info/rfc9110)》的[第6.4章](https://www.rfc-editor.org/rfc/rfc9110#section-6.4)描述了内容。

如果接收到的帧序列并非按照此顺序，那么{{< req_level MUST >}}将这种情况视作类型为`H3_FRAME_UNEXPECTED`（意料外的帧）的连接错误。
尤其是，在任何**标头帧**之前的**数据帧**，或在末尾的**标头帧**之后出现的**标头帧**或**数据帧**，均被视作非法。
其他类型的帧，尤其是未知类型的帧，可能会按照它们自己的规则被批准出现在某处；详见[第9章](#9_Extensions_to_HTTP3)。

服务器{{< req_level MAY >}}在响应消息的帧之前、之后、甚至是以穿插其中的方式，发送一个或多个**推送承诺帧**。
这些**推送承诺帧**不是响应的一部分；有关更多细节详见[第4.6章](#4.6_Server_Push)。
**推送承诺帧**不被允许出现在推送流中；如果被推送的响应中包含着**推送承诺帧**，那么{{< req_level MUST >}}将该情况视作类型为`H3_FRAME_UNEXPECTED`的连接错误。

未知类型的帧（详见[第9章](#9_Extensions_to_HTTP3)），包括类型值被保留使用的帧（详见[第7.2.8章](#7.2.8_Reserved_Frame_Types)），可以在本章描述的其他类型的帧之前、之后、甚至是以穿插其中的方式，被发送。

**标头帧**和**推送承诺帧**可能引用的是更新后的QPACK动态查找表。
尽管这些对查找表的更新并不直接属于本次消息通信，但是只有接收并处理了这些更新，才能处理消息。
有关更多细节，详见[第4.2章](#4.2_HTTP_Fields)。

传输编码（详见《[HTTP/1.1](https://www.rfc-editor.org/info/rfc9112)》的[第7章](https://www.rfc-editor.org/rfc/rfc9112#section-7)）不是为HTTP/3定义的；{{< req_level MUST_NOT >}}使用标头字段`Transfer-Encoding`。

当且仅当在某个请求的最终响应前存在一个或多个临时响应（状态码为`1xx`；详见《[HTTP](https://www.rfc-editor.org/info/rfc9110)》的[第15.2章](https://www.rfc-editor.org/rfc/rfc9110#section-15.2)）时，此次响应才{{< req_level MAY >}}由多条消息组成。
临时响应并不包含内容和挂载。

一轮完整的HTTP请求与响应的通信正好消耗掉一条由客户端发起的QUIC双向流。
在发送完请求后，客户端{{< req_level MUST >}}关闭流的发送部分。
除非使用的是`CONNECT`方法（详见[第4.4章](#4.4_The_CONNECT_Method)），否则客户端{{< req_level MUST_NOT >}}将接收到请求的响应作为关闭流的前置条件。
在发送完最终的响应后，服务器{{< req_level MUST >}}关闭流的发送部分。
此时，一条QUIC流就被完全关闭了。

一条流的关闭意味着最后那条HTTP消息的结束。
由于一些消息非常巨大，或没有设置边界，所以终端{{< req_level SHOULD >}}在消息接收到足以进行处理时就开始消耗部分HTTP消息。
如果在由客户端发起的流上还没有接收到足以创建完整响应的HTTP消息，这条流就被终止了，那么服务器{{< req_level SHOULD >}}使用错误码`H3_REQUEST_INCOMPLETE`（不完整的请求）来中止其响应流。

如果要创建的响应并不依赖于请求中尚未被发送或接收的部分，那么服务器可以在客户端发送出完整的请求前就发送回完整的响应。
当服务器不需要接收请求的剩余部分时，它{{< req_level MAY >}}中止读取请求流，将完整的响应发送回去，然后再干净地关闭流的发送部分。
当要求客户端停止在请求流上发送数据时，{{< req_level SHOULD >}}使用错误码`H3_NO_ERROR`（无错误）。
尽管客户端总是可以出于其他理由自行决定要不要忽略响应，但是客户端{{< req_level MUST_NOT >}}因为请求的发送过程被突然中止而将完整的响应忽略。
如果服务器发送的响应并不完整，或发送了完整的响应但是没有中止读取请求，那么客户端{{< req_level SHOULD >}}继续发送请求的内容，然后将流正常关闭。
