# 什么是机器学习，如何开始学习

> 原文：<https://medium.com/analytics-vidhya/what-is-machine-learning-and-how-to-get-started-with-it-9c205d0fd3a9?source=collection_archive---------6----------------------->

## 机器如何学习，从哪里学习，学习什么——这只是一种预测的艺术还是其他什么？

![](img/ec2e678578faa247deb0cc6dfe4dc36b.png)

好的，正如标题所暗示的，我们将要谈论机器学习和所有的数据资料，对吗？但在我们开始之前，先介绍一下我自己。

我是 Shivam，一名来自无名大学的本科生，也是一名活跃的数据科学参与者，在过去的两年里，我一直在自学机器学习。在这篇博文中，我将解释一个试图进入数据科学世界的新手的旅程中的所有起起落落。

> *所以，首先这个博客的这一部分都是关于* **“什么是机器学习？数据科学和机器学习的主要区别是什么？背后的主要理念是什么？”**
> 
> 第二部分是关于**“我应该选择哪门课程？我应该读哪本书？如何脱颖而出？我需要哪些技能？”**

# **什么是机器学习？**

顾名思义，机器就是在学习。现在它从哪里学习呢？这是从过去中学习。

> 在 1997 年， [Tom Mitchell](http://www.cs.cmu.edu/~tom/) 给出了一个“适定的”定义，这个定义已经被证明对工程类型更有用:**“如果一个计算机程序在 T 上的性能，如 P 所测量的，随着经验 E 的增加而提高，那么这个计算机程序可以说是从经验 E 中学习了一些任务 T 和一些性能测量 P。”**

它从过去的数据中学习，然后预测未来。它永远无法 100%准确地预测任何事情，因为那就像“它知道未来”一样，这是不可能的。记住它只能预测**“只要未来和过去没有太大不同”**。

(注:有很多人混淆了**数据科学**和**机器学习**所以这里有答案。数据科学属于商业决策，用于从数据中提取价值，而作为数据科学家的你必须以某种方式增加特定公司的利润——首席执行官或产品经理不会担心你使用的是什么算法，是简单的逻辑回归还是复杂的神经网络。他只想知道结果。ML 是人工智能的一个子集，用于数据科学。记住 ML 是一种技术，数据科学是一个领域。大多数非技术人员称之为 **AI**

我将用一些例子来通俗地解释它。

在传统编程(If-Else)中，我们有一个问题、一个规则/条件和一个答案。这里的问题是**“输入(x)是奇数还是偶数”**。而规则/条件是**“x % 2 = = 0”**。

现在要明白，我们将问题(输入)输入到我们的规则中，它会给我们答案/输出。然而，当我们拥有大量数据时，这是不可能的，这些数据更加复杂和非结构化，问题也更加复杂。

所以我们颠倒了这个过程，它变成了机器学习

规则/条件在这里被称为**模型**，在这个模型中，我们将机器学习中被称为**标签/输出**的答案与问题一起输入，模型将为我们提供也被称为**模式**的规则/条件(这就是为什么有时它被称为模式识别)。现在，该模式用于新的看不见的数据(**测试数据**)。

当在模型中输入我们的标记数据(带答案的问题)时，称为**训练。**在我们的模型符合数据后，我们用看不见的数据测试我们的模型，这被称为**测试。现在，你的下一个问题应该是…**

我们如何将数据输入模型？数据可以是任何东西—图像、语音剪辑、视频、IOT 设备发出的任何信号、表格数据等。不要担心，我们很快就会谈到这一点。

只要记住一件事，机器只能理解数字，不管你有什么样的数据。你总是要以某种方式把它转换成数字数据，这样你就可以把这些数字输入机器。例如，对于人类来说，图像是非常简单的分类对象，但是对于机器来说呢？他们怎么知道图像里是狗还是猫？想过吗？

图像只不过是代表一些值的一些数字的多维数组。对于灰度图像，每个像素具有从 0(全白)到 1(全黑)的值，并且所有的灰度都在 0-1 之间。对于 28×28 的灰度图像长度将是 784。所以这里每个像素的值就是我们的**特征**(我们提供从数据集提取的特征，而不是数据本身)。

你可以以面部识别为例，肤色、嘴唇的宽度、鼻子的长度等都可以作为特征。因此，每当机器看到具有这些特征的人时，它就能识别出这是它被训练过的面孔。

# 机器学习的类型

主要有 4 种类型，但经典的机器学习围绕 2 种类型进行

1.  **监督学习**:在监督学习中我们有 ***标注的数据*** (带答案的数据)所以我们只要从中提取出***模式*** 并用 ***测试数据*** (仅限问题)进行预测。假设我们有一桶不同大小的球(篮球、网球、乒乓球)，这里球的大小(球的半径)是我们可以用来训练模型的 ***特征*** 。现在，当我们用一个小球测试我们的模型时，它可以很容易地检测到球的类别，因为它是在球上训练的——它已经知道答案，因为我们已经提供了知识。
2.  **无监督学习**:在无监督学习中，我们用未分类/ ***标记为*** 的信息训练机器。在这里，我们没有答案，只有问题。机器本身在相似性、模式和差异的基础上对数据(问题)进行*聚类，而无需对带有标签/答案的数据进行任何预先训练*。假设我们的数据中有橙子和苹果的图像，而机器不知道数据中的对象是什么，因此它将自己进行聚类(具有相似属性的对象组)，然后对其进行标记。你可以说在这种学习中，机器创造了自己的 ***知识*** 。

(注意:ML 是统计学、概率定理和多元微积分的钻石，所以了解数学是有好处的。一个人可以在没有数学的情况下进行机器学习，就像一个人可以在不知道引擎内部机制的情况下驾驶汽车，但这没有任何乐趣。数学比实际的 ML 酷多了。过去所有的统计数据都在那里，但 ML 和 AI 仍然没有那么受欢迎，因为我们当时没有那么多数据。这个星球上 90%的数据是在过去 3-4 年中产生的。这是机器学习变得流行的主要原因。)

# **如何入门机器学习**

未完待续……