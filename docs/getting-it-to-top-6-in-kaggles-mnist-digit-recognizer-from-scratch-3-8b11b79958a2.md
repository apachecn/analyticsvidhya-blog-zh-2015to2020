# 让它在 Kaggle 的 MNIST 数字识别器中从零开始达到 6 %- 3。

> 原文：<https://medium.com/analytics-vidhya/getting-it-to-top-6-in-kaggles-mnist-digit-recognizer-from-scratch-3-8b11b79958a2?source=collection_archive---------24----------------------->

## 让我们的 CNN 更强大

![](img/f0c74a0b0c83a511564cdc3c7a35c99a.png)

机器学习:吴恩达·库塞拉

**3.1 批量标准化**

在第 1 部分中，我们通过在 0 到 1 之间重新调整图像的像素值来规范化我们的输入层。以类似的方式，我们甚至可以使用批处理规范化来规范化提供给隐藏层的输入。批量标准化有几个优点，但最有希望的是需要较少的历元数，隐藏层的学习更加相互独立，并且具有较小的协方差偏移，即它防止模型过拟合。我们可以直接在我们的模型中包括批量标准化层，使用:

```
model.add(BatchNormalization())
```

更多关于批量规格化 [*这里*](https://towardsdatascience.com/batch-normalization-in-neural-networks-1ac91516821c) 。

**3.2 辍学——调整我们的深层神经网络**

在神经网络的训练阶段，隐藏层中的一些节点根据其神经元值被随机丢弃(我们需要指定范围在 0 到 1 之间的丢弃率)。这允许网络在每次运行时在结构上不同，从而防止过度拟合。你可以从 0.4 的辍学率开始，然后继续做一点试验，看看它在哪里找到一个最佳点。我通常倾向于 0.4-0.6 之间。我们可以如下实现丢弃层:

```
model.add(Dropout(rate))
```

更多关于辍学 [*这里*](https://www.cs.toronto.edu/~hinton/absps/JMLRdropout.pdf) 。

**3.3 随着培训的进行，学习率下降**

我们可以在一组固定的历元之后降低学习率。这一点很重要，因为当我们在第 1 部分和第 2 部分中训练模型时，准确性不断波动，也就是说，随着学习的进展，准确性开始下降而不是上升。因此，有必要降低学习率，以防止其超过最优值。我们可以实现自己的学习率调度程序，如下所示:

```
from keras.callbacks import *LearningRateScheduler(lambda x: droppingrate**x)
```

**3.4 数据扩充**

深网总是渴求数据，你提供给网络的数据越多，它就越准确。数据扩充是任何机器学习模型中最关键的一步，尤其是如果我们正在构建复杂的神经网络。随着训练参数数量的增加，相应地需要更多的数据。TensorFlow 提供了一个实时扩充数据的选项，可实施为:

让我们用以上 4 个步骤来构建我们的 CNN:

该模型在 Kaggle 上的得分为 0.99728，排名在 150 以下(前 7%)。

> 在实现任何神经网络时，这 4 个步骤是至关重要的，并且肯定能使你的 CNN 模型更加健壮。

**3.5 组装不同型号:**

运行该模型 5 次，您将看到每次运行的训练和测试分数都不同。因此，不能保证相同的模型每次运行都能达到 0.99728 的准确度。因此，最好是:

a.运行模型几次迭代

b.尝试不同的模型，通过试验一些隐藏层、内核大小、池层、学习率、辍学率、密集层中的神经元等。选择具有较好验证分数的模型并集成它们。

保持模板不变，仅通过将卷积层中的核大小从 3 改变为(2，4，5)并集合不同模型的输出，我能够得到 0.99771 的分数，排名为 120，即截至 2020 年 3 月 31 日的前 6%。