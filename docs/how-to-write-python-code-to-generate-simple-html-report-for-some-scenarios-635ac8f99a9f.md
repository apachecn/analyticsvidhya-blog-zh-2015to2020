# 如何编写 python 代码来为一些场景生成简单的 html 报告，比如管道输出

> 原文：<https://medium.com/analytics-vidhya/how-to-write-python-code-to-generate-simple-html-report-for-some-scenarios-635ac8f99a9f?source=collection_archive---------4----------------------->

这是我在现实生活工作中遇到的一个场景，也是最令人兴奋的工作之一。为了生成一个简单的 html 报告，我写了一些复杂的东西，我被要求创建一个有点类似于 print 的函数，并确定是否有一个表要传入该函数

![](img/c6ea59c24732c4c4eda1b70519174923.png)

1.  正如你所看到的，我必须创建一个函数 file_appender，它将输入数据和输出路径作为参数。我希望这个函数是自由的，可以使用不同的输出路径，所以我没有将它限制为仅输出 html 路径
2.  正如我们所见，mp_print()函数用于确定我是否正在获取一个表，我将传递一个以“table”为关键字的字典，字典中可能还有其他关键字，如果您需要它们，您只需对关键字和值执行一个循环，并添加到 op_str，这是一个字符串连接

![](img/cacc09c58fe1e1f3da151552179602c5.png)

3.正如我们所看到的，table_html 用 html 语法给出了一个表格，如果有一个带“table”键的字典，我就在 mp_print 中使用它。

那么 mp_print 函数的使用有多简单呢？

![](img/e5736602597f33fd93e5ba7f318b3130.png)

**所以让我们看看输出会是什么样子:)mp_print 也只以 html 语法写入文件，所以你可以通过在 mp_print 中添加 print 语句来定制它，就像 print(all_args)一样，这样它将在你现有的 ide 或控制台中显示输出，并写入可以呈现的 html 文件**

![](img/10ab14852c67cbbeaeba9905934ba5d0.png)

输出看起来不错

在这个任务中，当我创建了一个复杂类型的打印函数时，我的学长问我，他说函数可以很复杂，但他想要接近打印函数的自由，这是我第一次尝试所缺乏的，后来我不得不用很少的条件来实现 mp_print

这一切都是关于探索、尝试、遇到错误、修复错误并获得好的结果

希望这有所帮助，请检查下面的文件链接代码是为了简单的目的越多的目的越复杂的代码将在大多数情况下

[https://drive . Google . com/file/d/13 ixkh 01 bxhj 1 b 2 mvo db 20 ARX xaye _ tb3/view？usp =分享](https://drive.google.com/file/d/13iXKh01bXhj1b2mvodB20aRXxayE_tb3/view?usp=sharing)

快乐学习！