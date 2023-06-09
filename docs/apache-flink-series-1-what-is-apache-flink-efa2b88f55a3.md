# Apache Flink 系列 1 —什么是 Apache Flink

> 原文：<https://medium.com/analytics-vidhya/apache-flink-series-1-what-is-apache-flink-efa2b88f55a3?source=collection_archive---------10----------------------->

![](img/352266837f8f909a97f624b028300972.png)

在本帖中，我将尝试解释**什么是阿帕奇 Flink** 、**什么是用于**以及阿帕奇 Flink 的**特性。**

# 什么是阿帕奇 Flink？

*   Apache Flink 是一个分布式流处理器，用于实现有状态或无状态处理应用程序
*   或者换句话说，Apache Flink 是一个针对大数据的分布式处理引擎，它对绑定和非绑定数据流执行有状态或无状态计算。Apache Flink 部署在各种集群环境中，用于对不同大小的数据进行快速计算。
*   简而言之:有了 Apache flink，您就有了包含主机器和从机器集群，您将您的工作交给主机器，然后从机器将以高效和容错的方式完成您的工作。

在传递“Apache Flink 的用例”之前，让我指出**有状态流应用意味着什么？**

**什么是有状态流处理？**

*   有状态流处理是一种用于处理无限事件流的应用程序设计模式。
*   这意味着数据是作为连续的事件流创建的
*   你可以想到网站或移动应用程序中的用户交互、订单的下单、服务器日志或传感器测量。所有这些都是事件流

**有状态流处理是如何工作的？**

*   当应用程序收到事件时，它可以执行任意计算，包括从状态读取数据或向状态写入数据。
*   状态可以在许多不同的地方存储和访问，包括程序变量、本地文件或嵌入式或外部数据库

现在这又引出了另一个问题:)**什么是状态？**

**什么是状态？**

*   在高层次上，我们可以将状态视为 Flink 中操作符的记忆，它可以记住关于过去输入的信息，并可用于影响未来输入的处理。
*   状态是计算过程中产生的数据信息，在 Apache Flink 的容错、故障恢复和检查点中起着非常重要的作用

你可能觉得 Flink 里什么是算子，我会在另一个帖子里解释。现在，我可以说运算符与流有关。

# Apache Flink 的用例

*   实时推荐(当顾客浏览零售商网站时推荐产品)
*   模式检测或复杂事件处理(信用卡交易中的欺诈检测)
*   异常检测(检测入侵企图——访问受限场所—)
*   传统同步方法的替代方案。不同存储系统中的数据。在传统方式中，存在称为 ETL(提取、转换、加载)的周期性作业。然而，它们不能满足许多用例的延迟要求。
*   Flink 也可以用于数据分析应用。

# Apache Flink 的特性

Flink 的特点:

*   事件时间和处理时间语义

事件-时间语义提供一致和准确的结果，尽管事件是无序的

处理时间语义(处理时间是指执行相应操作的机器的系统时间)可以用于延迟非常低的应用程序

*   恰好一个状态一致性保证
*   每秒处理数百万个事件时的毫秒延迟
*   使用方便
*   最常用存储系统的连接器，如 Kafka、Cassandra、ElasticSearch
*   能够在极少停机的情况下运行
*   它也可用于批处理机。