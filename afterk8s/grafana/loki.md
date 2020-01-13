# Loki

## Loki-stack的学习记录

Loki是Grafana公司开发的一套基于Promthues的集成日志监控的监控系统，Loki的灵感来源于Promthues。可以实现日志实时展示，通过fluent-bit以sidecar方式收集容器的日志并记录在Promthues中。

### Loki-stack的安装

#### 1. 通过Helm安装

**1.1 安装**

之前在helm的使用文档中已经记录了如何通过helm拉取和打包charts，这里我们仅介绍如何通过helm来部署Loki-stack。 通过如下命令部署fluent-bit/grafana/promthues。

```bash
helm upgrade --install loki loki/loki-stack \
    --set fluent-bit.enabled=true,promtail.enabled=false,grafana.enabled=true,prometheus.enabled=true,prometheus.alertmanager.persistentVolume.enabled=false,prometheus.server.persistentVolume.enabled=false
```

部署完成之后我们可以用kubectl来查看部署状态：

```bash
kubectl get pods -o wide
loki-0                                               1/1     Running   0          5d1h    192.168.252.14    worker2-156.vmwlab   <none>           <none>
loki-fluent-bit-loki-4xngc                           1/1     Running   0          5d1h    192.168.252.12    worker2-156.vmwlab   <none>           <none>
loki-fluent-bit-loki-8fn2m                           1/1     Running   0          5d1h    192.168.232.69    master2-144.vmwlab   <none>           <none>
loki-fluent-bit-loki-c2zpd                           1/1     Running   0          5d1h    192.168.246.68    master3-145.vmslab   <none>           <none>
loki-fluent-bit-loki-t67z5                           1/1     Running   0          5d1h    192.168.145.131   master1-143.vmwlab   <none>           <none>
loki-fluent-bit-loki-vnlvd                           1/1     Running   0          5d1h    192.168.143.140   worker1-155.vmwlab   <none>           <none>
loki-fluent-bit-loki-xzpgc                           1/1     Running   0          5d1h    192.168.203.71    worker4-158.vmwlab   <none>           <none>
loki-fluent-bit-loki-zpfbq                           1/1     Running   0          5d1h    192.168.106.75    worker3-157.vmwlab   <none>           <none>
loki-grafana-547cd59fd-qst52                         1/1     Running   0          4d23h   192.168.203.73    worker4-158.vmwlab   <none>           <none>
loki-prometheus-alertmanager-6874fd6879-glzk6        2/2     Running   0          5d1h    192.168.106.74    worker3-157.vmwlab   <none>           <none>
loki-prometheus-kube-state-metrics-6fbb56b47-72vnc   1/1     Running   0          5d1h    192.168.143.139   worker1-155.vmwlab   <none>           <none>
loki-prometheus-node-exporter-fb8sj                  1/1     Running   0          5d1h    10.0.209.157      worker3-157.vmwlab   <none>           <none>
loki-prometheus-node-exporter-fkj2b                  1/1     Running   0          5d1h    10.0.209.155      worker1-155.vmwlab   <none>           <none>
loki-prometheus-node-exporter-k7dk8                  1/1     Running   0          5d1h    10.0.209.158      worker4-158.vmwlab   <none>           <none>
loki-prometheus-node-exporter-zc9c4                  1/1     Running   0          5d1h    10.0.209.156      worker2-156.vmwlab   <none>           <none>
loki-prometheus-pushgateway-5f7f76f45-fzwc8          1/1     Running   0          5d1h    192.168.252.13    worker2-156.vmwlab   <none>           <none>
loki-prometheus-server-db6767f6f-d56h9               2/2     Running   0          5d1h    192.168.143.138   worker1-155.vmwlab   <none>           <none>
```

### 使用Loki

#### 通过kube-proxy使用Loki

在没有对任何参数进行修改的情况，由于当前K8s不对外暴露任何服务，所以我们需要使用kubeproxy来对grafana进行访问：

```bash
kubectl port-forward --namespace <YOUR-NAMESPACE> service/loki-grafana 3000:80
```

之后只要浏览器方位localhost就可以访问到集群中的grafana了。

