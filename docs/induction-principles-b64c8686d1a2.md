# 归纳原则

> 原文：<https://medium.com/analytics-vidhya/induction-principles-b64c8686d1a2?source=collection_archive---------39----------------------->

链表的归纳原则让我们想起了自然数的归纳原则。树呢？

![](img/85c40e20768db5a2f8ad3cf211497bf2.png)

**归纳法**，有时也叫*数学归纳法*，是一种对可数无限结构进行推理的工具。通常，我们可以用一种叫做“归纳法证明”的东西来验证存在于无穷远处的事实。

我们来复习一下。假设你在一所大学主修计算机科学，这是你的第一年:你被要求**展示**那个`a + b = b + a`。我们都希望你能按照以下步骤进行-

*   **你**:教授，`a`和`b`是什么？
*   **教授** : `a`和`b`是任意自然数
*   **你**:和`+`符号，是普通自然数加法？
*   T21 教授:是的
*   你:武断是什么意思？

教授更新了问题。“请出示`forall (a b : nat), a + b = b + a`”

我们希望你不要从下面的列表开始

```
0 + 1 = 1 + 0 
1 + 2 = 2 + 1 
0 + 2 = 2 + 0 
...
```

即使你意识到存在一个系统的列表程序，你也会因为缺少纸张而受挫，因为你被要求对一个*无限*集合的所有进行操作(或者，在短时间内，对 *a 类型*进行操作)。

相反，我们希望你开始下面的*证明*

*   **你**:设`a`和`b`为任意的`nat` s .通过对`a`的归纳，有两种情况需要证明。第一种情况，`a = Z`。我们需要验证的目标是`Z + b = b + Z`。由于`Z`是相对于`+`的中性`nat`，这就简化为`b = b`，我们通过反身性知道。在第二种情况下，让`n`是某个任意的`nat`并假设`n + b = b + n`。我们需要验证的目标是`(Suc n) + b = b + (Suc n)`。根据`+`的定义，我们可以将其简化为`Suc (n + b) = b + (Suc n)`，然后应用我们的假设，将我们的目标转换为`Suc (b + n) = b + (Suc n)`。如果我们配备了一个引理，使得`Suc`可以从右边的论证而不是左边的论证中提取出来，我们可以应用它来获得`Suc (b + n) = Suc (b + n)`，通过反身性我们可以看到它是真实的。
*   **教授**:非常好。让我们在接下来的课程中更仔细地了解一些细节。`nat`正是由一个**空构造函数**(或*常量*)和一个**一元构造函数**组成的数据类型。前者是著名的数字`Z`，后者是后继函数`Suc`，通称`Suc n = n + 1`。现在请给我们解释一下你提到的`+`的定义。
*   **你**:

Coq 中自然数上加号的定义

*   教授:太棒了！因为我的班上都是程序员，所以我希望他们中的一个能解释这是怎么回事？
*   你的**模式匹配**有两个分支，因为你的类型像教授说的有两个构造函数。当左自变量`n`为`Z`时，返回右自变量`m`，否则将`n`解构为`S n'`，其中`n'`也是`nat`，返回表达式`S (plus n' m)`。
*   **教授**:确实。换句话说，非零的情况看起来像`n + m = 1 + (n-1) + m`，因此该术语减少到`Z`。现在需要有人在板上写`nat`。辛普利西欧，你准备好了吗？
*   辛普利西奥:当然可以。

Coq 中自然数类型的定义

*   **教授**:谢谢。现在谁愿意证明一个列表的两次反转`l`等于`l`？
*   你没有。
*   教授:打扰一下？
*   教授，我认为他们的意思是，我们还不知道如何处理列表，也不知道如何反过来处理列表。
*   教授:很好。给我一点时间。

*   教授:如果你愿意的话
*   **你**:

Coq 中 reverse (reverse l) = l 的证明

在 Coq 中，每个归纳数据类型(即用`Inductive`关键字定义的类型)都配备了一个**归纳原则**。让我们观察下面的定义

一个愚蠢但非常真实的数据类型

这就是说正面是一个硬币，反面是一个硬币，如果你运行这段代码时观察 Coq 系统的输出，它会说

```
coin is defined 
coin_rect is defined 
coin_ind is defined 
coin_rec is defined
```

`coin_ind`是硬币数据类型的归纳原则。让我们运行`Check coin_ind`将其打印出来

```
coin_ind 
     : forall P : coin -> Prop, P heads -> P tails -> forall c : coin, P c
```

这只是一个函数类型。在英语中，它说“对于硬币上的每个谓词，如果它有头和尾，那么它适用于所有硬币。”

要查看它的运行，定义一个谓词(一个函数`coin -> Prop`，其中`Prop`代表命题)

专门针对特定谓词的归纳原则

我们已经*特殊化了*归纳原则，该原则被定义在一个特定谓词`is_heads`的所有谓词上。为了进一步特殊化它，我们需要提供一个`is_heads heads`的证明，并且为了一直特殊化它，我们需要一个`is_heads tails`的证明。`is_heads tails`不是可证命题，所以例子很傻。

我想向您展示*函数应用发生在类型级别*并且*专门化一个* `*forall*` *的行为就像向函数*提供一个参数。

*   **教授**:您在列表示例中用来生成目标和归纳假设的*战术*和`induction`正是由`listnat`的归纳原理赋予的。观察。

教授在黑板上写下`Check listna_ind`,就在它下面开始具体化以下类型

```
listnat_ind 
     : forall P : listnat -> 
       Prop, P empt -> 
       (forall (h : nat) (t : listnat), P t -> P (cons h t)) ->                      forall l : listnat, P l
```

*   教授:在英语中，这种类型是说，如果我有一个关于链表的任意谓词，如果我有一个证明它适用于空链表，并且证明如果它适用于某个链表，那么它适用于那个带有前缀的链表，那么我有效地证明了它适用于所有的链表。事实上，正是这种类型告诉`induction`关键词*如何*为你产生目标和假设。
*   辛普利西奥:当然，我相信。这让我想起了普通感应过`nat`。
*   **你**:那是因为类型差不多。两者都有一个常量构造函数和另一个带有一个自引用点的构造函数。
*   Simplicio :教授，您能打印出`nat`的归纳原理吗？
*   **教授** : `Check nat_ind`

```
nat_ind 
     : forall P : nat -> Prop, 
       P Z -> 
       (forall n : nat, P n -> P (S n)) -> 
       forall n : nat, P n
```

*   **你**:看，`listnat_ind`中的术语`h`很重要，但它不会影响字体的*形状*，因为影响*形状*的只有自参照点。
*   **教授**:非常好。

前一种类型的英文翻译是*字面上的*普通的、普通的“数学归纳”的定义。**类型** `**nat_ind**` **只不过是如何证明关于** `**nat**`的东西的算法描述，你会发现对于任何其他归纳类型也是如此(包括`bool`(我们看到的另一个名字是`coin`)、各种类型的树，甚至整个编程语言)。

*   教授:在我们结束归纳讨论之前，最后一个例子。你，请在黑板上写下树叶中有数据的树的类型。
*   **你**:

树叶中有数据的树的类型

棋盘闪烁着光芒，宣布:`tree is defined. tree_ind is defined.`

*   **教授**:辛普利西奥。请告诉我们你如何计算`tree`的感应原理，而不是让系统打印出来。
*   **辛普里西奥** : *大口*。好吧。您必须从量化谓词开始，所以我会从`forall P : tree -> Prop, ...`开始。我们有两个构造函数，所以将有两个证明来支持结论`forall (t : tree), P t`。然后，我注意到第一个构造函数没有任何自引用，所以需要的第一个证明是`forall (n : nat), P (leaf n)`。现在我们来看一下自引用构造函数，即`node`的构造函数。一个`node`结构中有两个`tree`，所以这个证明应该有两个前提，看起来像`_ -> _ -> _`，我知道会是`_ -> _ -> P (node l r)`。我们需要建立`l`和`r`，所以会有量词，最后我们需要假设减少的实参的谓词，所以第二个构造函数对应的证明是`forall (l : tree), P l -> forall (r : tree), P r -> P (node l r)`。现在我可以把它们都包起来，然后说

```
tree_ind 
     : forall P : tree -> Prop, 
       (forall (n : nat), P (leaf n)) -> 
       (forall (l : tree), P l -> forall (r : tree), P r -> P (node l r)) -> 
       forall (t : tree), P t
```

*要了解更多信息，请阅读* [*软件基础*](https://softwarefoundations.cis.upenn.edu) 。

*最初发表于*[*【http://github.com】*](https://gist.github.com/quinn-dougherty/a16724272a5d9bc793f61d4681accbfc)*。*