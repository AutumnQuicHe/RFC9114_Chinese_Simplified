---
title: "4.2. HTTP字段"
anchor: "4.2_HTTP_Fields"
weight: 40200
rank: "h2"
---

HTTP消息以名为“HTTP字段”的一系列键值对的方式传递元数据，详见《[HTTP](https://www.rfc-editor.org/info/rfc9110)》的[第6.3章](https://www.rfc-editor.org/rfc/rfc9110#section-6.3)和[第6.5章](https://www.rfc-editor.org/rfc/rfc9110#section-6.5)。
在维护于<https://www.iana.org/assignments/http-fields/>的“超文本传输协议（HTTP）字段名注册表”中可以找到所有已注册的HTTP字段。
就像HTTP/2一样，在可以用于字段名称的字符、标头字段`Connection`和伪标头字段方面，HTTP/3有着额外的考量。

字段名称是以ASCII字符形成的字符串。
《[HTTP](https://www.rfc-editor.org/info/rfc9110)》的[第5.1章](https://www.rfc-editor.org/rfc/rfc9110#section-5.1)详细讨论了HTTP字段名称及其值的各项属性。
字段名称中的所有字母 {{< req_level MUST >}}在编码前被转换为其小写形式。
在字段名称中处理大写字母的请求或响应 {{< req_level MUST >}}被视作为畸形的。

HTTP/3并不使用标头字段`Connection`来标识与连接有关的字段；在本协议中，使用其他方式来传递与连接有关的元数据。
终端创建的HTTP/3字段组中{{< req_level MUST_NOT >}}包含与连接有关的字段；任何包含与连接有关的字段的消息都 {{< req_level MUST >}}被视作为畸形的。

此项要求唯一的例外是标头字段`TE`，它可以出现在HTTP/3请求的标头中；在此情况下，它{{< req_level MUST_NOT >}}使用除了`trailers`外的任何值。

将HTTP/1.x消息转换为HTTP/3消息的中间设备 {{< req_level MUST >}}移除在《[HTTP](https://www.rfc-editor.org/info/rfc9110)》的[第7.6.1章](https://www.rfc-editor.org/rfc/rfc9110#section-7.6.1)中讨论的与连接有关的标头字段，否则这些消息会被其他HTTP/3终端视作畸形的。
