# ReplicationControllers

尽管已经不再推荐使用，[ReplicationController](https://kubernetes.io/docs/concepts/workloads/controllers/replicationcontroller/)是一个控制器，可确保在任何给定时间运行指定数量的Pod副本。如果Pod数量多于所需数量，则复制​​控制器将终止多余的Pod；如果Pod数量较少，则复制控制器将创建更多Pod以匹配所需数量。通常，我们不会独立部署Pod，因为如果终止错误，Pod将无法重新启动。推荐的方法是使用某种类型的复制控制器来创建和管理Pod。

