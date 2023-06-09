# 在 30 分钟或更短时间内学会使用人工智能模型(第 1 部分)

> 原文：<https://medium.com/analytics-vidhya/learn-to-use-an-artificial-intelligence-model-in-30-minutes-or-less-part-1-b52251d2604?source=collection_archive---------23----------------------->

人工智能(AI)已经成为 21 世纪最热门的词汇之一。无论“人工智能”让你想到喝着康普茶、穿着法兰绒的初创公司，还是一个阿诺德·施瓦森内格(Arnold Schwarzenneger)形状的机器人踢开你的门，你可能都很好奇人工智能模型是什么，它们是如何使用的，以及它们是用来做什么的。

# **什么是 AI？**

简而言之，人工智能领域是指对任何旨在模仿或显示类人智能品质的计算机系统、软件或设备的研究。大卫·普尔、艾伦·马克沃斯和兰迪·戈贝尔在 1998 年提出了一个最早的，也是我认为最好的定义:

> [人工智能]是研究智能代理的设计。代理是在环境中起作用的东西——它做一些事情。代理包括蠕虫、狗、恒温器、飞机、人类、组织和社会。智能代理是一个智能地行动的系统:它所做的事情适合于它的环境和它的目标，它对变化的环境和变化的目标是灵活的，它从经验中学习，并在给定感知限制和有限计算的情况下做出适当的选择。

在人工智能领域有许多原则和方法被研究，但最常见的，也是我们稍后将使用的，是机器学习的概念。

通常，当开发人员创建一个程序时，他们会给计算机明确的指令，告诉计算机如何执行程序的每一步，以达到预期的结果。对于更简单的程序来说，这可能非常容易，但是在许多应用程序中，这个过程变得非常繁琐或者非常复杂。

机器学习采取了不同的方法。当创建一个机器学习程序时，开发者为计算机的运行创建一套“规则”。然后，开发人员给计算机输入一大堆问题让它去解决，以及每个问题的解决方案。这组问题和解决方案被称为“训练数据集”然后，计算机进行关联以创建方法，这些方法将使它能够找到每个问题的每个解决方案，同时在先前定义的规则内操作。一个大的和集中的训练数据集将导致一个更有能力的最终程序，而一个小的或不集中的数据集可能导致一个无能的或过于一般化的程序。

深度学习是机器学习概念的延伸。在深度学习中，使用了多个“级别”的训练数据集。第一个训练数据集将提供一组潜在的问题，以及部分或简单的解决方案。第二个训练数据集将使用这些解决方案作为问题，并将提供更完整或复杂的解决方案的“更深”层。连续的层可以变得更加完整或复杂，增加模型的“深度”,直到达到可接受的精确度。

为了探索机器学习的应用，特别是深度学习，我们将与 IBM 的沃森人工智能平台合作。

![](img/17f025e2b9a39ad54cbd6cb0c2b10e7a.png)

来源:IBM Research

沃森可以说是最知名的现实世界人工智能，通过其惊人的 *Jeopardy 赢得了它的名声！*2011 年的表现。今天是这项成就的第九个周年纪念日，在此后的几年里，Watson 已经发展成为一个巨大的、易于访问的平台，供专业人士和研究人员将人工智能融入他们的工作。

我们将使用 IBM 的沃森自然语言理解产品。

沃森自然语言理解展示了自然语言处理(NLP)的人工智能概念。NLP 指的是计算机和“自然语言”或人类所说的语言(相对于我们在语法和用法规则下“应该”说的语言)之间的交互。

IBM 利用包含数百万个句子和短语及其相关概念、观点等的数据集来生成一个健壮的程序。IBM 已经让这个程序 100%免费，并且公众可以轻松访问！

# **让我们开始吧**

## 1

我们首先前往 https://cloud.ibm.com/login 的。在左侧，你会看到一个标有“创建 IBM 云账户”的按钮。继续选择它，然后按照步骤为 IBM Cloud 创建一个免费帐户。你必须使用注册时使用的电子邮件来验证你的帐户。

![](img/93ee4e0d0339e36d55ec820aeee600f6.png)

## 2

一旦你有了自己的账户，回到[https://cloud.ibm.com/login](https://cloud.ibm.com/login)，然后登录。在被允许进入之前，你必须接受 IBM 的条款和条件。现在，您将看到您的 IBM Cloud 仪表板。在这里，IBM 允许用户访问各种应用程序的大量服务和资源。

## 3

在仪表板的左上方，您应该会看到一个名为“资源摘要”的面板在这个面板中，应该有一个标记为“创建+”的小按钮。按下它以创建存储资源。

![](img/e65916a2d14e45713da6cf1e16c35948.png)

存储资源是 IBM 服务器的一部分，专门用于托管您的服务。

## 4

按下“创建+”按钮后，你将被带到 IBM 的目录。在左侧的类别列表下，您会看到一个名为“存储”的条目选择它，然后选择“对象存储”服务。

![](img/f0b22c6c767fe107530221b9b8947e96.png)

## 5

应该会为您选择“Lite”定价计划，它是 100%免费的，所以请按“创建”按钮。

![](img/24659c463b7f366c4d0bc68cbb1795b0.png)

## 6

在云对象存储页面上，您可以选择创建一个“存储桶”存储桶是在服务器中保存数据的东西。按下“创建存储桶”按钮。

![](img/7a8a510ae7e9aa418b5ab9e3bd1cf539.png)

## 7

IBM 为用户提供了真正个性化数据存储的选项，但是我们不需要任何高级的东西。选择“预定义存储桶”下的“标准”选项

![](img/e59f2ab63fd19f06b16c34ebe610c649.png)

## 8

我们不需要改变 IBM 的默认选择，不需要上传任何文件，也不需要设置一个测试桶，所以继续操作，点击“Next ”,直到创建完测试桶。

在命名存储桶时，您可能会收到一条错误消息。有时，IBM 不会自动分配一个惟一的名称，所以如果您遇到这个错误，只需在最后添加几个随机字符，直到您可以进入下一步。对于我们的目的来说，存储桶的名称并不那么重要。

![](img/d539ddf7d04b234ac68abd4e3405cef6.png)

## 9

完成后，选择左上角的“IBM Cloud”文本返回仪表板。

![](img/50a1fc4c2fac7eb0ed0c0a24a93d4d75.png)

## 10

现在我们要设置我们的人工智能服务。选择“创建资源”按钮。

![](img/b0073be1cf5cb99a18362d81bd9f6b8e.png)

## 11

这一次，在左侧的类别下，选择“人工智能”类别。然后，向下滚动服务选项，直到你看到“自然语言理解”，并选择它。

![](img/77b1754990137332e0b2ef9b336d49a6.png)

## 12

应该再次为您选择“精简”计划，因此点击右侧面板上的“创建”。

![](img/243b47a650764a8864404ec46f99c46e.png)

## 13

恭喜你！现在，您已经建立了人工智能资源。IBM 已经完成了训练计算机识别和处理人类语言的艰苦工作，我们将学习如何使用这个现有的模型。

## 14

选择左侧面板上的“管理”。

![](img/30ecbabed280e68573f589bfb3304309.png)

## 15

在这里，您将看到您的自然语言理解服务的凭据。本质上，这就是 IBM 如何识别他们产品的特定实例。显示的 URL 本质上是您的自然语言理解资源所在的地址，API 键实际上是一个“密码”，告诉 IBM 的服务器允许您访问该资源。

![](img/87f6568bab48b69a57616ce26587b929.png)

## 16

确保将这些信息保存在某个地方！我们很快就需要它了！

## 17

现在，我们将离开 web 浏览器，前往文件管理器。那将是 Windows 机器上的文件浏览器，或者 Mac 上的 Finder。在你的文档文件夹中，创建一个新的文件夹来保存使用你的 AI 模型的文件。

![](img/3010406578f2965f94664f0ec3623846.png)

正如你所看到的，我把我的文件夹命名为“LearningAI”，但是你可以随意选择不同的名称。

## 18

现在，我们将制作几个脚本来利用我们从 IBM 提供的人工智能模型。即使你没有多少计算机方面的背景知识，我也会尽量让你容易理解。

## 19

我们要做的第一个脚本是分析我们在网上找到的一篇文章中的语言。去[https://medium.com](/)，或者任何你喜欢的新闻来源，选择一篇你感兴趣的文章。作为参考，这里的[是我用的文章。准确地跟随可能更容易，但是如果你愿意，可以随意选择不同的文章。](/personal-growth/fasting-longevity-and-the-mitochondrial-connection-2aeb857a76a4)

## 20

这就是第 1 部分！如果你使用的是 Windows 或 macOS，下面的步骤会有所不同，所以点击链接进入第 2 部分，我们将开始使用人工智能模型。

[第二部:Windows](/@bharutj/learn-to-use-an-artificial-intelligence-model-in-30-minutes-or-less-part-2-windows-7ecf41eb9bb9)

[第二部分:macOS](/@bharutj/learn-to-use-an-artificial-intelligence-model-in-30-minutes-or-less-part-2-macos-328684718099)