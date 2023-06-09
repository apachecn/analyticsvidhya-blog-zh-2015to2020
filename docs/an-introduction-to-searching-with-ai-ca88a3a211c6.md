# 人工智能搜索简介

> 原文：<https://medium.com/analytics-vidhya/an-introduction-to-searching-with-ai-ca88a3a211c6?source=collection_archive---------24----------------------->

今年夏天，我在 Edx 上参加了 CS50 人工智能与 Python 入门课程。这是我学到的两个基本搜索算法的总结。

# 搭建舞台

好，我们想用人工智能搜索一些东西。但是我们在搜索什么呢？对于我要讲的搜索算法，我们搜索一些可以用一种特殊的图来表示的东西，这种图使用节点和边。一个**节点**是一个数据点，一个**边**显示各个数据点是如何连接的。

![](img/fccf4a21f195a76a020f13e752223a59.png)

节点图。这里，字母是节点，它们之间的线是边。

# 定义术语

在讨论算法本身之前，让我们先列出一些基本术语:

*   一个**状态**将是我们图上的一个节点。(甲、乙、丙、丁等。)
*   **目标状态**将是我们最终想要到达的节点。例如，我们可能想要到达节点 f。
*   **前沿**将是我们的人工智能知道并想要探索的所有节点。
*   **路径**将记录我们的 AI 已经检查过的所有状态，以便到达它正在检查的当前状态。

# 我们的第一个算法

我们看到的第一个算法是**深度优先搜索(dfs)** 。它将使用以下步骤:

1.  随机选择一个州或被分配一个起始州。
2.  将您选择的州通过边连接的所有州添加到边界*，如果这些州以前没有被检查过(我们不想检查两次)*。如果所选州在边界中，则将其从边界中移除。
3.  检查我们选择的州是否是目标州。
4.  如果是的话，我们就完了。返回所走的路。
5.  否则，从第二步开始重复最近添加到边界的状态*(这很重要)。*
6.  如果最后没有返回路径，也没有找到目标状态，则不存在可能的路径。

Dfs 最终会寻找一条可能的路径，直到它到达一个死胡同。如果没有达到某个目标状态，dfs 会备份到另一个状态可能被探测到的最后一个位置，并再次进入死胡同。为了了解这是如何工作的，让我们以上面的图表为例。我们称节点 F 为目标状态。

# dfs 的扩展示例

1.  我们给我们的 AI 节点 A 作为它的起点。
2.  它将 B、C 和 D 添加到边界中。
3.  它看到 A 不是目标。
4.  它现在看着 b。
5.  它把 E 加到了前沿
6.  它看到 B 不是目标。
7.  注意，E 是最近被添加到边界的。这是 dfs 的深度优先部分，因为我们先深入到 E 的“兔子洞”,然后再看其他东西。
8.  它没有给边疆增添任何东西。
9.  它看到 E 不是目标。
10.  它已经到达了一个死胡同，所以它返回到 C，C 现在是最近添加到边界的节点。
11.  它没有给边疆增添任何东西
12.  它认为 C 不是目标
13.  它看着 D
14.  它把 F 加到了前沿。
15.  它看到 D 不是目标。
16.  它看着 f。
17.  f 是目标，我们的 AI 已经完成了搜索！

# 我们的第二个算法:Dfs 的双胞胎

我们的第二个算法是**广度优先搜索(bfs)** 。它看起来非常类似于 dfs，只有步骤 5 不同。修订后的第五步如下所示:

5.否则，对最近最少添加到边界的州*重复第二步。*

这可能看起来微不足道，但它使我们的搜索呈现出完全不同的面貌。我们不去兔子洞，而是探索所有距离我们的起始节点一个距离的节点，两个距离的节点，等等。因此，我们挖的不是兔子洞，而是一个比一个浅的洞。诸如此类。只有当我们的洞都是 1 个单位深时，我们才回到第一个洞并扩大它。诸如此类。

我不会用这个搜索算法的另一个例子来烦你，但如果你想检查你的理解，我会给你一个顺序，如果 F 是目标状态，bfs 将会查看节点:A，B，C，D，E，F(这不是很好吗)。注意它在第一次探索 C 和 d 之前是如何不看 E 的。

# 关键要点

*   搜索算法将数据组织成**节点**和**边。**
*   **状态**是单独的节点
*   搜索算法有一个他们想要探索的状态的边界
*   ****深度优先搜索**从最近添加的州开始探索边界**
*   ****广度优先搜索**从最近添加的州开始探索边界**

# **进一步阅读**

**如果你想了解更多，我强烈推荐 edx 的 CS50 课程。如果你想继续问我或者有更多的问题，请发邮件到 yamanhabip@icloud.com**