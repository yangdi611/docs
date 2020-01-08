# 使用dashboard部署应用一

在接下来的几节中，我们将学习如何使用nginx：alpine Docker镜像部署nginx Web服务器。

#### 启动minikube并检查它是否正常运行

首先运行此命令：

```bash
$ minikube start
```

等待几分钟，以启动Minikube，然后验证Minikube的状态：

```bash
$ minikube status
host: Running
kubelet: Running
apiserver: Running
kubectl: Correctly Configured: pointing to minikube-vm at 192.168.99.100
```

#### 启动minikube dashboard

要访问Kubernetes Web IU，我们需要运行以下命令：

```bash
$ minikube dashboard
```

运行此命令将打开带有Kubernetes Web UI的浏览器，我们可以使用它来管理容器化的应用程序。默认情况下，仪表板连接到默认（default）名称空间。因此，我们将在本章中执行的所有操作将在默认（default）名称空间内执行。

![Deploying an Application - Accessing the Dashboard](../../.gitbook/assets/image%20%2810%29.png)

> 如果浏览器没有打开另一个选项卡并且没有按预期方式显示仪表板，请在终端中验证输出，因为它可能显示仪表板的链接（以及一些错误消息）。将该链接复制并粘贴到浏览器的新标签中。根据终端的功能，您也许可以单击或右键单击链接以直接在浏览器中打开。该链接可能类似​​于：
>
> ```text
> http://127.0.0.1:37751/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy/
> ```

可能唯一的区别是端口号，上面是37751。您的端口号可能不同。

注销/登录或重新启动工作站后，应该可以期待正常的行为（其中“ minikube仪表板”命令会在浏览器中直接打开一个新选项卡，显示“仪表板”）。

