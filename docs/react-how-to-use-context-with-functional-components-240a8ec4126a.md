# React:如何在功能组件中使用上下文

> 原文：<https://medium.com/analytics-vidhya/react-how-to-use-context-with-functional-components-240a8ec4126a?source=collection_archive---------0----------------------->

![](img/08a4438151516e7a31cd33547d243a6b.png)

乌戈·门德斯·多内利在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

当在所有需要状态的 React 组件之间共享状态变得太复杂时，上下文就来帮忙了。这里有一个简短的指南，介绍如何在 React 中将上下文与功能组件和钩子结合使用。

# 什么时候应该使用反应上下文

React 的上下文是针对需要在多个组件之间共享状态的用例的解决方案。如果没有上下文，所有想要使用相同状态的组件都必须有一个公共的父组件，通过 props 传递状态。

如果有几个组件需要使用同一个状态，并且它们都有一个可以存储它的公共父组件，这就很好了。但是一旦有太多的组件需要相同的状态和/或它们在 React 组件树中相距太远，通过 props 传递状态就变成了一场噩梦。写起来繁琐，理解起来复杂，维护起来困难。

如果您遇到这个问题，请考虑用 React 上下文替换您的状态。

一旦你掌握了它，建立一个上下文是非常简单的。然而，也很容易*忘记*如何做，因为你通常在你的 React 应用中需要很少(或者可能只有一个)上下文。我不得不重新学习如何去做，所以我想我可以通过写下这个过程来帮助自己和他人。

上下文以类似的方式对状态做出反应，但它不一定绑定到具体的组件。相反，所有需要访问存储在上下文中的信息的组件都可以简单地调用所需上下文的 useContext 钩子。

如果上下文没有绑定到特定的组件，它通常存储在一个单独的模块(文件)中。为了便于导航，您可以将所有这样的 React 上下文实例分组到一个公共文件夹中，与 React 组件分开。

首先，我们将看一下如何定义上下文，然后我们将讨论在哪里放置它以及如何在组件中使用它。

# 定义上下文

与 state 类似，使用 createContext 函数创建上下文。该函数将上下文的初始值作为参数，并返回创建的上下文。

注意，与 state 不同，context 没有允许您更改上下文值的 setContext 函数。相反，每个上下文使用一个接收值的提供者组件。该值被设置为上下文的值，并且可以被所有子组件访问。

最后，为了使您的组件能够使用上下文的内容，您还需要创建一个 useContext 挂钩。需要访问上下文值的组件可以使用这个钩子。

下面是一个简单的上下文示例，它提供了当前是否是白天的信息:

上下文提供者可以有更复杂的方法来确定要传递给其子代的值。上下文提供者本身可以有一个状态，然后提供者的状态值可以通过上下文传递。

注意，您需要导出您创建的上下文提供者和 useContext 挂钩。下一节将介绍如何在 React 应用程序中使用它们。

# 使用上下文

所有需要访问上下文的组件都应该作为上下文提供者的子组件来呈现。然后，提供者将提供其子代可以访问的值。

因此，您需要做的第一件事是识别包含所有需要使用上下文的组件的父组件，并在那里呈现提供者。

因为上下文通常包含许多组件所需的信息，所以通常在顶级应用程序组件中呈现提供者。这里有一个例子:

最后，可以通过 useContext 钩子访问上下文中的值。

本例中的上下文只包含一个布尔值，但是上下文不限于简单的值，还可以包含复杂的对象，就像 state 一样。

这就是在 React 中启动和运行上下文所需的全部内容！你可以在[官方文档](https://reactjs.org/docs/context.html)中寻找关于这个主题的更多信息。

我希望这将帮助您开始使用 React 上下文。感谢您的阅读！

# 小抄

这里有一个关于如何获得有效的 React 上下文的简短总结。

**设置您的上下文:**

*   使用 React.createContext()创建上下文
*   创建呈现上下文的上下文提供程序
*   创建 React.useContext 挂钩

**用你的语境:**

*   在父组件(通常是顶级应用程序组件)中呈现上下文提供程序
*   使用 useContext 挂钩从组件的上下文中获取值