# Authentication II

## 认证二

kubernets使用不同的认证模块来做认证：

* **用户证书 Client Certificates** 

  要启用客户端证书认证，我们需要通过将--client-ca-file = SOMEFILE选项传递给API服务器来引用包含一个或多个证书颁发机构的文件。文件中提到的证书颁发机构将验证提供给API服务器的客户端证书。本章末尾还提供了有关该主题的演示视频。

* **静态令牌文件 Static Token File**  我们可以使用--token-auth-file = SOMEFILE选项将包含预定义的承载令牌的文件传递给API服务器。当前，这些令牌将无限期地存在，并且不重新启动API服务器就无法更改它们。
* **临时令牌 Bootstrap Tokens** 

  该功能当前处于测试状态，主要用于引导新的Kubernetes集群。

* **静态密码文件 Static Password File** 

  它类似于静态令牌文件。我们可以使用--basic-auth-file = SOMEFILE选项传递包含基本身份验证详细信息的文件。这些凭据将无限期地保存，并且不重新启动API服务器就无法更改密码。

* **服务账户令牌 Service Account Tokens** 

  这是一个自动启用的身份验证器，它使用签名的承载令牌来验证请求。这些令牌使用ServiceAccount Admission Controller附加到Pod，该控制器允许集群内进程与API服务器通信。

* **OpenID连接令牌 OpenID Connect Tokens**

  OpenID Connect可帮助我们与OAuth2提供程序（例如Azure Active Directory，Salesforce，Google等）建立连接，以将身份验证转移到外部服务。

* **Web-hook令牌认证 Webhook Token Authentication** 使用基于Webhook的身份验证，可以将承载令牌的验证分流到远程服务。
* **认证代理 Authenticating Proxy**

  如果我们要编程其他身份验证逻辑，则可以使用身份验证代理。

我们可以启用多个身份验证器，第一个模块成功地对请求进行身份验证会使评估短路。为了获得成功，您应该至少启用两种方法：服务帐户令牌验证者和用户验证者之一。

