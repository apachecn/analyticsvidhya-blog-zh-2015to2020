# 监督学习算法

> 原文：<https://medium.com/analytics-vidhya/supervised-learning-algorithms-ad934e0b1834?source=collection_archive---------21----------------------->

![](img/03fd8ff7cd02a0ecacb8f6066eb0899f.png)

嗯嗯……算法哈！！！

正如我在上一篇文章中承诺的，我将在下一篇文章中写算法。

我是你的伙伴。

算法是构建机器学习模型的核心，在这里，我提供了用于监督学习的大多数算法的详细信息，以让您直观地了解在哪里使用它，在哪里不使用它。

到本文结束时，你将从直觉的理解水平上熟练算法。

> 警告:我不是在描述它背后的数学，而是它是如何工作的以及在哪里使用它。

伙计们，我们开始吧。

# 1.朴素贝叶斯

朴素贝叶斯是基于贝叶斯定理的分类算法，是机器学习中已知最多的基础算法。

优势:

1.  这对于处理大量的数据集和对如此大的数据集进行精确的数据概括是非常有用的。
2.  主要应用于分类问题，如垃圾邮件检测、垃圾邮件过滤、情感分析、欺诈检测、推荐引擎等。

缺点:

1.  它是幼稚的，即不理解像文本学习那样的有序格式的数据。(由于其速度和易用性，它仍然是首选)。

![](img/3ea7ffb7656790109cd0fd313e4d0e6b.png)

股票价格预测

# **2。逻辑回归**

逻辑回归的名字听起来是回归算法，但实际上它是一种分类算法。这是一个线性的和最简单的分类算法。

优点:

1.  它简单易懂。
2.  它最适用于线性数据，即当我们试图预测的类不重叠且线性可分时。

缺点:

1.  当类是非线性的，它将失败。
2.  它不能处理复杂的问题。

# **3。线性回归**

线性回归也是线性模型，但用于回归问题。

优势:

1.  它也很简单，可解释，很难过分拟合。
2.  当输入和输出变量之间的关系是线性时，这是最好的。

缺点:

1.  当输入和输出之间的关系是非线性时，它会对数据进行欠拟合，即它不能准确地概括非线性数据。
2.  它也不能模拟复杂的关系。

![](img/5a194aba57f5d7e9be60ff66081c5da1.png)

# 4.k _ 最近邻居

这是一种能够有效地模拟非线性数据和线性数据的算法。它用于回归和分类问题。

优势:

1.  尽管简单且可解释，但它在学习更复杂的非线性关系时非常灵活且高效。
2.  用于推荐系统，如网飞、spotify 等。

缺点:

1.  当观察值和特征数量增加时，它不能很好地工作，即不能很好地对大型数据集进行概化。

# 5.支持向量机(SVM)

SVM 是高度灵活的算法，可以在数据集之间建立一条独立的数据线。它可用于回归和分类。

优势:

1.  它也可以处理复杂的数据集。
2.  它也适用于非线性数据。

缺点:

1.  容易产生噪音。
2.  不适用于大型数据集。

![](img/5d4d3f8792b6f029049423fb9b81ef39.png)

# 6.基于树的方法

基于树的方法是为解决极其复杂领域的问题而开发的最有效的算法。它适用于分类和回归问题。

有许多基于树的方法:

> **1。决策树 2。装袋 3。随机森林 4。升压(梯度升压、Ada 升压、XG 升压)。**

优势:

1.  这些方法最适合预测问题的监督学习。
2.  以娴熟的方式处理复杂的关系以及缺失数据和分类特征。

缺点:

1.  难以解释，并且可能需要很长时间来训练模型。

![](img/34c6a67f148cb770f2acc5478d4c26c3.png)

# 7.神经网络:

神经网络是最先进的技术，可以概括世界上最复杂的问题。这些算法属于深度学习，这是最复杂但最有效的模型，可以处理繁琐的问题，并获得我们问题的最佳指标。由于这些方法非常复杂，我们应该在接触神经网络之前，首先尝试使用上述简单的线性模型。

Hooo🥱…..文章终于结束了，但不是学习的过程。我从直观的角度提供了对这些用于机器学习的算法的基本理解，以便您能够使用 breeze 来理解它们。接下来就看你能不能更熟练地掌握这些话题了。

我猜你已经从这篇文章中得到了一些关于这些算法的概念。我拿着我的笔。哎呀，我把手从键盘上拿开了😂😂。

无论如何…

谢谢你。是的，开心点，别担心。一次迈一小步，你会在瞬间到达顶峰。