# 如何构建一个 Lemmatizer

> 原文：<https://medium.com/analytics-vidhya/how-to-build-a-lemmatizer-7aeff7a1208c?source=collection_archive---------3----------------------->

![](img/7252920b3468877bf74b111ef74f00d3.png)

## 为什么

如果您对 NLP 感兴趣，您可能会无意中发现许多工具都有这种名为“词汇化”的巧妙特性。在本文中，我将尽力引导您了解什么是引理化，它为什么有用，以及我们如何构建一个引理化器！

如果您是从我上一篇关于如何制作一个 PoS Tagger 的文章中过来的，那么您已经掌握了进行词汇化的重要先决条件。如果没有，我将通过这篇文章的篇幅温和地呈现它们，所以让我们开始吧！

## 什么是词汇化？

![](img/ed0ff97652462fdfe4908f1b451257b6.png)

词条也称为单词的“词典形式”。

词汇化是一种自然语言处理技术，旨在将单词简化为其*词汇*，或**标准形式**。什么是引理？一个提示——它也被称为**字典形式**(一个简单的概念有很多名字，你不觉得吗？).

因此，一个引理是一个词的基本形式——这意味着任何与**时间或数量**相关的**变化**都被移除。例如，在名词中，复数(女孩，男孩，*语料库*)被简化为单数形式(女孩，男孩，*语料库*)；而在动词中，时间/分词变体(ate，bring，chatting)又回到现在时态(to eat，to bring，to chat)。

在某些情况下，词汇化可以包括消除性别差异(doctress → doctor)，尽管这在英语中非常少见，因为该语言趋向于性别中立——然而，在某些特定情况下可以做到这一点(例如专注于特定性别)。《出埃及记》:‘公牛’→‘母牛’，‘公鸡’→‘鸡’)。

![](img/38dd5dab4bd3a828f4442dc8e4b3cc15.png)

根据 WordWeb 免费词典，什么是 lemma？

尽管开始时词汇化可能看起来不太有用，但它是文本规范化的一个强大工具，因为它允许规范化以比词干化更符合语法的方式发生(动词仍然是动词，名词仍然是名词，等等)，我们在另一篇文章中分析过[。](/analytics-vidhya/building-a-stemmer-492e9a128e84)

现在，你可能已经注意到了，**一个单词的引理与其词性**密切相关。这是为什么呢？因为每个单词都是一个或多个词位的“家”。从词汇的角度来看，一个词位也是意义的基本抽象单位。在其他“单词”中，每个单词都可以有一个或多个意思(哦，天哪，一个短语有太多单词了！).我不会深入探讨这个形态学/词汇学/语义学的讨论，但是请记住最后一个假设。

因此，一个与它在句子中的角色(词性)相关的词可以解释某个词位，而在另一个角色中，它解释第二个词位。“*活着*”就是这种情况——如果是名词，它可以阐发一个与“*作品*有关的词位；如果是形容词，它可以阐发一个与“*活着*”有关的词位；如果是动词，它可以阐发一个与“*活着*”有关的词位。复杂？试着更进一步，你告诉我什么是复杂的！(另外，对于语言学家来说，请原谅我过于简化了词汇的概念)。

![](img/969d8fe69d92e85578cc72ab4e1b94f5.png)

什么是词位，WordWeb 先生？

考虑到这一点，我希望您能够开始理解词汇化的效用和重要性。如果没有，让我们说得更清楚些。

顺便说一下，我认为不用说，词汇化是每种语言特有的任务，对吗？

## 为什么以及何时应该使用词汇化？

这里有一个场景:你正在为一家真实的国有公司基于 twitter 上关于“住房”的帖子构建一个情绪分析工具。作为一名优秀的数据科学家，你设法获得了一系列带有该词的推文。现在，你要分析这些推文是正面的还是负面的。

![](img/69442827a6126628e714d9c5c9a8320b.png)

哥谭的房价太贵了！# my house isatfire # batmangetthesecorumps # holy expensive

由于您不知道词汇化有什么好处，您开始构建您的分析器:您使用一个*词干分析器*来减少输入的维度，您对一些输入进行分类，并制作一个准确率超过 90%的机器学习分类器！干得好，数据科学家先生！然而……有一天，有一场关于某个国家在其大使馆“庇护”恐怖分子的激烈讨论。你预计你“精心”设计的机器学习模型会发生什么？

在这种情况下，术语化很有用。例如，如果你在处理之前对条目进行了词条分类，你就可以丢弃带有“to house”的推文，而不是“housing”这个词。因此，词汇化可以像 stemmer 一样帮助您降低维度——然而，这种技术在保持句子结构和正确的词汇方面更精确(即使词汇化后句子可能看起来像胡言乱语)。

如果你想进一步了解词干化和词汇化之间的区别，我建议你去阅读这篇精心设计的理论文章:

[](https://towardsdatascience.com/stemming-lemmatization-what-ba782b7c0bd8) [## 词干？引理化？什么？

### 深入探究词干化和词汇化对自然语言处理任务的作用以及它们是如何实现的…

towardsdatascience.com](https://towardsdatascience.com/stemming-lemmatization-what-ba782b7c0bd8) 

另一种情况是基于模型的方法。今天，大多数 NLP 与机器学习一起使用，以自动生成模型和推理。然而，研究表明，在某些情况下，一个策划的模型更有效，如果不是必要的，来完成一项任务。

![](img/dd54ea9eddeae461b6dbc86c88d5821d.png)

我硕士论文中的图片——术语化有助于构建知识图。

在我的硕士学位中，我手工制作了一个关于奶牛养殖领域的知识图，用于回答问题。在这种情况下，使用基于模型的方法是有用的，因为我没有大型语料库，也没有上下文相关的工具来处理奶牛养殖领域的单词。我的工作所依赖的一件事是有一个 Lemmatizer 来帮助预处理每个问题单词，以匹配组成知识图的性别/时间/数量中立的单词节点(或词位表示)。

因此，词汇化条目是匹配的一个很好的选择——它减少了对一个单词的每个变体进行建模的需要，帮助您减少对访问模型的键的关注，而更多地关注模型(如果您使用像葡萄牙语这样的语言，这非常重要，因为葡萄牙语中有超过 6 种动词时态和性别变体)。

现在，如果你是一个 word2vec 婴儿，你可能看不到这种技术的真正用途——你可能会说，只是矢量化。等等，我相信在你的职业生涯中，你会发现向量不是最佳解的情况。

## 如何构建一个 Lemmatizer

该去工作了！词汇化中最困难的部分是检索单词的词性。谢天谢地，[我们已经在上一篇文章](/analytics-vidhya/part-of-speech-tagging-what-when-why-and-how-9d250e634df6)中做到了，有很多工具可以帮我们做到。

以下是一些简单的例子:

有了每个单词的位置，我们就可以讨论如何进行词条化。有两种主要方法:

*   **基于规则的方法:**使用一堆规则，告诉一个单词应该如何被修改来提取它的词条。例如:如果这个单词是一个动词，并且以-ing 结尾，做一些替换…这种方法非常棘手，可能不会给出最好的结果(很难用英语概括)。
*   **基于语料库的方法:**使用带标签的语料库(或带注释的数据集)为每个单词提供词条。基本上，它是一个巨大的单词列表和每个词性的相关词条(或者不是，如果你在做一个愚蠢的方法)。当然，这需要访问带注释的语料库，这可能很难(或者很贵)得到。

在本教程中，我将两者结合使用，对一般单词使用基于语料库的方法，对复数名词规范化使用基于规则的方法。所以，让我们开始吧！

## 构建基于语料库的词条分类器

在本系列的前面，我们在 GUM 语料库上训练了一个词性标注器，它在通用依赖项目中可用。该语料库以 ConLLU 格式呈现，这是一种用于语言注释文件的特定格式。在为 GUM 语料库提供的 ConLLU 文件中，除了为注释短语中的单词提供词性之外，语料库还提供单词 lemma，这将允许我们创建字典，以便在 lemma 化过程中快速检索它们。

我已经将整个 GUM 语料库合并到一个文件中，因此我们可以享受来自测试、训练和开发测试的术语(我们不需要在这里进行基准测试)。你可以在这里下载[合并的 conllu 文件。](https://gist.github.com/Sirsirious/e653e7d563f6beea08caa030bd14c7a3/archive/fed4934e63b9af9157bb0a6eb9353a74fd3528a4.zip)

让我们用文件中注释的单词编一本词典。为此，我们使用与 PoS 标记相同的 *conllu* 模块:

然而，有一个问题。当我们验证结果字典的长度时会发生什么？

```
>>> len(word_lemma_dict.keys())
12516
```

呃！虽然 GUM 语料库是由不同上下文的注释文本组成的，但不同单词的数量远低于预期。正如你所知道的，一个以**为母语的成年人的英语词汇量**在 **20k-35k 个不同的单词**之间，所以 12k(包括专有名词)可能不够。我们能做什么？

嗯，一种方法是使用其他语料库/词表。https://lexically.net/[有一个有趣的网站，这是一个专门制作单词表的网站。我们感兴趣的是“带有 c5 的词条列表 10”(在此下载:](https://lexically.net/wordsmith/support/lemma_lists.html)[' BNC lemma 10 _ 3 _ with _ C5 . txt '](https://gist.github.com/Sirsirious/af08f156718ccea1e9999fd93de72811/archive/1aeecc0347ce91dfba8c45cc59491113bb4b4c91.zip))，它实际上是从英国国家语料库(BNC)中提取的词条和变体的列表，并使用 *c5 标签集*(BNC 的标签集)进行注释。这里有一个简短的观点:

![](img/52deb6ba55aabbdfeb51e643ca67f5f4.png)

来自 lexically.net 的词条列表。它们是从英国国家语料库(BNC)中提取出来的，然后加上标签。

要使用它，我们必须制作另一个转换器(让它与我们当前的 PoS 标签一起工作):

好吧，这是一个丑陋的，但它完成了工作。此外，我不得不自己做——没有语言学家或语法专家的帮助。请注意，我将“< *UNC* >”转换为“名词”，因为在大多数情况下，这个单词列表中出现的名词被标记为“< *UNC* >”，而不是 *NN+something* 。

现在，让我们利用现有的知识，给我们的字典增加一些单词:

结果？

```
>>> len(word_lemma_dict.keys())
30987
```

好多了。当然，有些*小生*的话就不会有了。但是让我们考虑一下就够了。是时候派上用场了。让我向您展示一下现在对一个单词进行词汇化是多么容易(顺便说一句，如果这个单词和它的词性不在字典中，我会忽略，这可能不是最好的解决方案):

测试它:

```
>>> words = [('living', 'ADJ'), ('living', 'NOUN'),('living','VERB'), ('guns','NOUN')]
>>>for word_tuple in words:
...   print(lemmatize(word_tuple[0],word_tuple[1]))
...
living
living
live
guns
```

工作，但不完美。“Guns”一词没有单数形式。我们能做什么？把它加入我们的格言:

```
>>>word_lemma_dict['guns']['NOUN']=['gun']
```

当然，这不是最好的解决方案，但是，嘿，我们有 30k 个单词。如果一对情侣失败了，也没那么糟糕。我们也可以不断增加更多的语料库到我们的字典中(如果我们找到的话)。例如，我们可以使用**英语网络树库**，它有超过 254，000 个带标签的句子！你自己试试。我已经做到了 37k 个不同的单词。

让我们保存这个字典，这样我们就可以在我们的主要工具中使用它:

```
>>>import pickle
>>>pickle.dump(word_lemma_dict, open('word_lemma_dict.p','wb'))
```

## 一个基于规则的复数名词分类器

“枪”的问题困扰着我。我检查并发现我们的列表中没有一个将单数形式视为复数名词的引理。我们可以做一个专家系统来处理这个问题。下面是我在 https://www.grammar.cl/Notes/Plural_Nouns.htm 的[使用规则所做的](https://www.grammar.cl/Notes/Plural_Nouns.htm)

首先，我从 http://www.esldesk.com/vocabulary/irregular-nouns 的[那里得到了一个最常见的不规则名词列表。我对它进行了处理，并制作了一个 csv 文件，你可以在这里下载。](http://www.esldesk.com/vocabulary/irregular-nouns)

然后，我将它加载到一个 dict 中，并使用与主字典中相同的方式对其进行了腌制，使用复数的**作为键**，使用单数的*作为值*。下面是实现名词变形的函数(它将作为一个实用程序捆绑到我们的工具中):

简单明了，它适用于链接中提到的情况。还有，我们的‘枪’终于被词汇化为‘枪’了！既然我们已经有了大多数情况下的解决方案，是时候在我们的工具套件中实现它们了。

## 使用 NLPTools:

从本系列的开始，我们就一直在从头开始构建 NLP 工具套件。本节旨在继续这一进程。[目前为止的项目可在此处获得](https://github.com/Sirsirious/NLPTools/tree/128a27658e197a1b887800a94324b84e6746c6b5)。

对于文件夹结构，我们是这样的:

![](img/3b1fe6bb1311a2367d5ba36f75ed384e.png)

提醒一下:*绿色*是新增的，*黄色*是修改的。首先我们做一个新的文件夹脚手架，加入我们的单词 lemma 字典和我们的不规则名词字典(*预装/dictionary s/lemmas/*)。

我还创建了一个 utils 文件夹，并添加了一个 *word_utils.py* 文件，带有前面提到的 *inflect_noun_singular* 函数。

然后，我们可以创建我们的 lemmatization.py。这是迄今为止最简单的文件，尽管我尽了最大努力来测试它并使它防失败。

最后一次修改在 *__init__。py* ，在这里我向管道添加了词汇化(默认情况下移除了词干),并将 PoSTagger 默认设置为 UD 标签:

检查它是否工作:

```
>>> doc = NLPTools.process("There are many dogs living in my living place")
>>>for sent in doc.sentences:
...   print([(word.get(),word.PoS) for word in sent.tokens])
[('<SOS>', None), ('there', 'PRON'), ('are', 'VERB'), ('many', 'ADJ'), ('dog', 'NOUN'), ('live', 'VERB'), ('in', 'ADP'), ('my', 'DET'), ('living', 'NOUN'), ('place', 'NOUN'), ('<EOS>', None)]
```

精彩！

如果你提醒一下[词干分析器教程](/analytics-vidhya/building-a-stemmer-492e9a128e84)，我们在《世界人权宣言》中通过检查效率来测试词干分析器。我们从 551 个不同的单词减少到 476 个。让我们用引理化来看它:

一个细节:这里我使用我们的完整管道来处理数据。在词干测试中，我只是使用了一个简单的 split()模块，这导致了集合中的一些更多的单词(可能是附加了标点符号的单词)。考虑到这一点，下面是 lemmatizer 的结果:

```
The number of distinct words in the Universal Declaration of Human Rights is: 538\. After lemmatization, the number is: 486
```

不算太寒酸！尤其是考虑到我们现在保留了单词的部分句法功能。此外，我们预计缩减量会更小，因为我们不是基于词根，而是基于词典形式进行缩减。

所以我们完成了词汇化！

接下来，我们休息一下，用新的概念来玩一玩我们已经有的管道，并学习更多关于文本规范化和预处理的知识。

哦，这里是[当前提交](https://github.com/Sirsirious/NLPTools/tree/eeb2cc53c20014d1454746e68c9918ee48633817)的链接。

您可以通过查看主要的 Git Repo 来检查项目到目前为止的进展情况:

[](https://github.com/Sirsirious/NLPTools) [## 各种/各种工具

### 在我的中级 NLP 解释系列中创建的一组 NLP 工具。-先生/女士

github.com](https://github.com/Sirsirious/NLPTools) 

此外，不要拒绝留下评论或建议。另外，如果您在代码中发现任何 bug，您可以提交到 repo 中。

[1]https://en.wikipedia.org/wiki/Lexeme