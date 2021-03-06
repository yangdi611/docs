# 重构

现在，越来越多的现代企业有能力、知识和技术来构建支持其业务的云原生应用程序。

不幸的是，对于运行在旧式整体应用程序上的老牌企业而言并非如此。一些人试图将整体作为微服务来运行，并且正如人们所期望的那样，它并不能很好地工作。这其中的教训是，单片大小的多进程应用程序无法作为微服务运行，因此必须探索其他选择。从整体到微服务过渡的下一个自然步骤是重构。但是，通过重构将已有数十年历史的应用程序迁移到云中会带来严峻的挑战，并且企业将面临重构方法的困境：“大爆炸式”方法或增量式重构。

所谓的“大爆炸”方法将所有工作都集中在重构整体上，从而推迟了任何新功能的开发和实施-实质上延迟了进度，甚至可能在此过程中甚至破坏了业务的核心-整体。

增量重构方法可确保将新功能开发和实现为现代微服务，这些微服务能够通过API与整体通信，而无需附加整体代码。同时，功能从整体中重构出来，整体逐渐消失，而其全部或大部分功能都已现代化为微服务。这种渐进的方法可从传统的单片逐渐过渡到现代的微服务架构，并允许将应用程序功能分阶段迁移到云中。

一旦企业选择了重构路径，在此过程中还需要考虑其他因素。要将哪些业务组件从整体中分离出来以成为分布式微服务，如何将数据库与应用程序分离以将数据复杂性与应用程序逻辑分离，以及如何测试新的微服务及其依赖性，这是企业在决定重构时需要面临的问题。  

