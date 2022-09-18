---
title: "A.2.4. 关于如何定义新的帧类型的指引"
anchor: "A.2.4_Guidance_for_New_Frame_Type_Definitions"
weight: 130240
rank: "h3"
---

Frame type definitions in HTTP/3 often use the QUIC variable-length integer encoding. In particular, stream IDs use this encoding, which allows for a larger range of possible values than the encoding used in HTTP/2. Some frames in HTTP/3 use an identifier other than a stream ID (e.g., push IDs). Redefinition of the encoding of extension frame types might be necessary if the encoding includes a stream ID.

HTTP/3中的帧类型定义经常使用QUIC的可变长度整形编码。
特别是，流ID也使用了这种编码方式，这使得比HTTP/2更大的取值范围变为可能。
HTTP/3中的某些帧没有使用流ID作为标识符（例如使用推送ID）。
如果要将流ID编码进来，那么可能需要重新定义扩展帧类型的编码方式。

Because the Flags field is not present in generic HTTP/3 frames, those frames that depend on the presence of flags need to allocate space for flags as part of their frame payload.

因为HTTP/3的通用帧结构中不存在”标志“字段，所以依赖标志的那些帧需要在帧载荷中为标志分配空间。

Other than these issues, frame type HTTP/2 extensions are typically portable to QUIC simply by replacing stream 0 in HTTP/2 with a control stream in HTTP/3. HTTP/3 extensions will not assume ordering, but would not be harmed by ordering, and are expected to be portable to HTTP/2.

注意了以上事项后，通常只要将HTTP/2中的流`0`替换为HTTP/3中的一条控制流，就能简单地将HTTP/2的扩展帧类型移植到QUIC上。
HTTP/3扩展不会对数据包顺序做出假设，却也不会因为有序的数据包而受到损害，所以它们应该能被移植到HTTP/2中。
