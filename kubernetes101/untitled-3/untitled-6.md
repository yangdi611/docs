# Secrets

假设我们有一个Wordpress博客应用程序，其中我们的wordpress前端使用密码连接到MySQL数据库后端。在为wordpress创建部署时，我们可以在部署的YAML文件中包含MySQL密码，但是该密码不会受到保护。任何有权访问配置文件的人都可以使用该密码。

在这种情况下，[Secret](https://kubernetes.io/docs/concepts/configuration/secret/)对象可以通过允许我们在共享敏感信息之前对其进行编码来提供帮助。借助Secrets，我们可以像ConfigMaps一样以键值对的形式共享敏感信息，例如密码，令牌或密钥。因此，我们可以控制机密信息的使用方式，从而减少意外暴露的风险。在部署或其他资源中，将引用Secret对象，而不会暴露其内容。

重要的是要记住，secret数据以纯文本格式存储在etcd中，因此管理员必须限制对API服务器和etcd的访问。一项更新的功能允许secret数据在存储在etcd中时进行静态加密。需要在API服务器级别启用的功能。

