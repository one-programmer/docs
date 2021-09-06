#### 在所有响应中保持 JSON 缩小

额外的空白为请求增加了不必要的响应大小，许多供人类消费的客户端会自动“美化”JSON 输出。最好保持 JSON 响应最小化，例如：

```json
{"beta":false,"email":"alice@heroku.com","id":"01234567-89ab-cdef-0123-456789abcdef","last_login":"2012-01-01T12:00:00Z","created_at":"2012-01-01T12:00:00Z","updated_at":"2012-01-01T12:00:00Z"}
```

而不是例如：

```json
{
  "beta": false,
  "email": "alice@heroku.com",
  "id": "01234567-89ab-cdef-0123-456789abcdef",
  "last_login": "2012-01-01T12:00:00Z",
  "created_at": "2012-01-01T12:00:00Z",
  "updated_at": "2012-01-01T12:00:00Z"
}
```

您可以考虑通过查询参数（例如`?pretty=true`）或通过`Accept`标头参数（例如 `Accept: application/vnd.heroku+json; version=3; indent=4;`）为客户端提供一种可选的方式来检索更详细的响应。