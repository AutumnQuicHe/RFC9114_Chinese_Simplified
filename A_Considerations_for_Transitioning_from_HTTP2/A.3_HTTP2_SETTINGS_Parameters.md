---
title: "A.3. HTTP/2的设置帧参数"
anchor: "A.3_HTTP2_SETTINGS_Parameters"
weight: 130300
rank: "h2"
---

与HTTP/2的一个重要区别是设置只会被发送一次，也就是作为控制流的首个帧来发送，所以后续无法进行改变。
这消除了许多有关设置同步的边界情况。

HTTP/2通过**设置帧**定义的许多传输级选项都被HTTP/3中的QUIC传输参数取代了。
HTTP/2中HTTP级的设置在HTTP/3中得以保留，并维持相同的值。
被取代的设置是被保留使用的，接收到它们的情况会被视作为错误。
有关被维持的和被保留使用的值的讨论，详见[第7.2.4.1章](#7.2.4.1_Defined_SETTINGS_Parameters)。

下文列出了映射各种HTTP/2**设置帧**参数的方法：

`SETTINGS_HEADER_TABLE_SIZE`（值为`0x01`）：

:   详见《[QPACK](../RFC9204_Chinese_Simplified)》。

`SETTINGS_ENABLE_PUSH`（值为`0x02`）：

:   该设置被移除，请使用**最大推送ID帧**，后者能对服务器推送进行更细粒度的控制。
在HTTP/3**设置帧**中出现标识符为`0x02`的设置（对应参数`SETTINGS_ENABLE_PUSH`）的情况会被视作为错误。

`SETTINGS_MAX_CONCURRENT_STREAMS`（值为`0x03`）：

:   作为其流量控制逻辑的一部分，QUIC控制着开放流的最大ID。
在HTTP/3**设置帧**中出现标识符为`0x03`的设置（对应参数`SETTINGS_MAX_CONCURRENT_STREAMS`）的情况会被视作为错误。

`SETTINGS_INITIAL_WINDOW_SIZE`（值为`0x04`）：

:   QUIC在初始的传输层握手中要求同时指定流和连接的流量控制窗口尺寸。
在HTTP/3**设置帧**中出现标识符为`0x04`的设置（对应参数`SETTINGS_INITIAL_WINDOW_SIZE`）的情况会被视作为错误。

`SETTINGS_MAX_FRAME_SIZE`（值为`0x05`）：

:   该设置在HTTP/3中没有等价设置。
在HTTP/3**设置帧**中出现标识符为`0x05`的设置（对应参数`SETTINGS_MAX_FRAME_SIZE`）的情况会被视作为错误。

`SETTINGS_MAX_HEADER_LIST_SIZE`（值为`0x06`）：

:   该设置的标识符已被重命名为`SETTINGS_MAX_FIELD_SECTION_SIZE`。

在HTTP/3中，设置的值是可变长度整型（6、14、30或62比特长），而不是HTTP/2中固定长度32比特的字段。
这一方式通常能产生更短的编码结果，但也可能产生比完整使用了32比特长空间的设置还要长的编码结果。
移植自HTTP/2的设置可以选择重新定义它们的值，从而将值高效地编码且将长度限制至30比特以下，或者在需要的长度大于30比特时将62比特长的空间利用起来。

应该为HTTP/2和HTTP/3单独定义设置。
出于复杂性的考虑，《[HTTP/2](https://www.rfc-editor.org/info/rfc9113)》中定义的设置ID已经被移除。
注意，HTTP/3中的设置标识符值空间相当大（62比特位，而不是16比特位），所以许多HTTP/3设置不会有等价的HTTP/2码点。
详见[第11.2.2章](#11.2.2_Settings_Parameters)。

由于QUIC的流可能乱序抵达，建议终端不要在对其他流做出响应前持续等待对端设置的抵达。
详见[第7.2.4.2章](#7.2.4.2_Initialization)。
