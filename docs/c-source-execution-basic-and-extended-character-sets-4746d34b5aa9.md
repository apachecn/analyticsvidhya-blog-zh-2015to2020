# c:源、执行、基本和扩展字符集

> 原文：<https://medium.com/analytics-vidhya/c-source-execution-basic-and-extended-character-sets-4746d34b5aa9?source=collection_archive---------8----------------------->

`C`中有两个字符集。第一个是 ***源字符集*** ，这是一组字符，其中写入了一个 C 源文件。

因此，源字符集包括在 C 操作符中使用的字符集，例如`+=`，或者在 C 标识符中使用的字符集，例如在变量或函数的名称中，或者在 C 关键字中使用的字符集，例如`int`或`if`，或者在文字中使用的字符集，例如整数、字符或字符串文字。

第二个是 ***执行字符 se* t** 。这是字符集，程序在这里执行。例如，我们可以将 ASCII 作为源字符集，将 unicode 作为执行字符集。不要混淆 unicode 字符集，用 Unicode 编码比如`utf-8`或者`utf-16`...

编译 C 程序时，预处理完成后，源文件，源字符集，被 ***转换*** 为执行字符集。

这种转换包括转义序列的转换，如字符常量或字符串中的`\n`。

源字符集和执行字符集之间的转换，是在源文件被转换成目标代码之前的一段时间内完成的。

无论是源字符集还是执行字符集，都必须包含一个叫做 ***的基本字符集*** ，其中包括以下字符。

```
A B C D E F G H I J K L M N O P Q R S T U V W X Y Za b c d e f g h i j k l m n o p q r s t u v w x y z0 1 2 3 4 5 6 7 8 9! " # % & ’ ( ) * + , - . / :; < = > ? [ \ ] ^ _ { | } ~
```

从`0`到`9`的字符的 ***值*** ，在源和执行基本字符集中，必须一个接一个，增量为 1。

基本的源和执行字符集还必须包括 ***空格字符*** ，其例如通过在键盘上键入空格键而获得，并且通过水平线上的空列可视地显示，并且其例如在 ASCII 字符集和 unicode 字符集中都具有十进制数值`32`。

基本的源和执行字符集，还必须包括**:水平制表符、垂直制表符、换页符和换行符。**

**基本执行字符集还必须包括控制字符，它们代表:警告、退格、回车和空字符。**

**基本执行字符集控制字符在源文件的字符常量或字符串中表示，使用由反斜杠字符构成的转义序列，后跟一个或多个字符:`\a`表示警告、`\b`表示退格、`\r`表示回车、`\0`表示空、`\t`表示水平制表符、`\v`表示垂直制表符、`\f`表示换页、`\n`表示换行。**

*****null****字符，表示一串字符的结束。***

```
***printf("%s", "Hello world\n" );
/*Outputs Hello World , followed by a new line  */
Hello worldprintf("%s", "Hello\0World\n" );
/*Outputs only Hello , because of \0 */
Hello***
```

***空字符必须在执行字符集中用一个字节表示，所有位都设置为零。***

***[三字符](https://difyel.com/c/lexical/what-is-a-trigraph-in-c/)，可用于表示基本字符集中的字符，例如不能在某些键盘上输入的字符。三字符由两个询问标记和一个字符组成。例如`??=`代表`#`。***

***基本的源和执行字符集，必须用单字节 ***编码*** 。一个字节中的位数由实现定义，并且必须能够容纳基本执行字符集的任何字符。一个字节是一个可寻址的存储单位。***

**源字符集和执行字符集，也可以各包含一组 0 个或更多的*局部特定字符，不是基本字符集的一部分，称为扩展字符。这些字符可用于:字符常量、字符串文字、注释、头名称、标识符或不转换为标记的预处理标记。***

***如果一个字符在扩展字符集中，并且当它被用于标识符、字符常量或字符串文字时，它可以用它的 ***通用名称*** 来表示，其格式为`\u`后跟四个十六进制数字，或者`\U`后跟八个十六进制数字。***

**通用字符名是 unicode 中的短标识符。例如 YEZIDI 字母 ELIF，在 unicode 中有一个短标识符`10E80`，或`+10E80`，或`U10E80`，或`U+10E80`，在 C 语言中它的通用字符名是`\U00010E80`。**

**如果在源字符集中定义了一个字符，而在执行字符集中没有找到*，那么由实现选择一个相应的字符，只要它不是空字符。***

***扩展字符不是基本字符集的一部分，可以使用多个字节对 ***进行编码*** 。***

**给基本字符集和添加的本地特定字符集起的名字是 ***扩展字符集*** 。**

***原载于 2020 年 11 月 14 日 https://twiserandom.com*[](https://twiserandom.com/c/c-source-execution-basic-and-extended-character-sets/)**。****