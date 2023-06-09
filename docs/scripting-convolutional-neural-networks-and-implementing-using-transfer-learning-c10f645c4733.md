# 使用迁移学习编写卷积神经网络的脚本并实现

> 原文：<https://medium.com/analytics-vidhya/scripting-convolutional-neural-networks-and-implementing-using-transfer-learning-c10f645c4733?source=collection_archive---------17----------------------->

![](img/99f238fcddef037f80667fab7ee160ce.png)

在本文中，我们将讨论卷积神经网络(ConVNet 或 CNN)。该网络通常用于处理矩阵类型(图像)的输入数据。本文将试图让您更好地了解 CNN 的工作原理，以及如何使用 TensorFlow 实现它们。最后，我们将看到如何使用 CNN 的一些预制架构来构建一个新的 CNN 模型。本文包含以下部分。

1)卷积神经网络结构。

2)在来自手写数字的样本图像中应用上述内容。

3)手写数字识别的实现。

4)利用迁移学习制作猫狗识别器。

在 CNN 中，我们处理图像，而在计算机中，图像不过是一个从 0 到 255 的数字矩阵。对于彩色图像，我们有一组三个矩阵(“RGB”)，对应于像素值对应的每个点处的红色、绿色和蓝色。例如。

![](img/8b0da3a4a799003f91fa131db01408f9.png)

上图是一幅灰度图像，它的矩阵看起来像..

![](img/9e95415a97f95bf4856df6a7894e6fd9.png)

# **卷积神经网络** :-

CNN 的基本结构是一些卷积层，伴随着汇集层，接着是一些全连接层，最后是分类层。

— — — — — — — — — — — — -

分类图层可以有两个(二元)或两个以上的类(多类分类)。对于二元分类层，我们使用 sigmoid，而对于多类分类，我们使用 softmax 作为激活。

***卷积层和池层**

卷积层在 CNN 模型中扮演特征提取器的角色。假设一个狗和猫的分类器正试图分析一张草地上的狗的图片。对于一个分类器来说，它不是一个观察的交易，狗是否站在花园里，说谎，等等。它需要捕捉狗的特征，如毛茸茸的耳朵等。卷积层大大降低了计算开销，因为它在整个图像中共享相同的权重。此外，这也提供了将主物体放置在图像中任何位置的灵活性。那么如何对图像进行卷积呢？

![](img/3c6075f0190a96a2309b3967e3a1d01a.png)

在上述情况下，有 D(f)个特征检测器，我们分别应用于图像。

**#一种计算卷积的数值方法:-**

![](img/badcd52fe160767c06333471a8713dd8.png)

因此，为了对此进行卷积，我们取框 A1 的元素，并使其为，

![](img/c1761d0b8b2eeda066d6b81f48fa18ff.png)

类似地，按照指定的幻灯片，我们移动盒子，盒子变成 A2，我们得到，

![](img/0f93bfb689992123845aed45b4e60b9a.png)

以类似的方式，

![](img/58a0e53363bf7361b43ad149de5ba9ad.png)

现在我们求出点积的和。

![](img/fda3ff6f7353d1dbc2195ef07cd19543.png)![](img/26a12a78a4a45ba0b565abe6e41a744f.png)

为了将向量 1 转换回矩阵 2，我们试图将键合框在一行上滑动的次数可视化，这是结果矩阵中的列数，即对于第一次迭代，

![](img/a37a94fd0601d84e404ebb9c32cf865b.png)

对于图像中的多个通道，我们将矢量化第一个通道，然后连接向量(过滤器中的元素数量),然后连接第二个通道，依此类推。

在简单的可视化中，我们也说我们的内核在图像上以如下方式移动

![](img/1afe263d51ade79055e4b1d1c40e3048.png)

**汇集层**:汇集层通常在卷积层之后应用，以减小特征图的尺寸，同时放大检测到的特征。该层中没有使用任何参数。它主要有两种类型:-

1)平均池层

2)最大池层

我们大多使用 Max Poll。

![](img/88ace261afc2a6f46ad6e591bc2fd9ff.png)

最大池化

正在获取边界框的最大元素。

**全连接层**:-在一系列卷积和汇集层之后，该层的输入向量被展平，然后被传递到一系列全连接层，最后，我们有一个分类层，该分类层有 1 个神经元用于二进制分类或 n 个神经元用于多类分类(其中 n 是类的数量)。

## **构建手写数字识别器:-**

在继续之前，让我们看看卷积和最大池层在特征提取方面对我们的图像做了什么。我们可能不知道图像实际上发生了什么以及为什么会发生这种情况，但我们应该知道垂直图案(或线条)水平线等特征。被认为是我们将要研究的低级特征。

![](img/034a95158e951efd283c8273aedecf52.png)

IMG SR = http www . Google . com searchq = cats+images & rlz = 1c 1s qjl _ en in 912 in 912 & sx SRF = alekk 02 xmiij 5-cecqlkt 1 f-GB 22 x8 r7 ew 1596956079426 & TBM = isch & source = iu & ictx = 1 & fir = OQ CHD 3u c0 1 NPM % 252 czeqip 7 u 3 jok-M % 252 c _ & vet = 1 & usg = AI4 _。

这是我们要看的图像。但是让我们先看看三个通道的矩阵(RGB)是什么样子的。

![](img/15313a5ba5fcbac7942ff645198ce5bb.png)![](img/65da975bb8b3a87845d8345ba44182ec.png)

应用 9 个过滤器内核

因此，在此之后，卷积层的输出被展平并传递给完全连接的层。

现在让我们来看看手写数字识别程序。

![](img/c35711735fbafef495a3b239a568a0cc.png)

这将导入所有必需的库。

![](img/988621d1496d74bb3bcca997abdcbc33.png)

第一行导入数据。

第二行将图像放入 X，第三行将相应的标签放入 y。

第四步将数据分为训练数据和测试数据。

![](img/f91c1c345be190c82175c1f62fa48f3a.png)

图像已经很复杂了。

![](img/7692bb1f839daa1c6f4ed16a2d74316b.png)

这将使输入矩阵像素值标准化。

![](img/b00fb3faf6f18c31a83870a0cfac816e.png)

这就创建了模型。

![](img/2ef7fa7c195bb88d310ab2e709bbf594.png)

这是你的模特造型。

![](img/dad68f69373b3c8a1a105c2b53a8497c.png)

第一行使用 Adam optimizer 编译模型。验证分割设置为 20%。

![](img/95b1c5cd92bd6331f9a045a0ff16c071.png)

训练结束后，我们评估我们的模型，看不到的数据，准确率约为 95%。

![](img/fff45ed471e02fe559229fbf05a5f777.png)

这显示了随着时代的发展，验证和训练的损失。

![](img/80b519c9339a0a635704dc57cd44d0f8.png)

这显示了精度曲线。

**利用迁移学习构建猫狗识别器:-**

迁移学习意味着使用一些在大型计算机器上训练的预训练模型。因此，我们不需要再次训练它们，我们直接使用它们。

让我们直接进入程序；；；；；

![](img/c7ee12940aa151fbc7e23fd53e225c27.png)

导入库。

![](img/4edfdb6f1096c70a340aac801a686ec4.png)

下载包含猫狗图像的数据集。

![](img/7c3b616f1a4a066c6c19d68fcdf6dde9.png)

第一行指定 num_examples 中训练示例的数量，第二行指定类的数量。此外，我们拍摄了三张图像，可以注意到所有图像的大小都不一样。

![](img/f08b2136244d749ad219656282865f5d.png)

函数 format image 将所有图像整形为(224，224，3)格式，我们还导入了 MobileNetV2 以在程序中使用它。

![](img/d65507b73fe0bac4d87de24eeb8ac91c.png)

现在我们冻结模型的层，即我们要求模型不要训练架构的权重。此外，我们 model . compute _ output _ shape((224，224，3))告诉我们它将创建多大的输出。此外，我们所有的一些完全连接层和输出层，以满足我们的需要。

![](img/8ea626b57db77e07ba6ddb003f4ab193.png)

这显示了模型的摘要。请注意不可训练的参数，这些参数属于预训练的架构，因此我们不训练它们。

![](img/b123878089af824fb9c8458e257d2235.png)

在这里，我只运行了 2 个纪元，为了获得更好的结果，尝试运行更多的纪元。

![](img/e4c1b996669f2e40bbaa4cac34853bfa.png)![](img/c8c234948d40833a9a57bf277033169c.png)

谢谢你。