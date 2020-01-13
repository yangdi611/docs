# Jobs and CronJobs

[Job](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/)会创建一个或多个Pod以执行给定任务。Job对象负责Pod失败。确保给定任务成功完成。任务完成后，所有Pod都将自动终止。作业配置选项包括并行性-设置允许并行运行的pod的数量；完成次数-设置预期的完成次数；activeDeadlineSeconds-设置作业的持续时间；backoffLimit-设置Job标记为失败之前的重试次数；ttlSecondsAfterFinished-延迟清理已完成的作业。

从Kubernetes 1.4版本开始，我们还可以使用CronJobs在计划的时间/日期执行Jobs，其中每个执行周期大约创建一次新的Job对象。CronJob配置选项包括startingDeadlineSeconds-设置错过预定时间时启动作业的截止时间；concurrencyPolicy-允许或禁止并发作业或用新作业替换旧作业。

