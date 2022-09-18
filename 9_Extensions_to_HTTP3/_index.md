---
title: "9. HTTP/3的扩展"
anchor: "9_Extensions_to_HTTP3"
weight: 90000
rank: "h1"
---

HTTP/3 permits extension of the protocol. Within the limitations described in this section, protocol extensions can be used to provide additional services or alter any aspect of the protocol. Extensions are effective only within the scope of a single HTTP/3 connection.

HTTP/3允许对协议进行扩展。
只要不超过本章描述的限制，就能够使用协议的扩展来提供额外的服务或对协议的一部分进行更改。
扩展仅在单条HTTP/3连接的范畴内有效。

This applies to the protocol elements defined in this document. This does not affect the existing options for extending HTTP, such as defining new methods, status codes, or fields.

扩展适用于本文档定义的所有协议元素。
扩展不会影响已有的扩展HTTP的各种形式，例如定义新的方法、状态码，或字段。

Extensions are permitted to use new frame types (Section 7.2), new settings (Section 7.2.4.1), new error codes (Section 8), or new unidirectional stream types (Section 6.2). Registries are established for managing these extension points: frame types (Section 11.2.1), settings (Section 11.2.2), error codes (Section 11.2.3), and stream types (Section 11.2.4).

扩展可以使用新的帧类型（详见[第7.2章]()）、新的设置（详见[第7.2.4.1章]()）、新的错误码（详见[第8章]()），或新的单向流类型（详见[第6.2章]()）。
注册表的建立就是为了管理这些可扩展的点位：帧类型注册表（详见[第11.2.1章]()）、设置注册表（详见[第11.2.2章]()）、错误码注册表（详见[第11.2.3章]()），以及流类型注册表（详见[第11.2.4章]()）。

Implementations MUST ignore unknown or unsupported values in all extensible protocol elements. Implementations MUST discard data or abort reading on unidirectional streams that have unknown or unsupported types. This means that any of these extension points can be safely used by extensions without prior arrangement or negotiation. However, where a known frame type is required to be in a specific location, such as the SETTINGS frame as the first frame of the control stream (see Section 6.2.1), an unknown frame type does not satisfy that requirement and SHOULD be treated as an error.

实现**必须**忽略所有可扩展的协议元素中未知的或不受支持的值。
在未知的或不受支持的类型的单向流上，实现**必须**丢弃所有数据或中止对其读取。
这意味着扩展可以安全地使用以上所有扩展点位，而无需事先协商。
然而，当在特定位置要求使用某已知的帧类型时，例如作为控制流首个帧的**设置帧**（详见[第6.2.1章]()），如果接收到了不满足此要求的某未知类型的帧，那么**应该**将此情况视作为错误。

Extensions that could change the semantics of existing protocol components MUST be negotiated before being used. For example, an extension that changes the layout of the HEADERS frame cannot be used until the peer has given a positive signal that this is acceptable. Coordinating when such a revised layout comes into effect could prove complex. As such, allocating new identifiers for new definitions of existing protocol elements is likely to be more effective.

在使用改变了已有协议部件的语义的扩展前，**必须**进行协商。
举个例子，只有在对端发出了表明支持的信号后，才能使用改变了**标头帧**结构的扩展。
事实上，要兼容这种修改过的结构会使得实现变得复杂。
因此，为已有的协议元素的修改后的定义分配新的标识符可能更为有效。

This document does not mandate a specific method for negotiating the use of an extension, but it notes that a setting (Section 7.2.4.1) could be used for that purpose. If both peers set a value that indicates willingness to use the extension, then the extension can be used. If a setting is used for extension negotiation, the default value MUST be defined in such a fashion that the extension is disabled if the setting is omitted.

本文档没有指定某种特殊方法来协商要不要使用扩展，但提到了可以使用设置（详见[第7.2.4.1章]()）来做到这一点。
如果两端都用该设置表明其愿意使用某个扩展，那么就可以使用该扩展。
如果要将某个设置用于扩展的协商，那么其默认值**必须**使得此设置被省略时，扩展是被禁用的。
