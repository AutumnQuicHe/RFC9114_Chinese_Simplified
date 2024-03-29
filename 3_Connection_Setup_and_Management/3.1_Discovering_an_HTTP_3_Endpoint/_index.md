---
title: "3.1. 发现HTTP/3终端"
anchor: "3.1_Discovering_an_HTTP_3_Endpoint"
weight: 30100
rank: "h2"
---

HTTP需要依靠一种名为权威响应的概念：对于某个请求和某给定的创建响应时的目标资源状态，具有目标URI的身份标识的源服务器所能创建的最合适的那份响应。
有关如何为某HTTP URI定位其权威服务器，详见《[HTTP](https://www.rfc-editor.org/info/rfc9110)》的[第4.3章](https://www.rfc-editor.org/rfc/rfc9110#section-4.3)。

“https”协议将权威性与证书的持有者关联起来，该证书需要能够让客户端认定这个由URI的授权机构部分辨识的主机是可信的。
一旦在TLS握手中接收到了服务器证书，客户端就{{< req_level MUST >}}使用《[HTTP](https://www.rfc-editor.org/info/rfc9110)》的[第4.3.4章](https://www.rfc-editor.org/rfc/rfc9110#section-4.3.4)中描述的过程来验证证书与URI的源服务器是否匹配。
如果证书没有通过该验证，那么客户端{{< req_level MUST_NOT >}}将此服务器认定为这个源的权威服务器。

客户端{{< req_level MAY >}}尝试访问一项具有“https”URI的资源，方法是依次将主机标识符解析为IP地址，然后向该地址的默认端口发起和建立QUIC连接（包括验证服务器证书，如上文所述），最后经由这条安全的连接向着服务器发送指向该URI的HTTP/3请求消息。
除非使用了其他机制来选择HTTP/3协议，否则就在TLS握手期间的应用层协议协商扩展（ALPN；详见《[RFC7301](https://www.rfc-editor.org/info/rfc7301)》）中使用“h3”这个词。

连接性问题（例如，UDP被禁用）将导致无法建立QUIC连接；在这种情况下，客户端{{< req_level SHOULD >}}尝试使用基于TCP的HTTP版本。

服务器{{< req_level MAY >}}在任一UDP端口上提供HTTP/3服务；替代服务的对外宣告中总是明确包含着端口号，URI中总是要么显式包含着端口号，要么使用特定协议的默认端口号。
