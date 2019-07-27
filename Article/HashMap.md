## HashMap



#### 概述

底层是一个“数组+链表+红黑树”的数据结构

主要的属性有下面这几个:

```java
    transient Node<K,V>[] table;

    transient Set<Map.Entry<K,V>> entrySet;

    transient int size;

    transient int modCount;
   
    int threshold;

    final float loadFactor;

```

> table:底层的hash数组，是Node类型数组，可以把这个数组的每个位置理解为一个桶，每个桶存着一个链表，链表的目的是采用拉链法解决
>
> size: HashMap中实际存在的键值对数量
>
> threshold: HashMap所能容纳的最大数据量的Node(键值对)个数。threshold = length * Load factor。也就是说，在数组定义好长度之后，负载因子越大，所能容纳的键值对个数越多
>
> loadFactor:





#### Node类

```java

```



#### Hash值的计算

JDK1.7中的哈希运算

```java
final int hash(Object k) {
    int h = hashSeed;
    if (0 != h && k instanceof String) {
        return sun.misc.Hashing.stringHash32((String) k);
    }

    h ^= k.hashCode();
    h ^= (h >>> 20) ^ (h >>> 12);
    return h ^ (h >>> 7) ^ (h >>> 4);
}

static int indexFor(int h, int length) {
    return h & (length-1);
}
```

JDK1.8中的哈希运算

```java
 static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```

首先来看哪些方法调用了该函数

![](D:\Code\MarkdownPhotosRepository\Res\ArticlePic\HashMap中调用hash()的方法.png)

我们的希望是，哈希数组里面的元素分布的均匀些，尽量让每个位置只有一个元素，这样可以避免遍历链表或红黑树，可以优化查询效率。

JDK1.8中hash运算：

1. 取key的hashCode值

2. 高位运算：JDK1.8优化了高位运算，通过hashCode值的高16位和低16位运算得到，这样尽管在table的length比较小时，可可以让高位和低位都参与到运算中，同时开销较小

3. 取模运算：这一步在JDK1.8中没有封装，直接写在各个方法中，与1.7的indexFor相同

   > **位运算（&）代替取模运算的原理**
   >
   > X % 2^n = X & (2^n – 1)
   >
   > 所以只要保证length的长度是2^n的话，就可以实现取模运算了，而HashMap中的length也确实是2的倍数

   

   > **位运算（&）代替取模运算的好处**
   >
   > 1. 位运算直接对内存数据进行操作，不需要转成十进制，处理速度非常快
   > 2. 很好的解决负数问题

   





#### put

说起HashMap的Put方法，就不得不祭出这张图，清晰的表现了整个过程。

![](D:\Code\MarkdownPhotosRepository\Res\ArticlePic\hashmap-put全过程.jpeg)





#### 扩容

- 扩容是容量翻倍，所以原链表上的每个节点，现在可能存放在原来的下标，即low位， 或者扩容后的下标，即high位。 high位= low位+原哈希桶容量 
- 利用**哈希值 与运算 旧的容量** ，`if ((e.hash & oldCap) == 0)`,可以得到哈希值去模后，是大于等于oldCap还是小于oldCap，等于0代表小于oldCap，**应该存放在低位，否则存放在高位**。这里又是一个利用位运算 代替常规运算的高效点 