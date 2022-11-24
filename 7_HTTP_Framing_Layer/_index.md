---
title: "7. HTTP分帧层"
anchor: "7_HTTP_Framing_Layer"
weight: 70000
rank: "h1"
---

HTTP帧是使用QUIC流来传递的，详见[第6章](#6_Stream_Mapping_and_Usage)。
HTTP/3定义了三种流类型：控制流、请求流和推送流。
本章描述了各种HTTP/3帧的结构以及在哪种流中可以使用它们：[表1](#Table_1_HTTP3_Frames_and_Stream_Type_Overview)对此做了概述。
[附录A.2](#A.2_HTTP_Frame_Types)是一份针对HTTP/2和HTTP/3的更加详细的比较。

{{% block_ref
indx="Table_1_HTTP3_Frames_and_Stream_Type_Overview"
title="表1：HTTP/3帧与流类型的概述" %}}

| 帧           | 控制流  | 请求流 | 推送流 | 章节          |
|:------------|:-----|:----|:----|:------------|
| **数据帧**     | 否    | 是   | 是   | [第7.2.1章](#7.2.1_DATA) |
| **标头帧**     | 否    | 是   | 是   | [第7.2.2章](#7.2.2_HEADERS) |
| **取消推送帧**   | 是    | 否   | 否   | [第7.2.3章](#7.2.3_CANCEL_PUSH) |
| **设置帧**     | 是（1） | 否   | 否   | [第7.2.4章](#7.2.4_SETTINGS) |
| **推送承诺帧**   | 否    | 是   | 否   | [第7.2.5章](#7.2.5_PUSH_PROMISE) |
| **关闭帧**     | 是    | 否   | 否   | [第7.2.6章](#7.2.6_GOAWAY) |
| **最大推送ID帧** | 是    | 否   | 否   | [第7.2.7章](#7.2.7_MAX_PUSH_ID) |
| *被保留使用的帧*   | 是    | 是   | 是   | [第7.2.8章](#7.2.8_Reserved_Frame_Types) |

{{% /block_ref %}}

**设置帧**只能作为一条控制流的首个帧出现；这一点在[表1](#Table_1_HTTP3_Frames_and_Stream_Type_Overview)中使用”（1）“来表示。
有关如何使用各个类型的帧，详见各自的章节。

注意，与QUIC帧不同的是，HTTP/3帧可以横跨多个数据包。
