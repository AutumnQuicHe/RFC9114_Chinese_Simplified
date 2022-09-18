---
title: "A.3. HTTP/2的设置帧参数"
anchor: "A.3_HTTP2_SETTINGS_Parameters"
weight: 130300
rank: "h2"
---

An important difference from HTTP/2 is that settings are sent once, as the first frame of the control stream, and thereafter cannot change. This eliminates many corner cases around synchronization of changes.

与HTTP/2的一个重要区别是设置只会被发送一次，也就是作为控制流的首个帧来发送，所以后续无法进行改变。
这消除了许多有关设置同步的边界情况。

Some transport-level options that HTTP/2 specifies via the SETTINGS frame are superseded by QUIC transport parameters in HTTP/3. The HTTP-level setting that is retained in HTTP/3 has the same value as in HTTP/2. The superseded settings are reserved, and their receipt is an error. See Section 7.2.4.1 for discussion of both the retained and reserved values.

HTTP/2通过**设置帧**定义的许多传输级选项都被HTTP/3中的QUIC传输参数取代了。
HTTP/2中HTTP级的设置在HTTP/3中得以保留，并维持相同的值。
被取代的设置是被保留使用的，接收到它们的情况会被视作为错误。
有关被维持的和被保留使用的值的讨论，详见[第7.2.4.1章]()。

Below is a listing of how each HTTP/2 SETTINGS parameter is mapped:

下文列出了映射各种HTTP/2**设置帧**参数的方法：

SETTINGS_HEADER_TABLE_SIZE (0x01):
See [QPACK].

`SETTINGS_HEADER_TABLE_SIZE`（值为`0x01`）：

:   详见《[QPACK]()》。

SETTINGS_ENABLE_PUSH (0x02):
This is removed in favor of the MAX_PUSH_ID frame, which provides a more granular control over server push. Specifying a setting with the identifier 0x02 (corresponding to the SETTINGS_ENABLE_PUSH parameter) in the HTTP/3 SETTINGS frame is an error.

`SETTINGS_ENABLE_PUSH`（值为`0x02`）：

:   该设置被移除，请使用**最大推送ID帧**，后者能对服务器推送进行更细粒度的控制。
在HTTP/3**设置帧**中出现标识符为`0x02`的设置（对应参数`SETTINGS_ENABLE_PUSH`）的情况会被视作为错误。

SETTINGS_MAX_CONCURRENT_STREAMS (0x03):
QUIC controls the largest open stream ID as part of its flow-control logic. Specifying a setting with the identifier 0x03 (corresponding to the SETTINGS_MAX_CONCURRENT_STREAMS parameter) in the HTTP/3 SETTINGS frame is an error.

`SETTINGS_MAX_CONCURRENT_STREAMS`（值为`0x03`）：

:   作为其流量控制逻辑的一部分，QUIC控制着开放流的最大ID。
在HTTP/3**设置帧**中出现标识符为`0x03`的设置（对应参数`SETTINGS_MAX_CONCURRENT_STREAMS`）的情况会被视作为错误。

SETTINGS_INITIAL_WINDOW_SIZE (0x04):
QUIC requires both stream and connection flow-control window sizes to be specified in the initial transport handshake. Specifying a setting with the identifier 0x04 (corresponding to the SETTINGS_INITIAL_WINDOW_SIZE parameter) in the HTTP/3 SETTINGS frame is an error.

`SETTINGS_INITIAL_WINDOW_SIZE`（值为`0x04`）：

:   QUIC在初始的传输层握手中要求同时指定流和连接的流量控制窗口尺寸。
在HTTP/3**设置帧**中出现标识符为`0x04`的设置（对应参数`SETTINGS_INITIAL_WINDOW_SIZE`）的情况会被视作为错误。

SETTINGS_MAX_FRAME_SIZE (0x05):
This setting has no equivalent in HTTP/3. Specifying a setting with the identifier 0x05 (corresponding to the SETTINGS_MAX_FRAME_SIZE parameter) in the HTTP/3 SETTINGS frame is an error.

`SETTINGS_MAX_FRAME_SIZE`（值为`0x05`）：

:   该设置在HTTP/3中没有等价设置。
在HTTP/3**设置帧**中出现标识符为`0x05`的设置（对应参数`SETTINGS_MAX_FRAME_SIZE`）的情况会被视作为错误。

SETTINGS_MAX_HEADER_LIST_SIZE (0x06):
This setting identifier has been renamed SETTINGS_MAX_FIELD_SECTION_SIZE.

`SETTINGS_MAX_HEADER_LIST_SIZE`（值为`0x06`）：

:   该设置的标识符已被重命名为`SETTINGS_MAX_FIELD_SECTION_SIZE`。

In HTTP/3, setting values are variable-length integers (6, 14, 30, or 62 bits long) rather than fixed-length 32-bit fields as in HTTP/2. This will often produce a shorter encoding, but can produce a longer encoding for settings that use the full 32-bit space. Settings ported from HTTP/2 might choose to redefine their value to limit it to 30 bits for more efficient encoding or to make use of the 62-bit space if more than 30 bits are required.

在HTTP/3中，设置的值是可变长度整型（6、14、30或62比特长），而不是HTTP/2中固定长度32比特的字段。
这一方式通常能产生更短的编码结果，但也可能产生比完整使用了32比特长空间的设置还要长的编码结果。
移植自HTTP/2的设置可以选择重新定义它们的值，从而将值高效地编码且将长度限制至30比特以下，或者在需要的长度大于30比特时将62比特长的空间利用起来。

Settings need to be defined separately for HTTP/2 and HTTP/3. The IDs of settings defined in [HTTP/2] have been reserved for simplicity. Note that the settings identifier space in HTTP/3 is substantially larger (62 bits versus 16 bits), so many HTTP/3 settings have no equivalent HTTP/2 code point. See Section 11.2.2.

应该为HTTP/2和HTTP/3单独定义设置。
出于复杂性的考虑，《[HTTP/2]()》中定义的设置ID已经被移除。
注意，HTTP/3中的设置标识符值空间相当大（62比特位，而不是16比特位），所以许多HTTP/3设置不会有等价的HTTP/2码点。
详见[第11.2.2章]()。

As QUIC streams might arrive out of order, endpoints are advised not to wait for the peers' settings to arrive before responding to other streams. See Section 7.2.4.2.

由于QUIC的流可能乱序抵达，建议终端不要在对其他流做出响应前持续等待对端设置的抵达。
详见[第7.2.4.2章]()。
