# 调优 Apache Spark

> 原文：<https://medium.com/analytics-vidhya/tuning-apache-spark-e7ae5e02e9d4?source=collection_archive---------2----------------------->

![](img/8a5e54cda02d48aa00e8c46f1c87fc32.png)

在当今的数据处理架构中，由于打包的 Spark 及其 API，开发管道相当简单。但有趣和具有挑战性的部分是优化系统，以实现最佳性能和更快的处理吞吐量。在分布式系统中优化事情是一个棘手的部分，因为它并不总是通过调整配置的线性改进的函数。在进行优化之前，最重要的事情是知道你正在构建的系统主要是用来做什么的。优化不是童话，因为它是以解决权衡取舍为代价的，这些权衡取舍不会对你的系统的最终目标产生重大影响。底线是你必须知道你在优化什么。让我们讨论一些常见的调优机会，以提高 spark 处理的性能。

# **遗嘱执行人**

首先，这是 spark 处理中最容易被利用和误解的部分。当被赋予优化 Apache spark 应用程序的任务时，对此进行调优是最容易实现的。

**总核心数**

越多越好，所以分配我们所有的，似乎是显而易见的，对不对？大概不会！这是为执行器分配的用于执行其任务的系统核心数量。在为此得出一个数字时，您必须考虑:

1.  并行执行的任务越多，垃圾收集开销就越大。
2.  所有节点上都会有几个内核被操作系统进程和其他 HDFS 守护进程占用。

**总遗嘱执行人**

这是资源管理器运行或提交的 java 流程实例的数量。假设在一个集群中有 X 个可用的内核，并且您希望每个执行器有 Y 个内核，那么最终会有 X/Y 个执行器。在提交任务时，你必须考虑需要给 yarn 应用程序主程序分配一个执行程序。

**执行者记忆**

这是集群上的可用内存(节点间可用内存的总和减去操作系统和其他文件系统守护进程所需的内存)。假设每个节点上有 11 GB 内存，每个节点上运行 5 个执行器，那么您的操作系统和其他进程可能需要 1 GB 内存。您可以为每个执行器分配 10/5= 2GB 的内存。虽然 Yarn 使用了大约 10%的总执行器内存，但是您并没有有效地获得每个执行器 2 GB 的内存来进行处理。

# 垃圾收集工

Spark 流中的垃圾收集是一个关键问题，因为它以流或微批处理的形式运行。特别是在流的情况下，由于在运行时处理大量的对象，它会严重影响标准的 Java JVM 垃圾收集。这导致频繁的暂停，从而增加了实时应用的等待时间。这里的一个选项是使用并发标记清除(CMS)垃圾收集器作为驱动程序和执行程序的有效步骤，它通过与应用程序并发运行垃圾收集来减少暂停时间。限制创建的对象数量对于减少 GC 开销和获得更好的性能是非常重要的。

为了获得更大的堆大小和更好的性能以满足高吞吐量和低延迟的需求，建议使用 G1GC 垃圾收集器。然而，当 G1 垃圾收集器试图收集某些区域的垃圾时，它无法找到可以将活动对象复制到的空闲区域。这种情况称为撤离失败，通常会导致完全 GC。显然，G1 GC 中的全 GC 比并行 GC 更糟糕，所以我们必须尽量避免全 GC，以获得更好的性能。

为了避免 G1 GC 中的完全 GC，有两种常用的方法:

1.  减小`**InitiatingHeapOccupancyPercent**`值(默认值为 45)，让 G1 GC 在更早的时间开始初始并发标记，这样我们就有更大的机会避免完全 GC。
2.  增加`**ConcGCThreads**`值，让更多线程进行并发标记，这样我们可以加快并发标记阶段。请注意，该选项也可能会占用一些有效的工作线程资源，这取决于您的工作负载 CPU 利用率。

与 CMS 收集器类似，G1 根据堆的填充量启动并发处理。

# **并发任务执行**

当您希望您的应用程序更快地执行任务，并使处理后的数据在更短的周转时间内可用时，这是一个需要关注的重要领域。你必须并行执行任务，以确保充分利用计算能力，并确保集群得到最佳利用，从而确保你不会轻易降低计算成本。这里要考虑的一个事实是，要有最佳的分区大小，这样它们既不会太大，因为没有足够的资源而不允许您并行执行，也不会太小，最终会创建许多小任务，并在 IO 上花费大量成本。

**spark . SQL . files . maxpartitionbytes**和**spark . SQL . files . opencostinbytes**是需要配置的参数，以便让 spark 计算出集群上的理想分区大小。

分区大小的 Shuffle 属性—**spark . SQL . shuffle . partitions**属性确定每次 shuffle 操作时分区的大小。理想情况下，大小应该与文件的大小成正比。

# 序列化

序列化在优化任何分布式系统中扮演着重要的角色，对于 spark 性能调优也是如此。如果选择的序列化程序类型需要更长时间来压缩和转换对象，序列化会直接导致系统变慢。当我们必须选择特定的序列化格式时，主要是在易用性和性能之间进行选择。Spark 支持 Java 和 Kyro 序列化程序，两者共存是有原因的。

Java 序列化程序 Apache Spark 使用的默认序列化程序。它使用 Java 的对象输出流框架。该类需要用 java.io.Serializable 实现，它提供了很好的灵活性，可以无缝地为大多数格式工作。代价是它的速度较慢。

Kyro 序列化器——工作速度比默认的 java 序列化器快 8 倍左右，但缺点是它不支持所有的数据格式，需要您事先注册您的类。重写默认序列化程序-

spark conf . set(" spark . serializer "，" org . Apache . spark . serializer . kryoserializer ")

注意，适当的缓存有助于避免反序列化开销。

# 执行者动态分配

Spark 配备了弹性伸缩功能，可以根据集群中资源的需求和可用性动态添加或删除执行器，从而更好地优化计算能力。[**spark . dynamicallocation . enabled**](https://mallikarjuna_g.gitbooks.io/spark/content/spark-dynamic-allocation.html#spark_dynamicAllocation_enabled)是允许我们启用资源动态分配的属性。当 spark 上下文被初始化时，动态分配开始起作用，并且有两个策略，即当有未决任务时请求新的执行器并增加执行器的数量的向上扩展和移除已经空闲了[**spark . dynamicallocation . executoridletimeout**](https://mallikarjuna_g.gitbooks.io/spark/content/spark-dynamic-allocation.html#spark_dynamicAllocation_executorIdleTimeout)秒的执行器的向下扩展。注意，要启用动态分配，需要启用 spark 外部洗牌服务，这是由[**spark _ shuffle _ service _ enabled**](https://mallikarjuna_g.gitbooks.io/spark/content/spark-ExternalShuffleService.html#spark_shuffle_service_enabled)属性控制的。

# 接近存储的处理

根据构建分布式系统的经验法则，处理最希望靠近存储，以降低读取延迟和磁盘 IO 成本，并减少数据移动。Spark 中的位置配置有助于从五个受支持的设置中选择最合适的设置。

*   PROCESS_LOCAL —数据和处理在同一个 JVM 上本地化
*   NODE_LOCAL —数据和处理在同一个节点中，但在不同的执行器上。这个级别比前一个级别慢，因为它必须在处理的数据之间移动数据
*   RACK_LOCAL —数据位于处理节点之外的其他节点，但两个节点都在同一个机架上。显然，这里的数据是通过网络传输的。
*   NO _ PREF-表示没有地点偏好。
*   任何—数据在别处，但不在同一机架上。

但是，重要的是要知道，位置级别的工作方式类似于分层优先级，因此，如果在某个级别没有获取数据，spark 将在等待由 **spark.locality.wait** 设置设置的超时窗口后，寻找更低的位置级别。

# 公平调度

默认情况下，Spark 以 FIFO 顺序执行任务，但为了使独立任务实现更好的并行性，建议将 schedular 设置更改为公平调度，以便以循环方式挑选和执行任务。根据任务的互斥性创建任务投票。

# 使用广播变量和缓存

Spark 中的广播变量是在执行器之间共享只读变量的一种方式。如果没有广播变量，这些变量将被传输到每个转换和动作的每个执行器，这将导致网络开销。但是，对于广播变量，它们只被发送一次，并被缓存在所有执行器上以供将来参考。

只要可行并经过优化，就要留意数据帧的缓存需求，因为这将降低 GC 开销和内存利用率。

# 结论

重申了解您正在构建的系统的最终目标和最期望的性能属性是非常重要的。这是关于知道你想要什么，你可以妥协什么。有时候，为了实现一个性能目标，你需要牺牲另一个。