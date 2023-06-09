# 分布式数据处理新手编年史—第一季第 1 集:试点

> 原文：<https://medium.com/analytics-vidhya/a-newbies-chronicles-on-distributed-data-processing-s01e01-pilot-5c76fb052db5?source=collection_archive---------22----------------------->

作为一个刚刚从大学毕业并加入 Intuit 数据工程组织的人，获得分布式系统和相关技术栈的实践经验是一种特权。虽然我以前尝试过多个领域和开源项目，但是这个领域我还没有涉足太多。

我最近开发了一项新功能，通过这项功能，我学到了很多理论和实际的用例，并伴随着激烈的咆哮！我将试着在这一系列文章中结合我的学习和经历，希望这是一篇有趣且引人入胜的文章。

# **更多一点的上下文……**

分布式数据处理是指在分布式连接节点集群上处理数据。通常，数据非常庞大，分布式体系结构确保我们可以通过改进节点群集(节点数量、其内存、内核等)来相应地扩展不断增长的数据量。

![](img/fe2530ed28bfef792cd54b3b33d28cea.png)

多影克隆忍术:火影忍者版本的分布式架构。主火影忍者(名字节点)控制克隆体，每个克隆体(数据节点)做委托的任务！

为了支持这样的用例，我们需要一个合适的技术堆栈，这是众所周知的堆栈之一:

*   **Spark** —提交分布式作业
*   **Scala**——执行 spark 查询(这可以用其他 Spark 支持的语言来代替，比如 Python、Java 或 R)
*   **Hadoop** —利用分布式处理的工具，如 Hadoop 文件系统(HDFS)、资源分配和管理(YARN)等
*   **Hive** —读取、操作和存储分布式数据仓库中的数据

还有更多组件，如 Kafka、ZooKeeper 等，它们一起完成了分布式数据架构的图景。然而，这可能会有点令人困惑——所以让我们先从这些组件开始。

# 所需的能力

现在，让我解释一下我们需要的功能和最近增加的支持。作为数据工程组织，我们在拥有现成可消费数据的源团队和希望对如此大规模的数据运行高性能查询以获得洞察力的分析师之间充当桥梁。

在这种特殊的情况下，我们必须每天从源团队支持大约 16TB 的增量数据，并在第二天提供给分析师。为了给出一个更好的画面，让我们说我们的源团队(比如说，尼克·弗瑞)有复仇者联盟的数据——分析师(比如说，神盾局的科学家)想从中获得洞见。

我们的一个 hive 表中有一些现有数据，我们每天都在接收增量数据——我们需要将这些增量数据合并到我们的 hive 中。可能是这样的:

历史数据——“最初的复仇者联盟”。唷，好时光！

和流入的增量事件，看起来像这样:

*   **更新** —即更新+插入

增量更新+插入—直到“复仇者联盟 4：终局之战”:)

*   **删除**:

增量删除—也是由于“复仇者联盟 4：终局之战”(sob)

**预期的结果？**将增量数据与现有历史数据合并后，我们需要将这些物化数据存储回同一个 Hive 表中:

在应用插入/更新/删除之后，即在“复仇者联盟 4：终局之战”结束之后，最终的物化数据。也成为下次合并的历史数据！

当然，确切的顺序并不重要——因为 Hive 中没有主键的概念，在我们的处理过程中数据可能会被打乱。然而，最终数据的精确快照需要逐行匹配。

这种能力的主要限制不是合并本身，而是使它能够适应如此巨大的数据量:**~ 16TB/天**！

# **犯罪的伙伴:星火 ft。Scala**

我们的大部分工作都依赖于 Spark & Scala。在任何 spark 处理工作中，人们通常使用 3 种最基本的对象类型:

*   **RDD** (弹性分布式数据集)——不可变的数据块，没有模式属性
*   **DataFrame** —不可变的数据块，但是通过列将一些模式归属于它
*   **数据集** —同样是不可变的数据块，但通常是强类型的

我来自一个集中处理的背景，熟悉 Python3 Pandas 的数据帧，我以为我理解 Spark 数据帧是如何工作的——但是我大错特错了！🙃

Spark 数据帧的核心功能非常不同，因为它们分布在一个节点集群中。结果没有“*索引*的概念，所有 Spark 交互默认为“*懒*”。也就是说，直到一个“*动作*开始，它们才被执行。

例如，让我们看看这个:

错误调试 Spark 作业的 101 种方法——欢迎来到我的 Ted 演讲！

如果您像我一样来自同步编程背景，您可能会犯错误，试图解释记录的时间戳，并根据时间差对步骤进行基准测试。如果是熊猫数据帧的 Python3 代码，你就对了。 ***但是不对，这里不一样。***

![](img/12f04cafcee887254bc7adcbd9ec485b.png)

如果您尝试在 spark-shell 中运行类似的东西，您会更愿意查看时间戳之间的差异，并(不正确地)推断:

*   从 Hive 读取现有数据几乎是即时的
*   从 json 读取增量数据几乎不需要时间
*   联盟也很快！😮

![](img/66fbc415ed959f2a906e76772c934a76.png)

前三步执行得相当快，不是吗？

*   酪酪为什么。为什么 *count()* 运算要花很多时间？如果使用节点集群，应该会花费更少的时间！

![](img/39eb2073fb9aa220e9a4559457b918b9.png)

当 count()仍在执行时，重新评估人生决策

是的，那是真的——只是我们还没有理解 Spark 的神秘工作方式。因此，Spark 会根据我们的代码准备一个逻辑执行计划。所有的"*懒惰的*"部分从我们的代码中不断堆积，只有在遇到"*动作*时才会在我们的集群上执行。

在我们的例子中，*动作*是*计数()*操作。在后台，所有的步骤如读取 hive 数据、读取增量数据、执行 join/union；我们本质上都是懒惰的，只是不停地堆积。只有在遇到 *count()* 操作时，所有这些步骤才会基于 Catalyst 的逻辑执行计划实际执行。

这就是我们误解结果的原因——实际上，执行上述所有步骤是为了给出最终的时间戳差异；而不仅仅是 *count()* 操作。如果你是一个新手，这是一个让你感到惊讶的提示。😉

![](img/0258bcdf501210c9d3bbb773f89719f0.png)

那个痛苦而缓慢地实现的时刻

记住这些微小的细节，让我们进一步进入下一集第一季第二集，看看我们是如何构建 POC 并进行优化的。