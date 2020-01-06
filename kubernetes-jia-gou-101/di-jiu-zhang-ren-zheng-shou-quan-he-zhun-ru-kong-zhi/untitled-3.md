# 身份验证和授权练习指南

本练习指南假定使用以下环境，默认情况下，该环境使用/ var / lib / minikube / certs /中的证书和密钥以及RBAC模式进行授权：

* Minikube v1.0.1
* Kubernetes v1.14.1
* Docker 18.06.3-ce

该练习指南可以与下一页上的视频演示一起使用，并且已针对上述环境进行了更新，而视频则提供了带有Kubernetes v1.9的较旧的Minikube发行版。

启动Minikube：

```bash
minikube start
```

查看Kubectl客户端配置文件的内容，观察默认情况下创建的唯一上下文迷你kube和唯一用户迷你kube：

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

新建一个lsf158的命名空间：

```bash
$ kubectl create namespace lfs158

namespace/lfs158 created
```

创建rbac目录并cd到其中：

```bash
$ mkdir rbac

$ cd rbac/
```

使用openssl工具为`student`用户创建私钥，然后使用openssl工具为`student`用户创建证书签名请求：

```bash
~/rbac$ openssl genrsa -out student.key 2048

Generating RSA private key, 2048 bit long modulus (2 primes)
.................................................+++++
.........................+++++
e is 65537 (0x010001)

~/rbac$ openssl req -new -key student.key -out student.csr -subj "/CN=student/O=learner"
```

