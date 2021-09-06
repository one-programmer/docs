#### 为方便起见，支持非 id 解引用

在某些情况下，最终用户可能不方便提供 ID 来标识资源。例如，用户可能会想到 Heroku 应用程序名称，但该应用程序可能由 UUID 标识。
在这些情况下，您可能希望同时接受 id 或名称，例如：

```bash
$ curl https://service.com/apps/{app_id_or_name}
$ curl https://service.com/apps/97addcf0-c182
$ curl https://service.com/apps/www-prod
```

不要只接受名称而排除 ID。