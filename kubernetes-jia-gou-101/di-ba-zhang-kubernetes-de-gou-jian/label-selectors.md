# Label Selectors

控制器使用[Label Selectors（标签选择器）](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors)选择对象的子集。Kubernetes支持两种选择器：

* **Equality-Based Selectors（基于平等的选择器）** 基于平等的选择器允许根据标签键和值过滤对象。使用=，==（等于，可互换使用）或！=（不等于）运算符实现匹配。例如，使用env == dev或env = dev，我们选择env Label键设置为dev值的对象。
* **Set-Based Selectors（基于集合的选择器）**  
  Set-Based Selectors allow filtering of objects based on a set of values. We can use **in**, **notin** operators for Label values, and **exist/does not exist** operator for Label keys. For example, with **env in \(dev,qa\)** we are selecting objects where the **env** Label is set to either **dev** or **qa**; with **!app** we select objects with no Label key **app**.

  基于集合的选择器允许基于一组值过滤对象。我们可以对标签值使用in，notin运算符，对Label键使用exist/does not exist运算符。例如，在（dev，qa）中使用env时，我们将选择env Label设置为dev或qa的对象；使用！app，我们选择没有Label key应用的对象。

![Selectors](../../.gitbook/assets/image%20%2846%29.png)

