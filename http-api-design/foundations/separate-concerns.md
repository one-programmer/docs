#### 隔离关注点

通过隔离请求和响应生命周期中不同部分的关注点，在设计时保持简单。保持简单的规则可以让更多的注意力集中在更大和更难的问题上。

请求和响应用以解决特定资源或集合。使用路径（path）来表示身份，使用正文（body）来传输内容，使用头（header）来传达元数据。
查询参数（query param）也可以在边缘情况下用作传递标头信息的手段，但首选头信息，因为它们更灵活并且可以传达更多样的信息。