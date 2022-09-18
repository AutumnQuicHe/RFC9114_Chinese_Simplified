---
title: "A.4. HTTP/2的错误码"
anchor: "A.4_HTTP2_Error_Codes"
weight: 130400
rank: "h2"
---

QUIC has the same concepts of "stream" and "connection" errors that HTTP/2 provides. However, the differences between HTTP/2 and HTTP/3 mean that error codes are not directly portable between versions.

QUIC和HTTP/2提供的”流“错误与”连接“错误的概念是相同的。然而，HTTP/2与HTTP/3间的差异意味着不同HTTP版本间的错误码不能直接移植。

The HTTP/2 error codes defined in Section 7 of [HTTP/2] logically map to the HTTP/3 error codes as follows:

《[HTTP/2]()》的[第7章]()中定义的HTTP/2错误码在逻辑上可以这样映射到HTTP/3的错误码：

NO_ERROR (0x00):
H3_NO_ERROR in Section 8.1.

`NO_ERROR`（值为`0x00`）：

:   [第8.1章]()中的`H3_NO_ERROR`。

PROTOCOL_ERROR (0x01):
This is mapped to H3_GENERAL_PROTOCOL_ERROR except in cases where more specific error codes have been defined. Such cases include H3_FRAME_UNEXPECTED, H3_MESSAGE_ERROR, and H3_CLOSED_CRITICAL_STREAM defined in Section 8.1.

`PROTOCOL_ERROR`（值为`0x01`）：

:   除非是存在更准确的错误码的情况，否则可以映射为`H3_GENERAL_PROTOCOL_ERROR`。这些情况包括[第8.1章]()中定义的`H3_FRAME_UNEXPECTED`、`H3_MESSAGE_ERROR`和`H3_CLOSED_CRITICAL_STREAM`。

INTERNAL_ERROR (0x02):
H3_INTERNAL_ERROR in Section 8.1.

`INTERNAL_ERROR`（值为`0x02`）：

:   [第8.1章]()中的`H3_INTERNAL_ERROR`。

FLOW_CONTROL_ERROR (0x03):
Not applicable, since QUIC handles flow control.

`FLOW_CONTROL_ERROR`（值为`0x03`）：

:   不适用，因为QUIC处理了流量控制。

SETTINGS_TIMEOUT (0x04):
Not applicable, since no acknowledgment of SETTINGS is defined.

`SETTINGS_TIMEOUT`（值为`0x04`）：

:   不适用，因为没有定义对**设置帧**的确认。

STREAM_CLOSED (0x05):
Not applicable, since QUIC handles stream management.

`STREAM_CLOSED`（值为`0x05`）：

:   不适用，因为QUIC处理了流的管理。

FRAME_SIZE_ERROR (0x06):
H3_FRAME_ERROR error code defined in Section 8.1.

`FRAME_SIZE_ERROR`（值为`0x06`）：

:   [第8.1章]()中定义的错误码`H3_FRAME_ERROR`。

REFUSED_STREAM (0x07):
H3_REQUEST_REJECTED (in Section 8.1) is used to indicate that a request was not processed. Otherwise, not applicable because QUIC handles stream management.

`REFUSED_STREAM`（值为`0x07`）：

:   `H3_REQUEST_REJECTED`（[第8.1章]()）被用于表明请求未得到处理的情况。否则，它不适用于HTTP/3，因为QUIC处理了流的管理。

CANCEL (0x08):
H3_REQUEST_CANCELLED in Section 8.1.

`CANCEL`（值为`0x08`）：

:   [第8.1章]()中的`H3_REQUEST_CANCELLED`。

COMPRESSION_ERROR (0x09):
Multiple error codes are defined in [QPACK].

`COMPRESSION_ERROR`（值为`0x09`）：

:   《[QPACK]()》中定义了多种错误码。

CONNECT_ERROR (0x0a):
H3_CONNECT_ERROR in Section 8.1.

`CONNECT_ERROR`（值为`0x0a`）：

:   [第8.1章]()中的`H3_CONNECT_ERROR`。

ENHANCE_YOUR_CALM (0x0b):
H3_EXCESSIVE_LOAD in Section 8.1.

`ENHANCE_YOUR_CALM`（值为`0x0b`）：

:   [第8.1章]()中的`H3_EXCESSIVE_LOAD`。

INADEQUATE_SECURITY (0x0c):
Not applicable, since QUIC is assumed to provide sufficient security on all connections.

`INADEQUATE_SECURITY`（值为`0x0c`）：

:   不适用，因为假定了QUIC会在所有连接上都提供足够的安全性。

HTTP_1_1_REQUIRED (0x0d):
H3_VERSION_FALLBACK in Section 8.1.

`HTTP_1_1_REQUIRED`（值为`0x0d`）：

:   [第8.1章]()中的`H3_VERSION_FALLBACK`。

Error codes need to be defined for HTTP/2 and HTTP/3 separately. See Section 11.2.3.

应该为HTTP/2和HTTP/3单独定义错误码。详见[第11.2.3章]()。
