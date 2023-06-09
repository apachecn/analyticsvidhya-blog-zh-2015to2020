# Python 数据结构

> 原文：<https://medium.com/analytics-vidhya/python-data-structures-75e207966bed?source=collection_archive---------26----------------------->

# 介绍

Python 是一种松散类型的编程语言。这意味着，在 python 中，我们可以声明变量而不用提及或指定它的数据类型。根据存储在该变量中的数据，它将自身转换为该类型。比如 a = 1；那么变量 a 的类型是 int。a = 'Hi '这里，变量 a 的类型是 string。我们知道 python 有一系列强大的库。因此，为了充分利用它的优势，让我们先来看看什么是 **Python 数据结构**以及它的重要性。

# 什么是 Python 数据结构？

python 中的数据结构基本上是一种数据类型。Python 有一组数据类型，用于存储相应类型的数据，并在以后对其进行处理以获得一些结果。了解数据的状态有助于数据科学家和 Python 开发人员更有效地处理变量。因为不同类型数据需要不同种类的操作。例如，我们不能在**整数**类型的变量中追加值。
让我们看看 python 为我们提供的数据结构类型:

主要有两种类型的数据结构:

1)原始数据结构

2)非原始数据结构

让我们深入研究这两种类型；

1)原始数据结构:
原始数据结构是语言预定义的数据类型。这包括本质上固定的基本数据类型，变量可以存储该特定类型的值。以下是一些原始数据结构:

a)整数:
整数支持从–n 到 n 的非十进制数。也就是说，它支持负值、零和正值。
例:
a = 7

b) Float:
Float 支持小数。这也支持负数、零和正数。
例:
a = 2.5

c)布尔:
布尔存储值为真或假(0 或 1)。它广泛用于存储真值或假值。
示例:
男=真

d)字符串:
字符串用于存储句子、单词、字符等。
示例:
name='Parth '

2)非原语数据结构:
非原语数据结构是一种用户自定义的数据类型。python 提供了一些数据结构。让我们来看看:

a)阵列:

数组是具有相同数据类型的数据的集合。在 python 中，我们必须使用内置库来创建数组。像这样的库:

I)阵列

二)NumPy

b)列表:

该列表是不同异构数据类型的数据的集合。列表是一个可变的数据结构，这意味着你可以改变它的值而不改变序列。我们可以通过索引来访问这个列表。

c)元组:
元组是像列表一样的数据序列的集合。但是元组是不可变的。也就是说，一旦创建了元素，我们就不能添加、编辑、删除它。存储我们不想被操纵的信息是非常有用的。

d)集合:
集合是无序但可变的数据集合。集合用于存储唯一值。在集合中，没有重复的项目或值。适用于大型数据集。

e)字典:
字典是一种键值存储结构。字典中存储的值是基于键值的。我们可以成对存储数据。关键字用于检索、编辑字典中的数据。

f)文件:

文件可以是任何数据类型。我们通常使用文件类型来存储数据，以便以后使用。或者从现有文件中提取数据。

除了数组以外，所有这些非原始数据结构都可以包含各种数据类型的数据。事实上，我们也可以将列表存储到元组中，将元组存储到字典中，反之亦然。

我附上一个链接到我的 GitHub repo，其中包含这个非原始数据结构基本操作的文件[点击这里](https://github.com/ParthNipunDave/Python-Data-Structure-basic)。

# 关闭

我希望你喜欢这篇文章。请给喜欢，如果你真的喜欢这个视频，你的评论总是受欢迎的。与你的伙伴分享这个！下一篇文章再见，再见。