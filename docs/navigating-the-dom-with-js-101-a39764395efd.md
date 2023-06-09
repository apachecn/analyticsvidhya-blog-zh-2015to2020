# 用 JS 101 导航 DOM

> 原文：<https://medium.com/analytics-vidhya/navigating-the-dom-with-js-101-a39764395efd?source=collection_archive---------14----------------------->

![](img/a182692b02a28fedf63539ec5524d021.png)

# 那么什么是 DOM 呢？

作为一名 web 开发人员，需要知道许多不同的活动部分，但是这个过程中非常重要的一部分是使用 DOM 并理解如何导航它！DOM，即**文档对象模型**，允许开发者访问页面的结构模型。现在我们可以用我们选择的语言(在本例中是 JavaScript)编写脚本，并操纵我们的视图！

DOM 的结构很像编写 HTML 我们有一个等级制度，我们将不同的部分嵌套在其他部分中，从而形成一种技术家谱。事实上，它比人们想象的更接近于一个家族树:嵌套的元素被称为子元素，更高的元素(更接近于根)被称为父元素。我们可以用几种不同的方式在这个系谱树中导航，并且我们的标识符仍然是一种识别不同部分的方式(例如，具有 id、类、样式等的元素)。

# 文档对象

在使用 DOM 和浏览家谱时，这是最重要的。“文档”代表我们正在处理的整个程序，并且是我们层次结构的主要根，所以当在搜索时分支并发现或找到某些节点(DOM 中的元素)时，它是一个很好的起点。要找到这些元素，您必须询问结构的位置，您可以从“document”开始找到它下面的所有子元素(因为它是 DOM 中所有节点的顶级父节点)。

![](img/95dd910e257d1c9bd38feb99998ccb76.png)

例如，只需输入“document.head”来选择子元素，就可以简单地获取上面层次结构中的“head”节点！

# 更改找到的元素

很好，现在我们可以从 DOM 中获取节点，所以让我们来看看如何操作它。要更改找到的节点的内容，我们必须指定我们想要更改的内容(例如，节点的颜色、节点打印出的文本等)。要改变一个节点的内容，或者节点中的文本，我们找到这个节点，添加。innerHTML”，并将其设置为新的内容。完整来看，应该是这样的:

> document.body.innerHTML = '
> 
> ### 我是正文中的新文字！

正如你在上面看到的，当我在正文中添加新的文本时，我把它用

### 标签包围起来。改变内部文本不需要像这样的标签，但是如果你想让你的文本有某种样式或属性，你必须指定，就像我在这里做的一样容易！

这很好，但是如果我们正在处理一个大型的应用程序或网站，这将是一种非常单调的方法，一路向下寻找我们想要操作的正确的子元素；选择器的字符串会变得很长！

找到我们需要的东西的更好的方法是直接选择元素。我们可以像使用 CSS 选择器那样做，但是语法略有不同。不管怎样，它允许我们找到一个节点，并根据它的 ID 或类立即获取它！

![](img/2aa74d2c6e3d21d6ba2c0b5176e1297e.png)

查询选择器将返回与您正在搜索的内容相匹配的第一个元素；可能会有更多，但只有第一个会被返回！

> document.querySelector('h1 ')

您还可以使用 ID 或类查询 select，就像 CSS 选择器一样。另一种导航方式是通过按 ID 直接搜索来查找元素。当你非常了解你的 CSS IDs 时，这真的很有帮助！

> document.getElementById('intro ')

# 父母和孩子

我们一直在谈论父元素和子元素，那么难道没有一种方法可以让我们用这种导航方法来搜索某些节点吗？绝对的！

![](img/559b209fd9cf3d92c48c8b0d0f15159f.png)

请记住，由于它们在层次结构中的位置以及与根的关系，这些都是相互关联的。子节点离根更远，并且直接挨着所述节点。父节点更近，并且直接紧挨着所述节点。

语法是什么？这是要记住的两件事:

> 。parentNode 和。儿童

这些方法将允许您获得正在搜索的节点的所有子节点或所有父节点(如果没有，那么它将简单地返回 null，而不是错误)。例如，如果我们只想取回第一个孩子，我们可以使用属性。取而代之的是长子。这与使用查询选择器的想法相同；只有赢回第一场比赛。

# 更改元素的样式

![](img/b755bb91e24b0266f41408d57139d1fd.png)

一旦我们找到这些不同的元素和节点，我们很可能想用它们做些什么！对它们进行样式化是捕捉不同的子元素和父元素的一种非常常见的用法。我们可以轻松做到这一点！虽然语法与 CSS 选择器略有不同(我们没有用破折号分隔单词，而是使用了驼峰式大小写)，但它确实非常简单。下面是如何更改我们用查询选择器找到的元素的背景颜色:

> document . query selector(' # orange '). style . background color = ' green '

上面我们使用了一个查询选择器，所以它只返回它找到的第一个元素，然后我们通过它前面的标签所指示的类‘orange’进行搜索。在这之后，我们使用语法。如果说我们正在改变风格，那么。backgroundColor(不是。背景色)说我们想改变这一点。最后，我们简单地指定我们想要的新颜色。

# 下次

在下一篇博客中，我们将讨论用这种方法添加和删除节点，以及使用事件监听器开始创建一些交互性。这主要是让您导航 DOM 和更改找到的节点的小部分(如文本、颜色和其他 CSS 属性)的信息。继续搜索，成为一个快速的 DOM 导航者！这对任何开发者都有好处。