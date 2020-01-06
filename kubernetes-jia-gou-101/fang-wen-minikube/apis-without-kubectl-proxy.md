# APIs - without 'kubectl proxy'

当不使用Kubectl代理时，我们需要在发送API请求时向API服务器进行身份验证。我们可以通过在使用curl时提供"Bearer Token"或通过提供一组密钥和证书来进行身份验证。

"Bearer Token"是一种访问令牌，由身份验证服务器（主节点上的API服务器）生成，并返回给客户端。使用该令牌，客户端无需提供进一步的身份验证详细信息即可连接回Kubernetes API服务器，然后访问资源。

#### 得到Token

```bash
$ TOKEN=$(kubectl describe secret -n kube-system $(kubectl get secrets -n kube-system | grep default | cut -f1 -d ' ') | grep -E '^token' | cut -f2 -d':' | tr -d '\t' | tr -d " ")
```

#### 得到API server endpoint

```bash
$ APISERVER=$(kubectl config view | grep https | cut -f 2- -d ":" | tr -d " ")
```

通过发出以下两个命令并比较它们的输出，确认APISERVER与Kubernetes主IP存储了相同的IP：

```bash
$ echo $APISERVER
https://192.168.99.100:8443

$ kubectl cluster-info
Kubernetes master is running at https://192.168.99.100:8443 ...
```

#### 使用curl命令访问API服务器，如下所示：

```bash
$ curl $APISERVER --header "Authorization: Bearer $TOKEN" --insecure
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

除了访问令牌，我们还可以从.kube / config文件中提取客户端证书，客户端密钥和证书颁发机构数据。提取后，将对它们进行编码，然后使用curl命令进行传递以进行身份​​验证。新的curl命令类似于：

```bash
$ curl $APISERVER --cert encoded-cert --key encoded-key --cacert encoded-ca
```



