# Kubernetes Dashboard

如前所述，[Kubernetes仪表盘](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/)为Kubernetes集群管理提供了一个基于Web的用户界面。要从Minikube访问仪表盘，我们可以使用minikube dashboard命令，该命令在Web浏览器上打开一个新选项卡，显示Kubernetes仪表盘：`minikube dashboard`

![Kubernetes Dashboard](../../.gitbook/assets/image%20%2842%29.png)

> 如果浏览器没有打开另一个选项卡并且未按预期显示仪表板，请在终端中验证输出，因为它可能显示仪表板的链接（以及一些错误消息）。将该链接复制并粘贴到浏览器的新标签中。根据终端的功能，您也许可以单击或右键单击链接以直接在浏览器中打开。该链接可能类似​​于：
>
> ```bash
> http://127.0.0.1:37751/api/v1/namespaces/kube-system/services/http:kubernetes-dashboard:/proxy/
> ```
>
> 可能唯一的区别是端口号，上面是37751。您的端口号可能不同。
>
> 注销/登录或重新启动工作站后，应该会出现正常的行为（其中minikube仪表板命令在浏览器中直接打开一个新选项卡，显示仪表板）。

