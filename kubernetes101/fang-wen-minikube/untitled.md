# kubectl配置文件

要访问Kubernetes集群，kubectl客户端需要主节点端点和适当的凭据才能与在主节点上运行的API服务器进行交互。启动Minikube时，默认情况下，启动过程会在.kube目录（通常称为dot-kube-config文件）内创建一个配置文件config，该目录位于用户的主目录中。配置文件包含kubectl所需的所有连接详细信息。默认情况下，kubectl二进制文件会解析此文件以查找主节点的连接端点以及凭据。要查看连接详细信息，我们可以查看〜/ .kube / config文件的内容（在Linux上）或运行以下命令：

```yaml
$ kubectl config view
apiVersion: v1
clusters:
- cluster:
    certificate-authority: /home/student/.minikube/ca.crt
    server: https://192.168.99.100:8443
  name: minikube
contexts:
- context:
    cluster: minikube
    user: minikube
  name: minikube
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /home/student/.minikube/client.crt
    client-key: /home/student/.minikube/client.key
```

安装kubectl之后，我们可以使用kubectl cluster-info命令获取有关Minikube集群的信息：

```bash
$ kubectl cluster-info
Kubernetes master is running at https://192.168.99.100:8443
KubeDNS is running at https://192.168.99.100:8443//api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
```

您可以在[此处](https://kubernetes.io/docs/reference/kubectl/overview/)找到有关kubectl命令行选项的更多详细信息。

尽管对于Minikube安装的Kubernetes集群来说，〜/ .kube / config文件是自动创建的，但其他工具安装的Kubernetes集群却不是这样。在其他情况下，必须手动创建配置文件，有时需要重新配置文件以适合各种网络和客户端/服务器设置。

