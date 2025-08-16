## 一、HashMap底层数据结构
#### Java7
结构：数组+链表。每个hash槽上都是一个Entry对象。

```java
static class Entry<K, V> implements Map.Entry<K, V> {
    final K key;
    V value;
    Entry<K, V> next;
    int hash;
}
```

#### Java8
结构：数组+链表+红黑树。长度超过8的链表会转成红黑树，这样在这些位置进行查找的时候可以将时间复杂度从O(N)降低为O(logN)。

[￼HashMap](drawio/HashMap.drawio ':include :type=code')

## 二、HashMap寻址算法

1. map.put("age", 18);
2. hash("age")=hash值
3. (数组长度-1)&hash值=hash槽

## 三、HashMap的put过程

