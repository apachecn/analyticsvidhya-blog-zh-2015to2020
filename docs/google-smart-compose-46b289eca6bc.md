# 谷歌智能写作

> 原文：<https://medium.com/analytics-vidhya/google-smart-compose-46b289eca6bc?source=collection_archive---------13----------------------->

![](img/c5d2b4ba967aab89a630a36485643385.png)

**什么:** Google Smart Composer 是一个生成交互式单词建议的机器学习模型。给定你要写给谁的电子邮件、电子邮件的主题、以前的电子邮件内容(如果是回复的电子邮件)以及电子邮件内容的一小部分，然后模型预测下一个单词

**为什么:**只是为了安慰用户，通过预测下一个最可能的单词，让用户最终输入很少的单词。

**如何:**使用 seq2seq 基于注意力的编解码模型。

**理解本文的要求:**本文的灵魂假设读者熟悉 python，深度学习概念像 [RNN 的](https://colah.github.io/posts/2015-08-Understanding-LSTMs/)和它的聪明弟弟 [LSTM](https://colah.github.io/posts/2015-08-Understanding-LSTMs/) 。

如果你知道如何[从 Gmail](/@yernagulahemanth/download-emails-from-gmail-using-python-31e9bc62e501) 下载你的电子邮件，这是额外的优势(非强制性)

如果你熟悉闪回(RNN 和 LSTM 的概念😜)可以开始看这部电影了。

你可以选择你自己的电子邮件数据，但我选择了我自己的个人电子邮件。如果你想选择个人邮件以外的信息，我建议你下载安然数据集，但是相信我，清理这些数据是非常困难的，无论你清理多少，数据中都会有一些噪音，因为每封邮件的格式都不一样

正如我之前说过的，我下载我自己的个人电子邮件，把它们存储到文本文件中，并从文本文件中提取有用的数据。

下载并保存我的电子邮件到一个文本文件后，这个文本文件看起来如图 1 所示。

![](img/f53983e28da0498d6090a91ec6af8d6c.png)

图 1

如果我们观察文本文件中的“收件人”、“发件人”、“主题”和“类型”(数据类型)数据，那么电子邮件的内容在哪里？如果你仔细观察电子邮件的数据内容，你会发现除了下面的标签之外，什么都没有。

> 至:应用课程<team></team>
> 
> 出发地:赫曼斯·耶尔纳古拉<yernagulahemanth></yernagulahemanth>
> 
> 主题:关于期末专题
> 
> CType:文本/Html
> 
> 除了具有上述标记的行之外，还有电子邮件内容的行

数据中有噪声，对吗？别担心，我们的超级英雄巨蟒之子 regex 会帮我们移除它们。

好了，我们已经对数据有了足够的介绍，现在让我们快速浏览一下我们面前的任务，以达到本文的高潮。

1.  用我们现有的文本文件制作数据框。
2.  [清理数据](#d8f2)。
3.  [按照我们的要求展示数据](#6161)。
4.  [构建深度学习模型](#2e31)。

**制作数据框架:**如果你观察图 1，我们可以用简单的代码制作我们的数据框架。

我们的数据框将不得不，从，主题，以前的电子邮件，内容文件路径和电子邮件类型(撰写或回复)列。

> **注意:**如果是回复邮件，一行或多行将带有“已写:”标签。

1.  我们将创建一个类，其中每个对象都指向一封电子邮件
2.  我们将把文本文件中的行提取到一个变量中，并将它们赋给相应的变量。例如，如果该行有“To:”标记，则它被追加到变量 to_ 中，如果该行有“subject:”标记，则它被追加到变量 subj_ 中，依此类推。
3.  现在，我们将根据“已写:”标签来拆分内容，即 content.split('已写:')
4.  如果上面的行导致“n”个拆分，那么我们将创建“n”个电子邮件对象，对于第一个电子邮件对象，我们将分配普通的“收件人”、“主题”和剩余变量，但是对于第二个对象，收件人将是前一个对象的“发件人”，发件人将是前一个对象的“收件人”，第一个对象的前一封电子邮件(内容)将是 nan，但是从第二个对象开始，前一封电子邮件将是前一个对象的内容。
5.  在对所有的邮件(文本文件)应用上述观点后，我们将得到一个大的“收件人”列表、“发件人”列表、“主题”列表、“内容”列表以及所有剩余的变量。
6.  现在有了这些列表，我们将创建一个如图 2 所示的数据框。

![](img/9a394ccbb3427f743bdb1d42805eba90.png)

我们已经建立了我们的数据框架，现在我们必须清理我们的数据框架。

**清理数据:**在清理数据之前，我删除了内容部分长度为零的行。“来往于零件”被替换为基本的清理操作，如删除除“@”之外的特殊字符，并且它基于“@”进行拆分，以考虑“@”之前的唯一名称，其余所有字符都被弃用。主题和内容部分适用于基本的清理操作，特别是删除任何网站的网址，电子邮件地址和标签，如'收件人:'，'发件人:'，'主题:'等…

让我们看几张清理句子前后的图片

![](img/877095cd57076b199ba655dbeba8d9ea.png)![](img/3c533758860d75c52fc3d9235214b811.png)![](img/f649f155b8fa051841890625e42e1c5b.png)

完成清理操作后，一些不具备特定标准的行被删除，这些行的标准如下。

**To:** 行必须只有一个值，否则该行将被删除

**主题:**行必须少于 7 个单词，因为我们人类不考虑阅读长度很长的主题，而是更喜欢阅读内容部分。因此，主题部分超过 7 个单词的行将被删除

**内容:**如果内容部分的字数少于 7 个字，则删除该行，因为这样短的内容没有多大用处。

在这些数据中观察到的非常重要的事情是一些奇怪的句子，如图 5 和 6 所示

![](img/6d766da85100a2fd6fbf14bc51b5bdd0.png)

图 5

![](img/3a39732902179827a417d5c7e94961c6.png)

图 6

为了去除这种句子，我发现单词和字符的比例是很有用的，如图 7 所示

![](img/261a5fba2fcc45f5f1d7a3defa59aae5.png)

图 7

对于我的数据，我发现如果这个比率小于 0.09，那么这个句子就不是一个真正的句子。无论如何，对于其他数据，我们必须试验这个比率，并找到适合该数据的特定值。

**注意:**到目前为止，这篇文章中没有解释任何代码，因为到目前为止，所有代码都是基本代码，任何理解基本 python 的人都可以理解我的代码。

**特征化数据:**现在我们应该如何特征化数据呢？我们必须知道模型的输入是什么，模型的输出是什么，以便描述数据。

模型的输入将是单词(句子)的序列，模型的输出将是单词(s)，让我们看一个例子。

**举例:**

**收件人:** team@appliedai.com【预处理后:团队】

**主题:**关于注意力模型的疑问。[预处理后:关于基于注意力的模型的疑问]

**上一封邮件:**南

**内容:**你好团队，我对【预处理后:你好团队我对】有疑问

我们对模型的输入必须是上述所有句子的组合，模型必须预测下一个单词或单词。输出可能只是“注意力”或“基于注意力”或“基于注意力的模型”或甚至更多的单词组合。

***输出:*** *关注基础机型。*

因此，我们的输出依赖于“收件人”、“主题”、“以前的电子邮件”和“内容”，那么我们应该加入所有这些吗？是的，我们必须加入所有这些，但有一件重要的事情要做。

我们必须以这样一种方式分解我们的内容句子，即如果第一个“n”个单词是输入，则输出必须是下一个“m”个单词。

示例内容:-我是 hemanth，一名机器学习工程师，正在寻找一家公司，在打破这一限制后，我可以在那里展示我的技能。

![](img/20245e4aa3a0c4ba2bf932d7e102d50d.png)

图 8

**注意:**我们限制自己预测最少一个单词，最多五个单词，这样拆分后，每一行都将有“收件人”、“主题”和所有每一列，如下所示。

![](img/34d25fc1569a1b3ec895422bede3fc2f.png)

图 9

现在，我们必须将所有输入加入到模型中，即“收件人”、“主题”、“以前的电子邮件”、“x”，但是简单地加入会有帮助吗？我们必须通过用标签将它们分开来连接它们。例如,“to”以<to>开头,“subject”以[开头，依此类推(请参见下面的示例进行说明)。]</to>

> **Input:**<to>team<sub>关于关注型车型的疑问< prv > nan < cont >你好

输入和输出被包装在开始和结束标记之间，以使模型明白它已经到达了句子的末尾。

> **输入:** <开始> <到>团队<子>疑问关注基础车型<PRV>nan<cont>hello<end>
> 
> O **输出:** <开始>团队<结束>

*恭喜你！！！好啊，我们已经完全完成了对数据的清理和特征化。*

**构建模型:**由于我们的输入和输出将单词(句子)排序，我们将使用基于序列的模型，如编码器-解码器模型、基于注意力的模型。首先，我们将看到编码器-解码器模型的工作，以及它们的能力和缺陷，然后是基于注意力的模型如何解决编码器-解码器模型的缺陷。

**编码器层:**编码器是简单的 LSTM 层，在每个时间步接受单个输入，每个时间步的输出被否决，即返回序列为假，另一方面，返回状态将为真，即先前时间步的隐藏状态将输入当前时间步的 LSTM 状态，如下图 10 所示。

![](img/124e60d115112bb8023073cd8e8c6454.png)

图 10

一个数据序列，即一个句子，被传递到编码器，记住，在将这个序列数据传递到编码器或解码器之前，该序列必须应用填充和标记，与传递到 LSTM 模型的序列数据相同。

现在当你第一次把这个序列数据传递给一个编码器时，初始状态将是一个零向量(h0 c0)。第二个时间步的状态是在第一个时间步生成的状态，也就是说，我们可以说，前一个时间步生成的状态作为当前时间步状态传递。这种情况一直发生，直到它到达序列中的< end >标记，最后编码器生成一个权重向量，该向量保存句子的完整含义。与权重向量一起，输出也在编码器的最后一个时间步长生成。这个权重向量和输出被传递到关注层。

**关注层:**前一层(编码器)生成的权重向量和输出传递给关注层。在注意层中，隐藏状态和输出通过密集层，以便可以用反向传播来训练它们，在反向传播上应用 tanh 激活函数，然后乘以可训练向量(在我们的情况下，我们通过单个激活单元层)。在此之后，应用 softmax，由此整个向量的和变为 1，并且向量中的每个值从 0 到 1 变化，使得每个时间步长的每个输入的权重是已知的。

> 由于分数向量中的每个值具有 0 和 1 之间的值，当该向量与编码器输出按元素相乘时，重要的单词被聚焦，而剩余的单词将具有较小的权重。

在将分数向量与编码器输出逐元素相乘之后获得的最终向量被称为**上下文向量。**

**为什么使用注意层？**

如果你仔细观察最后几行，重要的编码器输出得到较高的值，即接近 1 的值，而不重要的单词得到较低的值，即接近 0 的值，因为分数向量中的每个值总和为 1，并且每个值从 0 变化到 1。

**解码器层:**该解码器层的输入是目标值。例如，如果目标序列是<开始> y1，y2，y3 <结束>，那么在每一个时间步中，每一个字都会被输入。

![](img/ccae7c87e6fb954878742125f34b0c31.png)

图 11

**只有那些字会被输入到解码层吗？如果是，那么在前一层中生成的上下文向量有什么用？**

不，不仅这些单词被输入到解码层，而且这些单词还与前一层(注意)中生成的上下文向量连接，该连接向量作为输入被传递到解码层，如图 11 所示。解码器将在每个时间步长生成输出。将这些生成的输出与目标值进行比较，并反向传播所有层以获得最佳权重。要了解更多关于基于注意力模型的编码器-解码器，您可以参考[参考](#544f)部分中的链接。

![](img/2c7c882e0b67314feda7134f7c4d8705.png)

**任务完成**

现在我们来看看一些预测的输出

![](img/6c2143af0d1d8be9ff6f9854bc6eef46.png)![](img/fc0a1c47b4118e769a4d50467faf3bce.png)![](img/5a7a6b1d1093826e17415c56b8eb906e.png)![](img/df7d0e1927319df3c01bdd06a390b387.png)

**注:**模型预测最少一个单词，最多五个单词。

在此获取此代码

**参考文献:**

1.  [https://www.appliedaicourse.com/](https://www.appliedaicourse.com/)
2.  [https://blog.floydhub.com/attention-mechanism/](https://blog.floydhub.com/attention-mechanism/)
3.  [https://www . tensor flow . org/tutorials/text/NMT _ with _ attention](https://www.tensorflow.org/tutorials/text/nmt_with_attention)
4.  [https://www . coursera . org/lecture/NLP-sequence-models/attention-model-lSwVa](https://www.coursera.org/lecture/nlp-sequence-models/attention-model-lSwVa)
5.  谷歌😜