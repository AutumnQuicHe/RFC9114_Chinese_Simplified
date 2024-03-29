---
title: "4.3. HTTP控制数据"
anchor: "4.3_HTTP_Control_Data"
weight: 40300
rank: "h2"
---

就像HTTP/2一样，HTTP/3也使用了一系列伪标头字段，这些字段的名称都以`:`（ASCII码为`0x3a`）开头。
这些伪标头字段传递的是消息的控制数据，详见《[HTTP](https://www.rfc-editor.org/info/rfc9110)》的[第6.2章](https://www.rfc-editor.org/rfc/rfc9110#section-6.2)。

伪标头字段不是HTTP字段。
终端{{< req_level MUST_NOT >}}创建在本文档列出的定义之外的伪标头字段。
不过，将来的扩展可以对此条限制做出修改，详见[第9章](#9_Extensions_to_HTTP3)。

伪标头字段仅在定义了它们的上下文中有效。
定义于请求中的伪标头字段{{< req_level MUST_NOT >}}出现在响应中；定义于响应中的伪标头字段{{< req_level MUST_NOT >}}出现在请求中。
伪标头字段{{< req_level MUST_NOT >}}出现在挂载。
终端{{< req_level MUST >}}将包含未定义的或不合法的伪标头字段的请求或响应视作为畸形的。

所有伪标头字段都{{< req_level MUST >}}出现在头部的普通标头字段之前。
任何包含着出现在头部的普通标头字段之后的伪标头字段的请求或响应都{{< req_level MUST >}}视作为畸形的。
