# 向他人发送数据前清理数据的清单

> 原文：<https://medium.com/analytics-vidhya/checklist-for-cleaning-data-before-sending-data-to-others-e60fbb71c831?source=collection_archive---------29----------------------->

![](img/a9b69434ee222ee1ebb52b42771866f7.png)

照片由[詹姆斯·庞德](https://unsplash.com/@jamesponddotco?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

在我的初创公司做了一段时间的数据科学家后，你需要为公司的项目寻求外部帮助。

您雇佣了一个外部团队来创建这个项目。

然后你会遇到这样一种情况，你不得不交出很大一部分原始数据来让项目运行。在这个过程中，**我了解到，当你与承包商合作时，我必须做一些事情来保持低价格并加快建造过程。**

遵循这些提示将极大地帮助你的承包商，并实际上赢得一位数据科学家同事的尊重。

以下是我在这个数据项目中获得的重要亮点:

**一、帮助他们理解你的数据；事情会变得更顺利。**

*注意:当你已经让你的法律团队检查了两家公司之间的保密协议的细节时，这是真的。*

*   创建 E.R.D .(实体关系图)——这将有助于承包商进一步了解他们的任务是构建什么样的数据或系统。请将此视为项目的概述。此外，请记住，这必须在图表中包含重要的 P.R.K .(主要)和 F.R.K .(外来)才能构成有效的图表。
*   创建一个非常详细的模式—这一部分将帮助您和您的承包商完全理解并回答以下问题:什么样的数据(数据类型)，什么不能丢失(非空)，什么数据链接到什么(P.R.K .，F.R.K .)，以及数据如何在整个系统中流动(核心表，连接表)
*   其他注意事项—这可能并不常见，但在某些情况下，系统可能有两个 P.R.K .或每列有唯一的默认值。这一部分还可以包括关于如何创建数据的特定过程，例如特殊索引等。

**二。数据集分析的基本信息，E.D.A .分析(主观意见)**

1.  D.B .编码格式(UTF 8(4 位)、ASCII-II 等。)
2.  检查特殊字符(返回字符、转义字符通常会在以文本格式上传时产生错误)
3.  大写和同义词一致性(即韩国、韩国(韩国)、韩国)
4.  缺少值，NaN(条目存在是因为它们存在吗？还是随机误差？)
5.  PRK 比率检查(索引 id 与实际条目的比率是否匹配？如果没有，为什么？或者我们有解决办法吗？[线索:多键模式是一种选择，或者是序列化？]
6.  范围分布(是否存在与集合不匹配的离群值？)
7.  单位(可能有一种以上的数据类型。即‘克、英制、毫升等’，‘检查是否需要统一)
8.  命名错误(某些下载集有编码错误，可能需要彻底清除，如特殊字符错误。)
9.  表描述(列名并不能说明全部情况，文档很重要)
10.  多平台检查(他们在 Excel，D.B，Python 上开的好吗？这是为了帮助其他数据科学家同事节省计算编码或手动清理新集合以供他们使用的时间。)
11.  移除不需要的观察(重复的、不相关的[需要领域知识])
12.  结构错误(某些层次结构、组和类可能非常适合您当前创建的数据模型。[即，在某些情况下，10-20 岁可能被视为青少年，但在其他情况下，18-20 岁可能被归入成人类别，这取决于实施该模型的背景。)
13.  缺少数据和解决方案实施文档(丢弃、推诿—缺少)
14.  标记和填充数据(由器械包创建者完成的替换术语和其他特殊系统标签)

在你实现了第一部分和第二部分之后，你就准备把你的海量数据交给你的队友或承包商(自由职业者)来帮助你创造一个令人敬畏的数据产品。

我希望本指南有助于进一步理解为什么数据科学家要花 80%的时间清理、标记甚至记录他们的数据集。此外，进一步强调在数据科学领域的团队中工作时，文档和模式的重要性。

我的同事 [**豪斯**](https://www.linkedin.com/in/1988-house-profile/)**(**[**studrdylab.com**](http://www.sturdylad.tech)**)**给了我很多关于弹性发展或其他计算机相关研究的好文章。

如果您认为我在这份清单中遗漏了一些变量，请随意填写并发表评论，这样我就可以更新这份清单，让其他人看到。