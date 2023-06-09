# 流行的监督学习算法的优缺点

> 原文：<https://medium.com/analytics-vidhya/pros-and-cons-of-popular-supervised-learning-algorithms-d5b3b75d9218?source=collection_archive---------1----------------------->

![](img/02647940d0074c46657a57fba0bdc0fe.png)

> 我们都曾使用以下监督学习算法之一进行预测分析:

1.  逻辑回归
2.  里脊回归
3.  套索回归
4.  线性判别分析(LDA)
5.  k 最近邻(KNN)
6.  朴素贝叶斯
7.  支持向量机(SVM)
8.  决策图表
9.  随机森林
10.  梯度推进

> 但是你想过它们的利弊吗？这里我列举了几个:

## **1。逻辑回归:**

优点:

a)当数据可线性分离时使用。

b)更易于实施、解释，培训效率也很高。

c)它给出了预测值在正向或负向的重要性的度量。

缺点:

a)它可能在高维数据集中过度拟合。

b)不支持预测值和结果之间的非线性关系。

## 2.岭回归:

优点:

a)防止在更高的尺寸上过度配合。

b)平衡偏差-方差权衡。有时，比零偏差更高的偏差比高方差和零偏差更适合。

缺点:

a)它增加了偏差。

b)我们需要选择最佳α(超参数)

c)模型可解释性低。

## 3.套索回归:

优点:

a)通过向零收缩系数来执行特征选择。

b)避免过度配合。

缺点:

a)选定的特征可能会有很大的偏差。

b)对于 n < < p(n-数据点数，p-要素数)，LASSO 最多选择 n 个要素。

c)对于不同的引导数据，选择的特征可以非常不同。

## 4.线性判别分析(LDA):

优点:

a)它是简单、快速和可移植的算法。当它的假设被满足时，它仍然胜过一些算法(逻辑回归)。

缺点:

a)需要对特征/预测值进行正态分布假设。

b)有时对少数类别变量不好。

## 5.k 最近邻(KNN)

优点:

a)这是仅用一个参数实现的最简单的算法。

b)用户可以插入任何距离度量，甚至是用户定义的距离度量。这允许处理复杂的对象，比如时间序列、图表、地理坐标，以及基本上任何可以定义距离度量的东西。

c)该算法可用于分类、排序、回归(使用邻居平均值或加权平均值)、推荐、缺失值插补等。

缺点:

a) KNN 是一个懒惰的学习者，因为它不从训练数据中学习模型权重或函数，而是“记忆”训练数据集。因此，推理比训练需要更长的时间。

b)这是一种基于距离的方法，因此模型可能会受到异常值的严重影响，换句话说，它容易过度拟合。

c)模型大小随着新数据的加入而增长。

d)该模型遭受维数灾难。

## 6.朴素贝叶斯

优点:

a)预测测试数据集的类别是容易且快速的。它在多类预测中也表现良好。

b)当独立性假设成立时，NB 分类器与其他模型(如逻辑回归)相比表现更好，并且您需要更少的训练数据。

缺点:

a)如果分类变量具有在训练数据集中未观察到的类别(在测试数据集中)，则模型将分配 0(零)概率，并且将无法进行预测。这就是通常所说的“零频率”。为了解决这个问题，我们可以使用平滑技术。最简单的平滑技术之一叫做拉普拉斯估计。

b)对特征的分布有一组强有力的假设，如正态、多项式等。

c)预测者也有独立性的假设。在现实生活中，我们几乎不可能得到完全独立的预测器。

## 7.支持向量机(SVM):

优点:

a)它工作得非常好，具有清晰的分离界限

b)在高维空间有效。

c)它在决策函数中使用训练点的子集(称为支持向量)，因此它也是内存高效的。

d)它在非线性模型拟合中也是有效的。

缺点:

a)当我们拥有大型数据集时，它的表现不佳，因为训练变得非常耗时。

b)当数据集具有更多噪声时，即目标类重叠时，它也不能很好地执行。

c) SVM 不直接输出概率。需要使用其他方法将 SVM 的输出转换为概率。

## 8.决策树:

优点:

a)易于理解和解释，非常适合视觉表现。

b)它需要很少的数据预处理，即不需要一次性编码、标准化等。

c)它是非参数模型。因此，不需要关于数据分布的假设。

d)特征选择自动发生。因此，不重要的特征不会影响结果。

缺点:

a)它倾向于过度拟合。

b)非常敏感。数据的微小变化会极大地影响预测(高方差)。

## 9.随机森林(RF):

优点:

a)它对异常值是稳健的。它降低了过度拟合的风险。

b)它也适用于非线性数据。

c)它在大型数据集上高效运行。

d)通常它比其他分类算法给出更好的准确性。

缺点:

a)发现随机森林在处理分类变量时有偏差。

b)慢训练。

c)它不适合具有大量稀疏特征的线性方法。

## 10.梯度提升:

优点:

a) Boosting 附带一个易于阅读和解释的算法，使其预测解释易于处理。

b)增压是一种弹性方法，可轻松抑制过度拟合。

缺点:

a)它对异常值敏感，因为每个分类器都必须修正先前分类器中的错误。因此，该方法过于依赖异常值。

b)提升几乎不可能扩大规模。这是因为每一个估计量的正确性都是建立在先前预测量的基础上的，因此使得这个过程很难简化。

> 我希望你喜欢这篇文章。请鼓掌，写下你的评论，并跟随我获取更多与数据科学相关的文章。
> 
> T 渴望阅读，再见！