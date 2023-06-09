# 二氧化碳排放估算模型

> 原文：<https://medium.com/analytics-vidhya/carbon-dioxide-emission-estimator-model-7d4721a330c2?source=collection_archive---------23----------------------->

这个故事旨在使用来自 [UCI 机器学习库](https://archive.ics.uci.edu/ml/index.php)的数据集建立一个简单的二氧化碳排放估算模型。

![](img/7cd8717522c851bbbc24f5225d878e23.png)

这是我所做的一个样本项目的一部分，该项目旨在找出公路旅行中二氧化碳排放量的估算。我使用的数据集来自 UCI 机器学习知识库，可以直接从[这里](https://archive.ics.uci.edu/ml/machine-learning-databases/auto-mpg/auto-mpg.data)下载。

首先，我们导入了[类型](https://docs.python.org/3/library/typing.html)，它为类型提示提供了运行时支持。

```
import typing
```

根据美国环境保护署(E.P.A)的数据，汽油每加仑产生的二氧化碳克数为 8，887 克/加仑，柴油每加仑产生的二氧化碳克数为 10，180 克/加仑。现在，我们使用 auto-mpg.data 作为数据，并将其转换为 pandas dataframe。

因为我正在处理的这个数据集有许多列，其中一列包含关于汽车名称的信息，另一列包含汽车型号等信息。我想做一个功能，提供汽车名称的建议，类似于用户将输入的汽车名称。

当用户输入汽车名称时，调用该函数，该函数将根据用户输入提供合适的建议。

```
choose_car = input()
car_suggestions(choose_car)
```

在这之后，用户在建议的汽车/车辆中选择一个，并且还输入要行驶的距离，但是在此之前，应该定义一个函数，该函数应该采用两个参数，即行驶的距离和来自建议的汽车的索引号，并且应该返回行驶的 co2 排放作为输出。所需的功能如下。

现在，用户从建议中输入汽车名称，还输入行程距离，这些都是上述功能的输入。

# **结论和结果**

通过这个我们可以得到一次公路旅行的二氧化碳排放量。针对 co2 排放(即 carbon_emission)的函数仅使用汽车 mpg(英里每加仑)值，这不足以计算碳排放值，但如前所述，这是一个简单的项目，因此我仅考虑 1 个参数。其他参数，如车龄、交通、位置、发动机类型、车辆类型(轻型、重型)等。在二氧化碳排放中扮演重要角色。

欢迎在下面评论。