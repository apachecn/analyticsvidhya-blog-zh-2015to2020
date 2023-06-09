# 我的 GUI 编程备忘单| Python3 | Jupyter

> 原文：<https://medium.com/analytics-vidhya/my-gui-programming-cheatsheet-python3-jupyter-f636d5c45fcc?source=collection_archive---------8----------------------->

![](img/93739c0b2010eaa371cf6c119e54e698.png)

**cleanpng.com | python.org**

*   您有一个 GUI 编程任务要完成，有一些严格的截止日期，并且您不太了解如何用 Python3 来完成它。
*   您一直在积极地用 Python3 编写代码，最近被告知要用 Python 探索图形用户界面的范围。
*   你不断从你的队友那里听说 Tkinter / ipywidgets，并最终决定知道它们到底是什么，它们有什么潜力。
*   你已经计划使用 Python 建立你的计算机视觉职业生涯，并且最近意识到 GUI 编程是追求计算机视觉的一个不可避免的方面。
*   您一直在探索 Python 中可用的不同包/模块，并遇到了 Tkinter、ipywidgets、wxpython 等。在线，现在好奇的开始一些基本的动手操作。

如果你属于以上五类人中的任何一类，那么这篇文章就值得你花时间去读。

## Python 提供了各种模块来创建完整的图形用户界面或小的可视部件。世界各地的计算机视觉社区积极使用的一些著名模块是:-

1.  Tkinter
2.  wxPython
3.  JPython
4.  **ipywidgets**

下面我将使用 Python 的 ipywidgets 包与大家分享 GUI 示例。

# **ipywidgets**

这是 Python 中一个非常流行的包，它为用户提供了各种图形用户界面控件，用于交互处理他们的数据。为了给出 ipywidgets 的另一种解释，您可以将 ipywidgets 视为 Jupyter 笔记本、JupyterLab 和 Ipython 内核的交互式 HTML 小部件。当你在你的 Jupyter 笔记本上使用 ipywidgets 时，你的笔记本就有了可以快速创建的交互式小部件。

这个包将为 Python 程序员提供所有标准的小部件和函数，以便能够交互地控制他们的数据，并可视化数据的各个方面。

Ipywidgets 库支持的小部件类型有**链接、容器、字符串、选择、布尔** & **数值。**

**那么，我们开始吧~**

# **先决条件:-**

*   我只需要你打开你的笔记本。
*   请安装并导入以下库:-

> ！pip 从 ipywidgets 安装 ipython
> 从 IPython.display 导入小部件
> 导入显示

# 数字小工具

1.  **IntSlider**

![](img/ceed40c60c2fb5384b55719a73f71728.png)

**IntSlider 小工具**

*   value:在 value 参数中，我们传递一个要绘制在滑块上的默认值。在这种情况下，它是 8。
*   min:在 min 参数中，我们传递滑块的最小值，滑块基本上就是从这里开始的。在这种情况下，它是 0。
*   max:在 max 参数中，我们传递滑块的最大值，滑块基本上在这里结束。在这种情况下是 15。
*   步长:在步长参数中，我们传递滑块在单张幻灯片上递增的速率(步长计数)。在这种情况下，它是 1。
*   描述:在 description 参数中，我们传递一个字符串，它将是给 IntSlider 小部件的名称/标签。在这种情况下，它是“First IntSlider:”。
*   disabled:在 disabled 参数中，我们传递一个布尔值(True 或 False)。通过传递 True，您希望保持您的小部件处于启用状态，反之亦然。
*   continuous_update:这个 continuous_update 参数基本上只在用户停止拖动滑块时触发值的变化。您再次在此参数中传递一个布尔值(True 或 False)。另外，请在上面的代码片段中将其理解为 continuous_update，而不是 continuous _ update。
*   orientation:在 orientation 参数中，我们根据我们希望如何在屏幕上对齐滑块来传递“水平”或“垂直”。在这种情况下，我们已经通过了“水平”，因此您可以看到滑块水平对齐。
*   读出:在参数读出中，我们传递一个布尔值(真或假)。当传递 True 时，我们打算在它旁边显示滑块的当前值，反之亦然。在这种情况下，我们已经传递了 True，您可以在滑块旁边看到 8(当前值)。
*   readout_format:在参数 readout_format 中，我们指定了用于表示滑块值的格式函数。默认情况下，它是' . 2f '，但你显然也可以发送其他格式函数，如' d '，' . 1f '等。然而，对于 IntSliders，理想的格式应该是‘d ’,因为无论如何 Slider 小部件会自动将 float 值转换为四舍五入(通常是 floor)的 int 值。
*   **每行末尾使用的反斜杠(\)是为了更好的可读性而进行的换行。它不是代码的内在或功能部分。*

**2。浮动滑块**

![](img/712ef196e536f2bc4ae65d142a6cb178.png)

**浮动滑块控件**

*   小部件的参数基本相同。FloatSlider()，因为它们是用于小部件的。IntSlider()。
*   不同之处在于传递给 value、min、max 和 step 参数的数值的性质。所以在 IntSlider 的情况下，传递给这些参数的值现在是 float，而不是 int。
*   此外，readout_format 参数将被传递“. 1f”或“. 2f”值，因为它显然是一个 FloatSlider 小部件。

**3。内滑道**

![](img/6e2a2fc210da2ce1590f1c56b9473c76.png)

**in trans slider 小工具**

*   在这个 IntRangeSlider 小部件中，除了 value 之外，参数的性质和列表都与我们在 IntSlider 中传递的相同。
*   在 value 参数中，我们不是像在 IntSlider 中那样传递单个整数值，而是以[x，y]的形式传递一系列值，这些值在滑块上默认绘制为 x-y。例如，在这种情况下，它是[5，15]。
*   因此，如果您在上面代码片段下面的小部件中看到，默认范围被选择为 5–15。

**4。浮动滑块**

![](img/3b4d999998f41cadbec63b54e18e1adc.png)

**FloatRangeSlider 小工具**

*   在这个 FloatRangeSlider 小部件中，除了 value 之外，参数的性质和列表都与我们在 FloatSlider 中传递的相同。
*   在 value 参数中，我们不是像在 FloatSlider 中那样传递单个浮点值，而是以[x，y]的形式传递一系列浮点值，这些值在滑块上默认绘制为 x — y。例如，在本例中，它是[4.5–7.5]。
*   因此，如果您在上面代码片段下面打印的小部件中看到，滑块的默认范围标记为[4.5–7.5]。

**5。IntProgressSlider**

![](img/85939b3e84d224fbd1ff9913eb84f486.png)

**IntProgressSlider 小工具**

*   IntProgressSlider 小部件通常用于显示任何种类的进度。它可以是正在做的工作的进展，或者正在实现的结果，等等。
*   同样，参数的性质和列表与我们在 IntSlider 小部件中传递的基本相同，除了 bar_style 参数。
*   min 和 max 参数非常简单明了，在这种情况下，它们的值分别为 0 和 15。
*   因为提供的值是 6，所以您将看到进度滑块标记了进度，直到表面上看起来在 0 到 15 的范围内是 6。
*   此外，在 bar_style 参数中，您也可以传递不同类型的值，如:“成功”、“警告”、“信息”、“危险”等。随着这些值的传递，您将看到进度滑块的颜色/外观发生了变化，从而适应不同用例的需求。

**6。FloatProgressSlider**

![](img/c7f4aab4edb456243e64f1797aa9d518.png)

**FloatProgressSlider 小工具**

*   小部件的参数基本相同。FloatProgressSlider()，因为它们是用于小部件的。IntProgressSlider()，但值和步长参数除外。
*   FloatProgressSlider 情况下的 value 参数传递的是浮点值(本例中为 3.5)，这与 IntProgressSlider 不同，后者传递的是整数值。
*   在 FloatProgressSlider 中，step 参数也能够接受浮点值(本例中为 0.1)。这意味着 FloatProgressSlider 上描绘的进度将在每一步以 0.1 的速率发生。

# 布尔小部件

1.  **切换按钮**

![](img/cec481a008cfdd20bca72d5e1ed69c31.png)

**ToggleButton 小工具**

*   ToggleButton 是 ipywidgets 中的一个布尔小部件，它基于一个非常布尔的概念工作。我们显示了一个可选按钮(例如，在本例中标记为“Click me ”)来表示这个小部件。
*   如果按钮被选中，则与此小部件相关的值为 True，如果按钮未被选中，则为 False。
*   小部件中使用的参数。ToggleButton 是很容易解释的，但是，我将对我认为您可能需要解释的按钮进行快速概述。
*   value 参数接受 True 或 False 作为值。如果为真，默认情况下按钮显示为自动选中或点击。如果为 False，则默认显示为未选中。
*   tooltip 参数被传递了一个字符串值，当用户将鼠标悬停在按钮上时，该字符串值基本上会显示出来。
*   button_style & icon 参数分别用于改变按钮和按钮上显示的图标的颜色。

**2。复选框**

![](img/d97f828ddf8be143293b0c8b0204ca4c.png)

**复选框小工具**

*   小部件。Checkbox()小部件提供了一种非常简洁明了的方式，使用很少的参数来显示复选框。
*   value 参数接受 True 或 False。对于前者，checkbox 小部件保持选中状态，对于后者，则保持取消选中状态。被选中/选中的复选框小部件意味着与小部件相关联的值为真，反之亦然。

# 选择部件

1.  **下拉菜单**

![](img/3992c5f4cbbb6aec0eb33628ac8c6995.png)

**Dropdpwn 小工具**

*   小部件。Dropdown()是 ipywidgets 下最常用的选择小部件之一。
*   这些参数大多是不言自明的。在 options 参数中，传递一个将出现在下拉列表中的值列表。
*   value 参数将被传递一个用户希望默认显示为选中状态的值。描述和禁用参数的作用与前面的小部件相同。

— — — — — — — — — — — — — — — — — — — — — — — — — — — — — — —

我将在这篇文章的下一次更新中分享更多的小部件。

谢谢！