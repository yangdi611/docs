# 活性（Liveness）

如果一个容器在Pod中运行，但是应用在这个容器中不相应我们的请求，这样这个容器其实对我们来说是没用的。这种情况会发生，比如，应用死锁或内存压力。这这种情况下，我们建议去重启这个容器来使它可用。

除了手动的重启，我们也可以使用活性探针。活动性探针会检查应用程序的运行状况，如果运行状况检查失败，则kubelet会自动重新启动受影响的容器。

活性探针可以定义：

* Liveness command
* Liveness HTTP request
* TCP Liveness Probe.

