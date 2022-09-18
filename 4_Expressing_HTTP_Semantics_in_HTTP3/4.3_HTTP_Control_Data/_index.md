---
title: "4.3. HTTP控制数据"
anchor: "4.3_HTTP_Control_Data"
weight: 40300
rank: "h2"
---

Like HTTP/2, HTTP/3 employs a series of pseudo-header fields, where the field name begins with the : character (ASCII 0x3a). These pseudo-header fields convey message control data; see Section 6.2 of [HTTP].

就像HTTP/2一样，HTTP/3也使用了一系列伪标头字段，这些字段的名称都以`:`（ASCII码为`0x3a`）开头。
这些伪标头字段传递的是消息的控制数据，详见《[HTTP]()》的[第6.2章]()。

Pseudo-header fields are not HTTP fields. Endpoints MUST NOT generate pseudo-header fields other than those defined in this document. However, an extension could negotiate a modification of this restriction; see Section 9.

伪标头字段不是HTTP字段。
终端**必须不**创建在本文档列出的定义之外的伪标头字段。
不过，将来的扩展可以对此条限制做出修改，详见[第9章]()。

Pseudo-header fields are only valid in the context in which they are defined. Pseudo-header fields defined for requests MUST NOT appear in responses; pseudo-header fields defined for responses MUST NOT appear in requests. Pseudo-header fields MUST NOT appear in trailer sections. Endpoints MUST treat a request or response that contains undefined or invalid pseudo-header fields as malformed.

伪标头字段仅在定义了它们的上下文中有效。
定义于请求中的伪标头字段**必须不**出现在响应中；定义于响应中的伪标头字段**必须不**出现在请求中。
伪标头字段**必须不**出现在尾部。
终端**必须**将包含未定义的或不合法的伪标头字段的请求或响应视作为畸形的。

All pseudo-header fields MUST appear in the header section before regular header fields. Any request or response that contains a pseudo-header field that appears in a header section after a regular header field MUST be treated as malformed.

所有伪标头字段都**必须**出现在头部的普通标头字段之前。
任何包含着出现在头部的普通标头字段之后的伪标头字段的请求或响应都**必须**视作为畸形的。
