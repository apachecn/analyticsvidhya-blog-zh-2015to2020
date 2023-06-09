# patent fill——使用 NLP 帮助律师起草专利

> 原文：<https://medium.com/analytics-vidhya/https-medium-com-sree-patentfill-9aba7b1bf5a1?source=collection_archive---------18----------------------->

起草一份专利申请可能是一项艰巨而乏味的任务。对发明者和法律事务所来说，律师的时间都很昂贵。起草一份专利申请最少需要 25 到 30 个小时(*来源:Quora* )。起草一份引人注目的专利申请的过程包括仔细列出相关在先发明的类别。这要求律师参考类似的先前申请或其他资源，这可能是劳动和时间密集型的。

在我的 Insight Data Science Fellowship 中，我咨询了 Cognition IP(一家技术授权的律师事务所)，以构建 Patentfill，这是一种通过为给定输入建议扩展文本来帮助提高起草专利申请过程效率的工具。让我们看一个 Patentfill 将做什么的例子。下面的引用是从其中一项专利中摘录的一句话。

*“在一个总的方面，本发明设想了配备有* *和* ***读取器(扫描仪)的实验室或实验室网络，例如条形码读取器、磁条读取器*** *或类似的设备……*

单词“阅读器(扫描仪)”是 Patentfill 的输入文本，输出是扩展文本列表“条形码阅读器、磁条阅读器”。每当律师遇到这种情况时，很难列出这些扩展文本。因此，Cognition IP 希望我训练一个能够记住/从这些实例中学习的模型。

Cognition IP 已经收集了超过 177，000 个公开可用的专利(json 文件),它们存储在 s3 存储桶中。我从他们的 s3 桶中下载了超过 133，000 项专利。一个典型的专利看起来类似于一篇研究论文，由标题为“摘要”、“概要”、“详细描述”等部分组成。“详细描述”是我们最感兴趣的部分，我从大约 133，000 个文件中的每一个文件中提取并编译成一个大的文本文件。在我进入任何细节之前，我想让你熟悉一些我一直在使用的术语。

因此，术语“例如，举例来说，也就是说，例如，像，包括”是律师在列出我们的目标扩展文本时经常使用的术语。所以我称这些词为“关键短语”,因为它们在这里起着重要的作用。特定关键短语前的单词也很重要，因为建议的扩展文本取决于关键短语前单词的上下文。所以我把每个关键短语前面的这些词叫做“关键分类器”。请注意，由于关键分类器也是我的输入单词，所以我可以互换使用这两个术语。好了，现在我们准备进入细节。

从包含每个专利的“详细描述”的大数据集中(顺便说一下，这是一个 6.8 gb 的文件)，我只提取了那些包含关键短语的句子。这给了我一个更小的子集(大小为 5 gb)来处理。但是这还不够，因为我只对我提取的子集的每个句子中包含的特定模式(关键分类器—关键短语—扩展文本)感兴趣。所以我创建了所有这些片段的子集。也就是说，我在每个关键短语之前取了 4 个单词的 n 元语法(在我的例子中 n=11 ),加上关键短语，然后是它之后的 6 个单词。这给了我一个更小的子集(超过 680 万个句子片段)来处理。这里必须提一下我选择 11 克的原因。语料库中有类似下面引用的句子。

此类指令可从另一个计算机可读/可用介质(如静态存储设备或磁盘驱动器)读入系统内存

在上面的句子中，在关键短语前只取一个单词并没有考虑到这里使用的单词“medium”的确切上下文。只有当我们考虑到“计算机可读/可用介质”这些词时，我们才能获得清晰的上下文，这就是为什么我选择在关键短语前取 4 个词，在关键短语后取 6 个词，因为看起来 11 个词足以从大多数句子中提取输入词的上下文。

接下来，我想看看我在尝试为哪些单词建议扩展文本。Cognition IP 告诉我，他们大多与技术领域的实用专利合作。所以我画了一张最常出现的关键分类器的直方图。

![](img/8fbc00971f78977410ceb0bf005813a1.png)![](img/ef87dd464e0843e32aa665646ff297fc.png)

常用关键分类器的直方图，在左边，最常见的扩展文本的词云在右边。

我想知道我们预期的扩展文本是什么样的，所以我绘制了一个词云，我们可以看到“存储、计算、可读设备”经常出现，这意味着这些可能是术语“设备”的扩展文本。

接下来，我去了一些主题建模。我的语料库包含超过 680 万个**模式的句子片段——关键分类器——关键短语——扩展文本。我对它们进行了词条化，去掉了标点符号和特殊字符，去掉了停用词(包括一些无用的词，如体现、表格、图形等等，还有数字)。对于主题建模，我首先尝试了潜在的狄利克雷分配。该模型未能区分“如，包括”和“系统，安全”等词。从下面的可视化中可以清楚地看到，即使在使用最佳一致性分数进行了一些超参数调整之后，主题集群仍有相当多的重叠。**

![](img/e1031e31b932f3c136ab442396307e6f.png)

LDA 主题的可视化

接下来，我通过将清理后的数据输入到 Gensim 的预训练 FastText 模型来执行迁移学习。我非常注意这个过程中的超参数。对于嵌入向量的大小，我选择了 80，也就是说，我们语料库中的每个单词都由一个 80 维的向量表示。我选择 skip-gram 模型，取窗口大小为 5。这意味着对于每个输入单词，将有 10 个输出单词，在输入单词之前有 5 个单词，在输入单词之后有 5 个单词。这 10 个输出单词用于学习输入单词的单词嵌入。对于一个词的最低频率，我选择了 5，但当然这可以增加。我小心翼翼地不去理会那些出现频率较低的单词。最后，我选择以 exp(-2)的因子对最频繁出现的单词进行降采样，以“平衡单词频率”。

让我们看看模型是做什么的。使用 FastText 模型的“最相似单词”功能，我绘制了语义上最相似的单词:CD、屏幕、磁盘、网络和存储设备。为了绘制嵌入在 2D 的 80 维单词，我使用 PCA 将向量的维数减少到 2 并绘制它们。我们可以清楚地将图中的单词与输入的单词进行匹配。这些只是示例，我只绘制了 5 个输入单词的输出。

![](img/213289c572c71dabbef097c066d08c5d.png)

在 2D 标绘的 5 个输入单词的快速文本输出。每个聚类中的单词匹配输入单词之一。

考虑到这个问题无人监管的性质，验证我的模型是一个挑战。我想深入了解该模型的实际表现。所以我做了一组输入词，收集了每个输入的前 5 个建议扩展词。然后我得到了它们的向量表示，并计算了输入文本和每个建议扩展词之间的欧几里德距离。现在，为了获得语料库中两个词之间的“近”和“远”的感觉，我选择了“cd”这个词。从上图可以看出‘CD’离‘DVD’这个词挺近，离‘screen’这个词很远。“cd”和“dvd”之间的距离约为 3.16，“cd”和“屏幕”之间的距离约为 6.95。我做了一个数据框架，下面是其中的一个片段，你可以看到模型输出的文本和输入文本之间的距离。

![](img/7a14a5d4b3224b8d88aac39543dd55f0.png)

**结论:**在我试过的所有模型中，FastText 表现最好。考虑到数据集的容量和随之而来的编码问题，数据集的最大挑战是从文本中提取/识别相关片段。此外，由于资源有限，FastText 需要 12 个多小时来运行实验范围，这在一定程度上受到了限制。然而，在未来，使用像 GPT-2 这样的模型进行迁移学习可能会很有意思。最后但同样重要的一点是，我非常享受作为 Insight Data Science fellow 向 Cognition IP 咨询的经历。