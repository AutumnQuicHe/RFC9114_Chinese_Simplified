---
title: "A.4.1. HTTP/2和HTTP/3错误间的映射"
anchor: "A.4.1_Mapping_between_HTTP2_and_HTTP3_Errors"
weight: 130410
rank: "h3"
---

An intermediary that converts between HTTP/2 and HTTP/3 may encounter error conditions from either upstream. It is useful to communicate the occurrence of errors to the downstream, but error codes largely reflect connection-local problems that generally do not make sense to propagate.

对HTTP/2和HTTP/3进行转换的中间设备可能遇到来自任一上游的错误。
尽管将错误的出现告知下游的做法具有它的作用，但是错误码很大程度上反应的是当前连接所涉及的终端自身的问题，它一般不适合向外传播。

An intermediary that encounters an error from an upstream origin can indicate this by sending an HTTP status code such as 502 (Bad Gateway), which is suitable for a broad class of errors.

如果某中间设备遇到了来自上游源服务器的错误，那么它可以通过发送HTTP状态码，如`502`（网关无响应）的方式来将此情况表达出来，它适用于多类错误。

There are some rare cases where it is beneficial to propagate the error by mapping it to the closest matching error type to the receiver. For example, an intermediary that receives an HTTP/2 stream error of type REFUSED_STREAM from the origin has a clear signal that the request was not processed and that the request is safe to retry. Propagating this error condition to the client as an HTTP/3 stream error of type H3_REQUEST_REJECTED allows the client to take the action it deems most appropriate. In the reverse direction, the intermediary might deem it beneficial to pass on client request cancellations that are indicated by terminating a stream with H3_REQUEST_CANCELLED; see Section 4.1.1.

存在一些极少数的情况，适合通过将错误映射为含义最邻近的错误类型后再传播给接收者。
例如，某中间设备从源服务器处接收到了类型为`REFUSED_STREAM`的HTTP/2流错误，同时它能明确知道请求未得到处理且可以被安全地重发。
将该错误以类型为`H3_REQUEST_REJECTED`的HTTP/3流错误来传播可以使客户端采取它认为最合适的措施。
在相反的方向上，中间设备可以认为最合适的操作是通过用`H3_REQUEST_CANCELLED`终止流的方式来传递客户端请求被取消的信息。

Conversion between errors is described in the logical mapping. The error codes are defined in non-overlapping spaces in order to protect against accidental conversion that could result in the use of inappropriate or unknown error codes for the target version. An intermediary is permitted to promote stream errors to connection errors but they should be aware of the cost to the HTTP/3 connection for what might be a temporary or intermittent error.

上文逻辑上的映射描述了如何在错误间的转换。
错误码被定义在互不重叠的空间中，以免发生意料外的转换，导致使用了在目标版本中不合适或未知的错误码。
中间设备可以将流错误提升为连接错误，但是它们应该提防间歇性的错误为HTTP/3连接带来的负担。
