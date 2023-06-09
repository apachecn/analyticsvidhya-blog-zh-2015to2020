# 识别纽约市的 EMS 志愿者活动

> 原文：<https://medium.com/analytics-vidhya/identifying-volunteer-ems-activity-in-nyc-95e4beb0899e?source=collection_archive---------17----------------------->

纽约开放数据公司 EMS 调度数据集的探索

# 数据

[NYC OpenData 的 EMS 事故派遣数据](https://data.cityofnewyork.us/Public-Safety/EMS-Incident-Dispatch-Data/76xm-jjuj)包括从 2013 年 1 月 1 日到 2018 年 12 月 31 日的 6 年记录:总共超过 200 万起事故。感兴趣的变量是时间(调度时间、到达现场的时间、在现场花费的时间)、呼叫类型(心脏病、生病、呼吸困难等)和处置代码(患者是否被送往医院、他们是否拒绝医疗护理、呼叫是否被取消等)。

# 取消呼叫分配

在纽约，志愿急救人员与 911 一起响应电话。如果志愿者单位对患者负责，911 调度记录会将其称为取消呼叫。虽然电话可以因为任何原因取消，不仅仅是因为志愿者单位的存在，理论上取消的电话应该在有大量志愿者活动的地区出现更大的比例。在这种情况下，我们希望在显示整个纽约市取消电话占总电话的比例的地图上看到的是，在志愿者团体活跃的地区，取消电话的比例更高。

这项研究特别关注晚上 7 点到午夜之间拨打的 911 电话。许多志愿者团体在晚上更活跃，因为他们的成员经常在白天工作。

作为参考，下面的地图显示了布鲁克林区几个活跃的志愿急救人员所在社区的邮政编码。

![](img/e239798725f0a5e6932bec95ab1ac448.png)

布鲁克林街区参考地图

下图显示了纽约市被取消电话的分布情况。布鲁克林区通话率较高的区域对应于上面参考地图上圈出的邮政编码。碰巧的是，包含 Bed-Stuy 志愿救护队(VAC)的 Bedford-Stuyvesant 是一个值得注意的区域，而 Park Slope 和 Prospect Heights 是另外两个区域，位于 Park Slope VAC 覆盖的半径范围内。

在斯塔滕岛，斯塔滕岛高速公路以南 80%的岛屿都在斯塔滕岛志愿心脏救护车的作业半径内，这也许可以解释那里的数字。

![](img/eea71b550997f59c23733d356a169977.png)

纽约市地图，显示每个邮政编码取消的 EMS 呼叫/总 EMS 呼叫的比率

蒂尔登堡(平地以南的狭长地带)、JFK 机场(平地以东的大区域)和中央公园(曼哈顿中部)是取消呼叫比例极高的三个区域，其细节需要进一步探索。

# 总体呼叫分布

当你看一下纽约市人均电话总数的分布时，这个数据的另一个有趣的方面变得显而易见。某些邮政编码平均每人会接到 1 个以上的电话。当你意识到某些邮政编码完全是商业性的，没有正式居民，但仍然可能需要 EMS 帮助那些不是居民但在任何给定时间仍然在其境内的人时，这就变得不那么令人困惑了。以 JFK 机场为例，整个邮政编码区有 16 个居民，但每天至少有数万人通过。

其他值得注意的地区是人均通话费率低得多的地方。除了协助拨打 911 电话的志愿者组织之外，还有另一个独立于 911 系统的志愿者组织 Hatzolah。他们有自己的私人紧急电话号码，并在纽约市更多的犹太人社区里操作几十辆救护车。在他们所在的地区，最近的 Hatzolah 成员通常要么是你的隔壁邻居，要么就在街区的另一边，这使得他们的响应时间无与伦比。正因为如此，那些地区的人们根本不打 911 急救电话，而是直接去找 Hatzolah。因此，预计这些地区的 911 紧急医疗服务事件数量将会减少。例如，看看布鲁克林区的博罗公园和弗拉特布什。他们确实比周围地区有更低的 EMS 呼叫率。

![](img/57aac57ddd96e5475b311b7f6d05ca1d.png)

纽约市地图，显示每个邮编的人均电话数(根据人口普查记录)

这个项目本质上是试图证明一个在实际观察数据之前就形成的假设。这一假设似乎是正确的:有证据表明，在有积极的志愿者 EMS 存在的地区，取消呼叫的比率上升。困难在于，数据集被清理以尽可能保持匿名——没有标识哪个 EMS 单位被分配给特定事件，也没有提及呼叫被取消的原因(或者如果是由另一个单位取消的，则是向谁取消的)。这意味着没有真正的方法来明确证明志愿者单位的参与。另一方面，这使得投机更加有趣！

下一步:不再仅仅将分析局限于晚上 7 点到午夜之间的通话，而是检查一天中的各个时间段，看是否存在相同的模式。如果是这样，必须做进一步的研究，以确定取消呼叫中与邮政编码相关的差异是否可能是由于志愿者单位以外的原因。此外，将联系各个纽约市志愿者单位以及纽约州志愿救护车和救援协会(NYSVARA ),以获取任何公开的统计数据和有关会员、活动、呼叫量等的一般信息。在一个完美的世界中，每个机构都应该拥有与纽约市 EMS 调度数据集相当的可共享数据(经过清理以保持患者和 EMTs 的匿名性)，但这不太可能。

有关该项目的更多细节，请参见 [GitHub](https://github.com/Chana-T/EMS_Volunteer_impact) 。

注意:这绝不是对纽约市志愿急救人员活动的权威性研究，它只是为了从一个有趣的数据集中获得一个有趣的视角。