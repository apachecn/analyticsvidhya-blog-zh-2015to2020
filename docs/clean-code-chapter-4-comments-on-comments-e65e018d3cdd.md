# 干净代码第 4 章:注释上的注释

> 原文：<https://medium.com/analytics-vidhya/clean-code-chapter-4-comments-on-comments-e65e018d3cdd?source=collection_archive---------18----------------------->

![](img/ca81edc52e3182da9d5c66e42b88138a.png)

照片由来自[佩克斯](https://www.pexels.com/photo/bonfire-burning-camp-campfire-1368382/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)的[弗拉德·巴加西安](https://www.pexels.com/@vladbagacian?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)拍摄

在我学习软件开发的时候，我写的代码可能是我读的两倍。那不是谦虚的吹牛，事实上恰恰相反。

这些天我的生活主要是编码挑战和项目建设。除了浏览对问题的堆栈溢出响应和阅读紧张而复杂的 [Python 文档](https://docs.python.org/3/)之外，我并不经常拿起一段代码，盯着已经写好的或评论的内容。

然而，据我所知，软件开发人员(尤其是初级开发人员)的生活包括大量阅读、审查和测试代码。

考虑到这一点，思考注释的作用，以及它们如何给我们的代码增加价值，难道不是很有用吗？这正是罗伯特·马丁(又名鲍勃大叔)的[干净代码第四章所涵盖的内容。](https://www.goodreads.com/book/show/3735293-clean-code)

作者对评论的哲学似乎是:越少越好。这并不令人震惊，与他对[功能](/@losimprov/functions-and-chapter-3-of-clean-code-bb41aa58796)的简洁观点非常相似。

这主要是出于消除无端和完全无益的评论的愿望。根据马丁的说法，以下是一些需要留意的(蹩脚的)评论:

*   **含糊不清**，也就是对普通读者来说毫无意义或不清楚的评论。
*   **冗余**(重复。你知道，重复说同样的话……)
*   **误导性评论**。随着软件的扩展和老化，大多数评论最终都属于这一类。注释从它们所依附的代码中分离出来，功能的变化使得注释的意义越来越模糊。
*   **强制注释。显然这是一些经理让他们的员工做的事情？这对于保持评论的精简和刻薄肯定不是很有帮助。**

那么，有什么好的评论吗？据作者说，很少。一个开发人员应该写一个注释，基本上有三四个原因，所有这些都涉及到预测和帮助下一个开发人员避免一大堆麻烦。

第一个建议的注释类型是对**提供信息**的注释。解释意图或进行澄清的注释是可以的，例如解释有助于产生代码字的复杂概念(只要它们不会占用太多空间并分散对实际操作的注意力)。

例如，如果有一个模糊或抽象的 CS 概念在起作用，使一个特定的功能软件工作，那么提到这一点是很好的，这样将来的开发人员就知道如果他们觉得需要以某种方式改变代码时会发生什么。

另一个很好的理由是向**解释**的意图。如果你检查了你的代码，并且认为人们应该理解你为什么以某种方式写了一些东西，尽一切办法写一个注释。

解释为什么你选择了一个特定的策略，而不是另一个，可能有助于未来的开发人员避免用错误的假设重写代码。这也可以帮助他们理解以前的尝试失败了什么以及为什么失败。当他们节省了时间，团队会感谢你的！

书中提到的另一个理由很充分，即如果你需要**警告后果**。例如，如果运行一个程序需要花费数小时来迁移文件，而这在代码中并不明显，那么最好在注释中包含这一细节(例如，除非有时间，否则不要运行这个程序……。)这样人们就知道自己在做什么了。

最后一个值得注意的例外是法律意见。有律师很重要，否则谁会得到报酬？我想是的。

无论如何，正是因为这个原因(我只能猜测)，马丁认为法律意见可以根据需要添加到文档中。这很有道理，并且不是我现在特别关心的事情，但是值得一提。

总的来说，干净代码这一章非常有帮助，让我重新思考我几乎利用了的编码的一个方面。我期待着手本书的下一章，并继续清理我的代码！