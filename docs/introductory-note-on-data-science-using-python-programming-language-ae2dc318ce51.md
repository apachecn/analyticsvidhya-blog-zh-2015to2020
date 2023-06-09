# 使用 Python 编程语言介绍数据科学。(文章 2: Python 日期和时间，以及 Python 对象的简要说明。)

> 原文：<https://medium.com/analytics-vidhya/introductory-note-on-data-science-using-python-programming-language-ae2dc318ce51?source=collection_archive---------31----------------------->

欢迎阅读另一篇文章“关于使用 python 编程语言进行数据科学的介绍”，正如标题所示，这篇文章将让您了解如何在 Python 中访问日期和时间，这是一个简单的应用程序，另一方面，我们将拥有高级 Python 对象，并使用 map()函数。如果您是数据科学的新手，我建议您阅读本系列的第一篇文章，因为这篇文章可能无法让您对 python 有一个更清晰的认识。我们做的许多分析可能与日期和时间有关。例如，让我们考虑寻找给定期间的平均销售数量，选择一个产品列表进行数据挖掘(如果它们是在给定期间购买的)，验证一段时间内的任务上传，或者尝试寻找在线论坛系统中最活跃的时期，因此您会看到我们有大量的应用程序。从时间和日期开始，我想提到的是，您不会看到关于时间和日期的详细讨论，但是我希望您一定要为这个主题建立一个坚实的基础，并且这篇文章的文字要比代码多。

# 日期和时间

让我们开始吧，首先，你应该知道日期和时间可以用许多不同的方式存储。用于在在线交易系统中存储日期和时间的最常见的传统方法之一是基于从纪元的偏移，即 1970 年 1 月 1 日。关于这一点有很多历史遗留问题，但是看到系统以秒或毫秒为单位存储交易日期的情况并不少见。因此，如果您在希望看到日期和时间的地方看到大量数字，您需要转换它们，以便从数据中获得更多的意义，并使用户更容易执行他们的工作。在 Python 中，您可以使用“时间”模块获得自纪元以来的当前时间。

```
import datetime as dt
import time as tmprint(tm.time())
#prints the time in seconds since epoch(January 1,1970)
```

这就是我所说的大数字，打印在您的控制台上的数字，因此让我们继续前进，想出更合理的输出。您可以使用 date-time 对象上的“fromtimestamp”函数创建时间戳。当我们打印出这个值时，我们看到年、月、日等等也可以打印出来，使用“dtnow.year”、“dtnow.month”、“dtnow.day”等等。

```
#Convert the timestamp to datetime.
dtnow = dt.datetime.fromtimestamp(tm.time())
print(dtnow)#prints the current date and timeprint(dtnow.year, dtnow.month, dtnow.day, dtnow.hour, dtnow.minute, dtnow.second)
#extract year, month, day, etc.from a datetime
```

日期时间对象允许使用时间增量进行简单的数学计算。现在，时间增量到底是什么？时间增量是表示两个日期之间差异的持续时间。例如，在下面给出的代码中，我们可以创建一个 10 天的时间增量。

```
delta = dt.timedelta(days = 10) # create a timedelta of 10 days
```

既然创建了时间增量，我们将使用日期时间对象在当前日期和给定时间增量持续时间之后的日期之间进行一些基本操作和比较。

```
today = dt.date.today()
print(today)
'''prints the current date (dtnow prints the date as well as time, hence this is the basic differnce).'''print('After 10 days: '+str(today + delta)) # the date 10 days 
print(today > today-delta) # compare dates
```

所以这只是对 Python 中的日期和时间的一点了解，对于基础知识来说，这就足够了。

接下来，我们将讨论 Python 对象。

# 关于对象的简要说明

到目前为止，我们还没有看到多少面向对象的 Python，到目前为止，您可能已经注意到了。函数在 Python 世界中起着很大的作用，Python 确实有可以附加方法的类，并且可以被实例化为对象。在继续之前，如果你认为这篇文章是关于 Python 或面向对象编程中对象的本质细节，那么你就错了。随着时间的推移，使用 Python 的次数越多，使用对象的次数就越多，在使用交互式环境时创建新类的可能性就越小，因为它有点冗长。但是我认为在 Python 中检查对象的一些细节是很重要的，这样当你看到它们时就不会感到惊讶了。

在 Python 中，使用“ *class* ”关键字定义一个类，后跟一个冒号。低于这个值的任何内容都将被认为是类的主体。我推荐你在命名一个类的时候遵循骆驼套管对流。下面你会发现几行代码，因为我想让你先看看一个类是如何定义的，之后，我会提供更多的代码细节。

```
class Person:
    department = 'School of Information' #a class variabledef set_name(self, new_name): #a method
        self.name = new_name
    def set_location(self, new_location):
        self.location = new_location
```

要定义一个方法，你只需要像编写一个函数一样编写它。一个变化是，要访问调用方法的实例，必须在方法签名中包含 self。类似地，如果您想要引用对象上设置的实例变量，您可以在它们前面加上单词 self，并加上句号。
在这个人的定义中，例如，我们写了两个方法。设置名称和位置。两个变更实例都绑定了变量，分别叫做名称和位置。当我们运行这个单元时，我们看不到输出。这个类是存在的，但是我们还没有创建任何对象。我们可以通过调用后面带空括号的类名来实例化这个类。然后我们可以调用函数，并使用大多数语言中常见的点符号打印出类的属性。

```
person = Person()
person.set_name('Christopher Brooks')
person.set_location('Ann Arbor, MI, USA')
print('{} live in {} and works in the department {}'.format(person.name, person.location, person.department))'''
OUTPUT:
Christopher Brooks live in Ann Arbor, MI, USA and works in the department School of Information
'''
```

Python 中面向对象编程有两个重要的含义，您应该从这个非常简单的例子中吸取。首先，Python 中的对象没有私有或受保护的成员。如果实例化一个对象，您就可以完全访问该对象的任何方法或属性，简单地说就是“它是公共的！”。其次，在 Python 中创建对象时，不需要显式的构造函数。如果需要，可以通过声明 *__init__* 方法来添加构造函数。现在我不打算再深入 Python 对象了，因为其中有很多微妙之处，而且说实话，Python 的大多数面向对象特性对于数据科学入门来说并不那么突出。

这对于本文来说已经足够了，但是肯定会有更多的文章发表，关注使用 Python 的数据科学。如果您对数据科学感兴趣并且喜欢这篇文章，请继续关注，如果您有任何疑问，我很乐意与您互动。
非常感谢您阅读这篇文章！我希望它对你有所帮助。

(GitHub:https://github.com/ayush-670)