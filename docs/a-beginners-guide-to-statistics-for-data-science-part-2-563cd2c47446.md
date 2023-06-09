# 数据科学统计学入门指南第 2 部分

> 原文：<https://medium.com/analytics-vidhya/a-beginners-guide-to-statistics-for-data-science-part-2-563cd2c47446?source=collection_archive---------18----------------------->

![](img/23d460c24f1709bb6b0eb0c438d47e25.png)

我们将在本系列的第三篇也是最后一篇文章中使用同样的图片

欢迎大家来到我们关于统计学基本术语和概念的三篇文章系列的第二部分，第一篇文章的反响非常好，我非常感谢大家。(我说真的太多了是啊，也是啊)

这是第一篇文章的链接，以防你错过了。

[](/@adebayotosin101/a-beginners-guide-to-statistics-for-data-science-55ce66f00aa2) [## 数据科学统计学入门指南

### 理解指导你作为数据人员的基本统计学概念。

medium.com](/@adebayotosin101/a-beginners-guide-to-statistics-for-data-science-55ce66f00aa2) 

在这个系列的第一部分，我们谈到了参数、总体、样本、变量、抽样方法和我的一些笑话(我不能过分强调这些事情的重要性)

在本系列的第二部分，我们将深入探讨更多的概念和术语，这些概念和术语有时会让数据人员感到困惑，不要担心，真的没那么深。这是第一篇文章的延续，第一篇文章谈到了我们将要建立的其他概念，我将一如既往地尽力解释我们将要考虑的所有概念。我已经说了 3 遍这样的概念，有人请在评论区给我另一个词。

![](img/5ebf82fd2e845ea0b689788a6b340297.png)

好了，让我们开始吧！(如果你熟悉 YouTube 上的动漫频道，这里通常会有某种介绍视频，不幸的是我们在这里做不到，但请在脑海中想象一下？是的，我看动漫，处理它。)

如果你记得清楚的话，在上一篇文章中，我们谈到了一大堆东西，是的，(你没读过吗？就回到本文开头)

我们谈到的其中一件事是一个叫做统计的术语，我们把它描述为从我们的样本中得到的计算结果，例如在我的遗产中支付的平均 NEPA 账单等等。我们想了解一些关于这个术语的有趣的东西，跟我来好吗？

我很想知道为什么尼日利亚大学的学生通常喜欢在考试前几天或几周阅读，这是一个非常有趣的现象，但事实就是如此。

所以我从尼日利亚西南部的 5 所大学中随机挑选了 10 名学生(因为我住在这里)，比如联邦理工大学、阿库雷大学、伊巴丹大学、奥巴费米·阿沃洛沃大学、圣约大学、拉各斯大学，我们确保从每所学校的每个院系中挑选一名学生，我们已经成功地生成了我们的样本，(我会让你们思考我们在这里使用的采样方法)。

我们问他们各种问题，并记录他们对我们研究目标的看法，我们的研究目标是了解为什么学生更喜欢在考试前几周阅读(以防你忘记)。

根据我们的发现，我们发现学生在考试前几周阅读的主要原因是因为他们并不真正关心学校，只想赚钱，我们还可能发现，我们样本中的大多数人只是为了考试而临时抱佛脚，临时抱佛脚听起来不太明智，我们还可能发现，学生的时间表， 时间表可能非常严格，所以我们样本中的学生没有足够的时间准备考试，直到学期的最后几周，学术工作和课程通常会逐渐减少，这给了他们更多的时间阅读。

这真的很难理解，有人知道我要去哪里吗？跟我来。

![](img/0ed3bc1ba79e99aefdba3b12a4d6b5cb.png)

哦哈利！

所有来自我们样本的发现，我们可以很容易地称之为推论，我们称之为**推论统计**。

本质上，推断统计学是指利用样本数据得出一些结论(即做出一些推论)。我们在这里试图做的是得到那些推论，看看它们是否能概括，所以在实际意义上，得到推论并不是我们真正想要的，我们想看看那些结论是否适用于样本所来自的一般人群。我们想概括一下。推理统计学是数据科学的一个重要组成部分，因为作为一名数据科学家，或者更确切地说，作为一名数据人员，你会发现自己所做的大部分事情都是试图从数据中找到意义，试图得出结论和描述，是的，描述。

我们也有**描述性统计**，描述性统计背后的想法是，它只适用于样本或与你一起工作的整个人群。

就是这个想法。

举个例子，我们收集我的房产有多少层楼的数据。我相信你已经厌倦了听我谈论我住在哪里，请耐心听我说。

如果我们发现我的庄园中的平均房屋是 2 层，这个统计只适用于我的庄园，它描述了我的庄园中建筑物的性质。

这是描述性统计的一个例子。

另一个例子是吗？

FUTA 计算学院的学生参加系统设计和分析考试，网络安全系的平均成绩是 35 分。考试分数超过 70 分(我知道你想知道，现在你知道了。)，这个平均分只适用于网络安全系的学生，因此它也是一个描述性统计，希望这有意义？(在心里回答，或者在评论区留言提问)。

任何时候你收集分数或数值，或者更确切地说，任何时候你收集这样的数据，它通常以一组分数或数值结束，我的朋友们称之为**分布。**

描述性统计仅适用于从中收集数据的样本或总体的成员。

既然我们已经讨论了各种类型的统计数据，让我们继续讨论不同的度量方法。

对于这篇文章，我们将讨论集中趋势的措施。

有时，我们希望看到我们的值是如何分布的，分布的形状，最频繁出现的值以及许多其他东西。

集中趋势，你可能知道，特别是如果你去过尼日利亚的中学(保证你会这样做)，是你的

平均

中位数

方式

我不会花太多时间讨论这个，但是为了内心的平静，让我们触摸它们

平均值:是分数分布的算术平均值。理解从样本中得到的平均值不同于总体的平均值(我知道你知道，不是吗？)

中位数:中位数是标记第 50 个百分位数的分数，指的是分布的 50%。简单来说，中位数代表分布的中间分数。中位数的主要功能之一是显示偏斜，无论分布是否偏斜(当我们说分布偏斜时，我们指的是什么？是吗？保持那个想法，我们将在一或五分钟内到达那里)

要计算中间值，请从最低到最高排列分布，并选择中间值，即第 50 个百分位数。我们也可以有两个中间值，特别是当分数总数是偶数时，我们要做的是找出中间两个分数的平均值，这就是你的中位数。

Mode:这是最少使用的中心倾向，因为它在三者中提供的信息最少(在你的思想中，你期望它是最少使用但最有用的？哈哈哈，你真的需要看你看的电影，生活很艰辛)。

它显示的基本上是出现频率最高的分数。到头来没什么用是吗？

发行版可以有多种模式。

对于具有两个模式的分布，它们被称为双峰分布；对于具有多个模式的分布，它们被称为多峰分布。

最后，关于我告诉过你们的那个想法，偏斜分布？不是那样想的，你的网络订阅很快就完了我知道，阿森纳和曼联是蹩脚的俱乐部，我知道，切尔西的转会禁令已经解除，利物浦是英格兰最好的球队，听着，我完全清楚。

![](img/ef496ad6e3009eb5bcb271786197f5e1.png)

但是让我们先讨论一下偏斜，好吗？

当我们说分布是偏斜的时，我们说的是分布中的大多数数据点或值倾向于某个方向。

举个例子，在一个专为老年人服务的地方，我去那里的目的是了解每个人的年龄，包括员工的年龄，并用它绘制一个图表，可能还会用我打算收集的数据得出一些推论。

我收集的大部分年龄是从 60 岁到 100 岁，甚至可能是 120 岁。

画出这个数据的图表，我们会发现大部分的数据会远离原点，出现在图表的右边。这就是我们所说的**负偏态分布。本质上，这只是意味着分布的尾部被拉到了图表的下端(我们说的下端是指更靠近原点，你知道什么是原点吧？).**

据说任何有负面的东西也会有正面，是的，我们有**正偏态分布，**在这种情况下，数据点被画向原点，尾部位于分布的高端，远离原点。

再举一个例子来说明这一点。我们进行了一项调查，以了解尼日利亚小学和高等院校的学生对学校教育重要性的看法，分数为 1 至 5 分。

我们选取了 30 名小学生作为样本，其中 13 人选择了 5(意味着非常重要)，7 人选择了 4(非常重要)，另外 5 人选择了 3，然后选择了 2，然后其中的马林鱼宝宝选择了 1，也就是 2。

从这个分布中我们已经可以看到，它趋向于原点，这使得这个分布是一个正偏态分布。

然后我们带着同样的问题去问他们在大学和理工学院的哥哥姐姐，显然我们已经知道了(学校 na 骗局，leemmaaao，然后看在上帝的份上退学)。

我们有三个人选 5，我们有另外三个人选 4，然后我们有八个人选 3，然后我们有七个人选 2 和马林鱼，其中八个人选 1(无意冒犯所有马林鱼，我也认为自己是某种形式的马林鱼)。

我们可以清楚地看到，这种分布也是偏斜的，但这次是负偏斜。

以防你错过了

1.推理统计:

2.描述性统计:

3.集中趋势测量

4.分布的偏斜:负的或正的偏斜。

![](img/e13e0b1e0960188a462be2afa40bb812.png)

这个迷因永远不会过时

你真的没想到我会再写一遍吧？我很高兴你没有，事实是我要留在这里是的，所以处理它。

问题、建议和推荐都在评论区公开，我真的真的很喜欢。

我们为这个特别的系列准备了最后一篇文章，将于 2019 年 12 月 11 日星期二发布。在我们开始另一系列之前，我可能会在下周五向我们展示一下我在深度学习方面做的事情，然后我们开始另一系列，讨论数据世界中的其他相关主题。

我的名字是德巴约·托米辛，感谢您的阅读！！星期二见。

![](img/93970028101a804576c840b315600185.png)