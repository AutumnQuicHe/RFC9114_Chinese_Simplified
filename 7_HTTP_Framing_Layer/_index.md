---
title: "7. HTTP分帧层"
anchor: "7_HTTP_Framing_Layer"
weight: 70000
rank: "h1"
---

HTTP frames are carried on QUIC streams, as described in Section 6. HTTP/3 defines three stream types: control stream, request stream, and push stream. This section describes HTTP/3 frame formats and their permitted stream types; see Table 1 for an overview. A comparison between HTTP/2 and HTTP/3 frames is provided in Appendix A.2.

HTTP帧是使用QUIC流来传递的，详见[第6章]()。HTTP/3定义了三种流类型：控制流、请求流和推送流。本章描述了各种HTTP/3帧的结构以及在哪种流中可以使用它们：[表1]()对此做了概述。[附录A.2]()是一份针对HTTP/2和HTTP/3的更加详细的比较。

Table 1: HTTP/3 Frames and Stream Type Overview
Frame	Control Stream	Request Stream	Push Stream	Section
DATA	No	Yes	Yes	Section 7.2.1
HEADERS	No	Yes	Yes	Section 7.2.2
CANCEL_PUSH	Yes	No	No	Section 7.2.3
SETTINGS	Yes (1)	No	No	Section 7.2.4
PUSH_PROMISE	No	Yes	No	Section 7.2.5
GOAWAY	Yes	No	No	Section 7.2.6
MAX_PUSH_ID	Yes	No	No	Section 7.2.7
Reserved	Yes	Yes	Yes	Section 7.2.8

{{% block_ref
indx="Table_1_HTTP3_Frames_and_Stream_Type_Overview"
title="表1：HTTP/3帧与流类型的概述" %}}

| 帧           | 控制流  | 请求流 | 推送流 | 章节          |
|:------------|:-----|:----|:----|:------------|
| **数据帧**     | 否    | 是   | 是   | [第7.2.1章]() |
| **标头帧**     | 否    | 是   | 是   | [第7.2.2章]() |
| **取消推送帧**   | 是    | 否   | 否   | [第7.2.3章]() |
| **设置帧**     | 是（1） | 否   | 否   | [第7.2.4章]() |
| **推送承诺帧**   | 否    | 是   | 否   | [第7.2.5章]() |
| **关闭帧**     | 是    | 否   | 否   | [第7.2.6章]() |
| **最大推送ID帧** | 是    | 否   | 否   | [第7.2.7章]() |
| *被保留使用的帧*   | 是    | 是   | 是   | [第7.2.8章]() |

{{% /block_ref %}}

The SETTINGS frame can only occur as the first frame of a Control stream; this is indicated in Table 1 with a (1). Specific guidance is provided in the relevant section.

**设置帧**只能作为一条控制流的首个帧出现；这一点在[表1]()中使用”（1）“来表示。有关如何使用各个类型的帧，详见各自的章节。

Note that, unlike QUIC frames, HTTP/3 frames can span multiple packets.

注意，与QUIC帧不同的是，HTTP/3帧可以横跨多个数据包。
