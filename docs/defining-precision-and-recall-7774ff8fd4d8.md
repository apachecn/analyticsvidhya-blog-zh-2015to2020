# 定义精确度和召回率。

> 原文：<https://medium.com/analytics-vidhya/defining-precision-and-recall-7774ff8fd4d8?source=collection_archive---------17----------------------->

在进行机器学习变通方法时，我们可能不得不应对不平衡的分类问题，其中一个特定类别的样本优于另一个类别。例如，我们正在构建一个垃圾邮件分类器，可以将电子邮件分为垃圾邮件和非垃圾邮件。想象一下，我们只有 50 个垃圾邮件样本(实际情况下没有)和 950 个非垃圾邮件样本。可以理解，这个任务非常突出，因为我们的目标类的样本是不平衡的。在这种情况下，我们不能完全依赖**精度**来评估所建模型的整体性能…