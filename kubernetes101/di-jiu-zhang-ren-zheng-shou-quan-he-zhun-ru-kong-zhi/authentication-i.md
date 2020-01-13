# Authentication I

## 认证一

Kubernetes没有叫user的对象，也不在对象数据（object store）中保存uasername和其相关的信息。尽管，在没有上述功能的情况下，Kubernetes仍然会使用usernames作为访问控制。

Kubernetes包含两种用户：

* **Normal Users** 它们通过Kubernetes集群之外的独立服务通过用户/客户端证书，列出用户名/密码，Google帐户等文件的独立服务进行管理。
* **Service Accounts** 与服务帐户用户一起，群集内进程与API服务器通信以执行不同的操作。大多数服务帐户用户是通过API服务器自动创建的，但也可以手动创建。服务帐户用户绑定到给定的命名空间，并挂载各自的凭据以作为Secrets与API服务器通信。

如果配置正确，Kubernetes还可以支持匿名请求以及普通用户和服务帐户的请求。还支持用户模拟，以使用户能够充当另一个用户，这是管理员在对授权策略进行故障排除时的一项有用功能。



