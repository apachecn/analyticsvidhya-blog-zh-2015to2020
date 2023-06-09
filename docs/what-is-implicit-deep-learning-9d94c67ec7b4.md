# 什么是内隐深度学习？

> 原文：<https://medium.com/analytics-vidhya/what-is-implicit-deep-learning-9d94c67ec7b4?source=collection_archive---------3----------------------->

劳伦特·埃尔·加维，加州大学伯克利分校( [BAIR](https://bair.berkeley.edu/) )和 [sumup.ai](https://www.sumup.ai/)

与方达杜，伯特特拉瓦卡，阿明阿斯卡里，艾丽西娅蔡，加州大学伯克利分校。

2019 年 8 月 28 日

在机器学习的世界里，神经网络和相关的深度学习模型正迅速占据主导地位，每天都有大量的工作被发表，往往展示出非常好的实证结果。在这一点上，公平地说，我们对这种模型的理论理解是非常有限的，特别是当涉及到诸如健壮性、架构学习、为什么这种“过度参数化”的模型有效等问题时。这个博客是一个更广泛的研究语料库的一部分，该语料库专注于神经网络的理论结构，旨在为“深度学习的科学”奠定基础。

深度学习中的预测规则是基于通过几层的正向递归计算。隐式规则远远不止于此，它依赖于隐式(或“定点”)方程的解，该方程必须用数值方法求解，以便进行预测。具体地，对于给定的输入向量 *u* ，预测向量 *y* 的形式为

*y*=**c***x*+**d***u*，其中*x*= ϕ(**a***x*+**b***u*。

这里， **A** 、 **B** 、 **C** 、 **D** 是包含模型权重的矩阵；ϕ是一个给定的(非线性)激活函数，如 relu 最后，所谓的“状态” *x* 是一个 *n* 向量，它存储了模型的所有隐藏特征，不是显式表达的——它是通过“定点”(或平衡)方程*x*= ϕ(**a***x*+**b***u*隐式定义的*。*

A t 乍一看，上面的型号类型似乎很具体。也许令人惊讶的是，它作为特例包括了大多数已知的神经网络结构，包括标准前馈网络、CNN、RNNs 等等。我们可以用激活函数ϕ的适当定义，并通过在模型矩阵中施加适当的线性结构来指定这样的体系结构。例如，将矩阵 **A** 限制为严格上块对角对应于前馈网络的类别。进一步指定每个块中的结构，例如沿对角线的相等元素，允许对卷积层进行编码。

![](img/5baafcc59bfe2e444878c3c5569e2c71.png)

LeNet-5 网络隐式表示的模型矩阵 **A** 。

左图展示了 LeNet-5 五层图像分类网络的模型矩阵 **A** 的结构。该矩阵具有严格的上块对角线结构，每个块的大小对应于 5 层中每一层的尺寸。卷积层显示为“对角线条纹”，反映了矩阵的这些部分沿对角线不变的事实。

根据给定维度的隐藏特征的参数数量，隐式模型比标准网络具有更强的表达能力。隐性规则的承诺是:想象一下，如果我们可以让网络的架构从数据中浮现出来，而不是由人来决定？事实上，上面的矩阵显示了一大片零，等待被填满…

最近关于隐含规则的工作已经展示了它们的潜力。Kolter 及其合作者[1，5]展示了他们的隐式框架的成功，称为“深度平衡模型”，用于序列建模的任务。陈*等*【2】用隐式方法构造了一类相关的模型，称为神经常微分方程。张及其合作者[6]已经证明了一种隐式规则在 RNNs 环境中的有效性，而[7]则证明了该方法如何帮助解决在训练 RNNs 中的一些长期存在的问题。在早期工作[4]的基础上，论文[3]为内隐学习提供了一些理论和算法基础。

隐式规则中的棘手问题之一是*适定性*和*数值易处理性*:我们如何保证存在唯一解 *x* ，如果存在，我们如何高效地计算它？在标准网络中，这个问题不存在，因为由于递归消除，即通过层的向前传递，人们总是可以以显式形式表达隐藏的状态变量。如[3]所示，矩阵**A**上有简单的条件保证适定性和易处理性，例如，矩阵*A*中元素绝对值的最大行和不超过矩阵*1*，在这种情况下，递归

*x(t+1)*= ϕ(**a***x(t)*+**b***u*，t=0，1，2，…

很快收敛到唯一解。最大的行和约束倾向于鼓励 **A** 的稀疏性，这反过来带来了许多好处:测试时的加速、架构简化和减少内存需求。

隐式学习的训练问题可以通过深度学习社区中流行的标准无约束优化方法来解决，例如随机梯度下降(SGD)。然而，在定点方程中计算梯度是具有挑战性的。此外，SGD 本身并不保证预测规则的适定性；正确处理相应的约束需要*约束*优化，例如块坐标下降(BCD)方法，这种方法往往收敛非常快【4】。

BCD 方法的一个好的方面是它们处理有趣的约束或惩罚的能力。在上面的隐式规则中，对输入矩阵的约束形式

![](img/99adb9a27a7b9c95a885877076c93be4.png)

利用κ小的正超参数，将促使 *B* 成为“列稀疏的”，即 *B* 的整列为零；反过来，得到的模型将选择重要的输入，并丢弃其他输入，通过深度学习有效地完成特征选择。

![](img/7f1233027c476cf4dd723f47c21210f8.png)

我们使用具有 *20* 隐藏特征、 *50* 输入和 *100* 输出以及列稀疏矩阵 **B** 的给定隐式模型，生成了一个由 *400* 个点组成的合成数据集。使用对 *10* 隐藏特征的不正确猜测的训练模型，BCD 算法仍然恢复生成模型的正确稀疏模式，这由学习矩阵(左列)和生成模型(右列)矩阵的列范数的向量匹配的事实证明。

隐式模型是新的，需要更多的工作来评估它们的真正潜力。它们可以被认为是“类固醇上的神经网络”，因为它们允许更大的参数模型。隐式模型还有许多其他潜在的好处，包括健壮性、可解释性和架构学习方面的一些令人兴奋的发展。稍后将详细介绍这一点！

**参考文献**

1.  Bai，s .，Kolter，J. Z .，和 Koltun，V. (2019 年)。深度均衡模型。预印本已提交。
2.  Chen，T. Q .，Rubanova，y .，Bettencourt，j .，和 Duvenaud，D. K. (2018)。神经常微分方程。在 NeurIPS 2018，第 6571–6583 页。
3.  El Ghaoui，l .，Gu，f .，Travacca，b .，和 Askari，A. (2019)。深度内隐学习。arxiv.org/abs/1908.06315[预印本](https://arxiv.org/abs/1908.06315)。
4.  Gu，f .，Askari，a .，和 El Ghaoui，L. (2018)。芬切尔提升网络:神经网络训练的拉格朗日松弛法。预印本 [arXiv:1811.08039](https://arxiv.org/abs/1811.08039) 。
5.  j . z . kolter(2019)。深度平衡模型:你只需要一个(隐含的)层。【2019 年 8 月在西蒙斯研究所的演讲。
6.  张，z .，卡格，a .，沙利文，a .，萨利格拉玛，V. (2019)。平衡递归神经网络:神经元延时自反馈提高准确性和稳定性。预印本[https://arxiv.org/pdf/1903.00755.pdf](https://arxiv.org/pdf/1903.00755.pdf)
7.  Kag，a .，Zhang，z .，Saligrama，V. (2019)。在平衡中发展的 RNNs:消失和爆炸梯度的解决方案。预印本[https://arxiv.org/abs/1908.08574](https://arxiv.org/abs/1908.08574)