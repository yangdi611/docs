# APIs - with 'kubectl proxy'

当kubectl代理运行时，我们可以通过代理端口8001上的localhost将请求发送到API（来自另一个终端，因为代理锁定了第一个终端）：

```bash
$ curl http://localhost:8001/
{
 "paths": [
   "/api",
   "/api/v1",
   "/apis",
   "/apis/apps",
   ......
   ......
   "/logs",
   "/metrics",
   "/openapi/v2",
   "/version"
 ]
}
```

通过上述curl请求，我们从API服务器请求了所有API端点。单击上面的链接（在curl命令中），它将在浏览器选项卡中打开相同的列表输出。

我们可以使用curl或在浏览器中探索每个单独的路径组合，例如：

```bash
http://localhost:8001/api/v1

http://localhost:8001/apis/apps/v1

http://localhost:8001/healthz

http://localhost:8001/metrics
```

