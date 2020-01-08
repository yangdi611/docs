# 使用Ingress访问服务

使用我们刚刚创建的Ingress资源，我们现在应该能够使用blue.example.com和green.example.com URL访问webserver-blue-svc或webserver-green-svc服务。由于我们当前的设置是在Minikube上，因此我们需要将工作站上的主机配置文件（在Mac和Linux上为/ etc / hosts）更新为这些URL的Minikube IP。更新后，文件应类似于：

```bash
$ cat /etc/hosts
127.0.0.1        localhost
::1              localhost
192.168.99.100   blue.example.com green.example.com 
```

现在，我们可以在浏览器中打开blue.example.com和green.example.com并访问每个应用程序。

