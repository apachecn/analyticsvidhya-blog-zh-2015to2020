# 从新冠肺炎的数据集中获得启示

> 原文：<https://medium.com/analytics-vidhya/drawing-insights-from-a-covid-19-data-set-4db2978dfc4e?source=collection_archive---------19----------------------->

## 关于如何使用探索性数据分析来清理、可视化新冠肺炎数据集并从中获得洞察力的逐步方法。

![](img/1cc0923bc9b4bc65482297978764be6b.png)

由[马库斯·斯皮斯克](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

自 2019 年在中国武汉爆发以来，新冠肺炎疫情一直是全球威胁。已经进行了调查和采访，以从不同国家和大洲的某些城市获取数据，从而得出真知灼见。

对于数据科学家或任何热衷于数据科学的人来说，数据是从个人、组织等收集的定量(数字)或定性(分类)的特征或信息，以获得洞察力、提高销售额、预测未来发生的情况等。

在从数据集中获得洞察力之前，请始终记住，没有放之四海而皆准的方法。数据探索包括排序(按升序或降序重新排列数据)、数据过滤(创建可用数据的子集)、数据处理(聚合和统计操作)、数据清理和准备，占项目总时间的 75%。

## 我分析数据集的步骤如下:

*   定义
*   数据清理
*   形象化
*   绘制洞察力

# 介绍

列的定义:

> 数据集来自 She Code Africa，包含新冠肺炎的案例。它包括**国家**、**总病例数** -特定国家的总病例数、**新病例数**-截至数据整理时的最近病例数、**总死亡数**-迄今死亡数、**新死亡数** -最近记录的死亡数、**总康复数**-已从疾病中康复的人数、**活跃病例数**-地面病例数
> **死亡**-死亡人数，**总计 _ 测试**-所有测试的次数，**测试**-进行的测试，**大陆**-国家的大陆。

我从导入模块和读取文件开始。

![](img/d58fb9b8e92319bf47dc8ac1ec0e16f7.png)

为了更好地理解，我查看了数据集的开头和结尾。

![](img/b562de22d19bbcf06fd210057189ede5.png)

*describe()* 函数用于提供所有定量变量的统计汇总。我检查了数据的形状和信息。所有这些都是为了能够理解我将与什么一起工作。

![](img/2b3f5476e9f9a32dde3d6ed068f7c6e0.png)

检查空值。我发现有些值丢失了，并检查了每一列中丢失值的总和。

![](img/6217adae9b5d3f3a929423eb11bb66ed.png)

我用 nan 替换了空格，并将其保存在一个名为 *new_* *data* 的新变量中，以帮助我处理空格。

![](img/2d86c5f19ef39232e32694199032174f.png)

# **数据清理**

数据清理就是从表或数据库中删除不准确的记录。在进行数据科学中的数据清理或探索性数据分析时，最常见的操作之一是操作/修复列名或行名，删除行或列。

![](img/eacd43245d4d0048364bc8c72498974d.png)

数据清理中的一大挑战是异常值的识别和处理，异常值是不同于其他数据点的观察值。

![](img/3d3fa389a0adaff36eeb526f55df90a4.png)

我总是喜欢尽可能多的检查信息。这也有助于了解对象类型。

![](img/146aa2d81176b892bfe1defd199653ce.png)

是啊！！空值已被替换为零。

![](img/c8fd2a298734886a555a59c139a7ef7d.png)

# 数据可视化

对于人脑来说，处理图片形式的信息比处理数字更容易。想象一下，向不具备快速处理数字专业知识的人展示一份摘要——很难，对吧？每个数据科学家都应该能够以图形或图表的形式展示他们的数据，如条形图、直方图、散点图等。

在这里，我将数据转换为 float 以便可视化，并使用条形图来表示数据。

![](img/1a5ee4dc766d769414402f1411a60499.png)

## 案件总数

![](img/1ff6f1f08c65f7a51c14a2ea98bbf14b.png)

## 死亡总数

![](img/83644a5dd336a6cf0c0e7fc96cb20e4b.png)

## 回收总额

![](img/8086684d46a8855069796e3e4ebd1392.png)

## 活动案例

![](img/c50e465c8ca902e1221a2d303be34ad9.png)

## 严重/危急病例

![](img/073e31052dcdc8efbf9286bf42195e29.png)

## 记录的死亡人数

![](img/a7d2db0199a277102e0dc0f063140c65.png)

## 参加的测试总数

![](img/86aed276b44961f3711855a84ec3e6fd.png)

# 绘制洞察力

在这里，我用柱状图来表示病例数最多的国家，即那些接受测试次数最多、记录病例数最多的国家。

从观察来看，美国有记录的病例数最高。

![](img/8cda62f1c8739e1b59fe6a8779214690.png)

## 恢复的案例

美国也是恢复病例最多的国家。

![](img/3a97a4dc6b099a3def37588a174a5a7e.png)

## 活动案例

大多数活跃的病例发生在美国。

![](img/4fc58745e8b862d1fd2287b8b7169983.png)

## 严重/危急病例

美国的严重或危急病例数量最高。

![](img/c5a1f81204334bb8128e88392af2c551.png)

## 死亡

圣马力诺记录的死亡人数最多，其次是比利时，荷兰记录的死亡人数最少。

![](img/8a7cfbd3daeed19fe74d49e8cb0b2594.png)

## 我们的观察总结

我观察到，截至收集这些数据时，非洲的检测、危重病例、活跃病例、新增死亡和新增病例数量较多，其次是欧洲、亚洲和南美洲，澳大利亚的记录最少。

![](img/463be9dd8093521075d31a67fd9eb441.png)

这里是[链接](https://github.com/LoisChoji/Analysing_Covid19_DataSet)到源代码和数据集。

## 除 python 工具外，数据科学中的一些可视化工具包括:

*   Tableau:也被称为可视化大师。它使用起来既快又简单，因为不需要任何编程知识。
*   Power BI:它由微软开发，提供商业智能和分析需求。
*   Chartblocks:是一个在线工具，也不需要任何编码。它为实时提要、电子表格、数据库等准备可视化。
*   谷歌图表:创建的图表是交互式的，有些还可以选择缩放，检查数据。你应该试着使用它。
*   QlikView:它可以在关联仪表板的帮助下获取数据，为用户将数据保存在服务器的 RAM 中，并具有加速开发的功能。

感谢您的阅读！