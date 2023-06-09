# 网页抓取:BeautfullSoup

> 原文：<https://medium.com/analytics-vidhya/webscraping-beautfullsoup-69ae54a9d16a?source=collection_archive---------22----------------------->

![](img/07402c5d536b00e991d594da9e5c35fc.png)

# 什么是网络搜集？

网络抓取是一个流行术语，指从互联网上提取数据的方法，甚至复制粘贴你最喜欢的歌曲的歌词也是一种网络抓取的形式。然而，“网络搜集”这个词通常指的是一个涉及自动化的过程。如果你曾经从一个网站上复制并粘贴信息，你已经完成了与任何网页抓取器相同的功能，只不过是手动的。数据是组织中的重要资产，web 抓取允许从各种 web 源中高效地提取这种资产。Web 抓取有助于将非结构化数据转换为结构化数据，从而可以进一步用于提取见解。

# **为什么要刮网？**

首先，一个我喜欢的短语。

***W*** *。* ***爱德华兹*** ***戴明***没有数据你只是另一个有观点的人。”

数据是一项重要的资产。相比之下，当你试图手动获取你想要的信息时，你可能会花很多时间点击、滚动和搜索。如果您需要来自定期更新新内容的网站的大量数据，情况尤其如此。手动抓取网页会花费很多时间和重复。

![](img/17e95ee6e7d0cb3416bdd7db0c9097fd.png)

**科技让提取数据变得容易**

![](img/8a1e2e6e26ecba4e2c0c345827be9265.png)

如今，几乎每个人都可以接触到技术，这使得几乎任何人都可以非常容易地进行大规模的网络抓取。

网络上有很多内容可以帮助你掌握网络抓取，甚至可能有更多的服务提供商如 Octoparse 帮助你收集数据。

有一些网络应用程序可以帮助你快速收集大量信息。此外，大规模部署机器人变得越来越容易。它使公司能够提取任何规模的数据。

**更好地访问公司数据**

![](img/aa96cd56e8854f751c7a2b5e3857dd8e.png)

任何拥有网站、社交媒体并接受任何类型的电子支付的企业都在收集有关客户、用户习惯、网络流量、人口统计等数据。如果你能学会获取，所有这些数据都充满了潜力。虽然做决定时会受到很多因素的影响。公司里发生了什么，全球新闻，但没有什么比有确凿的数据支持你更有说服力了。这是你不能错过的增加利润的力量。你可以从你的客户使用的社交网络和其他平台收集数据，从而了解你的客户对你的产品或公司的看法。

**人人品牌监测**

![](img/577e1235ef7216d12c8c2dbb62a31823.png)

品牌监测市场增长非常快。我想我们都同意，检查其他客户评论已经成为网上购物的一个基本步骤。顾客越来越精明:他们喜欢被推荐产品，并被保证他们做出了“正确的选择”。
有如此多的平台收集评论和评级，你需要从每个网站提取评论，然后汇总它们。正如我所说，你还可以监控社交网络，并将其与情绪分析相结合，以快速回应仇恨者或奖励爱你的用户。

如果你希望拥有一个成功的品牌，你必须监控你的客户的意见(数据)。

**机器学习和大数据集**

![](img/c8d9c507ef0fd2f216271c418db97246.png)

你的任务是建立一个将汽车分类的模型。你的产品负责人希望你使用深度学习，因为他认为这是这样一个用例的一个很好的选择。
你需要大量的数据来建立你的训练集。你不能用手来做，韦伯。刮痧！
需要预测竞争对手的定价吗？收集数据。网络抓取实际上是数据科学家最好的朋友。但你是数据科学家，不是该死的机器人！你想分析和建立预测模型，而不是清理和提取 web 数据。

**规模化市场分析**

![](img/dab88fe9d54f0cc401265db1e22feaf0.png)

大数据、商业智能和数据科学是当今的热门话题。归根结底，重要的是质量而不是数量你不需要大数据，而是智能数据。
假设您销售电子零部件。但是你怎么知道一个特定的零件卖多少钱呢？我的意思是，如果你能把价格优化 15%……想象一下规模带来的额外收入。网络抓取:你只需要收集分销商使用的特定网站的数据，你就可以从你提取的数据中建立一个大型数据库。
尽管在这种情况下，数据处理可能有点棘手，因为产品参考并不总是相同的。

# **BeautifulSoup？**

![](img/d2ac51294a6e46ee90a99320d35f5e46.png)

# **BeautifulSoup‍**

BeautifulSoup 是一个 Python 库，用于从 HTML 和 XML 文件中提取数据。它与您喜欢的解析器一起工作，提供导航、搜索和修改解析树的惯用方式。它通常可以节省程序员数小时或数天的工作。
这些说明举例说明了 Beautiful Soup 4 的所有主要特性。我向你展示图书馆的好处，它是如何工作的，如何使用它。首先，使用您的终端安装漂亮的汤库并导入它:

```
!pip install beautifulsoup
from bs4 import BeautifulSoupsoup = BeautifulSoup()
```

# 请求

我们需要做的第一件事就是下载网页。我们可以使用来自 Python 的库请求，并用 GET 进行请求。

```
from urllib.request import Requestpage = request.get("[https://github.com/bezerraluis/Luis-Paulo-Bezerra](https://bezerraluis.github.io/Luis-Paulo-Bezerr)")
```

# BeautifulSoup 命令:

# 查找()

使用 find()函数，我们可以在网页上搜索任何内容。
假设我们想根据产品的 id 获得产品的标题，我们只需要传递标签，例如标签‘b’。

```
soup.find('b')
```

# 查找全部()

最简单的过滤器是字符串。将一个字符串传递给一个搜索方法，Beautiful Soup 将根据该字符串执行匹配。这段代码查找文档中所有的‘b’标签(你可以用任何你想查找的标签替换 b)

```
soup.find_all('b')
```

# get_text()

我们使用 get_text()提取新发现的元素标题的文本部分。

```
soup.find_all('b').get_text()
```

![](img/7782b04766b9b74b01e095404801583e.png)

今天到此为止，我希望现在你能理解网络抓取的重要性，并能使用 BeautifulSoup 完成这项任务。