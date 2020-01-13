# 使用dashboard部署应用二

**用`nginx:alpine`镜像部署一个web服务器**

在仪表板上，单击仪表板右上角的+ CREATE选项卡。这将打开创建界面，如下所示：

![Deploy a Containerized Application Web Interface](../../.gitbook/assets/image%20%2828%29.png)

由此，我们可以使用文件的有效YAML / JSON配置数据创建应用程序，或者从**“**创建应用程序“部分手动创建应用程序。单击创建应用程序选项卡并提供以下应用程序详细信息：

* 应用名字叫**webserver**
* 要使用的Docker映像是nginx:alpine，其中alpine是映像标签
* The replica count, or the number of Pods, is 3
* 没有service，因为我们将在以后创建它。

![Deploy a Containerized Application Web Interface](https://github.com/yangdi611/docs/tree/9182ec0aacc614915b8202361f45684572b4ddf8/.gitbook/assets/image%20%2847%29.png)

如果我们点击“_Show Advanced Options_” ，我们可以定义一些现象如：Labels、Namespaces、环境变量等。默认情况下，app的Lable就是应用名称。在我们的示例中，k8s-app:webserver标签设置为此部署创建的所有对象：Pod和Services（公开时）。

通过单击“Deploy”按钮，我们触发部署。如预期的那样，部署Web服务器将创建一个ReplicaSet（webserver-74d8bd488f），该副本集最终将创建三个Pod（webserver-74d8bd488f-xxxxx）。

![Deployment Details](../../.gitbook/assets/image%20%287%29.png)

> 如果简单的nginx：alpine映像名称遇到任何问题，请在“容器映像”字段docker.io/library/nginx:alpine中添加完整URL（或改为使用k8s.gcr.io/nginx:alpine URL，如果可行）。

