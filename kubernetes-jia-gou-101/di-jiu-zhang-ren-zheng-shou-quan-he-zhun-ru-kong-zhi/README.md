# 第九章 认证，授权和准入控制

要访问和管理集群中的任何Kubernetes资源或对象，我们需要访问API服务器上的特定API端点。每个访问请求都经历以下三个阶段：

* **认证** 用户登入。
* **授权** 授权登录用户添加的API请求。
* **准入控制** 可以基于某些其他检查（例如预设配额 Quota）修改或拒绝请求的软件模块。

下图描述了以上阶段：（[Kubernetes.io](https://kubernetes.io/docs/admin/accessing-the-api/)）

![Accessing the API](../../.gitbook/assets/image%20%2821%29.png)

