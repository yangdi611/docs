# Helm

要部署应用程序，我们使用不同的Kubernetes清单，例如Deployments，Services，Volume Claims，Ingress等。有时，逐一部署它们可能很麻烦。在将所有清单模板化为明确定义的格式后，我们可以将其与其他元数据捆绑在一起。这样的捆绑包称为Chart。然后可以通过存储库（例如我们为rpm和deb软件包提供的存储库）提供这些Chart。

Helm是Kubernetes的软件包管理器（类似于yum和apt，适用于Linux），可以在Kubernetes集群中安装/更新/删除这些Chart。

