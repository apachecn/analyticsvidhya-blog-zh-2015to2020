# 估计自动驾驶汽车的深度

> 原文：<https://medium.com/analytics-vidhya/estimating-depth-for-self-driving-cars-2c49a7def45d?source=collection_archive---------17----------------------->

主要想法是教一个网络从 RGB 图像中看到深度。许多论文在那个领域取得了巨大的成就，我决定重现其中的一些，亲自看看它是如何工作的。因此，我对其工作原理的直觉是，人类可以通过观看放置在汽车挡风玻璃上的摄像机的实时流来驾驶汽车，所以如果人类可以看到深度，计算机也可以。

## 数据

我从一个名为卡拉的自动驾驶模拟中收集了我的数据。模拟给了我 RGB 相机的图像，以及分段相机和…