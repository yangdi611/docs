# Service类型：ExternalIP

如果服务可以路由到一个或多个辅助节点，则可以将其映射到ExternalIP地址。使用服务端口上的ExternalIP（作为目标IP）进入群集的流量将路由到服务端点之一。此类服务需要外部云提供商，例如Google Cloud Platform或AWS。

![ExternalIP](../../.gitbook/assets/image%20%2815%29.png)

请注意，外部IP不由Kubernetes管理。群集管理员必须配置将外部IP地址映射到节点之一的路由。

