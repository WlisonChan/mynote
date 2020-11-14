HashMap是以一种哈希表来存储键值对的数据结构，链地址法解决哈希冲突，在JDK1.8之后使用以下的组成结构优化。

**组成结构**：数组+链表+红黑树 

**负载因子loadFactory**：默认0.75

**初始容量initialCapacity**：默认为16

**扩容条件**：当前键值对的数量达到容量*负载因子的时候，进行扩容，扩容后的新数组长度为之前的2倍，需要进行rehash操作。

以下对HashMap重要的方法进行源代码分析：

hash方法：

```java
	//哈希操作:key为null的固定返回0，否则先求出hashcode，然后hashcode与自身无符号右移16位得到的数据进行位运算。
	static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }
```

**为什么要与自身无符号右移16位的数据进行位运算？**

个人认为：解决哈希冲突；会出现这么一种情况：如果恰好有多个key得到的hashcode低位全是0（我这是假设），采用hashcode&（容量-1）得到对应的key位置，此时容量比较小，hashcode高位无法参与到与运算中，导致冲突增加。

