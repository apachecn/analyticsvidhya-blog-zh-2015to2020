# 用 fast.ai 部署机器学习模型

> 原文：<https://medium.com/analytics-vidhya/machine-learning-model-deployment-with-fast-ai-658cfde878d0?source=collection_archive---------29----------------------->

在新冠肺炎颠覆整个世界之前，我正急切地开发我的第一个神经网络项目——一个可以将 APPL 股票数据作为特征并预测短期价格波动的模型。这个想法很简单；低买高卖。

在查看了我的测试数据集结果后，我兴奋地意识到这个模型*可以*工作。然而，我的热情是短暂的，因为我是一名化学工程专业的大四学生，从大学一年级开始就没有练习过编程。为了利用这一热门的新技术，我会全力以赴。

命运偏爱勇敢的人，我立即开始咨询我的网络，尽可能多地学习如何将我电脑上用 Python 编写的这个新颖程序变成更多的东西。幸运的是，我的室友和多年的朋友是软件工程专业的，我(天真地)认为他可以回答我所有的问题，尤其是我脑海中最突出的一个问题:**我如何将在我的 Python IDE 中开发的模型转化为可以在“真实”世界中使用的软件？**

我记不起我收到的确切答案，可能是因为太多的术语掠过我的脑海。对此我毫不动摇，在过去的一年里，我花了很多空闲时间自学如何回答这个问题，以及更多与利用这项技术相关的问题。

# 二进制序列化

在深入研究二进制序列化之前，最好先理解二进制数。简单来说，二进制往往是指二进制数字系统。在数学和计算机中，二进制数是以 2 为基数的数字系统。你可能最习惯的“普通”数字是以十为基数的数字系统。下面，我附上了一个我最喜欢的在线老师的视频，他能比我想象的更好地解释这一切。

现在，二进制序列化构成的是一种将“对象”(如经过训练的机器学习模型)转换为二进制信息的方法。就像你如何把数字 7 转换成二进制表示，反之亦然，你可以把一个对象，比如一个计算机程序，转换成二进制格式。

这一切有什么关系呢？你的模型可以想象成一个非常复杂的计算机程序。通过二进制序列化，您可以将模型的状态“保存”到一个由 1 和 0 组成的文件中，并在需要时将其反序列化回同一个复杂模型。

![](img/12356dbf6dd79ce17f668e8a0cbb7032.png)

进一步来说，假设您已经编写了一个简单的程序，如 hello_world.py 文件，其中包含以下代码行:

print('Hello world！')

现在想象一下，通过执行二进制序列化，您最终会得到以下二进制链:

101010010101010001010010101001010101001010010101010000101001

这些 1 和 0 对您来说没有任何意义，但是计算机可以将它们转换回 hello_world.py 文件，当调用该文件时，它将打印“Hello world！”。

假设不是序列化打印出字符串的程序，而是运行一个非常简单的线性回归模型:

斜率= 1.5
y _ intercept = 4
y _ pred =输入值*斜率+ y_intercept
print(y_pred)

为了运行这个模型，需要对应于 x 的 input_value，很像神经网络的输入向量。在重新序列化时，可以给这个程序任意的输入值，它会做出预测。

神经网络模型是程序中的对象，而不是程序本身。无论如何，这不是一个挑战，因为当使用正确的代码时，序列化一个模型是毫不费力的。

# 用 fast.ai 库部署

下面，我将说明使用 Python 中的 fast.ai 库来执行神经网络模型对象的序列化是多么容易。

由于这不是一个关于创建神经网络模型的教程，我将跳过使用 fast.ai 中的功能模型的开发。下面是一个名为“learn”的模型的性能分析。这是一个 CNN 模型，对熊的图像进行分类。

![](img/ae1dcd5a934c4456b2e05bcc331dccb2.png)

从这一点来看，序列化模型所需要的只是一行简单的代码:

learn.export()

的。export()方法创建一个名为“export . pkl”(pkl 读作“pickle”)的序列化文件。

这是您希望部署应用程序的任何地方都需要的文件。为了利用该模型，文件被反序列化回一个对象，并给定输入。

![](img/060faef678352aa65de18631a2b86af2.png)

从一个非常基本的角度来看，这就是全部！现在，将这种模型投入生产是一个非常不同的故事，需要一个服务器来托管模型和一个数据管道来伪实时地提供输入数据。