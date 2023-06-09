# 使用 CMU Sphinx4 训练自定义语音到文本模型—第 2 部分—为 CMU Sphinx 准备自定义数据集

> 原文：<https://medium.com/analytics-vidhya/training-custom-speech-to-text-model-using-cmu-sphinx4-part-2-preparing-your-custom-dataset-455f5a6bae82?source=collection_archive---------12----------------------->

![](img/66c4670f973e5bb34f4bf633f60b50ee.png)

这是关于使用 CMU 斯芬克斯非常准确地训练您的自定义语音到文本模型的系列文章的后续文章。

第一部分:[搭建 CMU 狮身人面像&环境](/analytics-vidhya/training-custom-speech-to-text-model-using-cmu-sphinx4-part-1-setting-up-cmu-sphinx-c90123c9d7ac?source=friends_link&sk=89e6949be620acfd4cfee26e76e545ae)

第二部分:你来了！👍

让我们开始吧。

以防你没有跟进以前的文章，这是我们的工作系统规格:

![](img/42a05ff6ca399e39deae55dd49bca937.png)

重要的事情先来。让我们决定我们的工作目录，这样我们就能保持在同一页上&你不会对文件的结构感到困惑。我们将在“/home/praveen/”内名为“data-prep”的目录中工作。所以最终的工作目录路径是“/home/praveen/data-prep/”。

开始之前，我们需要满足三个要求。

> 第一:拥有任何格式的大量数据。
> 
> 第二:任何格式的数据都应该转换成 16kHz 或 8kHz 的 wav 音频格式。CMU 狮身人面像在这些要求上非常具体&它不能在它的要求上妥协。
> 
> 第三:这些 wav 音频文件应该按时间拆分。您可以选择通过不同的扬声器进行分离，也可以选择您自己大小的窗口来分离音频。例如，如果您的音频中有两个或两个以上的人在交谈，那么您可以根据每个说话者说话的时间来拆分音频，或者如果您愿意，您可以在说出 10 到 20 个单词后拆分音频，而不管说话者是谁。**一定要注意，如果你决定按扬声器拆分，你需要大量的数据。**

如果您不知道如何将 mp3 文件转换为 16KHz 或 8KHz 的 wav 文件，Python 中有一个函数，它可以接收文件名并将其转换为 wav，然后根据您的要求转换为 16/8 kHz。如果你想对所有文件都这样做，你将不得不运行一个外部 for 循环来对你的音频文件 mp3 目录中的所有文件进行循环。

```
import os
def get_16_bit_wav(filename): # filename: example1.mp3,...

    file_path = {path to mp3 file directory + filename}

    wav_dir = {path to mp3 file directory}+"/wav"
    wav_16_dir = {path to mp3 file directory}+"/wav_16"

    if not os.path.exists(wav_dir):
        os.mkdir(wav_dir)

    if not os.path.exists(wav_16_dir):
        os.mkdir(wav_16_dir) if filename[-1:-4:-1] == "3pm": # reverse of mp3
        target_wav = wav_dir+filename[:len(filename)-4]+".wav"
        target_16_bit_wav = wav_16_dir+filename[:len(filename)-4]+"_16_bit.wav"
        try:
            os.popen("ffmpeg -i {0} {1}".format(file_path,target_wav)).read()
            os.popen("sox {0} -r 16000 -c 1 -b 16 {1}".format(target_wav,target_16_bit_wav)).read() # replace 16000 with 8000 if you want 8KHz output.
        except Exception as e:
            print "Error converting ",filename," to wave."
            return False
    return True
```

> 注意:上述函数要求您安装 sox & FFmpeg。如果你没有，推荐你参考这些链接: [SOX 下载](https://sourceforge.net/projects/sox/files/sox/)， [FFmpeg for Windows](https://www.wikihow.com/Install-FFmpeg-on-Windows) ， [FFmpeg on Ubuntu 20.04](https://linuxhint.com/install_ffmpeg_ubuntu_20-04/) 。

所以在你完成了上面的 3 个要求后，你应该有一个包含成千上万 wav 格式音频文件的目录。让我们将这个目录命名为“source”。所以我们有下面的目录。

```
data-prep
    - source
        - 16_bit_filename1.wav # 16 or 8 bit depends on you
        - 16_bit_filename2.wav
        - ...
```

接下来我们要准备我们的主数据集文件，也就是 fileids & transcription 文件。

准备一个 fileids 文件非常容易。您所要做的就是，运行下面的函数&它将为您生成一个 fileids 文件。难的是，准备一份转录文件。

```
import ossource_dir = input("Enter path to audio directory: ")def get_fileid_file(path: str): files = os.listdir(path) fileid_file_pointer = ("your-db-name.fileids","w") for file_name in files:
        fileid_file_pointer.write(file_name[:len(file_name)-4]+"\n")

    return True
```

> 注意:上面的脚本假设您已经将音频文件分割成一个 10 到 20 秒的随机窗口。如果你已经和说话的人分开了，那么它可能就不起作用了。在这种情况下，您只需要**稍微修改一下脚本&确保在将结果列表**写入 fileid 文件之前对其进行排序。

因此，没有工具来准备您的转录文件。所以你唯一的出路就是从 fileid 文件中一行一行地提取音频名称，听相应的音频，然后打开并把它写入一个转录文件。转录文件可以简单地在记事本或 vim 编辑器中打开。转录文件的格式是:

```
<s> some text .. blah blah blah.. </s> (filename)
```

***在< s >标记后应该有一个空格，&s</s>标记后&前应该有一个空格。大括号()内没有空格。***

确保在保存文件时，使用。转录扩展。另一个最重要的事情是保持 fileids 文件名和转录文件文本之间 1:1 的比例。

例如，如果您的 fileids 文件包含

```
filename1
filename2
filename3
```

那么转录文件应该包含

```
<s> some text which is spoken into filename1 file </s> (filename1)
<s> some text which is spoken into filename2 file </s> (filename2)
<s> some text which is spoken into filename3 file </s> (filename2)
```

***不能互换线路。fileids 文件中第 1 行的文件名应该与转录文件中相应的第 1 行相匹配。***

如果你决定根据说话者分割数据集，那么你的文件 id 和转录文件就会改变，按照 CMU·斯芬克斯喜欢的方式做事是非常重要的。如果你按说话者分割，这是你的音频目录更可能看起来的样子

```
data-prep
    - source
        - speaker1
            - filename1_speaker_1.wav
            - filename2_speaker_1.wav
            - filename3_speaker_1.wav
        - speaker2
            - filename1_speaker_2.wav
            - filename2_speaker_2.wav
            - filename3_speaker_2.wav
```

因此，您的 fileid 文件应该如下所示

```
speaker1/filename1_speaker1
speaker1/filename2_speaker1
speaker1/filename3_speaker1
speaker2/filename1_speaker2
speaker2/filename2_speaker2
speaker2/filename3_speaker3
```

> 注意:你不能洗牌。它必须是一个模型可以学习的序列。当有人问你名字时，这条规则同样适用于你。你回复“我叫 xxx”&而不是“名字 XXX 是我的”。音频必须是连续的&在一个流中。

您不必对转录文件进行更改。转录文件仍然与我们之前看到的一样。

现在，还有一件事要添加到转录文件中，那就是填充文本。填充文本只不过是音频中的停顿，不一定有助于模型训练。呼吸、噪音、停顿、咳嗽、咳咳、啊等等。是对模型训练没有任何意义的声音。如果你的录音中有这些声音，那么你应该确保把它们写入你的转录文件。

例如，在随机音频中，有一个人在说话时呼吸，所以，对应的转录行应该包含类似于下面的内容。

```
<s> hi there, this is me +breathing+ and talking something </s> (filename)
```

同样，您也可以添加其他声音。

这就完成了我们的数据集。恭喜🎁

在本系列的下一篇文章中，我们将研究如何构建您自己的语音、语言模型、字典和填充文件。敬请期待！

如果你有任何问题，请联系我[inbox.praveen.kumar17@gmail.com](mailto:inbox.praveen.kumar17@gmail.com)或者你也可以在评论区写下。

如果你喜欢这篇文章，请鼓掌👏！谢谢😃

下一步是什么？

关于以下主题的一系列文章即将发表。

建立你自己的语音，语言模型，字典和填充文件。

训练定制以及预训练声学模型。

超参数调整参数，找到最适合您的音频数据集。

使用 sphinx 配置实现最高精度。

构建定制语言模型。