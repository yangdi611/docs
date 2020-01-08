# 容器存储接口（CSI）

诸如Kubernetes，Mesos，Docker或Cloud Foundry之类的容器编排曾经有自己的使用Volumes管理外部存储的方法。对于存储供应商而言，为不同的业务流程管理者管理不同的Volume插件是一项挑战。来自不同协调器的存储供应商和社区成员开始合作，以标准化Volume接口。使用标准CSI构建的卷插件，该CSI设计用于不同的容器协调器。您可以在此处找到[CSI规格](https://github.com/container-storage-interface/spec/blob/master/spec.md)。

在Kubernetes版本v1.9和v1.13之间，CSI从alpha升级为稳定的支持（stable support），这使得安装新的，符合CSI的Volume插件非常容易。借助CSI，第三方存储提供商可以开发解决方案，而无需将其添加到核心Kubernetes代码库中。

