---
title: "7.2.4. 设置帧"
anchor: "7.2.4_SETTINGS"
weight: 70240
rank: "h3"
---

The SETTINGS frame (type=0x04) conveys configuration parameters that affect how endpoints communicate, such as preferences and constraints on peer behavior. Individually, a SETTINGS parameter can also be referred to as a "setting"; the identifier and value of each setting parameter can be referred to as a "setting identifier" and a "setting value".

**设置帧**（类型值为`0x04`）传递的是能影响终端的通信方式的配置参数，例如自身的偏好和对对端行为的限制。**设置帧**中的单个参数可以被称为”设置“；每个设置参数的标识符和值被称为”设置标识符“和”设置值“。

SETTINGS frames always apply to an entire HTTP/3 connection, never a single stream. A SETTINGS frame MUST be sent as the first frame of each control stream (see Section 6.2.1) by each peer, and it MUST NOT be sent subsequently. If an endpoint receives a second SETTINGS frame on the control stream, the endpoint MUST respond with a connection error of type H3_FRAME_UNEXPECTED.

**设置帧**总是应用在整条HTTP/3连接上，而从不会是单条流。**设置帧****必须**在每条控制流上（详见[第6.2.1章]()）被各个终端作为首个帧来发送，而且**必须不**被连续发送。如果终端在控制流上接收到了第二个**设置帧**，那么终端**必须**使用类型为`H3_FRAME_UNEXPECTED`的连接错误来响应这种情况。

SETTINGS frames MUST NOT be sent on any stream other than the control stream. If an endpoint receives a SETTINGS frame on a different stream, the endpoint MUST respond with a connection error of type H3_FRAME_UNEXPECTED.

在并非控制流的其他流上**必须不**发送设置帧。如果终端在另一条流上接收到了**设置帧**，那么终端**必须**使用类型为`H3_FRAME_UNEXPECTED`的连接错误来响应这种情况。

SETTINGS parameters are not negotiated; they describe characteristics of the sending peer that can be used by the receiving peer. However, a negotiation can be implied by the use of SETTINGS: each peer uses SETTINGS to advertise a set of supported values. The definition of the setting would describe how each peer combines the two sets to conclude which choice will be used. SETTINGS does not provide a mechanism to identify when the choice takes effect.

**设置帧**的参数不是由协商得到的；它们描述了接收方能够使用的发送方特性。不过，可以使用**设置帧**来隐式地协商：每个终端都使用**设置帧**来宣告一组支持的值。最终的设置就取决于各终端如何合并这两组设置。**设置帧**并没有提供一种分辨哪一方的设置最终起效的机制。

Different values for the same parameter can be advertised by each peer. For example, a client might be willing to consume a very large response field section, while servers are more cautious about request size.

终端间可以对相同的参数宣告不同的值。例如，客户端可能愿意接受一个非常大的响应字段组，而服务器对请求的尺寸更为敏感。

The same setting identifier MUST NOT occur more than once in the SETTINGS frame. A receiver MAY treat the presence of duplicate setting identifiers as a connection error of type H3_SETTINGS_ERROR.

同样的设置标识符**必须不**在同一**设置帧**中多次出现。接收方**可以**将接收到重复的设置标识符的情况视作类型为`H3_SETTINGS_ERROR`（设置错误）的连接错误。

The payload of a SETTINGS frame consists of zero or more parameters. Each parameter consists of a setting identifier and a value, both encoded as QUIC variable-length integers.

**设置帧**的载荷由数个参数组成（可以为零）。每个参数由一个设置标识符和一个值组成，两者均被编码为QUIC可变长度整型。

Setting {
Identifier (i),
Value (i),
}

SETTINGS Frame {
Type (i) = 0x04,
Length (i),
Setting (..) ...,
}
Figure 7: SETTINGS Frame

{{% block_ref
indx="Figure_7_SETTINGS_Frame"
title="图7：设置帧" %}}

```
设置 {
  标识符 (i),
  值 (i),
}

取消推送帧 {
  类型 (i) = 0x04,
  长度 (i),
  设置 (..),
}
```

{{% /block_ref %}}

An implementation MUST ignore any parameter with an identifier it does not understand.

如果实现不认识某个参数的标识符，那么实现**必须**忽略这个参数。
