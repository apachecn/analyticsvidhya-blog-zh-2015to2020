# 解释 ROC 曲线的工作原理

> 原文：<https://medium.com/analytics-vidhya/explaining-how-roc-curve-works-df7b46df6e65?source=collection_archive---------5----------------------->

**受试者工作特性曲线**或 **ROC 曲线**是评估**分类模型**性能的一个很好的工具。衡量一个模型的整体质量是很重要的。分类模型将输入数据的特征映射到属于不同类别的概率中，因此该模型输出一个**概率**值 X，而不是说该输入看起来像一只狗或一只猫，该值可以描述为——该输入属于一个特定的类或类别，置信分数为 X%百分比。为了保持更精确，我们设置了一些**阈值**，即 **T** ，我们说如果概率是…