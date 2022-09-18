---
title: "10.5. 对拒绝服务攻击的考量"
anchor: "10.5_Denial_of_Service_Considerations"
weight: 100500
rank: "h2"
---

An HTTP/3 connection can demand a greater commitment of resources to operate than an HTTP/1.1 or HTTP/2 connection. The use of field compression and flow control depend on a commitment of resources for storing a greater amount of state. Settings for these features ensure that memory commitments for these features are strictly bounded.

比起HTTP/1.1或HTTP/2的连接，HTTP/3的连接可能需要占用更多的资源来保持运转。字段压缩和流量控制依赖于充足的资源以存储更多的状态数据。对于这些特性的设置确保了它们的内存占用量是严格受限的。

The number of PUSH_PROMISE frames is constrained in a similar fashion. A client that accepts server push SHOULD limit the number of push IDs it issues at a time.

**推送承诺帧**的数量也以类似的方式得到限制。接受服务器推送的客户端**应该**为自身每次新批准的推送ID数量设置上限。

Processing capacity cannot be guarded as effectively as state capacity.

计算处理所需的资源量没法像状态存储所需的资源量那样被有效地限制。

The ability to send undefined protocol elements that the peer is required to ignore can be abused to cause a peer to expend additional processing time. This might be done by setting multiple undefined SETTINGS parameters, unknown frame types, or unknown stream types. Note, however, that some uses are entirely legitimate, such as optional-to-understand extensions and padding to increase resistance to traffic analysis.

向对端发送未知的且对端不得不忽略的协议元素的能力可能被滥用，使得对端花费额外的处理时间。这可以通过设置大量未定义的**设置帧**参数、未知的帧类型或未知的流类型来做到。不过要注意的是，未知协议元素的某些用法是完全合理的，例如某些可选支持的扩展和用填充来提高对流量分析的抵御能力。

Compression of field sections also offers some opportunities to waste processing resources; see Section 7 of [QPACK] for more details on potential abuses.

对字段组的压缩还提供了一些浪费对端处理资源的机会；有关潜在的滥用的更多细节，详见《[QPACK]()》的[第7章]()。

All these features -- i.e., server push, unknown protocol elements, field compression -- have legitimate uses. These features become a burden only when they are used unnecessarily or to excess.

所有以上特性——也就是服务器推送、未知的协议元素和字段压缩——均具有其合理的用途。只有在不必要地或过度地使用这些特性时，它们才会成为负担。

An endpoint that does not monitor such behavior exposes itself to a risk of denial-of-service attack. Implementations SHOULD track the use of these features and set limits on their use. An endpoint MAY treat activity that is suspicious as a connection error of type H3_EXCESSIVE_LOAD, but false positives will result in disrupting valid connections and requests.

没有对这些行为进行监控的终端会将自身暴露于拒绝服务攻击的风险之中。实现**应该**对这些特性的使用进行追踪，并对其使用设置限制。终端**可以**将可疑的活动视作类型为`H3_EXCESSIVE_LOAD`（负载过量）的连接错误，但是对情况产生的误判将导致合法的连接和请求被打断。
