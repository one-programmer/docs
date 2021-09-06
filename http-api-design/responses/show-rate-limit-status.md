#### 显示速率限制状态

来自客户端的速率限制请求，以保护服务的健康并为其他客户端保持高服务质量。
您可以使用[令牌桶算法](http://en.wikipedia.org/wiki/Token_bucket) 来量化请求限制。

返回`RateLimit-Remaining`响应标头中每个请求的剩余请求令牌数。