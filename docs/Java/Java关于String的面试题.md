## 一、String创建了几个对象

1. String s1 = "abc";

(1)首先会去字符串常量池中查找是否有该String对象；
(2)如果没有，创建一个String对象，并在字符常量池中创建一个Entry对象指向该String对象，s1同时也指向该String对象；
(3)如果有，就不会创建对象，s1就直接指向该对象。

2. String s1 = new String("abc");

首先创建"abc"字符串对象，先去字符串常量池中查找，如果没有就创建一个Entry对象，然后创建一个String对象，指向一个char数组(abc)，并用Entry对象记录这个String对象。然后new String()又创建一个对象，指针指向之前创建的String对象("abc")，并且将其地址赋予s1。

3. String s1 = "abc"; String s2 = new String("abc");

s1指向创建字符串"abc"时创建的String对象；s2指向new String()时创建的String对象。

[String内存图](drawio/String内存图.drawio ':include :type=code')

## 二、String为什么不可变
“不可变”是指对象属性的值是不可变的。
```
public final class String {
    private final char value[];
}
```

1. final表示该变量引用地址是不可变的。但仍然可以通过下标修改数组内容，比如： value[0]='x';
2. private表示私有变量，外部不能访问，并且String类并未对外提供该value数组的修改方法。
