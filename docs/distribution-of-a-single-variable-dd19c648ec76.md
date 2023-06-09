# 单一变量的分布

> 原文：<https://medium.com/analytics-vidhya/distribution-of-a-single-variable-dd19c648ec76?source=collection_archive---------10----------------------->

习惯上将原始数字称为数据，将数据分析的输出称为信息。您从数据开始，并希望以组织可以用来获得竞争优势的信息结束。

首先，我们区分总体和样本。一个**群体**包括所有感兴趣的实体。几乎不可能获得所有人口的信息。因此，我们经常试图通过检查人口的**样本**或子集来获得对人口特征的洞察

一个**数据集**通常是一个矩形的数据数组，变量在列中，观察值在行中。一个**变量**(或**字段**或**属性**)是群体成员的特征，如身高、性别或工资。一个**观察**(或**案例**或**记录**)是群体中单个成员的所有变量值的列表。如果可以对一个变量进行有意义的运算，那么这个变量就是数值型的。否则，变量为**分类**。第三个是日期变量。三种看似数字但通常被视为分类的变量是电话号码、邮政编码和社会保险号。意见变量为分类变量，它们仅仅是“强烈不同意”、“不同意”、“中立”、“同意”和“强烈同意”类别的代码如果一个分类变量的可能类别有自然的顺序，那么这个分类变量就是有序的。如果没有自然排序，变量为**名义**。

**虚拟变量**是特定类别的 0-1 编码变量。对于该类别中的所有观察值，它被编码为 1，对于不属于该类别的所有观察值，它被编码为 0。一个**装箱的**(或**离散化的** ) **变量**对应于一个已被分类为离散类别的数值变量。这些类别通常被称为**箱**。如果一个数值变量是由计数产生的，比如孩子的数量，那么它就是离散的。**连续**变量是基本连续测量的结果，如体重或身高。**横断面**数据集是在不同时间点的人口横断面数据。**时间序列**数据是随时间收集的数据。

> ***描述性措施*** *:*

对分类数据进行汇总的唯一有意义的方法是对类别中的观察值进行计数。我们可以对某个分类变量的任何类别使用虚拟(01)变量，例如 M 代表性别。重新编码变量，使每个 M 都被 1 代替，所有其他值都被 0 代替。现在你可以通过对 0 和 1 求和来计算男性的数量，通过对 0 和 1 求平均值来计算男性的百分比。

各种数字汇总措施可以分为几组:集中趋势的措施；最小值、最大值、百分位数和四分位数；可变性的测量；和形状的度量

*集中趋势的度量:*

![](img/b6e81e897aff0bddeb85c0727b2e9475.png)

**平均值**是所有值的平均值。**样本表示**，用(读作“X-bar”)表示。**人口平均数**用μ表示(希腊字母 *mu* )

**中位数**是数据从最小到最大排序时的中间观察值。如果观察值是奇数，中位数就是中间的观察值。如果观察次数为偶数，中位数通常定义为两次中间观察的平均值

对于高度倾斜的数据，中位数通常是更好的集中趋势的度量。中值不受极值的影响，而平均值对极值非常敏感。大多数人会认为中位数比平均数更能代表集中趋势。对于没有向一个方向或另一个方向倾斜的变量，平均值和中值通常非常接近。

**模式**是最常出现的值

对于任何百分比 *p* ，第 p 百分位是所有值的百分比 *p* 小于它的值。类似地，第一、第二和第三四分位数是对应于 p=25%、p=50%、p=75%和。注意，根据定义，第二个四分位数等于中位数。为了完成这组描述性测量，我们添加了最小**值**和最大**值**，具有明显的含义。

*可变性的度量:*

这些包括范围、四分位范围、方差和标准偏差以及平均绝对偏差。

**范围**是对可变性的粗略衡量。它被定义为最大值减去最小值。一个不太敏感的衡量标准是**四分位距**(缩写为 **IQR** )。它被定义为第三个四分位数减去第一个四分位数，因此它实际上是中间 50%数据的范围

![](img/0c1e7b6954acc9e68ef68e4962e29a92.png)

总体方差

![](img/6f1e53df4653325abf5b609fc85013db.png)

采样离散

**方差**本质上是平均值的平方偏差的平均值，其中 if 是典型的观察值，其与平均值的平方偏差是。在关于平均值的讨论中，有一个用表示的**样本方差**，和一个用表示的**总体方差**(其中是希腊字母 sigma)。

如果所有的观察值都接近平均值，那么它们与平均值的平方偏差就会相对较小，方差也会相对较小。另一方面，如果至少有一些观察值远离平均值，它们与平均值的平方偏差将很大，这将导致方差很大。请注意，因为与平均值的偏差是平方的，所以低于平均值一定量的观察值对方差的贡献与高于平均值相同量的观察值相同。方差以平方为单位。一个更自然的度量是方差的平方根。这被称为**标准差**。同样，标准差有两个版本。用 s 表示的**样本标准差**和用 sigma 表示的**总体标准差**。

*解释标准差的经验法则*:

这些经验规则为对称的钟形分布的标准偏差赋予了具体的含义。然而，对于偏态分布，它们往往不太准确。

*   大约 68%的观察值在平均值的一个标准偏差内
*   大约 95%的观察值在平均值的两个标准偏差之内
*   大约 99.7%的观察值(几乎全部)都在平均值的三个标准偏差范围内

![](img/30c1fe443160c7cc8c9238a5403f2b06.png)

*   **平均绝对偏差**(缩写为 **MAD** )作为可变性的另一种度量，特别是在时间序列分析中。它被定义为绝对偏差的平均值。

*形状的度量:*
我们说这些薪水是向右倾斜的**(或者说**正倾斜的**)因为这种倾斜是由于薪水确实很高。如果这种偏斜是由于非常小的值造成的(南极洲的低温可能会出现这种情况)，那么我们会称之为向左偏斜(或负偏斜)。**峰度**，与分布尾部相对于正态分布尾部的“丰满度”有关。**

Excel 的内置函数(AVERAGE、STDEV 和其他函数)用于计算许多汇总指标。更快的方法是使用 Palisade 的统计工具插件。

*数值变量图表:*

横截面变量的直方图和箱线图-箱线图和直方图是显示数值变量分布的互补方式。尽管直方图更受欢迎，也更直观，但箱线图仍然能提供大量信息。此外，如下一章所述，并列箱线图对于比较两个或多个总体非常有用。

时间序列图的全部目的是检测数据*中的历史模式。*传统的均值、中值、标准差等汇总指标对于时间序列数据往往意义不大，至少对于原始数据来说意义不大。但是，找出不同时期的数据差异或百分比变化，然后报告这些数据的传统汇总方法通常是有用的。

一个**异常值**实际上是一个值或一个完整的观察值(行),位于正常范围之外。有时仔细检查变量值，一次一个变量，不会发现任何异常值，但仍然可能有不寻常的值组合。例如，如果发现一个人的年龄等于 10 岁，身高等于 72 岁，那就很奇怪了。处理异常值的一个好方法是报告有异常值和没有异常值的结果。