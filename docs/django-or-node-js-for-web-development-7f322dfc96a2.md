# 用于 web 开发的 Django 或 Node.js

> 原文：<https://medium.com/analytics-vidhya/django-or-node-js-for-web-development-7f322dfc96a2?source=collection_archive---------16----------------------->

我在 2016 年开始使用 Django 进行小型 web 开发项目，从那以后我发现当时间对项目至关重要时，它非常简单，我将在文章的后面解释为什么。不过最近，我一直在我的 web 开发项目中使用 Nodejs with express，这个运行时环境有很多我喜欢的地方。在这篇文章中，我介绍了使用这两个框架的好处、坏处和坏处，以及我个人的偏好。

![](img/32778fcbbffa29e7f4c9310a64a1c4ba.png)

费萨尔·米在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

**建筑**

Django 是一个 python 框架，主要开发来遵循一个特定的设计模式，称为模型、模板视图开发模式，而 NodeJs 是一个运行时 javascript 环境，主要用 C++/C 和 javascript 开发，在 web 应用程序开发方面没有模式。

**易用性**

从 Django 开始，我很快意识到让事情起步并不需要太多代码。就像你只需要写*django-admin create project prj*，然后用一行代码在项目中创建一个应用程序。这就是在 Django 开始一个项目所需要的一切。当然，创建一个应用不仅仅是这两行代码，任何额外的代码都取决于你的功能。另一方面，使用 express 在 Nodejs 中创建应用程序或项目需要一些额外的代码，而不仅仅是 Django 中的两个命令。首先，您需要创建一个服务器，虽然有一些不需要太多编码就可以实现的方法，但是这些方法通常会附带许多您的项目可能不需要的样板代码。好像这还不够，你还需要为你的应用程序配置正确的路线，这样你才能让任何东西运行起来。

**可定制性**

说到定制应用程序，Django 可以是好的，也可以是坏的，这取决于你从哪个角度来看待它。Django 框架使用 MTV 模式开发模式，正如在架构一节中提到的，因此开发人员被限制在特定的开发风格中。虽然这对于希望在整个开发周期中保持一定一致性的开发人员来说可能是好的，但是像我这样的其他人发现这相当有限。缺乏用 django 定制应用程序的适当方法使得它对像我这样的程序员来说不太有吸引力，因为我发现新的项目可能不一定需要 MTV 模式的开发方式。Nodejs 运行时环境没有针对 web 开发的特定模式。这为开发人员提供了一个令人兴奋的机会，他们可以在开发周期中探索不同的模式，以选择最适合他们业务需求的模式。虽然缺乏约束可能意味着更多的代码，并要求开发人员对他们正在做的事情有广泛的了解，但人们可以经历不同的模型或开发模式的迭代，这在大多数情况下有助于最小化服务器成本，如果你决定托管你的应用程序。

**功能**

Django 更适合于构建静态站点和 web 应用程序，它们几乎不需要事件驱动的响应。Django 没有 I/O 套接字(阻塞 I/O)的能力，并且是同步的，这基本上意味着你可以一次执行一个请求。然而，Django 通道的引入使得 Django 拥有某种程度的异步能力成为可能。Nodejs 是完全同步和非阻塞的，这使得它非常适合事件驱动的编程。从某种意义上说，如果你想开发一个需要大量事件驱动编程、I/O 套接字、数据操作的应用，Nodejs 非常适合这些任务/工作。如果你想开发静态网站或 web 应用，不需要使用大量的事件驱动编程，并且想使用模板维护一个特定的结构，Django 可能会成为首选框架。

**现实世界应用**

当谈到流行的网站或应用程序时，Django 的主要现实应用程序是 Instagram。Instagram 的大部分后端主要是用 django 开发的，虽然 Instagram 目前有一个基础设施问题，但他们还没有从 django 改变他们的后端。Spotify 和 Dropbox 等其他公司也在后端使用了某种程度的 django。说到 Nodejs，PayPal、易贝和网飞等公司都表示，他们的后端有相当大一部分是用 NodeJs 开发的。网飞声称 Nodejs 帮助他们提高了大约 70%的应用程序加载，而 PayPal 报告称 NodeJs 帮助他们以两倍的速度和三分之一的代码创建了应用程序。

**我的拍摄**

在介绍了两种不同的 web 开发工具之后，我相信现在的大多数应用程序都是事件驱动的，并且需要某种程度的数据操作，因此只会增加 NodeJs 的受欢迎程度。然而，django 也有机会引进 Django 频道。就我个人而言，自从我开始使用 Node 以来，我对它的体验非常好，并且在不久的将来可能会继续使用它，因此如果我要在这两者之间进行选择，我肯定会选择 Nodejs。

*感谢您的阅读，如果您对本文有任何问题或观点，请不要犹豫，在评论区提出来。*