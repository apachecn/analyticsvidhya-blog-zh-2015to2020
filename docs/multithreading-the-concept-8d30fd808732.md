# 多线程基础

> 原文：<https://medium.com/analytics-vidhya/multithreading-the-concept-8d30fd808732?source=collection_archive---------3----------------------->

![](img/166fd6dd49a3e7d0f0d2dfb6703d194c.png)

多线程是许多程序员害怕的话题。这可能是因为如果编写不当，多线程程序会比单线程程序造成更大的错误，并且多线程造成的损害更难评估。有的人觉得很难理解，有的人懒得去研究。就我个人而言，我发现这是编程语言最迷人的特性之一。

通过这篇博客，我努力用最简单的术语解释多线程编程的细微差别。让我们从 CPU 内核、计算机进程和线程的基本概念开始，然后尝试并了解多线程编程的优势。

# 计算机进程 VS 线程

![](img/9f45670af57d10b5d0a57bdd8e0d3094.png)

图 1 —线程和进程(来源—[Java point](https://www.javatpoint.com/multithreading-in-java)

简单地说，一个**进程**就是一个正在执行的**程序**。当我们执行一个程序或应用程序时，一个进程就开始了。每个进程由一个或多个线程组成。

一个**线程**只不过是一个进程的**段。线程是执行任务的执行实体，执行的应用程序是为其创建的。当所有线程完成执行**时，一个**进程完成。**

# CPU 核心的作用

进程中的每个线程都是 CPU 要完成的任务。如今，大多数处理器都能够通过创建额外的虚拟内核，让一个内核同时运行两项任务。这称为同步多线程，如果是英特尔 CPU，则称为超线程。这些处理器被称为多核处理器。因此，双核处理器本质上有 4 个内核；两个物理和两个虚拟。**每个内核一次只能执行一个线程。**

# 为什么是多线程？

如上所述，一个进程有多个线程，一个 CPU 内核一次可以执行一个线程。如果我们编写一个程序，串行运行线程，也就是说，将执行排队到 CPU 的一个核心，我们就没有充分利用多核处理器的潜力。当存在需要被执行的任务时，其他核心只是处于空闲状态。如果我们以这样的方式编写程序，它为耗时的独立函数创建多个线程，那么我们将能够利用其他空闲的 CPU 内核。我们将能够并行执行我们的线程，从而减少进程的执行时间。

# 让我们把手弄脏吧！

我们将编写一个程序，其中我们将运行两个函数(执行两个任务)。首先，我们将使用同一个线程依次执行这些函数，然后我们将为每个函数创建单独的线程。我们将记录这两种方法的执行时间，并看看其中的神奇之处！

Summation1 和 Summation2 是我们需要执行的两个虚拟类。这可以是您想要实现的任何业务逻辑。我刚刚添加了随机循环来增加执行时间，这样可以帮助我更好地展示结果。

![](img/4fc9b3b115305755827e8f2d7d55100b.png)

图 2 —代码的输出

惊喜惊喜！！！(不太会！)

与串行执行函数相比，使用线程的执行时间减少了三分之一。这就是多线程的威力。

这只是一个简单的例子，帮助您理解多线程的使用。在无数的用例中，您可以对应用程序的执行方式进行显著的改进。我恳求你去探索(看我在那里做了什么😉)可以应用多线程以充分利用计算资源的场景。

我相信理解某件事情的基础是学习它的最好方法。我希望这篇博客能帮助你清楚地理解多线程编程的必要性和实用性。

关于线程，还有很多东西需要学习；线程生命周期、同步性问题以及如何处理它们等。你可以关注我的 [**和其他博客**](/@rajat.gogna30/thread-life-cycle-java-4d5682eedf7d) 。

***熟能生巧！***

*查看我的*[*GitHub*](https://github.com/rgog/TutorialsWorkspace/tree/master/Multithreading/src/multithreadingPackage)*获取本博客的代码，以及其他项目。*