# 查找行数最多的文件

> 原文：<https://medium.com/analytics-vidhya/finding-files-with-largest-number-of-lines-922ef6c1fd92?source=collection_archive---------1----------------------->

问题:我很难阅读和理解包含超过 100 行代码的源文件。因此，如果一个文件达到了这个大小，我通常会尝试将内容提取到另一个文件和/或类中。有时，时间并不总是允许我创建漂亮的小类和文件。当有时间重构和清理代码时，我需要一种方法来找到我应该关注的文件。

解决方案:在没有使用静态代码分析器(我可能应该在某个时候研究一下)的情况下，我只是找到了这个 shell 代码片段来…