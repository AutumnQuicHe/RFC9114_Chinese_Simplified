---
title: "A.2.2. 字段压缩的差异"
anchor: "A.2.2_Field_Compression_Differences"
weight: 130220
rank: "h3"
---

HPACK是按照有序交付的前提来设计的。
经编码的字段组序列必须以编码的顺序抵达终端（并依次解码）。
这确保了两侧终端处动态的状态数据保持同步。

由于QUIC并未提供这种全局的数据包排序，所以HTTP/3使用了HPACK的修改版本，称为QPACK。
QPACK使用一条单向流来对动态查找表进行操作，确保所有更新在整体上是有序的。
所有包含经编码字段的帧都只不过是在引用查找表在某个时刻的状态，而不会修改它。

《[QPACK](../RFC9204_Chinese_Simplified)》中还提供了其他细节。
