---
title: "A.4. HTTP/2的错误码"
anchor: "A.4_HTTP2_Error_Codes"
weight: 130400
rank: "h2"
---

QUIC和HTTP/2提供的”流“错误与”连接“错误的概念是相同的。
然而，HTTP/2与HTTP/3间的差异意味着不同HTTP版本间的错误码不能直接移植。

《[HTTP/2]()》的[第7章]()中定义的HTTP/2错误码在逻辑上可以这样映射到HTTP/3的错误码：

`NO_ERROR`（值为`0x00`）：

:   [第8.1章]()中的`H3_NO_ERROR`。

`PROTOCOL_ERROR`（值为`0x01`）：

:   除非是存在更准确的错误码的情况，否则可以映射为`H3_GENERAL_PROTOCOL_ERROR`。
这些情况包括[第8.1章]()中定义的`H3_FRAME_UNEXPECTED`、`H3_MESSAGE_ERROR`和`H3_CLOSED_CRITICAL_STREAM`。

`INTERNAL_ERROR`（值为`0x02`）：

:   [第8.1章]()中的`H3_INTERNAL_ERROR`。

`FLOW_CONTROL_ERROR`（值为`0x03`）：

:   不适用，因为QUIC处理了流量控制。

`SETTINGS_TIMEOUT`（值为`0x04`）：

:   不适用，因为没有定义对**设置帧**的确认。

`STREAM_CLOSED`（值为`0x05`）：

:   不适用，因为QUIC处理了流的管理。

`FRAME_SIZE_ERROR`（值为`0x06`）：

:   [第8.1章]()中定义的错误码`H3_FRAME_ERROR`。

`REFUSED_STREAM`（值为`0x07`）：

:   `H3_REQUEST_REJECTED`（[第8.1章]()）被用于表明请求未得到处理的情况。
否则，它不适用于HTTP/3，因为QUIC处理了流的管理。

`CANCEL`（值为`0x08`）：

:   [第8.1章]()中的`H3_REQUEST_CANCELLED`。

`COMPRESSION_ERROR`（值为`0x09`）：

:   《[QPACK]()》中定义了多种错误码。

`CONNECT_ERROR`（值为`0x0a`）：

:   [第8.1章]()中的`H3_CONNECT_ERROR`。

`ENHANCE_YOUR_CALM`（值为`0x0b`）：

:   [第8.1章]()中的`H3_EXCESSIVE_LOAD`。

`INADEQUATE_SECURITY`（值为`0x0c`）：

:   不适用，因为假定了QUIC会在所有连接上都提供足够的安全性。

`HTTP_1_1_REQUIRED`（值为`0x0d`）：

:   [第8.1章]()中的`H3_VERSION_FALLBACK`。

应该为HTTP/2和HTTP/3单独定义错误码。
详见[第11.2.3章]()。
