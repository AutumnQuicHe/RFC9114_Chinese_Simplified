---
title: "4.2. HTTP字段"
anchor: "4.2_HTTP_Fields"
weight: 40200
rank: "h2"
---

HTTP messages carry metadata as a series of key-value pairs called "HTTP fields"; see Sections 6.3 and 6.5 of [HTTP]. For a listing of registered HTTP fields, see the "Hypertext Transfer Protocol (HTTP) Field Name Registry" maintained at <https://www.iana.org/assignments/http-fields/>. Like HTTP/2, HTTP/3 has additional considerations related to the use of characters in field names, the Connection header field, and pseudo-header fields.

HTTP消息以名为“HTTP字段”的一系列键值对的方式传递元数据，详见《[HTTP]()》的[第6.3章]()和[第6.5章]()。
在维护于<https://www.iana.org/assignments/http-fields/>的“超文本传输协议（HTTP）字段名注册表”中可以找到所有已注册的HTTP字段。
就像HTTP/2一样，在可以用于字段名称的字符、标头字段`Connection`和伪标头字段方面，HTTP/3有着额外的考量。

Field names are strings containing a subset of ASCII characters. Properties of HTTP field names and values are discussed in more detail in Section 5.1 of [HTTP]. Characters in field names MUST be converted to lowercase prior to their encoding. A request or response containing uppercase characters in field names MUST be treated as malformed.

字段名称是以ASCII字符形成的字符串。
《[HTTP]()》的[第5.1章]()详细讨论了HTTP字段名称及其值的各项属性。
字段名称中的所有字母**必须**在编码前被转换为其小写形式。
在字段名称中处理大写字母的请求或响应**必须**被视作为畸形的。

HTTP/3 does not use the Connection header field to indicate connection-specific fields; in this protocol, connection-specific metadata is conveyed by other means. An endpoint MUST NOT generate an HTTP/3 field section containing connection-specific fields; any message containing connection-specific fields MUST be treated as malformed.

HTTP/3并不使用标头字段`Connection`来标识与连接有关的字段；在本协议中，使用其他方式来传递与连接有关的元数据。
终端创建的HTTP/3字段组中**必须不**包含与连接有关的字段；任何包含与连接有关的字段的消息都**必须**被视作为畸形的。

The only exception to this is the TE header field, which MAY be present in an HTTP/3 request header; when it is, it MUST NOT contain any value other than "trailers".

此项要求唯一的例外是标头字段`TE`，它可以出现在HTTP/3请求的标头中；在此情况下，它**必须不**使用除了`trailers`外的任何值。

An intermediary transforming an HTTP/1.x message to HTTP/3 MUST remove connection-specific header fields as discussed in Section 7.6.1 of [HTTP], or their messages will be treated by other HTTP/3 endpoints as malformed.

将HTTP/1.x消息转换为HTTP/3消息的中间设备**必须**移除在《[HTTP]()》的[第7.6.1章]()中讨论的与连接有关的标头字段，否则这些消息会被其他HTTP/3终端视作畸形的。
