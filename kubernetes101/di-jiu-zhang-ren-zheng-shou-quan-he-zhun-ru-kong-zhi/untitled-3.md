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

为证书签名请求对象创建一个YAML配置文件，并将其保存为请求字段的空白值：

```yaml
~/rbac$ vim signing-request.yaml

apiVersion: certificates.k8s.io/v1beta1
kind: CertificateSigningRequest
metadata:
  name: student-csr
spec:
  groups:
  - system:authenticated
  request: <assign encoded value from next cat command>
  usages:
  - digital signature
  - key encipherment
  - client auth
```

查看证书，将其编码为base64，然后将其分配给signing-request.yaml文件中的request字段：

```yaml
~/rbac$ cat student.csr | base64 | tr -d '\n'

LS0tLS1CRUd...1QtLS0tLQo=

~/rbac$ vim signing-request.yaml

apiVersion: certificates.k8s.io/v1beta1
kind: CertificateSigningRequest
metadata:
  name: student-csr
spec:
  groups:
  - system:authenticated
  request: LS0tLS1CRUd...1QtLS0tLQo=
  usages:
  - digital signature
  - key encipherment
  - client auth
```

创建证书签名请求对象，然后列出证书签名请求对象。它显示一个挂起状态：

```bash
~/rbac$ kubectl create -f signing-request.yaml

certificatesigningrequest.certificates.k8s.io/student-csr created

~/rbac$ kubectl get csr

NAME          AGE   REQUESTOR       CONDITION
student-csr   27s   minikube-user   Pending
```

批准证书签名请求对象，然后再次列出证书签名请求对象。它显示了已批准和已发布状态：

```bash
~/rbac$ kubectl certificate approve student-csr

certificatesigningrequest.certificates.k8s.io/student-csr approved

~/rbac$ kubectl get csr

NAME          AGE   REQUESTOR       CONDITION
student-csr   77s   minikube-user   Approved,Issued
```

从证书签名请求中提取批准的证书，使用base64对其进行解码，并将其另存为证书文件。然后在新创建的证书文件中查看证书：

```bash
~/rbac$ kubectl get csr student-csr -o jsonpath='{.status.certificate}' | base64 --decode > student.crt

~/rbac$ cat student.crt

-----BEGIN CERTIFICATE-----
MIIDGzCCA...
...
...NOZRRZBVunTjK7A==
-----END CERTIFICATE-----
```

通过分配密钥和证书来配置`student`用户的凭据：

```bash
~/rbac$ kubectl config set-credentials student --client-certificate=student.crt --client-key=student.key

User "student" set.
```

在Kubectl客户端的配置文件中为`student`用户创建一个新的上下文条目，该条目与minikube集群中的lfs158命名空间相关联：

```bash
~/rbac$ kubectl config set-context student-context --cluster=minikube --namespace=lfs158 --user=student

Context "student-context" created.
```

再次查看kubectl客户端配置文件的内容，观察新的上下文条目student-context和新的用户条目student：

```yaml
~/rbac$ kubectl config view

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
- context:
    cluster: minikube
    namespace: lfs158
    user: student
  name: student-context
current-context: minikube
kind: Config
preferences: {}
users:
- name: minikube
  user:
    client-certificate: /home/student/.minikube/client.crt
    client-key: /home/student/.minikube/client.key
- name: student
  user:
    client-certificate: /home/student/rbac/student.crt
    client-key: /home/student/rbac/student.key
```

在默认的minikube上下文中，在lfs158命名空间中创建一个新部署：

```bash
~/rbac$ kubectl -n lfs158 create deployment nginx --image=nginx:alpine

deployment.apps/nginx created
```

在新的上下文（context）中，student-context尝试列出pods。尝试失败，因为学生用户没有为学生上下文配置权限：

```bash
~/rbac$ kubectl --context=student-context get pods

Error from server (Forbidden): pods is forbidden: User "student" cannot list resource "pods" in API group "" in the namespace "lfs158"
```

以下步骤将在student-context中为学生用户分配一组有限的权限。

为pod-reader角色对象创建YAML配置文件，该文件仅允许在lfs158名称空间中针对pod对象获取，监视，列出操作（get, watch, list）。然后创建角色对象，并从默认的minikube contect（但从lfs158命名空间）列出该对象：

```yaml
~/rbac$ vim role.yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: lfs158
rules:
- apiGroups: [""]
  resources: ["pods"]
  verbs: ["get", "watch", "list"]

~/rbac$ kubectl create -f role.yaml

role.rbac.authorization.k8s.io/pod-reader created

~/rbac$ kubectl -n lfs158 get roles

NAME         AGE
pod-reader   57s
```

为rolebingding对象创建YAML配置文件，该文件将pod-reader角色的权限分配给学生用户。然后创建角色绑定对象，并从默认的minikube contxt（但从lfs158命名空间）列出该对象：

```yaml
~/rbac$ vim rolebinding.yaml

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-read-access
  namespace: lfs158
subjects:
- kind: User
  name: student
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io

~/rbac$ kubectl create -f rolebinding.yaml 

rolebinding.rbac.authorization.k8s.io/pod-read-access created

~/rbac$ kubectl -n lfs158 get rolebindings

NAME              AGE
pod-read-access   23s
```

现在，我们已经为student用户分配了权限，我们可以成功地列出来自新上下文“student-context”的pod。

```bash
~/rbac$ kubectl --context=student-context get pods

NAME                    READY   STATUS    RESTARTS   AGE
nginx-77595c695-f2xmd   1/1     Running   0          7m41s
```

