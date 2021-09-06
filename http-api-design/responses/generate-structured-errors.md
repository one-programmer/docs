#### 生成结构化错误

针对错误生成一致、结构化的响应主体。包括一个机器可读的错误`id`，一个人类可读的错误`message`，以及可选的`url`指向客户端关于错误和如何解决它的更多信息，例如：

```
HTTP/1.1 429 Too Many Requests
```

```json
{
  "id":      "rate_limit",
  "message": "Account reached its API rate limit.",
  "url":     "https://docs.service.com/rate-limits"
}
```

记录您的错误格式和`id`客户可能遇到的可能错误。