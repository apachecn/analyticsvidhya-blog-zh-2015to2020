# 初学者的量子机器学习

> 原文：<https://medium.com/analytics-vidhya/quantum-machine-learning-for-beginners-725cbd1cbb9e?source=collection_archive---------22----------------------->

在这个算法日益复杂的时代，考虑到我们有限的时间限制，用于分析和评估数据的经典方法已经开始变得低效和越来越不可行。我们目前在机器学习领域也遇到了这样的问题，特别是在子领域，即深度学习。

这篇短文旨在介绍理解量子机器学习的必要概念。

![](img/c1f9df16c5abd1a83c4067875992a3a9.png)

术语“**量子**”具体指的是什么？这个流行词有一个共同的解释，它涉及到一个叫做**量子力学**的古怪科学，处理微观世界的行为。但是当然，这要比那复杂得多。当我们将术语“量子”与“机器学习”结合在一起时，我们基本上是指使用来自“古怪”科学的概念来进行人工智能，我们都知道这是一个流行词。

学术界的量子力学以在数据中生成**反直觉模式**而闻名，这本来对建模很有帮助，但我们根据我们的问题陈述选择性地使用来自该领域的概念。

# 那么什么是基于量子的算法呢？

> 量子算法是一组利用量子计算机解决问题的指令(量子计算机反过来在其实现中利用量子力学概念，如纠缠和叠加)。

![](img/a367c854390fb6f27ba1b8ddb5edfca4.png)

循环加工

量子机器学习软件通常利用这些算法作为其整体实现的子集。这些量子算法有可能在诸如**数据检索、操作、处理和线性代数**(广泛用于通用 ML 算法)等任务中胜过经典算法。这种潜力被称为**量子加速**。这种潜力通常使用经验实验而不是形式主义来评估

> 例如，量子计算机可以在与 N 的平方根成正比的时间内搜索一个有 N 个条目的未排序数据库，而经典计算机可以在与 N 成正比的时间内完成同样的任务

我们经常看到算法的量子对应物确实比经典对应物在指数上更好。当我们将一系列像这样的算法打包到机器学习实现中时，我们会看到效率的巨大提升，并且我们能够在建模数据的同时更加复杂地工作。

![](img/698f5cf61a6ce453d7805d0ad97a5503.png)

使用主成分分析的特征约简

大体上说，我们在嵌入机器学习算法的线性代数运算中看到了这种形式的效率优势。一个这样的例子是经典的 **PCA 降维算法**。它通常用于生成低维模型，以获得更好的计算可行性。巧合的是，这是一个非常元的例子，因为我们在这里使用量子实现来加速简化模型的过程，从而加速建模本身。

![](img/fd1cd67e978c0854bc367cf8bbdb4da7.png)

另一个著名的例子是 **KNN 算法**，其特点是计算距离度量，例如数据点的欧几里德距离和内积。与 KNN 算法的经典模拟相比，最好的情况显示了复杂度的指数/超指数降低，而最差的情况仍然显示了复杂度的多项式降低。

总之，我们能够收集到量子机器学习在计算复杂性降低和可行性方面有一个充满希望的未来，这对机器学习实践者和研究人员来说都是一个巨大的福音。这不仅是机器学习的一大步，也是整个计算机科学的一大步。