# 决策树—简介

> 原文：<https://medium.com/analytics-vidhya/decision-tree-introduction-ca789f403d8d?source=collection_archive---------16----------------------->

![](img/5152ca18c4336f1ccea56fa3148e476f.png)

决策树学习是一种逼近离散值目标函数的方法，其中学习的函数由决策树表示。学习过的树也可以重新表示为 if-then 规则集，以提高人类的可读性。这些学习方法是最流行的归纳推理算法之一，并已成功应用于从学习诊断医疗案例到学习评估贷款申请人的信用风险的广泛任务。

# 决策树表示法

决策树通过从根到某个叶节点对实例进行排序来对实例进行分类，这提供了实例的分类。树中的每个节点都指定了实例的某个属性的测试，从该节点向下的每个分支都对应于该属性的一个可能值。通过从树的根节点开始，测试由节点指定的属性，然后向下移动对应于给定示例中的属性值的树分支，对实例进行分类。然后对以新节点为根的子树重复这个过程。

![](img/f3baad77273ae3a3cdb948e3d2b76c63.png)

典型的决策树

*(前景=晴朗，温度=炎热，湿度=高，风=强)* 将被分类到该决策树的最左边的分支，并且因此将被分类为负实例，因为该树给出的输出为“否”

# 使用决策树解决适当的问题

已经发现许多实际问题符合决策树的特征，例如学习根据疾病来识别医疗患者，根据原因来识别设备故障，以及根据拖欠付款的可能性来识别贷款申请。这类问题的任务是将示例分类到一组离散的可能类别中，通常称为分类问题。

# 基本的决策树算法

大多数为学习决策树而开发的算法都是核心算法的变体，核心算法采用自上而下、贪婪地搜索可能的决策树空间。

我们的基本算法 ID3 通过自顶向下构建决策树来学习决策树，从问题“应该在树根处测试哪个属性？”为了回答这个问题，使用统计测试来评估每个实例属性，以确定它单独对训练示例进行分类的效果如何。最佳属性被选择并用作树的根节点处的测试。然后为该属性的每个可能值创建根节点的后代，并将训练示例排序到适当的后代节点。然后，使用与每个后代节点相关联的训练示例来重复整个过程，以选择在树中的该点处测试的最佳属性。这形成了对可接受的决策树的贪婪搜索，其中算法从不回溯以重新考虑早先的选择。

ID3 算法的核心选择是选择在树中的每个节点测试哪个属性。我们希望选择对分类示例最有用的属性。什么是衡量一个属性价值的好的量化标准？我们将定义一个统计属性，称为*信息增益*，它衡量给定属性根据目标分类将训练示例分离的程度。ID3 使用这个信息来获得度量，以便在生长树的每一步在候选属性中进行选择。

# **熵测量实例的同质性**

为了精确地定义信息增益，我们首先定义一个在信息论中常用的度量，称为*熵，*它描述了任意样本集合的不纯度。给定一个集合 S，包含一些目标概念的正例与反例，S 相对于这个布尔分类的熵为

![](img/120ba0b425592ea889d5d87a2c2c0d47.png)

熵公式

其中 Pc 是 S 中正例的比例，Pc2 是 S 中负例的比例。

让我们假设 S 是一些布尔概念的 14 个例子的集合，包括 9 个正例子和 5 个负例子，那么熵 S 相对于这个布尔分类是:

![](img/2e0d13357c950c030316a1bb645cbe1e.png)

> 如果 S 的所有成员属于同一个类，则熵为 0。

对于多个类别，熵可以重写为:

![](img/07d7e449cf72b829d01c8a10a37fdddb.png)

# 信息增益衡量熵的预期减少

给定熵作为训练样本集合中杂质的度量，我们现在可以定义属性在分类训练数据中的有效性的度量。我们将使用的度量，称为*信息增益，*仅仅是根据属性划分示例所导致的熵的预期减少。更准确地说，是属性 A 的信息增益 Gain(S，A ),即创建决策树最顶端节点的算法。应该首先测试树中的哪个属性？ID3 确定每个候选属性的信息增益，然后选择具有最高信息增益的一个。

决策树有助于我们在现实生活中做出决策。例如，如果你必须选择一家餐馆与家人或朋友共进晚餐，你会考虑多种因素，如距离、评级、评论、美食、预算等，以做出决定。你还会优先考虑一个属性，比如距离，你希望餐馆在 5 公里范围内，接下来你会喜欢那些提供中国菜的餐馆，你会不断问不同的问题，以最终决定选择哪家餐馆。你可能已经注意到，决策树也以同样的方式工作。