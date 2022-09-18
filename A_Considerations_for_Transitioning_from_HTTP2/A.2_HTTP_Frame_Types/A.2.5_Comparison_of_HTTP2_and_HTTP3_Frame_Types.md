---
title: "A.2.5. HTTP/2和HTTP/3帧类型的比较"
anchor: "A.2.5_Comparison_of_HTTP2_and_HTTP3_Frame_Types"
weight: 130250
rank: "h3"
---

DATA (0x00):
Padding is not defined in HTTP/3 frames. See Section 7.2.1.

数据帧（值为`0x00`）：

:   HTTP/3的帧中没有定义”填充（Padding）“。详见[第7.2.1章]()。

HEADERS (0x01):
The PRIORITY region of HEADERS is not defined in HTTP/3 frames. Padding is not defined in HTTP/3 frames. See Section 7.2.2.

标头帧（值为`0x01`）：

:   HTTP/3的帧中没有定义**标头帧**的”优先级“部分。HTTP/3的帧中没有定义”填充“。详见[第7.2.2章]()。

PRIORITY (0x02):
As described in Appendix A.2.1, HTTP/3 does not provide a means of signaling priority.

优先级帧（值为`0x02`）：

:   如[附录A.2.1]()所述，HTTP/3没有提供一种提示优先级的方式。

RST_STREAM (0x03):
RST_STREAM frames do not exist in HTTP/3, since QUIC provides stream lifecycle management. The same code point is used for the CANCEL_PUSH frame (Section 7.2.3).

流重置帧（值为`0x03`）：

:   **流重置帧**不会出现在HTTP/3中，因为QUIC提供了流的生命周期管理。其码点被用于**取消推送帧**（详见[第7.2.3章]()）。

SETTINGS (0x04):
SETTINGS frames are sent only at the beginning of the connection. See Section 7.2.4 and Appendix A.3.

设置帧（值为`0x04`）：

:   **设置帧**仅会出现在连接的起始位置。详见[第7.2.4章]()和[附录A.3]()。

PUSH_PROMISE (0x05):
The PUSH_PROMISE frame does not reference a stream; instead, the push stream references the PUSH_PROMISE frame using a push ID. See Section 7.2.5.

推送承诺帧（值为`0x05`）：

:   **推送承诺帧**并不会指向一条流；取而代之的是，推送流使用推送ID来指向**推送承诺帧**。详见[第7.2.5章]()。

PING (0x06):
PING frames do not exist in HTTP/3, as QUIC provides equivalent functionality.

Ping帧（值为`0x06`）：

:   **Ping帧**不会出现在HTTP/3中，因为QUIC提供了等价的功能。

GOAWAY (0x07):
GOAWAY does not contain an error code. In the client-to-server direction, it carries a push ID instead of a server-initiated stream ID. See Section 7.2.6.

关闭帧（值为`0x07`）：

:   **关闭帧**中并不包含错误码。在从客户端至服务器的方向上，它传递的是一个推送ID而不是一个由服务器发起的流的ID。详见[第7.2.6章]()。

WINDOW_UPDATE (0x08):
WINDOW_UPDATE frames do not exist in HTTP/3, since QUIC provides flow control.

窗口更新帧（值为`0x08`）：

:   **窗口更新帧**不会出现在HTTP/3中，因为QUIC提供了流量控制机制。

CONTINUATION (0x09):
CONTINUATION frames do not exist in HTTP/3; instead, larger HEADERS/PUSH_PROMISE frames than HTTP/2 are permitted.

继续帧（值为`0x09`）：

:   **继续帧**不会出现在HTTP/3中；取而代之的是，允许使用比HTTP/2中更大的**标头帧**和**推送承诺帧**。

Frame types defined by extensions to HTTP/2 need to be separately registered for HTTP/3 if still applicable. The IDs of frames defined in [HTTP/2] have been reserved for simplicity. Note that the frame type space in HTTP/3 is substantially larger (62 bits versus 8 bits), so many HTTP/3 frame types have no equivalent HTTP/2 code points. See Section 11.2.1.

如果仍然适用，那么由HTTP/2扩展定义的帧类型需要在HTTP/3中被单独注册。出于复杂性的考虑，《[HTTP/2]()》中定义的帧类型ID被保留使用。注意，HTTP/3中的帧类型值空间相当大（62比特位，而不是8比特位），所以许多HTTP/3帧类型不会有等价的HTTP/2码点。详见[第11.2.1章]()。
