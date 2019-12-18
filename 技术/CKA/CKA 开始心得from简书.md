# 简书上的CKA考试心得

<!-- 版权信息在最后 -->

上周末通过了 CKA 考试，Kubernetes 在国内的热度越来越高，相信以后会有更多人对 Kubernetes 的官方认证考试产生兴趣，所以记录一下这次备考过程中有参考价值的细节，希望能对后来者有所帮助。

先简单介绍一下 CKA 吧，全称是 Certificated Kubernetes Administrator，也就是官方认证的 Kubernetes 管理员，由 Kubernetes 的管理机构 CNCF 授权。对于想做 Kubernetes 运维类工作的朋友，拿到 CKA 应该算是除了相关工作经验外，最有力的能力背书了。对于想做 Kubernetes 开发类工作的朋友，虽然不直接相关，但也是一个很好的入门方式。

## 1. 报名
### 1.1 形式

首先要说明的是 CKA 报名仅仅包含的是考试的费用，培训并不在其中，需要自行备考，虽然 CNCF 有对应的 CKA 备考培训，但是要单独收费。

CKA 的报名地址是：https://www.cncf.io/certification/cka/
培训的报名地址是：https://www.cncf.io/certification/training/

### 1.2 费用和优惠

接下来说说考试的费用，正常价格是 300 美元，折算过来差不多是 2000 人民币，配套的在线培训课程（Kubernetes Fundamental）价格是 299 美元，价格不算便宜。但 Linux Foundation 和 CNCF 的认证和培训也是会打折的，我在报名时，赶上了黑色星期五的大促，179 美元包含了 CKA 考试和 Kubernetes Fundamental 课程，原价是 599 美元，还是挺划算的，建议准备报考的朋友多多留意。

即使没有赶上大促，也还是有办法拿点小优惠的，下面这个链接提供的是九折优惠：Linux Foundation coupon

### 1.3 中文名字的麻烦

国内报考 CKA 有一点需要特别注意，CKA 的考试机构要求注册的用户姓名必须是拉丁字母，而且必须和 ID 上的一致（可以理解，不然怎么知道是一个人），中文显然不满足。如果有护照，那就方便了，直接可以用，没有的话，就要想办法做公证，我就是到当地的公证处做的身份证公证。

就我的经历来看，申请公证很难一次通过，可能需要补好几次材料，再加上等待时间，差不多要两周，所以最好提前准备好，以免扰乱备考计划。

## 2. 备考
### 2.1 备考教程

我想大家一定对到底如何准备 CKA 考试非常感兴趣：比如应不应该报名 Linux Fundamental？应该看什么资料？考试范围是什么？我就谈谈自己的心得。

先说说我学过的几门备考课程吧，因为黑五的优惠，所以报名了官方的备考课程 Kubernetes Fundamental；之前购买的 Linux Academy 会员，里面正好有 [CKA 的备考课程](https://linuxacademy.com/cp/coursescheduler/view/id/208836)，以及对应 Kelsey Hightower 在 github 上的 [Kubernetes the hard way 教程](https://linuxacademy.com/cp/coursescheduler/view/id/208836)。

#### 2.1.1 Linux Foundation
首先 Linux Fundamental 虽然是官方推荐的配套教程，但这个教程的内容并不是专门为备考准备的，如果只是为了备考而购买，大概率是要失望的。
这个教程的内容就像名字一样是 Kubernetes 的基础教程，涵盖的内容非常广泛，很大一部分知识是根本没法在短时间内的考试里进行考察的，而且有相当一部分的考试的细节在教程中也是没有体现的，还有一点对我来说是不够贴心的，那就是教程不附带实验环境，需要自己去单独购买服务器部署 Kubernetes 环境。
我的体会是，作为 Kubernetes 入门，这个课程相当不错，不过不适合备考，对有实战经验的 Kubernetes 工程师的价值也不是很大。

#### 2.1.2 Linux Academy's CKA training
再者是 Linux Academy 的 [CKA 的备考课程](https://linuxacademy.com/cp/coursescheduler/view/id/208836)，这个相对来说，针对性还是很强的，很多内容是直接在考试中可以用到的，但如果只是掌握里面的内容，恐怕还是难以保证考试通过。
我猜测这是因为 Linux Academy 作为第三方的培训机构，课程内容是会受到限制，毕竟 CNCF 是不希望给外界这么个印象，只要上了培训课，就一定可以拿到 CKA，这样的话，含金量就显得太低了。

#### 2.1.3 Linux Academy's Kubernetes the hard way training

Kubernetes the hard way 的教程: https://linuxacademy.com/cp/coursescheduler/view/id/208836 Kubernetes the hard way 是 Kubernetes 的经典教程，对理解 Kubernetes 的工作原理有很大价值，但如果只是熟练操作，还是不够，因为考试还会涉及到更深入的细节。

#### 2.1.4 备考建议

总结下来，现在市面是没有针对性很强的备考教程的，多半是 CNCF 有意造成的局面，这是好事，如果太容易通过，那认证就是只是花钱买张纸了。所以大家要多积累 Kubernetes 在工作中的实战经验，同时多读官方文档，这是最重要的学习资料，细节的翔实程度远超教程，而且这也是考试时唯一允许查阅的参考资料。

我的备考建议是，如果基础较为薄弱，可以考虑报名一个备考教程，系统的学习一下，如果已经有了一定基础，就不必要了，可以参考 CKA 的考试大纲来自行对照一下，对知识点进行查缺补漏。这里推荐给大家一个 git repo：[Kubernetes-Certified-Administrator](https://github.com/walidshaari/Kubernetes-Certified-Administrator)，作者将考试大纲对应的知识点，和有价值的参考资料汇总到了一起，可以节省不少时间。

最后也是最重要的，就是大量练习了，kubectl 命令必须足够熟悉，因为考试时间有限，必须了解如何用命令行创建诸如 deploy, service 等资源，不然一行行写 yaml 恐怕时间是来不及的。

### 2.2 练习环境

要做练习，就需要有环境，如果自己的电脑足够强，那当然最好，如果没有，那就需要用到云环境了，云环境我比较推荐 GCP，主要原因是对新用户用优惠，注册时花一美元，送 300 美元的体验金，这足够折腾一阵子了。

## 3. 考试

### 3.1 语言

考试是允许提前十五分钟进入考试界面的，而且考试开始前，需要做例行的检查，这部分会消耗一定的时间，而且会计入考试时间，主要是检查环境是否符合考试要求，所以建议尽量利用好这考前的十五分钟，而且不要迟到，迟到超过十五分钟，就失去了考试资格。
另外考试的操作环境是在浏览器窗口里，所以很多操作和平时是不太一样的，尤其是复制和粘贴，需要花时间适应。

### 3.2 答题记录

还有一点需要注意，考试时是没法检查哪些题已经做完的，如果跳过了一些题目，非常有必要在记事本（考试环境里提供的记事本功能，考试不允许使用电脑中的其他程序）里记录一下，以免漏答。

---
</br>作者：blackpiglet
</br>链接：https://www.jianshu.com/p/629525af31c4
</br>来源：简书
</br>著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
