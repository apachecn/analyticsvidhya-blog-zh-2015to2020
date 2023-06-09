# 变量——它们是什么？

> 原文：<https://medium.com/analytics-vidhya/variables-what-are-they-79dbba7f0b64?source=collection_archive---------14----------------------->

![](img/cb660158245a68f5813cf458e25580fa.png)

保罗·尼科勒洛在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

可变是一个因物而异的量。例如，我们在一个选定的地块上测量了 50 棵芒果树的高度，并将结果排列在一个表格中。在这里，物体(树)之间变化的量是它的高度。因此，在这个例子中，高度是唯一的变量。包含变量值集合的表称为“数据集”或样本。

# **自变量对因变量**

让我们考虑一个例子。藻类净初级生产力(每年单位面积的碳质量(克 C (m^-2) (yr^-1))是在不同的温度和光照强度设置下测量的。在这个实验中，涉及三个变量:初级生产力、温度和光照强度。然而，在三个变量中，只有一个变量(初级生产力)被衡量；两个变量的其他强度在实验设置中被控制。其变化不依赖于其他变量的变量称为自变量。在这个例子中，温度和光强度都是独立变量，因为这些变量的值的变化不依赖于其他变量。温度和光照强度都不依赖于初级生产力。然而，初级生产力取决于温度和光照强度。在这个例子中，初级生产力是一个因变量，其值依赖于其他变量。

为了检验一个变量是独立的还是非独立的，一个有用的策略是替换这个句子中的可疑变量，看看这个陈述是否有意义:(自变量)引起(因变量)的变化，而(因变量)不可能引起(自变量)的变化。例如，让我们考虑两个变量“学习时间”和“考试成绩”。(花在学习上的时间)导致(考试分数)的变化，而(考试分数)不可能导致(花在学习上的时间)的变化。我们看到“花在学习上的时间”必须是独立变量，“考试分数”必须是因变量，因为这句话反过来就没有意义了。注意以防“时间”(或相关概念，如“年龄”等。)在实验中被当作一个变量，它将永远是一个自变量。可以采用一种更正式的程序来检验两个变量是否相互依赖，即相关性检验(也称为协变)。对于定量数据，可以使用皮尔逊的相关性检验，而对于分类数据，可以使用皮尔逊的χ2 独立性检验。但是，相关性并不能揭示哪个是因变量，哪个是自变量。注意模块 2 中讨论的统计混淆问题。相关性将在后面的模块中详细讨论。

在科学实验中，通常只测量因变量。因变量因此被称为结果变量，因为它决定了实验的结果。这些结果变量的值反过来依赖于(并决定于)独立变量。作为实验设计的一部分，实验者经常控制独立变量的值，这些变量也被称为治疗变量或反应变量。因子是一个独立的治疗变量，其设置(值)由实验者控制和改变。因子的强度设置是级别。水平可以是数量数字，或者在许多情况下，仅仅是“存在”或“不存在”(“0”或“1”)。例如，为了发现温度对电阻器的影响，在将电阻器放置在设定为不同温度的 3 个烘箱中之前和之后测量电阻。这里，因变量是阻力，独立变量是温度。不同的温度设置是“级别”，这里是 3。在另一个例子中，为了发现温度和加热时间对电阻器的影响，在将电阻器放置在设置为三种不同温度的三个烘箱中三个不同时间段之前和之后，测量电阻器的电阻。在这种情况下，“温度”和“时间”都是因素。

# **定性与定量变量**

还可以根据变量是否可以用数字表示来对变量进行分组。无法用数字表达的变量(例如，幸福程度、美貌、道德、爱情等。)称为定性变量。一些定性变量可以分为不同的标签或类别(例如，性别、国籍等。).这种变量被称为分类变量或属性变量。可以用数字表示的变量，如身高、体重、摩尔浓度、光子通量密度等。被称为数量变量。

# **离散变量与连续变量**

离散变量或分类变量是指只能接受可数个(有限或无限)值的变量。例如，生育的孩子数量、一块肥皂中的原子数量、通过交通信号灯的周期数等。价值是这样表述的；离散变量不涉及“舍入”。换句话说，离散变量是可以计数的。然而，离散变量的值不一定是整数。例如，一个班级中女生的平均人数可能是 17.5，掷骰子得出偶数的概率是 0.5，或者午餐费用是 45.50 印度卢比；所有这些值都有分数。离散变量的其他例子有:

*   每分钟到达呼叫中心的电话数量。
*   两个竞争队比赛中的进球数。
*   特定年龄组每年的死亡人数。
*   在给定的时间间隔内股票价格上涨的次数。
*   在同质性假设下，网络服务器每分钟被访问的次数。
*   100 微升细胞悬液中的细胞数
*   在一定量的辐射后，给定的一段 DNA 的突变数量。

另一方面，连续变量可以(有效地)在其范围内取不可数的无穷多个值。例如身高或体重。身高通常只精确到厘米。例如，当一个人被报道身高为 178 厘米时，这个人的身高可能是 177 . 5986718961815cm，取决于测量方法的精度。常规尺子可以精确到毫米(例如 3.4 厘米)，而游标卡尺可以进一步提高精度(3.4412 厘米)。

# **精度与准确度**

精度是测量中的小数位数。例如，在前面的例子中，用直尺测量得到的值是 3.4 厘米，只有一个小数位，而用游标卡尺测量得到的值是 3.4412 厘米，有四个小数位。可以说直尺的精度是 1，而游标卡尺的精度是 4(因此，比直尺的精度高)。但是，请注意，高精度并不意味着测量准确。准确度是测量值和“真实值”的绝对差值。真实值是该变量的真实值。例如，一枚认证重量为 1 克的标准银币在高精度珠宝级称重机上的重量为 1.9975 克。精度为 4(因为测量中有四个小数位)。然而，测量值非常不准确。该测量的精度为 1.9975–1.0000 = 0.9975。零值是可能的最高精度级别；与零的偏差表明测量不准确。

# **极限和范围**

当报告一组数字时，该组的名义极限是报告的最低值和最高值(换句话说，分别是最小值和最大值)。例如，三个人的体重分别为 43 公斤、55 公斤和 67 公斤。名义上限和下限分别为 43 公斤和 67 公斤。另一方面，实际下限是名义下限可以向上舍入的最低值。例如，我们的名义下限 43 公斤可以从 42.5 公斤向上取整。相反，实际上限是名义上限可能向下舍入的最高值(不含)。例如，我们的名义上限 67 公斤可以从 67.5 公斤向下舍入(注意:这个数字是不包含在内的)。这些真实的极限取决于测量的精度。标称极限之间差异被称为不相容范围。在我们的示例中，不含范围= 67–43 = 24 千克。实际极限之间差异被称为包含范围。在我们的示例中，包含范围= = 67.5–42.5 = 25。如果舍入到最接近的整数，包含范围是不包含范围+ 1。

# **总结**

*   变量是一个可能因对象而异的量。变量值的集合称为样本或数据集
*   其变化不依赖于其他变量的变量称为独立(处理)变量，也称为因子。—其值依赖于其他变量的变量称为因变量(结果变量)。
*   在科学实验中，只测量因变量。自变量的水平被设置为科学实验设计的一部分。
*   为了找到两个变量之间的联系，可以采用相关性检验。但是，相关性并不能揭示哪个是因变量，哪个是自变量。
*   不能用数字表示的变量叫定性变量，能用数字表示的叫定量变量。
*   离散变量或分类变量是一种只能取有限数量的值的变量。连续变量在其范围内可以(有效地)取不可数的无穷多个值。连续变量通常四舍五入到最接近的整数，其测量取决于精度和准确度。
*   精度是测量中的小数位数，而准确度是测量值和“真实值”之间的绝对差值。
*   虽然名义限额与最小值和最大值同义，但实际限额要么是名义下限可能向上舍入的最低值(实际下限)，要么是名义上限可能向下舍入的最高非包含值(实际下限)。实际极限之间差异称为包含范围，而名义极限之间的差异称为排除范围。

## 打个招呼。

**领英:**[*www.linkedin.com/in/RiteshpratapS/*](https://www.linkedin.com/in/riteshprataps/)