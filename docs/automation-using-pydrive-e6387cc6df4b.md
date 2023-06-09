# 使用 Pydrive 实现自动化

> 原文：<https://medium.com/analytics-vidhya/automation-using-pydrive-e6387cc6df4b?source=collection_archive---------13----------------------->

我在想如何自动化我工作中的一些流程，我意识到有一个是关于下载和上传机构视频的，平均每天 300 个。这项活动是手动的，需要一群人从云平台下载视频，并上传到我们的业务 Google drive 帐户，因为云平台只保留视频 15 天。

![](img/94359022c79d46e1bf91d39e1ebe87c6.png)

像蚂蚁一样工作

为了避免像蚂蚁一样工作，我将此作为一个挑战，并开始了一个关于视频大小、所需权限、版权保护、本地服务器和 Google drive 的存储容量以及这些视频上传频率的清单。

除此之外，我选择使用 python 是因为 Pydrive 库简化了 Google Drive API 任务的使用。

![](img/234f5085b073b53e6fe33c8cac789323.png)

第一步是使用我们的 Google 商业账户启用 Google Drive API，遵循以下步骤:[https://developers.google.com/drive/api/v3/enable-drive-api](https://developers.google.com/drive/api/v3/enable-drive-api)

然后，我为我的项目创建了 OAuth 2.0 凭证，并将 client_secrets.json 文件下载到我的 python 脚本的同一个目录中，在那里我实现了命令行身份验证:

[https://python hosted . org/py drive/py drive . html # py drive . auth . Google auth . command line auth](https://pythonhosted.org/PyDrive/pydrive.html#pydrive.auth.GoogleAuth.CommandLineAuth)

![](img/cf8ffd8e24d26b2ff9fe5317bc669e84.png)

除此之外，我还创建了 settings.yaml，在其中定义了客户端机密，并创建了凭证文件，用于保存 Google auth 生成的令牌访问。

![](img/c213c20cab5d68a8f23462fa2442e3e1.png)

settings.yaml 配置

其次，在我的 python 脚本中，我定义了两种方法，在这两种方法中，我基于当前日期在 Google Drive 中创建了一个文件夹:

![](img/c2cc341ed2069fed7aaa8c89e44103ba.png)

继续我的脚本，我必须从我们的本地服务器上获取视频，并通过一个可共享的公共链接上传到 Google Drive，而无需作者共享和复制需要作者权限:

![](img/7ec3534dbc4ee217879c206cf670e061.png)

“替代链接”显示可共享的公共链接

之后，我实现了一种使用线程并行上传视频的方法:

![](img/6d1aae34856c9f435aff50b9ab992a52.png)

导入线程库

![](img/d9c99ad79e450626a174cd2aba8f5cba.png)

定义线程的数量

![](img/b2bdf0e5a3a6507e410a1e9e0a4b0f1f.png)

定义使用线程的方法

另一方面，我准备了一个 python 脚本，将视频从云平台下载到我们的本地服务器。这个过程是在我把它们上传到 Google Drive 之前。

为此，我使用请求库从 URL 下载视频，并使用 Shutil 包将文件对象复制到我们的本地服务器上。

在这种自动化之前，我们需要 6 个人来下载和上传视频，他们在工作时间表中花了一些时间来完成这项工作，所以这项任务在一天结束时完成。自动化后，整个过程只需要 2 个小时，不需要任何人来完成。