# 用 Python 中的基础数据和机器学习预测回报

> 原文：<https://medium.com/analytics-vidhya/predicting-returns-with-fundamental-data-and-machine-learning-in-python-a0e5757206e8?source=collection_archive---------1----------------------->

![](img/6178d143133e6f4adce96ab49c18068c.png)

[吉利](https://unsplash.com/@gillyberlin?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## ML 算法可以利用基本面预测赢家和输家吗？

2020 年 5 月 8 日，我在 TD Ameritrade 的网站上部署了一个 web scraper，将当时标准普尔 500 证券的所有基本数据收集到一个干净的、Python 就绪的数据库中。本地机器上的 csv 文件。现在，六个月后，我已经掸掉了数据库上的灰尘，并对各种机器学习算法在各种回归/分类任务中利用它的能力进行了深入研究，这些任务可能会导致市场中的交易优势，以及对基本面特征在预测业绩方面的相对重要性的洞察。这篇博文将总结我的发现，但是感兴趣的读者可以在[库](https://github.com/FoamoftheSea/mod5project)的 Jupyter 笔记本中找到完整的技术工作流程，以及数据集。

本文将探讨数据集，讨论其包含的基本财务特征背后的含义，建立有助于建模特征选择过程的领域知识。在数据探索和清理之后，将探索输入缺失数据的不同方法，以及对机器学习算法进行超参数调整的演示，这些算法在研究中的回归/分类任务中表现最佳，所有这些都是用 Python 实现的。在分析了建模过程的结果后，本文将以使用模型预测构建长/短股票策略投资组合的演示结束，并模拟这种投资组合的绩效和风险管理。有关用于收集本研究中所用数据的 web scraper 的详细信息，请参见即将发布的博客帖子。

请注意，本文的内容仅旨在演示量化金融环境下的数据科学工作流程，而不是任何形式的个人投资建议。

# 概观

我们对几个目标变量进行了建模，以了解这些数据在回答什么样的问题时是有用的。首先，从价值投资的角度来看，数据被用来模拟证券的当前交易价格。这背后的逻辑是，由于市场在反映资产的潜在价值方面大多是[有效的](https://www.investopedia.com/terms/e/efficientmarkethypothesis.asp)，创建一个回归证券当前交易价格的模型可以取代价值投资策略中的资产定价模型，如[资本资产定价模型(CAPM)](https://www.investopedia.com/terms/c/capm.asp) ，以评估证券的内在价值。沃伦·巴菲特(Warren Buffett)等价值投资者开发资产定价模型，以寻找当前市场价格与他们对证券内在价值的估计之间的差异，他们用这些差异对模型显示当前估值过高/过低的证券进行定向押注。所谓的[安全边际](https://www.investopedia.com/terms/m/marginofsafety.asp#:~:text=Margin%20of%20safety%20is%20a,is%20the%20margin%20of%20safety.)代表内在价值估计值和当前市场价格之间的比例距离。如果安全边际足够大，那么价值投资者将相应地买入或做空股票，期望市场价格随着时间的推移向其计算的内在价值调整。这种分析的主要好处是模型不需要未来的定价数据。本实验的假设是，训练模型的残差可以作为安全边际，并与刮擦后六个月的证券对数收益呈线性关系，但这种关系无法得到证实。

其他三个目标变量都与自刮擦之日起标准普尔 500 证券的对数收益有关。这些回报 1)作为连续变量回归，然后 2)分类为代表赢家和输家的二元分类变量(高于和低于零)，最后 3)分类为代表相对于整个指数的表现超出/低于的二元分类变量(从所有回报中减去指数均值，然后分类为高于/低于零)。最后一个目标被证明是最有用的。从单个证券的回报中减去指数的平均值背后的推理是，在不同的观察时间段内，市场将作为一个整体上升或下降，影响其中所有证券的回报，但这些运动与整个市场的行为关系更大，而与市场内公司基本面的差异关系较小。在所研究的时间段内，市场总体呈上升趋势，部分原因与 3 月下旬新冠肺炎相关崩盘后的复苏有关。由于指数中的大多数公司在此期间都有收益，因此在没有对整体市场运动进行调整的情况下，根据收益本身训练的模型可能会错误地将某些特征与实际上由宏观经济过程引起的收益相关联，并且当根据回报对证券进行分类时，这些更广泛的市场运动会影响类别成员。对指数的变动进行调整，使模型能够关注其内部证券之间的行为偏差，这些偏差是由基础基本面的差异驱动的，在不同的市场条件下可能会更加稳健。不幸的是，由于数据收集方式的一次性，无法在不同的时间段评估针对这三个目标训练的模型的性能。在研究期间对抵制集合的测试表明，模型确实可以给交易者带来市场优势，但这需要通过未来的调查来验证。无论如何，这些模型已经表明，基本面数据确实包含对业绩的预测能力，进一步的研究是有保证的。

# 数据探索

web scraper 收集了标准普尔 500 每个证券的大量信息，而不仅仅是严格意义上的基本数据。本研究关注两种主要形式的目标变量:一种是刮削日的证券价格，另一种是刮削日以后的收益。对于这两个任务，选择什么功能的考虑是不同的。对于开发一个资产定价模型(当前价格建模的第一个任务)，重要的是忽略任何包含资产价格信息的特征，因为这是一种目标泄漏的形式。请记住，[基本面分析](https://www.investopedia.com/terms/f/fundamentalanalysis.asp)的要点是使用公司的基本面信息来估计证券的内在价值，以便将其与当前市场价格进行比较，因此允许当前市场价格泄漏到模型中将不利于通过这一过程来估计内在价值(这实际上是一个隐藏变量，取决于投资者的计算)。

让我们看看完整的特性列表，并为我们的两个主要建模任务执行单独的一轮特性选择。如果我们打电话。info()在原始数据帧上，我们看到以下内容。

```
<class 'pandas.core.frame.DataFrame'>
Index: 501 entries, A to ZTS
Data columns (total 106 columns):
% Above Low                                     274 non-null float64
% Below High                                    227 non-null float64
% Held by Institutions                          495 non-null float64
52-Wk Range                                     501 non-null object
5yr Avg Return                                  501 non-null float64
5yr High                                        501 non-null float64
5yr Low                                         501 non-null float64
Annual Dividend $                               394 non-null float64
Annual Dividend %                               394 non-null float64
Annual Dividend Yield                           389 non-null float64
Ask                                             494 non-null float64
Ask Size                                        492 non-null float64
Ask close                                       22 non-null float64
B/A Ratio                                       492 non-null float64
B/A Size                                        501 non-null object
Beta                                            488 non-null float64
Bid                                             499 non-null float64
Bid Size                                        492 non-null float64
Bid close                                       22 non-null float64
Change Since Close                              112 non-null object
Change in Debt/Total Capital Quarter over Qua...473 non-null float64
Closing Price                                   500 non-null float64
Day Change $                                    501 non-null float64
Day Change %                                    501 non-null float64
Day High                                        501 non-null float64
Day Low                                         501 non-null float64
Days to Cover                                   495 non-null float64
Dividend Change %                               405 non-null float64
Dividend Growth 5yr                             354 non-null float64
Dividend Growth Rate, 3 Years                   392 non-null float64
Dividend Pay Date                               501 non-null object
EPS (TTM, GAAP)                                 490 non-null float64
EPS Growth (MRQ)                                487 non-null float64
EPS Growth (TTM)                                489 non-null float64
EPS Growth 5yr                                  438 non-null float64
Ex-dividend                                     394 non-null object
Ex-dividend Date                                501 non-null object
FCF Growth 5yr                                  485 non-null float64
Float                                           495 non-null float64
Gross Profit Margin (TTM)                       422 non-null float64
Growth 1yr Consensus Est                        475 non-null float64
Growth 1yr High Est                             475 non-null float64
Growth 1yr Low Est                              475 non-null float64
Growth 2yr Consensus Est                        497 non-null float64
Growth 2yr High Est                             497 non-null float64
Growth 2yr Low Est                              497 non-null float64
Growth 3yr Historic                             497 non-null float64
Growth 5yr Actual/Est                           497 non-null float64
Growth 5yr Consensus Est                        497 non-null float64
Growth 5yr High Est                             497 non-null float64
Growth 5yr Low Est                              497 non-null float64
Growth Analysts                                 497 non-null float64
Historical Volatility                           501 non-null float64
Institutions Holding Shares                     495 non-null float64
Interest Coverage (MRQ)                         389 non-null float64
Last (size)                                     501 non-null float64
Last (time)                                     501 non-null object
Last Trade                                      112 non-null float64
Market Cap                                      501 non-null object
Market Edge Opinion:                            485 non-null object
Net Profit Margin (TTM)                         489 non-null float64
Next Earnings Announcement                      489 non-null object
Operating Profit Margin (TTM)                   489 non-null float64
P/E Ratio (TTM, GAAP)                           451 non-null object
PEG Ratio (TTM, GAAP)                           329 non-null float64
Prev Close                                      501 non-null float64
Price                                            1 non-null float64
Price/Book (MRQ)                                455 non-null float64
Price/Cash Flow (TTM)                           468 non-null float64
Price/Earnings (TTM)                            448 non-null float64
Price/Earnings (TTM, GAAP)                      448 non-null float64
Price/Sales (TTM)                               490 non-null float64
Quick Ratio (MRQ)                               323 non-null float64
Return On Assets (TTM)                          479 non-null float64
Return On Equity (TTM)                          453 non-null float64
Return On Investment (TTM)                      437 non-null float64
Revenue Growth (MRQ)                            487 non-null float64
Revenue Growth (TTM)                            489 non-null float64
Revenue Growth 5yr                              490 non-null float64
Revenue Per Employee (TTM)                      459 non-null float64
Shares Outstanding                              495 non-null float64
Short Int Current Month                         495 non-null float64
Short Int Pct of Float                          495 non-null float64
Short Int Prev Month                            495 non-null float64
Short Interest                                  495 non-null float64
Today's Open                                    501 non-null float64
Total Debt/Total Capital (MRQ)                  460 non-null float64
Volume                                          500 non-null float64
Volume 10-day Avg                               500 non-null float64
Volume Past Day                                 501 non-null object
cfra                                            479 non-null float64
cfra since                               479 non-null datetime64[ns]
creditSuisse                                    337 non-null object
creditSuisse since                       337 non-null datetime64[ns]
ford                                            493 non-null float64
ford since                               493 non-null datetime64[ns]
marketEdge                                      484 non-null float64
marketEdge opinion                              484 non-null object
marketEdge opinion since                 484 non-null datetime64[ns]
marketEdge since                         484 non-null datetime64[ns]
newConstructs                                   494 non-null float64
newConstructs since                      494 non-null datetime64[ns]
researchTeam                                    495 non-null object
researchTeam since                       495 non-null datetime64[ns]
theStreet                                       496 non-null object
theStreet since                          496 non-null datetime64[ns]
dtypes: datetime64[ns](8), float64(82), object(16)
memory usage: 418.8+ KB
```

在这里，我们可以看到一个非常丰富的数据集，包括当前/历史价格、历史波动率、交易量、股息、各种增长率、基本面、价格比率，甚至分析师评级信息。我们可以看到原始数据集中有 501 家公司，总共有 106 列。请注意，几乎没有一列包含指数中每家公司的数据，对这种稀疏性本质的调查显示，只有 77 行的每一列都有数据，这意味着使用该数据集需要进行插补。如上所述，对于本项目中的两个主要任务，特性选择的过程是不同的，因此将分别进行研究。首先，我们需要获得定价数据，以便开发我们的目标变量。

## 获取定价数据

刮削的日期是 2020 年 5 月 8 日，分析的日期是 2020 年 10 月 29 日，因此我们需要获得这段时间内标准普尔 500 的定价信息。一个简单的方法是使用 python 中的 yfinance 包，如下所示。在存储库中，有一个. csv 文件，其中包含了在抓取中使用的所有代码。他们中的一些人在收集数据的过程中没有得到数据，也没有被 yfinance 认可，所以他们将被删除，另外两家公司 AGN 和 ETFC 被其他公司收购，不再存在，所以他们将从我们的分析中删除。

![](img/a86658cbe9232d8492d40c34d816bc4e.png)

我们看到我们的收盘价，股票代码作为列名

现在，让我们继续计算我们的对数收益。这就像把价格转换成日志价格，然后从最近的收盘价中减去最早的收盘价一样简单。使用对数价格和回报而不是美元和百分比回报的好处在这里[详细讨论，但是可以说，由于股票价格的对数正态性和对数回报的时间可加性(与百分比回报的乘法性质相反)，在金融分析中使用对数价格/回报是常见的行业惯例。我们还将放弃 AGN 和 ETFC，这两家公司在研究期间都被其他公司收购了。](https://quantivity.wordpress.com/2011/02/21/why-log-returns/#:~:text=Benefit%20of%20using%20returns%2C%20versus,price%20series%20of%20unequal%20values.)

![](img/a4668c2e745876628e5f63d7e16b25ca.png)

让我们来看看这些日志回报的分布。

![](img/cc0028e6c0c13587ef4efeab8e155632.png)

我们可以看到，对数收益率呈对称的正态分布，其均值远大于零(0.11)，反映了市场在研究期间的整体向上运动。

请记住，我们的第一个任务将是对抓取日期(即 log_close 的第一行)的日志价格进行建模，第二个任务将是对 log_returns 进行回归和分类建模。

# 特征选择和建模

本节将分为两个主要的建模任务:资产价格建模和回报建模。对于每一个，我们将执行特征工程，然后比较插补技术的功效，训练机器学习算法，然后解释我们的建模结果。

# 资产价格建模

此任务的目的是使用与公司相关的基本面数据对证券价格进行建模，以便对内在价值进行伪基本面分析，从而根据我们模型的残差制定价值投资式交易策略，该残差代表安全边际，换言之，与模型估计的价值相比，给定证券的价值高估或低估程度。假设是，残差将与数据被剔除后六个月期间的证券回报相关，因为随着市场向其真实价值调整，高估的股票将看到其价格下跌，而低估的股票将上涨。

在金融分析中，[内在价值](https://www.investopedia.com/terms/i/intrinsicvalue.asp#:~:text=Intrinsic%20value%20is%20a%20measure,market%20price%20of%20that%20asset.)至少部分是主观的，因为不同的分析师将通过构建他们自己的个人专有定价模型来得出不同的估计值，这意味着在这个实验中，我们实际上是在尝试使用当前交易价格来为一个隐藏变量建模。为什么这可能起作用背后的逻辑是市场至少是部分[有效的](https://www.investopedia.com/terms/e/efficientmarkethypothesis.asp)，这意味着资产的当前价格可靠地反映了它们的价值，存在一些误差和噪音。因此，当我们使用市场价格作为目标变量时，我们希望我们构建的模型能够找到特性对整个市场价格的影响方式，并且我们的残差将反映与实际(内在)价值的偏差。

## **功能选择:**

如上所述，使用包含证券当前价格信息的特征将导致目标泄漏，并破坏我们估计内在价值的目标。例如，[市盈率](https://www.investopedia.com/terms/p/price-earningsratio.asp) (P/E)的计算方法是证券价格除以其[每股收益](https://www.investopedia.com/terms/e/eps.asp) (EPS)。由于我们的特征中存在 EPS 和 P/E，该模型可以很容易地分解出证券的当前价格，并且过于准确地反映了当前的交易价格，而不是内在价值。因此，我们必须小心地移除或修改所有会允许这种目标泄漏的特征。

第一步是删除所有与价格直接相关的特征，例如任何与周期高/低、高于/低于这些标记的百分比以及任何与开盘/收盘/最后/出价/要价相关的特征。价格比率可以通过将证券的当前价格除以比率本身来分解价格，只留下收益/账面/销售/现金流，这代表了公司的基本信息。我们还将删除任何数据非常不完整的列，以及日期时间列，因为这些列在此任务中都没有用。年股息率%和年股息率代表的是同一个东西，后者缺失的数值更多，所以会被去掉。

```
<class 'pandas.core.frame.DataFrame'>
Index: 501 entries, A to ZTS
Data columns (total 65 columns):
% Held by Institutions                          495 non-null float64
5yr Avg Return                                  501 non-null float64
Annual Dividend %                               394 non-null float64
Beta                                            488 non-null float64
Change in Debt/Total Capital Quarter over Qua...473 non-null float64
Days to Cover                                   495 non-null float64
Dividend Change %                               405 non-null float64
Dividend Growth 5yr                             354 non-null float64
Dividend Growth Rate, 3 Years                   392 non-null float64
EPS (TTM, GAAP)                                 490 non-null float64
EPS Growth (MRQ)                                487 non-null float64
EPS Growth (TTM)                                489 non-null float64
EPS Growth 5yr                                  438 non-null float64
FCF Growth 5yr                                  485 non-null float64
Float                                           495 non-null float64
Gross Profit Margin (TTM)                       422 non-null float64
Growth 1yr Consensus Est                        475 non-null float64
Growth 1yr High Est                             475 non-null float64
Growth 1yr Low Est                              475 non-null float64
Growth 2yr Consensus Est                        497 non-null float64
Growth 2yr High Est                             497 non-null float64
Growth 2yr Low Est                              497 non-null float64
Growth 3yr Historic                             497 non-null float64
Growth 5yr Actual/Est                           497 non-null float64
Growth 5yr Consensus Est                        497 non-null float64
Growth 5yr High Est                             497 non-null float64
Growth 5yr Low Est                              497 non-null float64
Growth Analysts                                 497 non-null float64
Historical Volatility                           501 non-null float64
Institutions Holding Shares                     495 non-null float64
Interest Coverage (MRQ)                         389 non-null float64
Market Cap                                      501 non-null object
Market Edge Opinion:                            485 non-null object
Net Profit Margin (TTM)                         489 non-null float64
Operating Profit Margin (TTM)                   489 non-null float64
P/E Ratio (TTM, GAAP)                           451 non-null object
PEG Ratio (TTM, GAAP)                           329 non-null float64
Price/Book (MRQ)                                455 non-null float64
Price/Cash Flow (TTM)                           468 non-null float64
Price/Earnings (TTM)                            448 non-null float64
Price/Earnings (TTM, GAAP)                      448 non-null float64
Price/Sales (TTM)                               490 non-null float64
Quick Ratio (MRQ)                               323 non-null float64
Return On Assets (TTM)                          479 non-null float64
Return On Equity (TTM)                          453 non-null float64
Return On Investment (TTM)                      437 non-null float64
Revenue Growth (MRQ)                            487 non-null float64
Revenue Growth (TTM)                            489 non-null float64
Revenue Growth 5yr                              490 non-null float64
Revenue Per Employee (TTM)                      459 non-null float64
Shares Outstanding                              495 non-null float64
Short Int Current Month                         495 non-null float64
Short Int Pct of Float                          495 non-null float64
Short Int Prev Month                            495 non-null float64
Short Interest                                  495 non-null float64
Total Debt/Total Capital (MRQ)                  460 non-null float64
Volume 10-day Avg                               500 non-null float64
cfra                                            479 non-null float64
creditSuisse                                    337 non-null object
ford                                            493 non-null float64
marketEdge                                      484 non-null float64
marketEdge opinion                              484 non-null object
newConstructs                                   494 non-null float64
researchTeam                                    495 non-null object
theStreet                                       496 non-null object
dtypes: float64(58), object(7)
memory usage: 258.3+ KB
```

事情看起来更干净了。这项任务的下一步是去除分析师评级，因为这些并不是真正的基础。另一个要处理的问题是我们有重复的市盈率。名为“P/E Ratio (TTM，GAAP)”的列是对象数据类型，下面的“Price/Earnings (TTM，GAAP)”也是数字数据类型。我们还有“价格/收益(TTM)”，其中包含所谓的[非 GAAP 收益](https://www.investopedia.com/terms/n/non-gaap-earnings.asp)，它有目的地忽略了公司最近发生的任何可能混淆财务分析的大额非经常性费用，并被认为有利于我们的目的。因此，我们将放弃 GAAP 收益，保留非 GAAP 收益。PEG 比率是市盈率除以年 EPS 增长率，两者我们都已经有了，所以可以降下来。

事情越来越干净，但我们有更多的考虑。市值列仍然以字符串格式编码，但它也包含价格信息，并将导致目标泄漏。市值的计算方法是发行在外的股票数量乘以证券的当前价格，因为我们有一个发行在外的股票栏，所以我们需要完全去掉市值。此外，我们还没有检查我们的数据集为南的邪恶异父兄弟:inf。让我们放下市值，进行这项检查。

```
True
```

我们确实找到了一些罪犯。不要担心，因为我们已经计划处理丢失的数据，我们只是将它们重新编码为 nan，稍后再处理。

已经完成了。我们现在需要检查，以确保我们的特性集中没有不存在于我们的目标数据中的公司。我们可以使用列表理解进行检查，如下所示:

```
['AGN', 'ETFC']
```

这些都是熟悉的面孔，他们是在学习期间被别人收购的公司。我们需要将它们从我们的功能集中删除。

最后，我们需要通过将当前价格除以每个比率来删除定价比率中的定价信息。可以这样做:

```
<class 'pandas.core.frame.DataFrame'>
Index: 499 entries, A to ZTS
Data columns (total 52 columns):
% Held by Institutions                          493 non-null float64
5yr Avg Return                                  499 non-null float64
Annual Dividend %                               392 non-null float64
Beta                                            486 non-null float64
Change in Debt/Total Capital Quarter over Qua...471 non-null float64
Days to Cover                                   493 non-null float64
Dividend Change %                               403 non-null float64
Dividend Growth 5yr                             354 non-null float64
Dividend Growth Rate, 3 Years                   392 non-null float64
EPS (TTM, GAAP)                                 488 non-null float64
EPS Growth (MRQ)                                485 non-null float64
EPS Growth (TTM)                                487 non-null float64
EPS Growth 5yr                                  437 non-null float64
FCF Growth 5yr                                  483 non-null float64
Float                                           493 non-null float64
Gross Profit Margin (TTM)                       420 non-null float64
Growth 1yr Consensus Est                        473 non-null float64
Growth 1yr High Est                             473 non-null float64
Growth 1yr Low Est                              473 non-null float64
Growth 2yr Consensus Est                        494 non-null float64
Growth 2yr High Est                             495 non-null float64
Growth 2yr Low Est                              494 non-null float64
Growth 3yr Historic                             495 non-null float64
Growth 5yr Actual/Est                           495 non-null float64
Growth 5yr Consensus Est                        494 non-null float64
Growth 5yr High Est                             495 non-null float64
Growth 5yr Low Est                              494 non-null float64
Growth Analysts                                 495 non-null float64
Historical Volatility                           499 non-null float64
Institutions Holding Shares                     493 non-null float64
Interest Coverage (MRQ)                         388 non-null float64
Net Profit Margin (TTM)                         487 non-null float64
Operating Profit Margin (TTM)                   487 non-null float64
Quick Ratio (MRQ)                               322 non-null float64
Return On Assets (TTM)                          477 non-null float64
Return On Equity (TTM)                          451 non-null float64
Return On Investment (TTM)                      435 non-null float64
Revenue Growth (MRQ)                            485 non-null float64
Revenue Growth (TTM)                            487 non-null float64
Revenue Growth 5yr                              488 non-null float64
Revenue Per Employee (TTM)                      457 non-null float64
Shares Outstanding                              493 non-null float64
Short Int Current Month                         493 non-null float64
Short Int Pct of Float                          493 non-null float64
Short Int Prev Month                            493 non-null float64
Short Interest                                  493 non-null float64
Total Debt/Total Capital (MRQ)                  458 non-null float64
Volume 10-day Avg                               498 non-null float64
Book (MRQ)                                      453 non-null float64
Cash Flow (TTM)                                 466 non-null float64
Earnings (TTM)                                  447 non-null float64
Sales (TTM)                                     488 non-null float64
dtypes: float64(52)
memory usage: 210.5+ KB
```

我们有了它，一个包含所有连续数字变量的清晰的特性集。需要注意的是，这里的要素之间存在多重共线性，但由于我们在当前价格建模任务中不会考虑要素的重要性，因此不需要解决这个问题，因为模型的准确性不会受到负面影响。但是，我们将在下一个回归建模任务中处理多重共线性，其中将检查要素的重要性。

## **输入缺失数据:**

Scikit-learn 在他们的[简单估算器](https://scikit-learn.org/stable/auto_examples/impute/plot_missing_values.html)、 [KNNImputer](https://scikit-learn.org/stable/modules/generated/sklearn.impute.KNNImputer.html) 和[迭代估算器](https://scikit-learn.org/stable/auto_examples/impute/plot_iterative_imputer_variants_comparison.html#sphx-glr-auto-examples-impute-plot-iterative-imputer-variants-comparison-py)类之间提供了多种有效的估算方法。简单估算者可以利用多种策略来填充 nans:使用平均值、中值、众数或常数值。KNNImputer 利用 K 最近邻模型，使用其他数据列作为预测值来估计缺失值。IterativeImputer 是实验性的，允许将任何估计量传递给它，它用循环方式估计缺失值。强烈建议读者在前面的链接中查阅这些类的文档。

这些选项之间的最佳选择取决于手头的数据、任务和算法，这就是为什么通常最佳做法是用每种插补方法训练一个模型实例并比较结果。下面，我将建立一些辅助函数，它们是上述 scikit-learn 文档链接中的代码的修改版本，这将帮助我们在问题的上下文中比较这些插补方法。请注意，迭代估算器需要导入一个名为“enable _ iterative _ imputer”的特殊项目才能工作。让我们导入我们需要的内容并创建我们的函数，这将为我们的每种插补方法生成交叉验证分数，并给出我们可以用来比较它们的结果。

请注意，缩放是这个过程的一部分。现在，将上述所有功能合并到一个包装器功能中很有帮助，这将使测试和绘制各种回归变量和任务的所有插补方法的分数变得容易，而不必重复任何代码。

很好，现在我们有了一个框架来测试我们选择的任何估算器的插补方法，并返回方便的 axes 对象来查看它们。存储库中的笔记本对当前价格建模任务中的许多不同回归变量进行了比较，但是为了节省空间，我们将重点关注在此任务中表现最佳的回归变量:scikit-learn 的 GradientBoostingRegressor。首先，我们实例化一个现成的回归变量，并将其与我们的数据一起传递到包装函数中，以查看插补方法的比较情况。请注意，compare _ imputer _ scores 函数接受一个与 IterativeImputer 一起使用的估计器列表，它可以有可变参数，我们需要创建这个列表以将其传递给函数。

![](img/75db689ac61778e5c327366c926b7ef2.png)![](img/3a38628d33eda627784a59521a2ea26c.png)

上面的图表显示了 SimpleImputer、KNNImputer 和现成的 IterativeImputer 的得分。下图比较了迭代估算器与估算器列表中每个估算器的性能。我们可以看到一些实实在在的 R 平方分数，但最好的性能发生在 KNNImputer 上。在执行网格搜索之前，看到一个计算强度较低但得分最高的估算器(即不是迭代估算器)总是令人欣慰的，因为迭代估算器需要更长的时间，当拟合数千个模型时，这确实会产生明显的差异。

## **建模:**

既然我们已经选择了最佳回归变量/估算变量组合，我们可以进行网格搜索以找到模型的最佳超参数，然后继续研究残差。

太棒了，过一会儿我们就有了最好的模型。让我们看看它们产生的最佳参数和预测精度。

```
Best Model: 
 r-squared: 0.8738820298532683{'imputer__n_neighbors': 5,
 'regressor__learning_rate': 0.1,
 'regressor__max_depth': 2,
 'regressor__n_estimators': 1000,
 'regressor__subsample': 0.7}
```

很好，我们可以看到该模型的 R 平方得分为 0.87，使用了 1000 个弱学习者，每个树的最大深度仅为 2 级，对 70%的数据进行子采样，学习率为 0.1。

现在我们有了一个用我们的数据训练的资产定价模型，我们可以看到实际的当前价格如何偏离残差中的模型预测，然后查看这些残差是否与自刮擦日期以来的回报相关。提醒一下，希望资产价格高于模型估计值越高，越有可能向下移动，或者资产价格低于模型估计值越低，越有可能随着时间的推移向估计值向上移动。由于残差的计算是实际减去预测，如果我们的假设是正确的，残差应该与回报负相关。让我们首先生成残差，并看看它们。

![](img/04d62fceee1374a52f086f6e96f2ad61.png)

我们可以看到残差在零附近有一个薄的、相当对称的分布，有一些大的异常值。让我们看看他们之间是否有线性关系。我将移除超过三个标准差的异常值，并拟合一个线性回归模型来完成这项工作。要查看这种编码，请参考[库](https://github.com/FoamoftheSea/mod5project)，它只是使用残差作为独立变量对日志回报进行简单的线性回归。

![](img/a14861851e04b754311795e35e1c8c44.png)

这种线性回归虽然显示了轻微的负斜率，但提供了 0.004 的 R 平方和 0.186 的 p 值，用于将残差与回报相关联的系数，因此这并不表示强有力的线性关系。因此，假设我们的资产定价模型的残差将与六个月期间的回报相关，因为刮擦是不支持的。尽管这可能令人失望，但这就是科学的本质。在更长的时间段内研究这一点可能是合适的，因为这一假设是从价值投资的角度得出的，价值投资者通常计划持有资产更长的时间，通常至少超过一年。顺便说一句，对于更短的时间段，如本研究中使用的时间段，人们可以认为高估或低估的证券将经历价格动量，吸引交易者以当前趋势进行交易，从而助长当前趋势，因此这些证券可能会在市场朝着其潜在价值自我修正之前在短期内朝着同一方向移动。此外，上面使用的残差分布非常紧密，因为回归是使用计算残差的所有样本进行训练的。在维持集上重复这一过程并比较结果可能是值得的。

# 建模回报

现在我们继续第二项任务:对自刮刮之日起六个月内的回报进行建模。这可以通过多种方式实现，这里我们将采用三种方式。第一个是尝试使用数据作为预测器回归自刮擦以来的连续回报值，第二个是创建一个二元分类器来预测获利者/输家(高于或低于或等于零的回报)，第三个是对超过或低于市场表现的股票进行分类(高于或低于或等于指数平均回报的回报)。如上所述，两种不同分类任务的原因是，牛市(上涨)将导致指数内所有股票平均向上移动，同样熊市(下跌)将导致所有股票平均向下移动。通过从该特定时间段的回报中减去回报的平均值，该模型可以更好地推广到不同时间段的应用。不幸的是，我们没有数据来测试另一个时间段，所以我们将无法测试该理论，也无法评估在一个时间段训练的模型在预测另一个时间段的回报方面的性能。我们将能够在拒不合作的证券集合上测试该模型，尽管是在与培训相同的时间段内。尽管有这个不幸的细节，使用我们的特征的模型可以成功地预测任何时间段的赢家和输家，这表明这些特征具有预测能力，值得对未来不同时间段进行进一步的研究。我们还将能够观察模型的特征重要性，这可以提供对哪个特征在预测回报中贡献最大的洞察。

让我们将目标变量放在一个方便的数据框架中:

![](img/b449af3f312e8cab056535e90b3adfd2.png)

## **功能选择:**

因为我们这次不预测证券的价格，所以包含价格信息的特性不再是一个问题，这意味着我们可以包含上次没有包含的特性。但是，由于我们想要准确了解要素的重要性，因此需要处理要素之间的多重共线性。有时，不处理多重共线性可以提高模型的准确性，因此通常最好分别训练一个模型并进行比较，使用最准确的模型进行预测，使用训练有多重共线性的模型进行分析。在这种情况下，发现在多重共线性被管理后，分类模型甚至更加准确，但是回归模型的(已经令人失望的)性能由于移除多重共线性而受到影响。为了节省篇幅，我将简要介绍一下回归的结果，这些结果并不令人印象深刻，然后继续讨论消除多重共线性和执行分类。

对于任何即将到来的任务，要做的第一件事是删除我们肯定不会使用的功能，并看看接下来应该采取什么步骤来清理数据。我们再次从完全原始的数据框架开始。

```
<class 'pandas.core.frame.DataFrame'>
Index: 501 entries, A to ZTS
Data columns (total 83 columns):
% Held by Institutions                          495 non-null float64
52-Wk Range                                     501 non-null object
5yr Avg Return                                  501 non-null float64
5yr High                                        501 non-null float64
5yr Low                                         501 non-null float64
Annual Dividend %                               394 non-null float64
Ask                                             494 non-null float64
Ask Size                                        492 non-null float64
B/A Ratio                                       492 non-null float64
B/A Size                                        501 non-null object
Beta                                            488 non-null float64
Bid                                             499 non-null float64
Bid Size                                        492 non-null float64
Change in Debt/Total Capital Quarter over Qua...473 non-null float64
Closing Price                                   500 non-null float64
Day Change %                                    501 non-null float64
Day High                                        501 non-null float64
Day Low                                         501 non-null float64
Days to Cover                                   495 non-null float64
Dividend Change %                               405 non-null float64
Dividend Growth 5yr                             354 non-null float64
Dividend Growth Rate, 3 Years                   392 non-null float64
EPS (TTM, GAAP)                                 490 non-null float64
EPS Growth (MRQ)                                487 non-null float64
EPS Growth (TTM)                                489 non-null float64
EPS Growth 5yr                                  438 non-null float64
FCF Growth 5yr                                  485 non-null float64
Float                                           495 non-null float64
Gross Profit Margin (TTM)                       422 non-null float64
Growth 1yr Consensus Est                        475 non-null float64
Growth 1yr High Est                             475 non-null float64
Growth 1yr Low Est                              475 non-null float64
Growth 2yr Consensus Est                        497 non-null float64
Growth 2yr High Est                             497 non-null float64
Growth 2yr Low Est                              497 non-null float64
Growth 3yr Historic                             497 non-null float64
Growth 5yr Actual/Est                           497 non-null float64
Growth 5yr Consensus Est                        497 non-null float64
Growth 5yr High Est                             497 non-null float64
Growth 5yr Low Est                              497 non-null float64
Growth Analysts                                 497 non-null float64
Historical Volatility                           501 non-null float64
Institutions Holding Shares                     495 non-null float64
Interest Coverage (MRQ)                         389 non-null float64
Last (size)                                     501 non-null float64
Market Cap                                      501 non-null object
Market Edge Opinion:                            485 non-null object
Net Profit Margin (TTM)                         489 non-null float64
Operating Profit Margin (TTM)                   489 non-null float64
P/E Ratio (TTM, GAAP)                           451 non-null object
PEG Ratio (TTM, GAAP)                           329 non-null float64
Prev Close                                      501 non-null float64
Price/Book (MRQ)                                455 non-null float64
Price/Cash Flow (TTM)                           468 non-null float64
Price/Earnings (TTM)                            448 non-null float64
Price/Earnings (TTM, GAAP)                      448 non-null float64
Price/Sales (TTM)                               490 non-null float64
Quick Ratio (MRQ)                               323 non-null float64
Return On Assets (TTM)                          479 non-null float64
Return On Equity (TTM)                          453 non-null float64
Return On Investment (TTM)                      437 non-null float64
Revenue Growth (MRQ)                            487 non-null float64
Revenue Growth (TTM)                            489 non-null float64
Revenue Growth 5yr                              490 non-null float64
Revenue Per Employee (TTM)                      459 non-null float64
Shares Outstanding                              495 non-null float64
Short Int Current Month                         495 non-null float64
Short Int Pct of Float                          495 non-null float64
Short Int Prev Month                            495 non-null float64
Short Interest                                  495 non-null float64
Today's Open                                    501 non-null float64
Total Debt/Total Capital (MRQ)                  460 non-null float64
Volume                                          500 non-null float64
Volume 10-day Avg                               500 non-null float64
Volume Past Day                                 501 non-null object
cfra                                            479 non-null float64
creditSuisse                                    337 non-null object
ford                                            493 non-null float64
marketEdge                                      484 non-null float64
marketEdge opinion                              484 non-null object
newConstructs                                   494 non-null float64
researchTeam                                    495 non-null object
theStreet                                       496 non-null object
dtypes: float64(73), object(10)
memory usage: 348.8+ KB
```

B/A 大小被编码为字符串，但是我们有用于出价大小和要价大小的数字特性，所以我们根本不需要 B/A 大小特性，并且它可以被删除。还有我们之前发现的字符串格式的重复市盈率(TTM，GAAP)功能，可以再次删除。有两列,“市场优势意见”和“市场优势意见”是相同的，因为在“市场优势”列中有与此分析师评级相对应的数字，所以我们可以删除这两个“市场优势意见”列。过去一天的交易量特征是一个字符串格式的分类变量，它告诉我们与证券的典型交易日相比，前一个交易日的交易量是低、低于平均值、平均值、高于平均值还是高。此功能可以是一个热编码，但由于有一个数量 10 天平均功能，这是一个数字，更能描述最近的数量趋势，我们将只放弃过去一天的数量。在开始插补步骤之前，我们只剩下五个需要管理对象数据类型的特征:52 周范围、市值、瑞士信贷、研究团队和华尔街。让我们删除不需要的列，然后仔细看看这 5 列。

![](img/9ed66f6bf67fbba09ca50122d7f42d46.png)

看起来我们可以修改 52 周范围内的字符串，得到两个数字列:52 周低和 52 周高。然后，我们可以继续转换市值列，但我们需要知道它包含的所有唯一字母，这些字母代表与它们相关的数字的数量级。让我们处理 52 周的范围，并检查我们的结果。

![](img/f38dfd09f6c65b5244a137e6f7248fcc.png)

好吧，这个成功了。现在我们需要确定 Market Cap 列中的数字有哪些字母后缀，因此我们可以构建一个函数来将该列转换为数字数据类型。我们也可以删除旧的 52 周范围列。

```
array(['B', 'T', 'M'], dtype=object)
```

这里我们可以看到，该列有 B、T 和 M 后缀，分别表示十亿、万亿和百万。知道了这些，我们就可以做一个函数来对待这个列了。

![](img/6283928220499fac63702761c08a4559.png)

太好了，又一个成功的故事。旧列可以删除。现在我们需要检查我们的特性集中是否有任何公司没有出现在 log_returns 中。回想之前，我们应该可以在这个列表中找到 AGN 和 ETFC。

```
['AGN', 'ETFC']
```

正如所怀疑的。我们需要从特性集的索引中删除这些。因为我们从前面也知道我们有需要重新编码为 nan 的 INF，所以我们也将在这一步中这样做。

现在，在继续处理多重共线性之前，我们还有一项工作要做:我们需要对编码为字符串的分类分析师评级进行一次热编码。既然我们已经修复了所有其他列，我们可以简单地调用 pandas get_dummies 方法，并确保我们将 drop_first 设置为 True，以避免[虚拟变量陷阱](https://www.kaggle.com/getting-started/149280)。

```
<class 'pandas.core.frame.DataFrame'>
Index: 499 entries, A to ZTS
Data columns (total 82 columns):
% Held by Institutions                          493 non-null float64
5yr Avg Return                                  499 non-null float64
5yr High                                        499 non-null float64
5yr Low                                         499 non-null float64
Annual Dividend %                               392 non-null float64
Ask                                             492 non-null float64
Ask Size                                        490 non-null float64
B/A Ratio                                       490 non-null float64
Beta                                            486 non-null float64
Bid                                             497 non-null float64
Bid Size                                        490 non-null float64
Change in Debt/Total Capital Quarter over Qua...471 non-null float64
Closing Price                                   498 non-null float64
Day Change %                                    499 non-null float64
Day High                                        499 non-null float64
Day Low                                         499 non-null float64
Days to Cover                                   493 non-null float64
Dividend Change %                               403 non-null float64
Dividend Growth 5yr                             354 non-null float64
Dividend Growth Rate, 3 Years                   392 non-null float64
EPS (TTM, GAAP)                                 488 non-null float64
EPS Growth (MRQ)                                485 non-null float64
EPS Growth (TTM)                                487 non-null float64
EPS Growth 5yr                                  437 non-null float64
FCF Growth 5yr                                  483 non-null float64
Float                                           493 non-null float64
Gross Profit Margin (TTM)                       420 non-null float64
Growth 1yr Consensus Est                        473 non-null float64
Growth 1yr High Est                             473 non-null float64
Growth 1yr Low Est                              473 non-null float64
Growth 2yr Consensus Est                        495 non-null float64
Growth 2yr High Est                             495 non-null float64
Growth 2yr Low Est                              495 non-null float64
Growth 3yr Historic                             495 non-null float64
Growth 5yr Actual/Est                           495 non-null float64
Growth 5yr Consensus Est                        495 non-null float64
Growth 5yr High Est                             495 non-null float64
Growth 5yr Low Est                              495 non-null float64
Growth Analysts                                 495 non-null float64
Historical Volatility                           499 non-null float64
Institutions Holding Shares                     493 non-null float64
Interest Coverage (MRQ)                         388 non-null float64
Last (size)                                     499 non-null float64
Net Profit Margin (TTM)                         487 non-null float64
Operating Profit Margin (TTM)                   487 non-null float64
PEG Ratio (TTM, GAAP)                           329 non-null float64
Prev Close                                      499 non-null float64
Price/Book (MRQ)                                453 non-null float64
Price/Cash Flow (TTM)                           466 non-null float64
Price/Earnings (TTM)                            447 non-null float64
Price/Earnings (TTM, GAAP)                      447 non-null float64
Price/Sales (TTM)                               488 non-null float64
Quick Ratio (MRQ)                               322 non-null float64
Return On Assets (TTM)                          477 non-null float64
Return On Equity (TTM)                          451 non-null float64
Return On Investment (TTM)                      435 non-null float64
Revenue Growth (MRQ)                            485 non-null float64
Revenue Growth (TTM)                            487 non-null float64
Revenue Growth 5yr                              488 non-null float64
Revenue Per Employee (TTM)                      457 non-null float64
Shares Outstanding                              493 non-null float64
Short Int Current Month                         493 non-null float64
Short Int Pct of Float                          493 non-null float64
Short Int Prev Month                            493 non-null float64
Short Interest                                  493 non-null float64
Today's Open                                    499 non-null float64
Total Debt/Total Capital (MRQ)                  458 non-null float64
Volume                                          498 non-null float64
Volume 10-day Avg                               498 non-null float64
cfra                                            478 non-null float64
ford                                            491 non-null float64
marketEdge                                      482 non-null float64
newConstructs                                   492 non-null float64
52-Wk Low                                       499 non-null float64
52-Wk High                                      499 non-null float64
marketCap                                       499 non-null float64
creditSuisse_outperform                         499 non-null uint8
creditSuisse_underperform                       499 non-null uint8
researchTeam_hold                               499 non-null uint8
researchTeam_reduce                             499 non-null uint8
theStreet_hold                                  499 non-null uint8
theStreet_sell                                  499 non-null uint8
dtypes: float64(76), uint8(6)
memory usage: 303.1+ KB
```

## 回归

太好了，我们有一个清晰的数据框架来执行回归。研究发现，scikit-learn 和 xgboost 测试的各种回归算法在使用这些数据回归自刮擦日期以来的回报的任务中一直表现平平。scikit-learn 的 RandomForestRegressor 提供了最佳性能，在零或均值插补后，平均 r 平方得分略低于. 21，得分差异很大。下面我们可以看到这些结果。

![](img/9605611ce62fdd2b40b44e68f694628a.png)![](img/e681fa54627214d248e759cd2e1eb771.png)

这些分数并不特别令人印象深刻，但它们足够高，表明该模型在某种程度上是有效的。R-squared 告诉我们模型所解释的目标方差的比例，即使这个方差的大部分没有被解释，但它的很大一部分被解释了，这意味着我们的特征提供了一些预测能力。尽管回归的结果有些令人失望，但是即将到来的分类任务是很有收获的。如上所述，消除多重共线性实际上有助于提高分类的性能，因此从这里开始，我将首先演示如何消除多重共线性，然后继续构建分类模型。

## **消除多重共线性:**

由于我们希望从我们训练的模型的特征重要性中获得有意义的见解，我们现在需要处理特征之间的多重共线性。要开始这个过程，我们需要生成一个热图来可视化所有特性的关联矩阵。我们可以用熊猫的组合来做这件事。corr 方法和 seaborn 的热图。

![](img/5c69d6c6ad2aa3d0a26bf6434b57e3ee.png)

首先要注意的是，年股息率与许多基于价格的特征呈负相关，尤其是与 5 年平均回报率呈负相关。我们可以查看这种关系的散点图，以获得更详细的了解。

![](img/fd03632ba99f65b90d6931b9ba287455.png)

我们可以在这里看到很强的负相关性。还可以注意到，其中一些年度股息相当大，一路高达每股价格的 17.5%。无论何时向股东支付股息，都要从支付日的股价中扣除。因此，如果一个年度股息是 5%，那么该股票将需要有 5%的收益，才能在支付所有股息后达到收支平衡。这解释了上面看到的负相关性:股息支付越高，股价下跌越多，当年每股价格的收益就越少。我们将取消年度股息%功能。

接下来，我们可以看到所有与价格相关的特性都是高度相关的，这并不奇怪。虽然像 5 年高点，5 年低点，要价，出价，收盘价，日高，日低等特征。都表达了关于价格的不同的东西，它们是如此紧密相关，以至于没有办法确定它们对模型的单独影响，并且因为我们将考虑特征的重要性，我们需要剔除与价格相关的特征。此外，增长估计有一些强烈的多重共线性，所以我们也将放弃其中一些。

另一个很强的相关性存在于流通股和流通股之间。这些是非常相似的特征。浮存代表可供公开交易的股票数量，而流通股则是一家公司已发行的股票总数。这些必然是高度相关的，但我们可以看到如何与另一个散点图相关。

![](img/f3baeb4b5ceff364d8aeeca3a332916c.png)

上面我们可以看到是什么验证了我们对这些特征的理解:一家公司有一定数量的已发行股票，但其中一些股票可能无法公开交易。因此，我们看到，浮存永远不会高于流通股，这种关系沿着斜率 1(恒等函数)的直线几乎是完美的线性关系。我们可以放弃浮存特征，转而支持流通股。

资产回报率和投资回报率有很强的相关性。[正如本文](https://smallbusiness.chron.com/roa-vs-roi-formulas-36950.html)中所解释的，跨行业的资产回报率比较可能没有意义，在这些情况下最好使用投资回报率，包括我们发现自己所处的这种情况。因此，我们将降低资产回报率。

净利润率是营业利润率减去税收和利息，因此两者高度相关。由于先验包含更多的信息，并且它们有相同数量的缺失值，我们可以降低营业利润率。

成交量与 10 日平均成交量高度相关，由于后者包含更多信息，我们可以忽略前者。历史波动率与贝塔系数有很强的相关性，贝塔系数是一个类似的衡量指标，表示市场的相对波动率。我们将放弃历史波动，转而支持贝塔。

市盈率(TTM)和市盈率(TTM，GAAP)高度相关。如上所述，非 GAAP 指标被认为在定量财务分析中更有用，因为它忽略了最近财务报表中可能出现的大量非经常性成本，所以我们将放弃用 GAAP 收益计算的功能。

3 年的股息增长率和 5 年的股息增长率高度相关，因为它们包含许多相似的历史信息。3 年的增长率应该足以预测 6 个月的回报，因此我们将降低 5 年的股息增长率。

我们还可以看到一些短期利益相关特征之间的共线性。让我们一起来看看这些功能是什么样子的。

![](img/7a510823578fb02a2c3feb86f16c619a.png)

我们可以看到，Float 和 Short Int Pct 列是相同的，但前者的分辨率更高，因此我们可以删除后者。短 Int 当前月和短 Int 上一月彼此高度相关，并且有理由认为当前月特征更有价值，因此我们可以删除短 Int 上一月。

![](img/841e06cdeba339a267cd52922205b67d.png)

这看起来好多了。尽管变量之间仍然存在一些相关性，但我们已经处理了大多数冗余特征和多重共线性。这是进入插补和建模阶段的好地方。

## 分类—第 1 类:赢家/输家

现在我们继续我们的分类任务，两者都有二元分类目标。第一类是获利者/输家，第二类是相对于市场而言表现优异/不佳的人

**输入缺失数据(类别 1:获益者/失败者):**

这项研究表明，xgboost 包中的 XGBClassifier 在这个目标上具有更好的预测性能，因此我们将在这里对它进行研究。关于各种分类器的完整的表现性比较，请看笔记。

![](img/49a38710029e34244829f9cbbac40ff0.png)![](img/11bb2d5b20b2ad986ed6d54dbf688951.png)

我们可以看到简单估算器给了我们不错的性能，尽管差异很大。带有 DecisionTreeRegressor 的 IterativeImputer 做得最好，带有 KNeighborsRegressor 的 IterativeImputer 做得很好，具有更紧密的方差，这有时对于健壮性更好。在准备使用这些估算器进行网格搜索时，有一点要记住，由于名字中隐含的原因，迭代估算器会使搜索过程变得更长。在本研究中，我们对两者进行了比较，结果表明简单估算法既更快，又能产生更好的模型，因此我们将在此用这种估算方法进行演示。

**建模(第一类:赢家/输家):**

我们现在可以构建一个管道和一个网格搜索来为这个任务构建一个最佳模型，然后我们可以对其进行分析。

过了一会儿，我们完成了网格搜索。让我们看看它产生的最佳交叉验证分数和参数。

```
Best CV roc_auc score: 0.7014277550220106{'clf__colsample_bylevel': 1,
 'clf__colsample_bytree': 1,
 'clf__learning_rate': 0.001,
 'clf__max_depth': 5,
 'clf__n_estimators': 1000,
 'clf__reg_lambda': 0.5,
 'clf__subsample': 1}
```

交叉验证的最佳 auc 分数是 0.701，既不可怕也不可怕。让我们看看测试集的预测概率如何与对数收益相关。

![](img/094a2f9a42698f0ff0130775c98af827.png)

该模式的预测概率和对数收益之间的简单线性回归的 r 平方非常低，为 0.059，但概率系数非常显著，p 值为 0.015。我们可以在上面的散点图中看到类别的不平衡，因为在研究期间市场呈上升趋势，指数中的大多数股票都有收益，因此属于收益类。现在让我们看看随机选择一个收益者的概率是多少，用收益者类别中的证券数量除以指数中的证券总数。

```
0.7294589178356713
```

这里我们可以看到，由于市场的上涨，投资者在研究期间有 73%的机会随机选择收益者。我们现在可以开始看到，如果我们打算获得洞察力或建立一个在不同时期都有用的预测模型，为什么对盈利者/亏损者建模没有多大意义，因为盈利者/亏损者的比例(类别成员)将根据市场的行为而变化，并且该模型可能将盈利归因于预测特征，这些特征与那些特征关系不大，而与市场的运动关系更大。一会儿，我们将移除市场的平均回报以对此进行调整，并相对于市场对表现优异/表现不佳者进行建模，但首先让我们快速查看一下该模型的准确性和 roc 曲线。

```
Accuracy Score (test set): 0.81
```

![](img/7f4252732382a03c8aa306b02c49f425.png)

我们可以看到，该模型在 81%的时间里预测了正确的类别，这听起来令人印象深刻，但知道随机选择收益者的概率是 73%，这并不完全如此，我们知道，在市场趋势不同的另一个时期，这种准确性可能会有很大差异。测试集上的 auc 分数是 0.68，不算超级可观。让我们继续为我们的最后一个目标建模，现在我们知道它将为我们提供对股票之间相对表现的最强有力的洞察力。

## 分类——第 2 类:表现优秀/不优秀

正如我们从上面的建模和分析中所看到的，在给定的时间段内对盈利者/亏损者建模有一个很大的缺点，因为股票所属的类别高度依赖于该时间段内整体市场的行为。这可能导致模型在评估特征对股票表现的贡献时有偏差，因为一些表现可以归因于市场的行为，而不是股票本身的潜在特征。为了对此进行调整，我们可以从每个证券的回报中减去一段时间内市场的平均回报，从而从目标中排除整体市场运动的影响，并只关注指数中证券之间的表现差异。通过对相对业绩建模，投资者可以根据模型预测，使用长/短股票策略构建对冲投资组合，该投资组合应对不断变化的市场条件保持稳健。

**输入缺失数据(类别 2:表现优秀/表现不佳):**

来自 xgboost 的 XGBRFClassifier 被发现是最有效的分类器。我们来看看插补方法是如何与这个分类器相处的。

![](img/c0d63a9ccb3b16e4a3181dbcdaae0a85.png)![](img/9b4f7f4adb1edcc11ba9395035bfe0cd.png)

我们可以看到，使用简单估算器实现了最佳性能，虽然这些结果并不特别令人兴奋，但对分类器进行一些超参数调整会有所帮助。让我们开始网格搜索。

**模特(第二类:优秀/不优秀演员):**

运行上面的单元后，我们有一个合适的网格搜索对象，它将使我们的最优估计量适合训练集。让我们来看看这个估计器的一些特性。

```
Best CV training score: 0.690298076923077{'clf__colsample_bylevel': 0.8,
 'clf__colsample_bynode': 1.0,
 'clf__colsample_bytree': 0.6,
 'clf__learning_rate': 0.001,
 'clf__max_depth': 7,
 'clf__n_estimators': 100,
 'clf__reg_lambda': 0.75,
 'clf__subsample': 0.8}
```

在训练集上的网格搜索中，最好的交叉验证分数是 0.69 的 roc_auc，这既不伟大也不可怕。现在，我们可以使用网格搜索中的最佳估计值来生成拒绝集合的预测概率，然后查看这些概率与证券的实际对数回报之间是否存在线性关系。

![](img/3eb09172d02343c2c82df95e76a5c668.png)

我们可以看到，这些预测概率的数值范围非常小，但分类器仍然是有效的。使用预测概率对对数收益进行简单线性回归的 r 平方非常低，为 0.088，但概率系数非常显著，p 值为 0.003，这意味着对数收益和模型生成的预测概率之间存在线性关系。这对我们来说意味着，不是简单地选择高于/低于阈值概率的证券，而是可以通过创建一个围绕阈值概率的窗口，只选择概率窗口之外的股票做多或做空，来创建一个更保守的投资组合。为简单起见，这里我们只看我们选择的阈值两边的渴望或做空，但首先我们需要通过查看 roc 曲线来找出这个模型的最佳阈值。

![](img/3f38ae871302983c8ee22d1329f54cc8.png)

我们可以看到维持集的 roc_auc 分数是. 70，这还不算太差。我们还可以看到，在真阳性率略低于 0.8 和假阳性率约为 0.4 附近似乎有一个最佳点。我们可以使用 roc_curve 函数创建这些比率及其阈值的数据框架，并使用它来确定用于我们模型的最佳阈值。

![](img/5d41bf61e34cada4696e82cdcad30ded.png)

很好，现在我们可以切入这个数据框架，找到与我们在上面的 roc 曲线上看到的最佳时机相对应的概率阈值。

![](img/6cd6fa1922b5124932de044480720e07.png)

现在我们知道了，tpr 和 fpr 之间的最佳平衡点似乎在指数 21 处。我们可以通过索引该列来获得阈值的完整分辨率，并使用它来手动生成使用该阈值的预测。将使用我们选择的阈值的预测准确度与使用标准概率阈值 0.5 获得的准确度进行比较将是有用的。

```
Accuracy Score with Standard Threshold: 0.62
Accuracy Score with Selected Threshold: 0.69
```

正如我们所看到的，微调我们的分类阈值使我们的总体预测准确率又提高了 7%，达到了 69%。这比随机猜测好得多，对于这个目标来说，随机猜测有 53%的机会选择一个表现优异的人。我们可以通过指数中的股票总数除以表现出色的股票数量，来了解正确选择表现出色的股票的概率。

```
0.5270541082164328
```

我们可以看到，相对于随机选择一只表现出色的股票，这个模型确实给了我们一个预测优势。现在我们有了预测，我们可以开始使用长/短股票策略构建投资组合，并查看该投资组合的回报如何与在同一时期购买和持有市场指数进行比较。

然而，在我们继续进行投资组合构建之前，让我们看一下模型提供给我们的关于特性重要性的信息。有两种方式来看待这个问题:第一种是从训练过程中查看[特征重要性](https://scikit-learn.org/stable/auto_examples/ensemble/plot_forest_importances.html)，第二种是查看维持集上的[排列重要性](https://scikit-learn.org/stable/modules/permutation_importance.html)。比较这两者可以为模型如何处理两组数据提供有趣的见解。首先，让我们看看培训过程的特征重要性。

![](img/cb32ea7f524451168c2b560f0a7e2c4b.png)

上面我们可以看到模型拟合中特征的相对重要性，模型拟合用于进行预测。有趣的是，主要特点是福特股票研究的分析师评级，也许他们会很高兴知道。在这下面，我们可以看到我们的基本特征列表，除了一些虚拟变量，所有这些特征都在评估中扮演着重要的角色。这是非常有用的，但是它仅仅给了我们一个视图，让我们了解模型是如何构建自己来最好地适应训练集的。为了了解这些特性如何影响维持集的预测精度，我们需要使用排列重要性。置换重要性是通过反复改组每个特征(打破与目标的关系)并测量这样做所导致的预测性能损失来生成的。如果打乱某个特征会导致性能的重大损失，那么它可以说是预测中的一个重要特征。下面我们来看看这个。

![](img/a53133f30909517135766dadf59898ee.png)

上面我们可以看到每个特性的 10 次洗牌对模型性能影响的分布。我们可以注意到，尽管有一些一致性，但重要性的顺序被打乱了。这是因为预测训练集的证券目标的重要因素可能不同于预测测试集中发现的证券目标的重要因素。位于这两个列表顶部的特性可以被认为总体上是重要的，例如预计 1 年最高增长率、员工人均收入(TTM)、福特、marketCap 和 Beta。在一些排列重要性接近图表底部的情况下，特性的随机洗牌实际上提高了维持集的预测准确性！

尽管我们知道某些特征对模型很重要，但人类可能并不完全清楚它们与目标变量的关系，正如我们在下面通过绘制福特评级(显然非常有价值)的对数回报所看到的那样。

![](img/a1ae26a0380f211fdbd2c519b1b3dbef.png)

福特是一个重要特征，但与目标的关系不明确。

## 构建投资组合

为了证明我们的模型如何对投资者有用，我们将使用模型对表现优异/不佳者的预测构建一个基本的长/短股票投资组合。多头/空头股票策略的好处是，投资者通过持有等量的多头和空头头寸来对冲市场风险。这样，投资组合就不会受到市场整体向上或向下运动的影响，而只会受到投资者对其中证券相对表现的预测程度的影响。我们的模型以 69%的准确率正确识别了表现过度和表现不佳的股票，这并不完美，但当这种预测能力与长/短股票策略相结合时，它可以通过建立许多头寸来分散投资组合的风险，并对冲市场，从而带来持续盈利的交易策略。如果市场上涨，投资者将从他们的多头头寸中获利，同时损失他们的空头头寸，反之，如果市场下跌，他们将从他们的空头头寸中获利，同时损失他们的多头头寸。通过比随机预测更准确地预测哪些证券的表现将超过或低于市场，投资组合中赢的一方的收益将大于输的一方的损失，无论市场走向如何。这就是为什么从我们的目标变量中减去市场的平均回报如此重要，因为它导致开发了一个只关注相对表现的模型，忽略了市场的影响。

我们可以使用测试集的证券来模拟这种投资组合的表现，方法是使用每个证券的类别预测来改变它们各自的对数回报的符号，并将所有这些调整后的回报进行平均。这相当于投资者对测试集中的每种证券进行等量的美元投资，做空模型预测表现不佳的任何股票，做多预测表现出色的任何股票。因为做空一只股票会将亏损转化为收益，反之亦然，所以我们将持有所有预测级别为 0 的证券，并反转符号。

![](img/938401be361e7925e59d8b13d09e0422.png)

现在，我们准备确定一个投资组合的回报，该投资组合在坚守组合的每只股票中持有相等的美元价值头寸，在预测表现不佳的股票中做空，在预测表现出色的股票中做多。我们可以这样做:

```
Portfolio Return (test set): 0.10186008839757786
Market Return (test set): 0.14362567579115593
```

我们可以看到投资组合的表现大大低于市场，但这是对冲策略的本质。这是因为市场本身在所研究的时间段内表现很好，所以投资组合的空头出现了亏损，这从购买和持有市场指数的潜在收益中扣除了。为了在市场表现不佳的情况下保护投资组合，相对于这样一个繁荣的市场牺牲了比较业绩。为了看到这一点，让我们重复这一比较，但使用模拟熊市(下跌)市场，我们可以通过从每个证券中减去平均市场回报两次来创建这一市场，从而使市场的整体运动与实际发生的情况相反。然后，我们可以比较在这些厌恶的环境下，投资组合与购买和持有市场指数的情况。

![](img/9792c644999dce47d4018b093e78ed9b.png)

现在，我们可以重复上面的过程，看看我们的投资组合表现如何，与购买和持有这个模拟熊市相比。

```
Portfolio Return (test set): 0.050753389653326986
Market Return (test set): -0.06931890230988941
```

在这里，我们可以看到对冲投资组合设计的真正亮点:在购买并持有市场的人会遭受损失的情况下，对冲投资组合实际上由于空头头寸而获得了收益，从而证明了多空股票投资组合策略如何降低市场风险，并带来更稳定的盈利能力。

**可视化投资组合表现:**

通过可视化投资组合与市场的表现，我们可以更深入地了解正在发生的事情。在这种情况下，我们实际上看到的是处于抵制集内的那部分市场。在我们通过调用 pandas 创建了一个代表指数中所有证券的每日收益的数据框架之后，我们可以快速地看到整体市场与我们的测试集有何不同。log_close 数据框上的 diff()方法，该方法将计算日志价格的每日变化，也称为日志回报。

![](img/87f38fba8e25a1915bea3e966d9203b1.png)

我们可以用累计总和来表示一段时间内的累计回报。下面我们将看看测试集如何与整个索引进行比较。

![](img/c3c9ef8608827c4d53424a1ed9212d1f.png)

我们可以从上面看到，标准普尔 500 在测试集中的部分明显优于指数，但整体形状几乎完全相同。回想一下上面的例子，坚守者的平均回报率是 0.14，而指数的整体回报率是 0.11。现在，我们可以使用模型给出的表现不佳和表现不佳的预测，将测试集中的证券分为多头和空头。投资组合将做多所有被预测表现优异的公司，做空所有被预测表现不佳的公司。为了构建这个投资组合，我们将把 log_returns_full 框架拆分为长边和短边，反转短边的符号，并将两者重新组合。然后，我们将看一张图表，比较测试集的买入并持有策略与我们所做的长/短投资组合的回报。

![](img/df42ba78f61456f4604d7df92b47bacb.png)

现在我们可以真正看到这里发生的事情的更多细节。我们看到，测试集的最终回报率为 0.14，投资组合的总回报率为 0.10，正如我们之前计算的那样，但现在我们可以看到通向该点的历史。请注意，投资组合的波动性比买入并持有策略要小得多，波峰和波谷要微妙得多。这是由于对冲。为了更好地了解正在发生的事情，我们可以分别看看投资组合的长端和短端的回报。

![](img/e3f9d14496a550a25454c319028deb09.png)

在这里，我们可以最清楚地看到我们战略背后的机制。绿线对应的是我们多头头寸的集体收益，红线显示的是我们空头头寸的集体收益。请注意，它们看起来几乎是彼此的倒影，但是绿线比红线走得更远。这就是我们行动战略的全部目标！镜像的波峰和波谷共同创造了投资组合相对于买入并持有策略的平滑线，因为所有股票经历的市场波动大部分被这种对冲抵消了。事实上，红线的损失没有绿线的收益多，这要归功于我们的模型的帮助，它成功地帮助我们选择了更有可能超过或低于市场表现的股票。因为当价格上涨时，空头头寸会使投资者亏损，这些头寸作为一个整体表现低于市场的事实意味着损失将被最小化，而投资者的多头头寸可能会使收益最大化。

为了解释为什么我们愿意为我们的对冲策略牺牲利润，让我们创建一个模拟熊市，看看同样的投资组合在这种环境下会有什么表现。为了创造熊市，我们可以从每家公司当天的日志回报中减去每天日志回报的平均值两次，有效地逆转了这段时间内的市场流向。让我们这样做，并创建一个可视化来验证它已经按预期工作。

![](img/17ba9817d239554b3dfc49e4d42f7e87.png)

我们可以看到，这种数学反映了指数的累积回报，并给了我们一个市场低迷的模拟。让我们看看在这些市场条件下，由我们的模型预测确定的相同投资组合头寸与测试集相比会有怎样的表现。

![](img/6ac91ec644449165f94484ea89fd6754.png)

在这里，我们可以看到我们的战略之美。在这些不利的条件下，空头头寸产生的利润超过了多头头寸的损失，因此投资组合实际上获得了收益，而买入并持有的投资者可能会遭受损失，而且没有所有的剧烈波动。让我们再来分别看一下投资组合的两个部分。

![](img/ca2d9f89f5204429004cc2b97b40a6c3.png)

我们可以看到，代表空头头寸的红线正在推动利润，该模型的预测能力通过根据最有可能跑赢或跑输市场的证券来配置我们的投资组合，再次导致了这些条件下的整体胜利。请注意，绿线最终几乎达到收支平衡，而测试集下降到-.06，这意味着我们的多头头寸确实跑赢了市场。根据我们的计划，由于空头头寸共同提供了 0.16 的累积对数回报，他们实际上看到了 0.16 的损失，这意味着他们在艰难时期的表现远远低于市场。

# 结论

在这项研究中，从 web 刮刀获得的数据已用于许多调查。首先，我们试图创建一个资产定价模型，通过回归刮刮当天的股票价格，并试图使用残差作为安全边际，来帮助制定 6 个月的价值投资策略，但没有成功。然后，我们开始用多种方法对刮擦后的回报进行建模。回归对数收益的连续值会导致 r 平方在 0.20 左右的黯淡表现，但进一步的测试可能会显示这些模型在为交易者提供优势方面并非完全无用。然后对模型进行训练，以对赢家和输家进行分类，但这些模型被发现存在缺陷，因为它们偏向于自刮擦之日起的牛市环境，类别成员受市场运动的影响，而不仅仅是受公司基本面差异的影响。因此，最有成效的调查来自训练模型，以确定哪些股票相对于市场表现更好或更差。这种模型使用基本面特征来确定哪些股票的表现优于或劣于指数中的其他股票，这对于变化的市场条件来说是一种更稳健的比较，因为市场有上升和下降趋势的时期。一旦训练了一个适当的模型来对指数中表现优异和表现不佳的股票进行分类，就在长/短股票策略中构建了一个模拟投资组合，以显示如何通过为开发对冲投资组合提供指导来使用模型的预测为投资者提供交易优势。这一模拟表明，该模型确实有助于创建一个持续盈利的策略，该策略具有多样化的头寸风险，并对冲了市场风险。

这一领域的进一步研究将是非常适宜的。尽管 data scraper 给了我们一个很棒的数据集，但在未来的工作中还有一些地方需要改进。首先，无论是在开发还是在部署过程中，web 抓取过程都非常耗时，并且导致只有 500 家公司的有限数据集可供研究。选择这条道路是为了避免为这种深入的基本面数据付费的成本，但现在可以证明，这种数据可以与机器学习算法配对，以提供交易优势，为这种数据付费是合理的。此外，通过数据订阅，可以研究多个时间段(以及各种长度)，而不是一个时间段。这将极大地有助于训练模型和测试它们在样本外数据上的部署。这种数据也可能具有较少的缺失值，因此需要估算的部分更少，从而导致更好的模型性能。