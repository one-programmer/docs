#### 为自省提供请求 ID

在每个 API 响应中包含一个 `Request-Id` 标头，填充一个 UUID 值。
通过在客户端、服务器和任何支持服务上记录这些值，它提供了一种跟踪、诊断和调试请求的机制。