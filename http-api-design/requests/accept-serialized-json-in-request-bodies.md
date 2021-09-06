#### 接受请求正文中的序列化 JSON

接受序列化JSON在 `PUT`/`PATCH`/`POST` 请求机构中，代替或除了的form-encoded的数据。
这创建了与 JSON 序列化响应主体的对称性，例如：

```bash
$ curl -X POST https://service.com/apps \
    -H "Content-Type: application/json" \
    -d '{"name": "demoapp"}'

{
  "id": "01234567-89ab-cdef-0123-456789abcdef",
  "name": "demoapp",
  "owner": {
    "email": "username@example.com",
    "id": "01234567-89ab-cdef-0123-456789abcdef"
  },
  ...
}
```