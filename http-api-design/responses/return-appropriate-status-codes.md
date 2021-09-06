#### 返回适当的状态代码

为每个响应返回适当的 HTTP 状态代码。成功的响应应根据本指南进行编码：

* `200`：请求成功了`GET`，`POST`，`DELETE`，或`PATCH`调用同步完成，或`PUT`调用同步更新现有资源
* `201`：请求成功`POST`，或`PUT`同步创建新资源的调用。提供指向新创建资源的"位置"标头也是最佳实践。
这在POST上下文中特别有用，因为新资源将具有与原始请求不同的 URL
* `202`：接受请求的`POST`，`PUT`，`DELETE`，或`PATCH`通话将被异步处理
* `206`：请求成功`GET`，但只返回了部分响应：见[上面的范围](../foundations/divide-large-responses-across-requests-with-ranges.md)

注意认证和授权错误码的使用：

* `401 Unauthorized`：请求失败，因为用户未通过身份验证
* `403 Forbidden`：请求失败，因为用户无权访问特定资源

出现错误时返回合适的代码以提供附加信息：

* `422 Unprocessable Entity`：您的请求已被理解，但包含无效参数
* `429 Too Many Requests`： 你被限速了，稍后重试
* `500 Internal Server Error`：服务器出现问题，检查状态站点和/或报告问题

有关用户错误和服务器错误情况的状态代码的指导，请参阅[HTTP 响应代码规范](https://tools.ietf.org/html/rfc7231#section-6) 。