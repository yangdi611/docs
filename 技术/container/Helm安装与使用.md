# Helm

## Helm的安装

1. 下载Helm安装包：
https://github.com/helm/helm/releases

2. 直接二进制安装：
解压缩之后拷贝到/usr/local/bin中。

这样命令就可以用了。

## Helm的使用

>这里按照部署Grafana Loki为例，用helm的方式进行部署。

1. 下载Grafana Loki的helm charts来分别查看安装Loki都需要那些介质
<br>首先增加一个charts的源：
```bash
helm repo add loki https://grafana.github.io/loki/charts
```
然后把这个下载这个charts:
```bash
helm pull loki/loki-stack
```
pull下来之后就是打包好的charts，然后我们打开看一下这个package解开里面的内容：
```bash
loki-stack-0.23.0.tgz
tar xvf loki-stack-0.23.0.tgz
```
然后我们来看一下这个文件目录的结构：
我们就能看见charts目录下面的每个目录都是一个应用目录，其中values.yaml中的会含有他所需要的docker image，这些docker image我们先下载好之后放到我们的私库中并在这些文件中改相应我们私库中的tag。
```bash
[root@localhost ~]# tree loki-stack
loki-stack
├── charts
│   ├── fluent-bit
│   │   ├── Chart.yaml
│   │   ├── README.md
│   │   ├── templates
│   │   │   ├── clusterrolebinding.yaml
│   │   │   ├── clusterrole.yaml
│   │   │   ├── configmap.yaml
│   │   │   ├── daemonset.yaml
│   │   │   ├── _helpers.tpl
│   │   │   ├── NOTES.txt
│   │   │   ├── serviceaccount.yaml
│   │   │   ├── service-headless.yaml
│   │   │   └── servicemonitor.yaml
│   │   └── values.yaml
│   ├── grafana
│   │   ├── Chart.yaml
│   │   ├── dashboards
│   │   │   └── custom-dashboard.json
│   │   ├── README.md
│   │   ├── templates
│   │   │   ├── clusterrolebinding.yaml
│   │   │   ├── clusterrole.yaml
│   │   │   ├── configmap-dashboard-provider.yaml
│   │   │   ├── configmap.yaml
│   │   │   ├── dashboards-json-configmap.yaml
│   │   │   ├── deployment.yaml
│   │   │   ├── headless-service.yaml
│   │   │   ├── _helpers.tpl
│   │   │   ├── ingress.yaml
│   │   │   ├── NOTES.txt
│   │   │   ├── podsecuritypolicy.yaml
│   │   │   ├── _pod.tpl
│   │   │   ├── pvc.yaml
│   │   │   ├── rolebinding.yaml
│   │   │   ├── role.yaml
│   │   │   ├── secret.yaml
│   │   │   ├── serviceaccount.yaml
│   │   │   ├── service.yaml
│   │   │   ├── statefulset.yaml
│   │   │   └── tests
│   │   │       ├── test-configmap.yaml
│   │   │       ├── test-podsecuritypolicy.yaml
│   │   │       ├── test-rolebinding.yaml
│   │   │       ├── test-role.yaml
│   │   │       ├── test-serviceaccount.yaml
│   │   │       └── test.yaml
│   │   └── values.yaml
│   ├── loki
│   │   ├── Chart.yaml
│   │   ├── templates
│   │   │   ├── _helpers.tpl
│   │   │   ├── networkpolicy.yaml
│   │   │   ├── NOTES.txt
│   │   │   ├── pdb.yaml
│   │   │   ├── podsecuritypolicy.yaml
│   │   │   ├── rolebinding.yaml
│   │   │   ├── role.yaml
│   │   │   ├── secret.yaml
│   │   │   ├── serviceaccount.yaml
│   │   │   ├── service-headless.yaml
│   │   │   ├── servicemonitor.yaml
│   │   │   ├── service.yaml
│   │   │   └── statefulset.yaml
│   │   └── values.yaml
│   ├── prometheus
│   │   ├── Chart.yaml
│   │   ├── README.md
│   │   ├── templates
│   │   │   ├── alertmanager-clusterrolebinding.yaml
│   │   │   ├── alertmanager-clusterrole.yaml
│   │   │   ├── alertmanager-configmap.yaml
│   │   │   ├── alertmanager-deployment.yaml
│   │   │   ├── alertmanager-ingress.yaml
│   │   │   ├── alertmanager-networkpolicy.yaml
│   │   │   ├── alertmanager-podsecuritypolicy.yaml
│   │   │   ├── alertmanager-pvc.yaml
│   │   │   ├── alertmanager-serviceaccount.yaml
│   │   │   ├── alertmanager-service-headless.yaml
│   │   │   ├── alertmanager-service.yaml
│   │   │   ├── alertmanager-statefulset.yaml
│   │   │   ├── _helpers.tpl
│   │   │   ├── kube-state-metrics-clusterrolebinding.yaml
│   │   │   ├── kube-state-metrics-clusterrole.yaml
│   │   │   ├── kube-state-metrics-deployment.yaml
│   │   │   ├── kube-state-metrics-networkpolicy.yaml
│   │   │   ├── kube-state-metrics-podsecuritypolicy.yaml
│   │   │   ├── kube-state-metrics-serviceaccount.yaml
│   │   │   ├── kube-state-metrics-svc.yaml
│   │   │   ├── node-exporter-daemonset.yaml
│   │   │   ├── node-exporter-podsecuritypolicy.yaml
│   │   │   ├── node-exporter-rolebinding.yaml
│   │   │   ├── node-exporter-role.yaml
│   │   │   ├── node-exporter-serviceaccount.yaml
│   │   │   ├── node-exporter-service.yaml
│   │   │   ├── NOTES.txt
│   │   │   ├── pushgateway-clusterrolebinding.yaml
│   │   │   ├── pushgateway-clusterrole.yaml
│   │   │   ├── pushgateway-deployment.yaml
│   │   │   ├── pushgateway-ingress.yaml
│   │   │   ├── pushgateway-networkpolicy.yaml
│   │   │   ├── pushgateway-podsecuritypolicy.yaml
│   │   │   ├── pushgateway-pvc.yaml
│   │   │   ├── pushgateway-serviceaccount.yaml
│   │   │   ├── pushgateway-service.yaml
│   │   │   ├── server-clusterrolebinding.yaml
│   │   │   ├── server-clusterrole.yaml
│   │   │   ├── server-configmap.yaml
│   │   │   ├── server-deployment.yaml
│   │   │   ├── server-ingress.yaml
│   │   │   ├── server-networkpolicy.yaml
│   │   │   ├── server-podsecuritypolicy.yaml
│   │   │   ├── server-pvc.yaml
│   │   │   ├── server-serviceaccount.yaml
│   │   │   ├── server-service-headless.yaml
│   │   │   ├── server-service.yaml
│   │   │   └── server-statefulset.yaml
│   │   └── values.yaml
│   └── promtail
│       ├── Chart.yaml
│       ├── templates
│       │   ├── clusterrolebinding.yaml
│       │   ├── clusterrole.yaml
│       │   ├── configmap.yaml
│       │   ├── daemonset.yaml
│       │   ├── _helpers.tpl
│       │   ├── NOTES.txt
│       │   ├── podsecuritypolicy.yaml
│       │   ├── rolebinding.yaml
│       │   ├── role.yaml
│       │   ├── serviceaccount.yaml
│       │   ├── service-headless.yaml
│       │   └── servicemonitor.yaml
│       └── values.yaml
├── Chart.yaml
├── requirements.lock
├── requirements.yaml
├── templates
│   ├── datasources.yaml
│   ├── _helpers.tpl
│   ├── NOTES.txt
│   └── tests
│       ├── loki-test-configmap.yaml
│       └── loki-test-pod.yaml
└── values.yaml
```

2. 我们需要对charts再进行打包
<br>改好之后我们就可以把这个目录重新打包，放到我们helm的私库中（就是一个http server中并打上helm的标签）：
```bash
helm package loki-stack
helm repo index ./
```
然后我们就可以看到在包目录中生成了一个index.yaml的文件，这个文件就是包的说明文件：
```bash
index.yaml
loki-stack-0.20.0.tgz
```
然后我们把这个目录加入到helm的repo中，就可以使用这个repo了：
```bash
NAME    URL                      
loki    http://10.0.209.10/charts
```
使用方法，如下：
```bash
helm upgrade --install loki loki/loki-stack \
    --set fluent-bit.enabled=true,promtail.enabled=false,grafana.enabled=true,prometheus.enabled=true,prometheus.alertmanager.persistentVolume.enabled=false,prometheus.server.persistentVolume.enabled=false
```
然后，我们先取得loki的admin登录密码：
```bash

```
