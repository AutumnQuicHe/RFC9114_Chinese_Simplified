---
title: "9. HTTP/3的扩展"
anchor: "9_Extensions_to_HTTP3"
weight: 90000
rank: "h1"
---

HTTP/3允许对协议进行扩展。
只要不超过本章描述的限制，就能够使用协议的扩展来提供额外的服务或对协议的一部分进行更改。
扩展仅在单条HTTP/3连接的范畴内有效。

扩展适用于本文档定义的所有协议元素。
扩展不会影响已有的扩展HTTP的各种形式，例如定义新的方法、状态码，或字段。

扩展可以使用新的帧类型（详见[第7.2章](#7.2_Frame_Definitions)）、新的设置（详见[第7.2.4.1章](#7.2.4.1_Defined_SETTINGS_Parameters)）、新的错误码（详见[第8章](#8_Error_Handling)），或新的单向流类型（详见[第6.2章](#6.2_Unidirectional_Streams)）。
注册表的建立就是为了管理这些可扩展的点位：帧类型注册表（详见[第11.2.1章](#11.2.1_Frame_Types)）、设置注册表（详见[第11.2.2章](#11.2.2_Settings_Parameters)）、错误码注册表（详见[第11.2.3章](#11.2.3_Error_Codes)），以及流类型注册表（详见[第11.2.4章](#11.2.4_Stream_Types)）。

实现{{< req_level MUST >}}忽略所有可扩展的协议元素中未知的或不受支持的值。
在未知的或不受支持的类型的单向流上，实现{{< req_level MUST >}}丢弃所有数据或中止对其读取。
这意味着扩展可以安全地使用以上所有扩展点位，而无需事先协商。
然而，当在特定位置要求使用某已知的帧类型时，例如作为控制流首个帧的**设置帧**（详见[第6.2.1章](#6.2.1_Control_Streams)），如果接收到了不满足此要求的某未知类型的帧，那么{{< req_level SHOULD >}}将此情况视作为错误。

在使用改变了已有协议部件的语义的扩展前，{{< req_level MUST >}}进行协商。
举个例子，只有在对端发出了表明支持的信号后，才能使用改变了**标头帧**结构的扩展。
事实上，要兼容这种修改过的结构会使得实现变得复杂。
因此，为已有的协议元素的修改后的定义分配新的标识符可能更为有效。

本文档没有指定某种特殊方法来协商要不要使用扩展，但提到了可以使用设置（详见[第7.2.4.1章](#7.2.4.1_Defined_SETTINGS_Parameters)）来做到这一点。
如果两端都用该设置表明其愿意使用某个扩展，那么就可以使用该扩展。
如果要将某个设置用于扩展的协商，那么其默认值{{< req_level MUST >}}使得此设置被省略时，扩展是被禁用的。
