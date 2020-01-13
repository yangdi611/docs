# 安全和Pod安全策略

有时我们需要为Pod和Containers定义特定的特权和访问控制设置。[安全context](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)允许我们为对象访问权限，特权运行，功能，安全标签等设置自由访问控制。但是，它们的作用仅限于在规范部分中结合了此类上下文设置的各个Pod和Containers。

为了将安全设置应用于群集范围内的多个Pod和Container，我们可以定义[Pod安全策略](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)。它们允许进行更细粒度的安全设置，以控制主机名称空间，主机网络和端口，文件系统组，卷类型的使用，实施容器用户和组ID，根特权升级等。

