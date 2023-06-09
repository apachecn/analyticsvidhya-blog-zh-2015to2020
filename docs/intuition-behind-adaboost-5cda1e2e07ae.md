# AdaBoost 背后的数学直觉…

> 原文：<https://medium.com/analytics-vidhya/intuition-behind-adaboost-5cda1e2e07ae?source=collection_archive---------7----------------------->

![](img/e857da2a0770af16c915f1bb50dcbfad.png)

助推

助推技术最近在 Kaggle 竞赛和其他预测分析任务中有所上升。我将尽可能清晰地解释 Boosting 和 AdaBoost 的概念

# 🔒装袋模型的局限性

> 在随机森林的情况下，每个单独的树都是独立的，因此它不关注前一个树做出的错误或预测，这实际上有助于下一个树关注数据的某些方面，以便提高整体性能。
> 
> 例如，如果有一个特定的数据点被第一棵树错误地预测，那么第二棵树很有可能也预测错误，这对于随机森林中的所有其他树都是有效的。因此，这意味着这些树彼此之间没有关系，并且在不同的数据子集上并行工作。因为每棵树都不会向其他树学习。
> 
> 现在，我们有没有办法从以前的树中学习，以提高整体性能。这在随机森林中是不可能的，因为随机森林中的树是平行建造的。因此，我们来看看另一种助推技术。在 boosting 的情况下，模型相互依赖，并且每个即将到来的模型试图通过处理前一个模型的不正确预测来减少整体误差。因此，这些模型是按顺序建立的。

# 💥增压:

> 定义:术语“增强”指的是将弱学习者转化为强学习者的一系列算法。
> 
> 在我们继续之前，这里有另一个问题:如果第一个模型错误地预测了一个数据点，然后是下一个(可能是所有模型)，组合预测会提供更好的结果吗？这种情况通过升压来解决。
> 
> 升压是一个连续的过程，其中每个后续模型都试图纠正前一个模型的错误。后续模型依赖于前一个模型。让我们在下面的步骤中了解升压的工作方式。
> 
> 1.从原始数据集创建一个子集。
> 
> 2.最初，所有数据点被赋予相同的权重。
> 
> 3.在该子集上创建基础模型。
> 
> 4.该模型用于对整个数据集进行预测。

![](img/2d07e782b7072fd96b3dad0e703904a2.png)

> 5.使用实际值和预测值计算误差。
> 
> 6.被错误预测的观察值被给予较高的权重。
> (此处，三个错误分类的蓝色加分将被赋予更高的权重)
> 
> 7.创建另一个模型，并对数据集进行预测。
> (这个模型试图纠正前一个模型的错误)。

![](img/b4ee69740b165cfec5e1fa97605dbeaa.png)

> 8.类似地，创建多个模型，每个模型纠正前一个模型的错误。
> 
> 9.最终模型(强学习者)是所有模型(弱学习者)的加权平均值。

![](img/98680ae54aaf9b6ba733b1b4e851246f.png)

> 10.因此，增强算法将多个弱学习器结合起来形成一个强学习器。单个模型在整个数据集上表现不佳，但在数据集的某一部分上表现良好。因此，每个模型实际上都提高了整体的性能。

![](img/474753c06ab443583b0db4793fc45cd1.png)

## 🚩升压算法:

> Ada 增强
> 
> 马恩岛
> 
> XGBM
> 
> 轻型 GBM
> 
> CatBoost

# 💥AdaBoost:

> 自适应增强或 AdaBoost 是最简单的增强算法之一。通常，决策树用于建模。创建多个顺序模型，每个模型纠正上一个模型的错误。AdaBoost 将较高的权重分配给预测不正确的观测值，随后的模型将正确预测这些值。

# **💢AdaBoost 背后的三个概念:**

> 因此，让我们首先使用决策树和随机森林来解释 AdaBoost 背后的三个概念。
> 
> 概念-1。在随机森林中，每次你做一个决策树，你就做了一个完整大小的树。有些树可能比其他树大，但没有预先确定的最大深度。相比之下，在用 AdaBoost 生成的森林中，树通常只有一个节点和两片叶子，称为“树桩”。

![](img/45869ef94abd47c1a086023dafe64292.png)

柱

# 💫树桩:

> 树桩不擅长进行精确的分类。
> 
> 只有一个节点和两片叶子的树叫做树桩。因此，这里我们称之为树桩森林，而不是树木森林。

![](img/8b2d11e56ed558dad2a0cb6a4b5168ca.png)

> 例如，如果我们使用上面(左)给出的数据来确定某人是否患有心脏病。然后，一个完整的决策树将利用我们测量的所有 4 个变量(胸痛、良好的血液循环、阻塞的动脉和体重)做出如上图(右图)所示的决定。相比之下，树桩只能使用一个变量来做出决定，如下图所示。因此，树桩是技术薄弱的学习者。

![](img/2807168c3795e8d99ba1d96cc21c0cd9.png)

假肢

> 概念-2。在随机森林中，每棵树在最终分类上都有同等的投票权。相比之下，在由 AdaBoost 生成的树桩森林中，一些树桩在最终分类中比其他树桩获得更多发言权(性能)。
> 
> 概念-3。最后，在随机森林中，每棵决策树都是独立于其他决策树而生成的。换句话说，我们制作树的顺序并不重要，因为随机森林是并行构建树的。相比之下，在用 AdaBoost 制作的树桩森林中，顺序很重要。因为树桩森林是连续形成的。

# **🪐To 评论，AdaBoost 背后的三个想法是:**

> 1.AdaBoost 结合了很多‘弱学习者’进行分类。弱学习者是“树桩”。
> 
> 2.有些树桩在分类中比其他树桩更有发言权(表现)。
> 
> 3.每个树桩都是通过考虑前一个树桩的错误而形成的。

# **💥AdaBoost 背后的直觉和数学:**

> 假设我们有样本，分为训练集和测试集，还有一堆分类器。它们中的每一个都是在训练集的随机子集上训练的(注意，这些子集实际上可以重叠——这与交叉验证不同)。
> 
> 然后，对于每个子集中的每个观察值，AdaBoost 分配一个权重，该权重确定该观察值出现在训练集中的概率。具有较高权重的观察值更有可能被包括在训练集中。
> 
> 因此，AdaBoost 倾向于为那些被错误分类的观察值分配更高的权重，以便它们将代表下一个分类器训练集的更大部分，以此为目标，这一次，下一个训练的分类器将对它们执行得更好。

![](img/dc8d6dacbd766877c05e8335727e86e3.png)

升压算法起作用

# **🚦如何使用 AdaBoost 创建树桩森林:**

> 下面是我们将要构建树桩森林的数据集。我们用 AdaBoost 创建了一个树桩森林来预测病人是否患有心脏病。我们将根据患者的胸痛、动脉阻塞状况和体重来做出这些预测。

![](img/ebe59fccb751d8dbbde4a3a43834a85a.png)

数据集

# **🧡STEP 1:给每个样品称重** t

> 我们做的第一件事是给每个样本一个权重，以表明正确分类的重要性。开始时，所有样本都获得相同的权重，即 1/(样本或记录总数)。由于数据集包含 8 个记录，每个样本得到 1/8 作为样本权重，这使得样本或每个记录同等重要。然而，在我们完成第一个树桩后，这些权重会发生变化，以指导如何创建下一个树桩。

![](img/a3f1cd31eb522211f788788ffbcc26b1.png)

> 注意:样本重量与患者重量不同。

# **💛第二步:在森林里制作第一个树桩。**

> 现在我们需要在森林里做第一个树桩。这是通过找到变量，胸痛，阻塞的动脉或病人体重来完成的，这些变量对样本的分类效果最好。所以，基于上面的数据集我们对样本进行分类。
> 
> 注意:因为所有的权重都相同，所以我们可以忽略数据集中的样本权重列。

![](img/3f9216f75dd748873d4dd3513c8f8b7b.png)

> 我们先来看看胸痛对样本的分类有多好。在患有胸痛的 5 个样本中，3 个被正确分类为患有心脏病，两个被错误分类。
> 
> 在三个没有胸痛的样品中，两个被正确分类为没有心脏病，一个被错误分类。
> 
> 现在我们对阻塞的动脉和病人体重做同样的事情。

![](img/61d51b9a94d1be03a0477efa0876b345.png)

动脉阻塞

![](img/7aaf20bc00998d07670cd6c630db5e09.png)

患者体重

> 注:我们使用决策树技术来确定 176 是分离患者的最佳权重。

# **💙第三步:现在我们计算三个树桩的基尼系数或熵。我用过基尼指数。**

# **💥基尼**

基尼说，如果我们从一个人口中随机选择两个项目，那么它们必须是同一类，如果人口是纯的，这种概率是 1。

> 它适用于分类目标变量“成功”或“失败”。
> 
> 它只执行二进制分割
> 
> 基尼值越高，同质性越高。
> 
> CART(分类和回归树)使用 Gini 方法创建二元分裂。

# 🚩计算基尼系数的步骤:

> 使用成功和失败概率的平方和公式(p +q)计算子节点的基尼系数。
> 
> 使用分裂每个节点的加权基尼系数来计算分裂的基尼系数

# 🚩重量分割:

> 计算，左子节点的基尼= (1)*(1)+(0)*(0)=1
> 
> 右侧子节点的基尼系数= (4/5)*(4/5)+(1/5)*(1/5)=0.68
> 
> 计算分割权重的加权基尼= (3/8)*1+(5/6)*0.68 = 0.8
> 
> 基尼指数=1-加权基尼
> 
> =1–0.8
> 
> =0.2
> 
> 同样，使用上述公式计算所有其他分割的基尼指数。我们得到的基尼指数如下图所示。

![](img/c43564691ec037f65636189a471c5496.png)

基尼指数

> 体重属性的基尼指数最低。这将是我们的第一次巡回演出。

![](img/7aaf20bc00998d07670cd6c630db5e09.png)

第一个树桩

> 现在我们需要确定这个树桩在分类中有多少发言权。如前所述，一些树桩在最终分类中比其他树桩更有发言权。我们根据一个树桩对样本的分类程度来决定它在最终分类中有多少发言权。上面的树桩犯了一个错误，如错误栏中所述。如下图红框所示。这个体重不到 176 磅的病人有心脏病，但残肢说他们没有。我们认为体重超过 176 只会导致心脏病。

![](img/c3d96a0ac40ee73b41b8696893b87e65.png)

# **💜步骤 4:计算特定树桩的发言权数量**

📢注:说的量可以作为一个树桩的表现。

> 树桩的总误差是与错误分类的样本相关的权重之和。因此，在这种情况下，总误差是 1/8，因为只有一个样本被错误地分类。

![](img/4de8f4d21630bbbb0cb25689dc4c2a07.png)

总误差

> 注意:因为所有样本权重相加为 1。对于完美的树桩(正确分类每个数据点的树桩)而言，总误差将总是在 0 和糟糕的树桩(错误分类每个数据点的树桩)之间。
> 
> 我们使用总误差来确定这个残肢在最终分类中的数量，公式如下。

![](img/8db66d104d123c6aa7aaf7ddf91089f9.png)

发言权的大小

> 我们可以通过插入一串 0 到 1 之间的数字来绘制总误差的图表。蓝线告诉我们 0 到 1 之间的总误差值，如下图所示。当一个树桩做得很好并且总误差很小时，那么 Say 的量相对较大，为正值。当一个树桩在分类上不太好时，即一半的样本被正确分类，一半被错误分类，这意味着总误差是 0.5，那么 Say 的量是 0，当一个树桩做得很差，总误差接近 1，换句话说，如果树桩错误地分类了所有的样本，那么 Say 的量将是大的负值。

![](img/eeabda242c44b42af06be5b94c41d1f8.png)

> 注:如果总误差为 1 或 0。那么说等式的量会变大。在实践中，会添加一个小误差项来防止这种情况发生。
> 
> 因此，当患者体重> 176 时，总误差为 1/8。

![](img/445f87724417f629d9502ed1db08c01d.png)![](img/917d137bd2b85a19e7e70636dacc4960.png)

> 因此，第一个树桩对最终分类的发言权=0.97

# **💛第五步:修改每个样本的权重**

> 现在我们需要学习如何修改权重，以便下一个 stump 将当前 stump 产生的错误考虑在内。
> 
> 让我们回到我们制作的第一个树桩，当我们制作第一个树桩时，所有样品的重量都是相同的，这意味着我们没有强调正确分类任何特定样品的重要性，但是由于第一个树桩错误地对样品进行了分类，如下面的红框所示。我们将强调下一个树桩通过增加其样本权重和减少所有其他样本权重来正确分类的需要。

![](img/b7632fa50134b405083ac7abff1ebcfb.png)

> 让我们从增加错误分类样品(上面红框中显示的样品)的样品重量开始。
> 
> 下面是我们用来增加错误分类样本的样本重量的公式。

![](img/4d31c7e7ec71b01c9b2889e49174a3a6.png)![](img/ec6e163253ffd27de3fbfdae7d01cb05.png)

> 现在，我们需要降低所有正确分类样本的样本权重。
> 
> 下面是我们用来减少正确分类样品的样品重量的公式。

![](img/169516e990e25378518be4cff26cf5fe.png)![](img/311f6422228aff098e91e15ee7635f8d.png)

# 🧡STEP 6:跟踪新的样品重量并标准化。

![](img/a0cad981e9091ce169e5b6026a32b216.png)

> 现在，我们需要对新的样本权重进行标准化，以便它们的总和为 1。现在，如果你添加新的样本权重，你将得到 0.05+0.05+0.05+0.33+0.05+0.05+0.05+0.05 = 0.68
> 
> 因此，我们将每个新样本的权重除以 0.68，得到如下所示的标准化值。

![](img/905f56963990f4a615aac00c05f40f00.png)

> 现在，当我们添加新的样本权重时，我们得到 1(加上或减去一点舍入误差)。
> 
> 现在我们只是将标准化的样本权重转移到样本权重列，因为这些是我们将用于下一个树桩的，如下图所示。

![](img/1c6cd8cc32a2f079edd754d273a483b9.png)

# 💙步骤 7:为下一个模型创建新的数据集。

> 我们可以制作一个新的样本集合，其中包含具有最大样本权重的样本的副本。

![](img/6b48cd89c8b6b9e142ee4ef619265a13.png)

新的空数据集

> 因此，我们首先创建一个新的空数据集，它与原始数据集相同。然后，我们在 0 和 1 之间选择一个随机数，当我们像使用分布一样使用样本权重时，我们会看到这个数字落在哪里。
> 
> 如果数字在 0 和 0.07 之间，那么我们将下面红框中显示的样本放入空数据集。

![](img/005bc68acfc3ab203c5bb4c9ceda9a9f.png)

> 如果数字介于 0.07 和 0.14 (0.07+0.07)之间，我们将下一个样本放入新的数据集中。如果数字在 0.14 和 0.21 (0.14+0.07)之间，我们将第三个样本放入新的数据集，如果数字在 0.21 和 0.70 (0.21+0.49)之间，我们将错误分类的样本放入新的数据集。这个过程一直持续到我们填充新的数据集。因此，根据我们得到的随机数，我们将相应的样本放入新的数据集中。
> 
> 注意:在上述情况下，我们必须只将 8 个样本放入新的数据集。新数据集的大小应该与数据集的实际大小相同。

![](img/22860a09e5afba3ffba9b9a28ebe5e5c.png)

> 由于误分类样本具有更大的权重，因此它比其他样本更有可能进入新的数据集。
> 
> 现在我们去掉原来的数据集，用新的数据集作为训练下一个模型的数据集。我们将再次分配样本权重，并重复上述步骤，直到所有样本都被正确分类，或者使用停止标准，如估计数。
> 
> 现在我们需要谈谈 Ada Boost 创建的树桩森林是如何进行分类的。
> 
> 假设我们已经为给定的数据样本训练了这 10 个模型中的 10 个模型，其中 6 个模型(树桩)预测此人患有心脏病，4 个模型预测此人没有心脏病。因此，我们计算归类为心脏病的所有六个树桩的 Say 量的总和以及归类为没有心脏病的所有四个树桩(模型)的 Say 量的总和。现在我们把病人归入 Say 量总和最高的那一类。

如果你发现帖子中有什么错误，或者你有什么要补充的，让我们在评论中讨论吧…
谢谢。

**信用和来源:**

1.  [StatQuest](https://statquest.org/)
2.  [www.Analyticsvidhya.com](https://www.analyticsvidhya.com/blog/2016/03/pca-practical-guide-principal-component-analysis-python/)