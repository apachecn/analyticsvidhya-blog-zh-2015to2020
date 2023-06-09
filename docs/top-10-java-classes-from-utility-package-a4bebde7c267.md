# 实用程序包中的前 10 个 Java 类

> 原文：<https://medium.com/analytics-vidhya/top-10-java-classes-from-utility-package-a4bebde7c267?source=collection_archive---------2----------------------->

**Java.util** 包包含集合的框架、遗留集合类、事件模型、日期和时间工具、国际化和其他实用程序类(一个字符串标记器、一个随机数生成器和一个位数组)。以下是最常用的十大 Java 实用程序类列表:

# 1.Java 数组类

Java.util 包提供了一个 [**数组类**](https://www.alphacodingskills.com/java/java-util-arrays.php) ，该类包含一个静态工厂，允许数组被视为列表。该类包含各种操作数组的方法，如排序和搜索。如果指定的数组引用为空，此类中的方法将引发 NullPointerException。

# 2.Java 向量类

Java.util 包提供了一个 [**vector 类**](https://www.alphacodingskills.com/java/java-util-vector.php) 来建模和实现向量数据结构。vector 是一个序列容器，实现大小可变的数组。与数组不同，当追加或删除元素时，向量的大小会自动改变。它类似于 ArrayList，但有两个不同之处:

*   矢量同步。
*   Vector 包含许多不属于集合框架的遗留方法。

# 3.Java LinkedList 类

Java.util 包提供了一个 [**LinkedList 类**](https://www.alphacodingskills.com/java/java-util-linkedlist.php) ，它具有 List 和 Deque 接口的双向链表实现。该类具有所有可选的列表操作，并允许所有元素(包括 null)。索引到列表中的操作将从列表的开头或结尾遍历列表，无论哪一个更接近指定的索引。

# 4.Java 日历类

Java.util 包提供了一个 [**日历类**](https://www.alphacodingskills.com/java/java-util-calendar.php) 来表示一个特定的时刻，精度为毫秒。

# 5.Java 集合类

Java.util 包提供了一个 [**集合类**](https://www.alphacodingskills.com/java/java-util-collections.php) ，它专门由操作或返回集合的静态方法组成。它包含对集合进行操作的多态算法，即“包装器”，它返回一个由指定集合支持的新集合。如果提供给该类的集合或类对象为空，则该类的所有方法都会引发 NullPointerException。

# 6.Java HashMap 类

Hashtable 建模并实现了 Map 接口。这个实现提供了所有可选的映射操作，并允许空值和空键。一个 [**HashMap 类**](https://www.alphacodingskills.com/java/java-util-hashmap.php) 大致相当于 Hashtable，除了它是不同步的并且允许空值。这个类不保证地图的顺序。它不保证顺序会随时间保持不变。

# 7.Java 随机类

Java.util 包提供了一个 [**随机类**](https://www.alphacodingskills.com/java/java-util-random.php) 。此类的一个实例用于生成伪随机数流。该类使用 48 位种子，该种子使用线性同余公式进行修改。由类 Random 实现的算法使用一个受保护的实用程序方法，该方法在每次调用时可以提供多达 32 个伪随机生成的位。

# 8.Java UUID 类

Java.util 包提供了一个 [**UUID 类**](https://www.alphacodingskills.com/java/java-util-uuid.php) ，表示一个不可变的通用唯一标识符(UUID)。一个 UUID 代表一个 128 位的值。它用于创建随机文件名、web 应用程序中的会话 id、事务 id 等。有四种不同的基本类型的 uuid:基于时间的、DCE 安全的、基于名称的和随机生成的 uuid。

# 9.Java 扫描器类

Java.util 包提供了一个 [**扫描器类**](https://www.alphacodingskills.com/java/java-util-scanner.php) 。一个简单的文本扫描器使用正则表达式解析原始类型和字符串。扫描器使用分隔符模式将其输入分成标记，默认情况下，分隔符模式匹配空格。然后，可以使用各种 next 方法将得到的令牌转换成不同类型的值。

扫描操作可能会阻止等待输入，并且对于没有外部同步的多线程使用来说是不安全的。

# 10.Java 数组列表类

Java.util 包提供了一个 [**ArrayList 类**](https://www.alphacodingskills.com/java/java-util-arraylist.php) ，它提供了 List 接口的可调整大小的数组实现。它实现了所有可选的列表操作，并允许所有元素，包括 null。除此之外，该类还提供了一些方法来操作内部用于存储列表的数组的大小。这个类几乎等同于 Vector 类，只是实现不是同步的。

上面提到的所有类在编程中被广泛用于解决日常问题。了解这些课程会使解决问题的技能处于不同的水平。