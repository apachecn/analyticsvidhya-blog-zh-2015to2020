# 数字索引和切片

> 原文：<https://medium.com/analytics-vidhya/numpy-indexing-and-slicing-987706ee33f8?source=collection_archive---------16----------------------->

![](img/8360305582f8edcf6b88053911095b6f.png)

在上一篇文章中，我们已经看到了如何使用 numpy 创建数组。一旦创建完成，我们必须能够访问它们。在本文中，我们将看到如何通过索引、数组切片和其他一些创建数组的函数来访问数组。

## 数字索引:

我们可以通过引用数组的索引号，通过数组索引来访问数组。

NumPy 中的索引从 0 开始。

![](img/65f00421a5f5229727a45e881505e0d5.png)

我们还可以访问和添加数组的元素；

![](img/48b9d58b55688bec7bbf4df9102802ec.png)

## 访问二维数组:

要访问二维数组，我们可以使用逗号分隔的整数来表示元素的维度和索引。

![](img/433f684124ddea302e9faa0e0c368068.png)

## 访问三维数组:

要访问三维数组，我们可以使用逗号分隔的整数来表示元素的维度和索引。

![](img/b1450b4bb800ef87c2e7d3699a858e3b.png)

## 负索引:

我们使用负索引从末尾访问数组。

![](img/baea92681dd31e12d2fc234c659789bb.png)

## 数字切片:

python 中的切片意味着将元素从一个给定的索引带到另一个给定的索引。

我们传递切片而不是索引；[开始:结束]

如果我们不通过 start，它将被视为 0。

如果我们不传递 end，它会取那个维度中数组的长度。

如果我们不通过步骤，它被认为是 1。

![](img/6d4ba076013dfa37571d05de51148ea7.png)

包括开始索引，不包括结束索引。

## 负切片:

为了从末端切片元素，我们使用负切片。

使用减号运算符从末尾开始引用索引。

![](img/09df859e4952fd05e1e0e3fd4c8fd7f9.png)

## 步骤:

使用步长值来确定切片的步长。

![](img/3e2624fb03c33c8937980caf33bb406d.png)

**创建数组涉及的函数:**

**重塑:**

整形意味着改变数组的形状。
一个数组的形状是每个维度中元素的个数。
通过重塑，我们可以添加或删除维度，或者改变每个维度中的元素数量。

![](img/3ef496d4638ee14267dddeb81d288175.png)

**形状:**

数组的形状是每个维度中元素的数量。
NumPy 数组有一个名为 shape 的属性，它返回一个元组，每个索引都有相应元素的数量。

![](img/e37f6e7a4e8836a90066c6d3e6091d74.png)

**Linspace:**

该函数生成彼此等距的元素。

![](img/79770f4ad6b5e6cbd8c4d1dbcbfae94d.png)

**排列:**

该函数返回给定间隔内均匀分布的值。包括开始但不包括停止的时间间隔。

![](img/260c7d2890b615fce4b1fd90fc6d757c.png)

**零:**

此函数用零填充参数中指定的形状。

![](img/89616aaca8795f3bf93e95838d52521c.png)

**个:**

此函数用 1 填充参数中指定的形状。

![](img/df0db9e628715e236aa31f708b2e16b1.png)

**单位矩阵:**

此函数仅使用对角线元素填充参数中指定的形状。

![](img/3ac94b09f7820d35b457f03a407b3b66.png)

**对角矩阵:**

这个函数提取并构造一个对角线数组。

![](img/cde743b06b13e688ccba00642b427930.png)

就这样，我们来到了这篇文章的结尾。

快乐编码…😊😊😊