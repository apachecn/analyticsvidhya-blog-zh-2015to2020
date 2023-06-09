# scikit-learn 的数据预处理技术

> 原文：<https://medium.com/analytics-vidhya/data-preprocessing-techniques-with-scikit-learn-f403e72d532?source=collection_archive---------19----------------------->

![](img/2c42e4b4c30345941a224842a390a625.png)

scikit-learn 库包括用于数据预处理和数据挖掘的工具。它是通过语句`import sklearn`导入 Python 的。

# 1.使标准化

数据可以包含各种不同的值。当数据取任何范围的值时，都很难解释。因此，我们应该将数据转换成标准格式，以便于理解。数据的标准格式是指 0 均值和单位方差。这是一个简单的过程。对于每个数据值， *x* ，我们减去数据的总体平均值μ，然后除以总体标准偏差σ。

scikit-learn 数据预处理模块被称为`sklearn.preprocessing`。这个模块中的函数之一，`[scale](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.scale.html#sklearn.preprocessing.scale)`，将数据标准化应用到一个 NumPy 数组的给定轴上。

如果出于某种原因，我们需要跨行而不是列标准化数据，我们可以将`scale`函数中的`axis`关键字参数设置为 1。在分析观测数据而非要素数据时，可能会出现这种情况。

# 2.数据范围

在本节中，我们将学习如何将数据值压缩到指定的范围。

## 范围缩放

我们可以通过将数据压缩到一个固定的范围来扩展数据，而不是将其标准化。其中一个常见的用例是将数据压缩到[0，1]范围内。

这是一个分两步走的过程。

*   对于给定的数据值，我们首先计算该值相对于数据的最小值和最大值的比例
*   然后，我们使用该值的比例缩放到指定的范围。

![](img/bc9ff00c3dd7fc164f543ccd7dbab498.png)![](img/7606e90a9662da5ac5d4086407c54b78.png)

## scikit 中的范围压缩-学习

在前一章中，我们使用了一个函数`scale`来执行数据标准化，剩下的章节将集中讨论如何使用其他的 transformer 模块。

`[MinMaxScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html#sklearn.preprocessing.MinMaxScaler)`转换器使用之前的公式执行范围压缩。默认范围是[0，1]。

`MinMaxScaler`包含一个名为`fit_transform`的函数，允许它接收输入数据数组，然后输出缩放后的数据。该函数是对象的`fit`和`transform`函数的组合，前者接受一个输入数据数组，后者根据来自`fit`函数输入的数据转换一个(可能不同的)数组。

# 3.稳健缩放

离群值是指距离其他数据点非常远的数据点。

前两章的数据缩放方法都受到异常值的影响。数据标准化使用每个要素的平均值和标准差，而范围缩放使用最大值和最小值，这意味着它们都容易受到异常值的影响。

我们可以通过使用数据的中位数和[四分位间距(IQR)](https://en.wikipedia.org/wiki/Interquartile_range) ，稳健地扩展数据，即避免受异常值的影响。它们不受离群值的影响。对于缩放方法，我们只需从每个数据值中减去中值，然后缩放到 IQR。

# 4.数据插补

现实生活中的数据集通常包含缺失值。如果数据集缺少太多的值，我们就不使用它。但是，如果只有几个值缺失，我们可以进行数据插补，用其他值替代缺失的数据。

-* *数据插补有几种方法。在 scikit-learn 中，`[SimpleImputer](https://scikit-learn.org/stable/modules/generated/sklearn.impute.SimpleImputer.html#sklearn.impute.SimpleImputer)`转换器执行四种不同的数据插补方法。

*   使用平均值
*   使用中间值
*   使用最频繁的值
*   使用常数值

通过在初始化`SimpleImputer`对象时使用`strategy`关键字参数，我们可以指定不同的插补方法。

数据插补不限于这四种方法。

还有更高级的插补方法，如[k-最近邻](https://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm)(根据 kNN 算法的相似性分数填充缺失值)和 [MICE](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3074241/) (应用多重链式插补，假设缺失值随机分布在观察值中)。

# 5.主成分分析

当数据集包含这些类型的相关数字特征时，我们可以执行[主成分分析(PCA)](https://en.wikipedia.org/wiki/Principal_component_analysis) 进行降维(即减少数据阵列中的列数)。它可以提取数据集的主成分，这些主成分是不相关的潜在变量集，包含原始数据集中的大部分信息。

我们可以在 scikit-learn 中用一个转换器将 PCA 应用于数据集，在本例中是 PCA 模块。在初始化`PCA`模块时，我们可以使用`n_components`关键字来指定主成分的个数。默认设置是提取 *m - 1* 个主成分，其中 *m* 是数据集中特征的数量。