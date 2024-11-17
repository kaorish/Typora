# 一. 包装类

## 八大Wrapper类

八大基本数据类型都有对应的类，称为包装类，因为如果是类的话就能使用里面的方法，更加方便。

`byte` -> `Byte`，`short` -> `Short`，  `int` -> `Integer` ，`long` -> `Long`，`float` -> `Float`，`double` -> `Double`，`char` -> `Character`，`boolean` -> `Boolean`。



继承关系：

`Boolean `、`Character `、`Number ` 继承 `Object  ` 类，`Byte `、`Short `、`Integer` 、`Long `、`Float `、`Double  ` 继承 `Number `。



## 自动装箱与自动拆箱

包装类与基本数据类型可以直接相互赋值，原因是包装类有自动装箱与自动拆箱的功能，这就很方便了。

`Integer integer = value;` 这条语句就使用了自动装箱，底层其实执行的是 `Integer integer = Integer.valueOf(value);`

`int i = integer;` 这条语句就使用了自动拆箱，底层其实执行的是 `int i = integer.intValue();`



## int 与 Integer 使用 == 进行比较

**int 与 int 相比：**

`int` 是基本数据类型，使用 `==` 就是直接比较值是否相等。



**Integer 与 Integer 相比：**

`Integer` 是 `int` 的包装类，两个 `Integer` 的对象使用 `==` 比较就是在比较他们是否是同一个对象。因为两个对象使用 `==` 比较实际上就是在比较他们的地址是否相等，而两个对象的地址相等就说明这两个对象是同一个对象。而使用 `new` 关键字创建的对象，他们的地址一定不相同，所以两个使用 `new` 关键字创建的对象使用 `==` 比较的结果一定是 'false'。



若不是new出来的而是直接赋值，此时又需要看他们是否满足条件：数值是否在-128~127范围内。

通过阅读 `Integer.ValueOf();` 的源码可知，设计者为了效率，提前为一些常用的数值设置成静态的，存入一个缓存数组中，其值的范围即为-128~127。而且该数组是 `Integer` 类型的对象数组，里面的数其实都是 `Integer` 的对象，而且 `Integer.ValueOf();` 返回的也是 `Integer` 的对象。故两个在-128~127的 `Integer` 对象其实是cache数组返回的，所以若两个 `Integer` 对象的值相等且在-128~127之间，那么这两个对象其实是同一个对象，使用 `==` 比较的结果为'true'。

注意：**只有 `Integer integer = value;` 这种写法才需要判断值的范围！**因为这里采用自动装箱了，底层是调用的 `Integer.ValueOf();`， 这个函数才有静态缓存数组。



**int 与 Integer 相比**

`int `与 `Integer` 使用 `==` 相比时，`Integer ` 会自动拆箱成 `int` 类型的，就又回到了两个 `int` 类型使用 `==` 相比，所以比较的是值。



```java
package com.hao.test;

public class casual {

    public static void main(String[] args) {

        Integer i1 = new Integer(127);
        Integer i2 = new Integer(127);
        System.out.println(i1 == i2); //false 两个new出来的对象一定不相等

        Integer i3 = new Integer(128);
        Integer i4 = new Integer(128);
        System.out.println(i3 == i4); //false 两个new出来的对象一定不相等

        Integer i5 = 127;
        Integer i6 = 127;
        System.out.println(i5 == i6); //true 数值在-128~127之间且两对象值相同，故为同一对象

        Integer i7 = 128;
        Integer i8 = 128;
        System.out.println(i7 == i8); //false 数值在在-128~127之间，采用new关键字新建对象，故两对象不相等

        Integer i9 = 127;
        Integer i10 = new Integer(127);
        System.out.println(i9 == i10); //false i9是缓存数组返回的，i10是使用new关键字创建的，故两对象不相等

        Integer i11 = 127;
        int i12 = 127;
        System.out.println(i11 == i12); //true i11自动拆箱，两int使用==比较，比较的是值

        Integer i13 = 128;
        int i14 = 128;
        System.out.println(i13 == i14); //true i13自动拆箱，两int使用==比较，比较的是值
    }
}

```

写在最后：要想让两个 `Integer` 类型比较是否相等，最好用 `a.equals(b)` 而非 `==`， 因为 `a.equals(b)` 比较的是值。



# 二. 集合

Java中的集合分为两种体系：Collection和Map，其中Collection分为List、Set。我们主要学习的部分中，List为ArrayList、LinkedList和Vector，Set为HashSet、LinkedHashSe和TreeSet，Map为HashMap、LinkedHashMap、Hashtable、Properties和TreeMap。

Collection用于存储单列元素，即数据是单个的，Map用于存储双列元素，以键-值对的形式存储。

![img](https://img-blog.csdnimg.cn/20210312115815460.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MzMyNjE2,size_16,color_FFFFFF,t_70)



## 1. Collection

![img](https://img-blog.csdnimg.cn/20210312115826405.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MzMyNjE2,size_16,color_FFFFFF,t_70)

### 1.1 List

List允许存储重复的元素。

ArrayList、LinkedList和Vector的使用方式几乎一样，没啥区别。

#### ArrayList

ArrayList的底层是Object的可变数组，所以可以存储任何元素。

ArrayList在第一次使用时需要进行扩容，在之后空间使用到一定程度时也需要进行扩容。

ArrayList在进行扩容时，是将原数组中的所有元素拷贝到一个更大的数组中。

ArrayList若进行插入操作时指定的索引有元素了，那么就会将该索引及后面的所以元素依次向后移动，此过程耗时较长，所以建议查改多时使用ArrayList，增删多时使用LinkedList，因为LinkedList底层是一个双向链表，增删元素时非常方便。



#### LinkedList

LinkedList的底层是一个Object的双向链表，同样可以存储任何元素。



#### Vector

Vector底层也是Object的可变数组，但是ArrayList和LinkedList都是线程不安全的，而Vector是线程安全的。且现在不建议使用Vector了，具体原因我还没去细查。



### 1.2 Set

Set不允许存储重复的元素。

#### HashSet

HashSet的底层是HashMap，而HashMap的底层是数组+链表+红黑树，所以HashSet底层也是数组+链表+红黑树。

HashSet的底层是一个HashMap，只不过value被常量PRESENT充当，相当于一个占位符占据了value的位置，我们使用HashSet时都是使用key这个部分。



#### LinkedHashSet

LinkedHashSet的底层是数组+链表（+双向链表）+红黑树。

与HashSet很类似，只不过底层多维护了一个双向链表，所以保证了元素的取出顺序与插入顺序一致。能进行的操作和底层细节均与HashSet一模一样，两者唯一的区别就是取出顺序与插入顺序能否保持一致。



#### TreeSet

TreeSet的底层是TreeMap，TreeMap的底层是红黑树。

TreeSet可排序，通过自定义比较器进行排序，若创建对象时未传入构造器则采用自然排序。

TreeSet自定义比较器及遍历：

```java
package com.hao.hspjava.gather;

import java.util.Comparator;
import java.util.Iterator;
import java.util.TreeSet;

@SuppressWarnings("all")
public class TreeSetTest {

    public static void main(String[] args) {

//        TreeSet<String> treeSet = new TreeSet<>(); // 自然排序，对于字符串来说就是自动调用String类型的compareTo()方法，即按字典序进行排序。若compareTo()返回0，则认为是相同的key，此时会将value进行替换。但是TreeSet的底层的TreeMap的value被PRESENT占据了，所以替换也是用PRESENT进行替换，相当于没变；若不为0,则按顺序添加进集合。
        TreeSet<String> treeSet = new TreeSet<>(new Comparator<String>() { // 根据传入的Comparator匿名内部类里的compare()方法进行排序，若compare()方法返回0,则认为是相同的key，此时会将value进行替换。但是TreeSet的底层的TreeMap的value被PRESENT占据了，所以替换也是用PRESENT进行替换，相当于没变；若不为0则按顺序添加进集合。
            @Override
            public int compare(String o1, String o2) {
                // 按字符串长度进行排序，o1是第一个参数，o2是第二个参数。按长度排序也就不能放入长度出现过的字符串了，会认为是相同的key，此时会将value进行替换。但是TreeSet的底层的TreeMap的value被PRESENT占据了，所以替换也是用PRESENT进行替换，相当于没变。
                return o1.length() - o2.length(); // 参数1减参数2，则按长度从小到大进行排序
//                return o2.length() - o1.length(); // 参数2减参数1，则按长度从大到小进行排序
            }
        });

        treeSet.add("c");
        treeSet.add("java");
        treeSet.add("python");
        treeSet.add("cpp");
        treeSet.add("go");
        treeSet.add("c#");

        // TreeSet的两种遍历方式：增强for和迭代器
        // 增强for
        for (String str : treeSet) {
            System.out.println(str);
        }

        // 迭代器
        Iterator<String> it = treeSet.iterator();
        while (it.hasNext()) {
            String str = it.next();
            System.out.println(str);
        }
    }
}


```



## 2. Map

![img](https://img-blog.csdnimg.cn/20210312115840164.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM0MzMyNjE2,size_16,color_FFFFFF,t_70)

### HashMap

HashMap的底层是数组+链表+红黑树。

key和value都可以存入null。实际上在HashMap、ConcurrentHashMap、Hashtable和TreeMap中，只有HashMap的key可以为null，只有HashMap和TreeMap的value可以为null。



### LinkedHashMap

LinkedHashMap和HashMap很类似，只不过LinkedHashMap多维护了一个双向链表，所以保证了元素的取出顺序与插入顺序一致。



### TreeMap

TreeMap的底层是红黑树。

TreeMap可排序，通过自定义比较器进行排序，若创建对象时未传入构造器则采用自然排序，且排序都是对于key来说的，即对key进行排序。

不是线程安全的。

key不可以存入null，value可以是null。

TreeMap自定义排序及遍历：

```java
package com.hao.hspjava.gather;

import java.util.*;

@SuppressWarnings("all")
public class TreeMapTest {

    public static void main(String[] args) {

//        TreeMap<String, String> treeMap = new TreeMap<>(); // 使用自然排序法，只对key进行排序。对于字符串来说就是自动调用String类型的compareTo()方法，即按字典序进行排序。若compareTo()返回0,则认为是相同的key，此时会将value进行替换；若不为0则按顺序添加进集合。
        TreeMap<String, String> treeMap = new TreeMap<>(new Comparator<String>() { // 根据传入的Comparator匿名内部类里的compare()方法进行排序，若compare()方法返回0，则认为是相同的key，此时会将value进行替换；若返回不为0，则按排序添加进集合中。
            @Override
            public int compare(String o1, String o2) {
                // 按字符串长度进行排序，o1是第一个参数，o2是第二个参数。按长度排序也就不能放入长度出现过的字符串了，会认为是相同的key，此时会将value进行替换。
                return o1.length() - o2.length(); // 参数1减参数2，则按长度从小到大进行排序
//                return o2.length() - o1.length(); // 参数2减参数1，则按长度从大到小进行排序
            }
        }); // 使用自然排序法
        treeMap.put("mysql", "尚学堂");
        treeMap.put("c", "翁恺");
        treeMap.put("java", "hsp");
        treeMap.put("cpp", "黑马程序员");

        // Map的遍历方式：获得keySet再用get方法获取value、获得values集合单独获取value、获得entrySet再用getKey和getValue方法获得key和value
        // 获得keySet再用get方法获取value
        Set<String> keySet = treeMap.keySet();
        for (String key : keySet) { // 增强for
            System.out.println(key + "-" + treeMap.get(key));
        }

        Iterator<String> it = keySet.iterator(); // 迭代器
        while (it.hasNext()) {
            String key = it.next();
            System.out.println(key + "-" + treeMap.get(key));
        }

        // 获得values集合单独获取value
        Collection<String > values = treeMap.values();
        for (String value : values) { // 增强for
            System.out.println(value);
        }

        Iterator<String> iterator = values.iterator(); // 迭代器
        while (iterator.hasNext()) {
            String value = iterator.next();
            System.out.println(value);
        }

        // 获得entrySet再用getKey和getValue方法获得key和value
        // 使用泛型
        Set<Map.Entry<String, String>> entries = treeMap.entrySet();
        for (Map.Entry entry : entries) { // 增强for
            System.out.println(entry.getKey() + "-" + entry.getValue());
        }

        Iterator<Map.Entry<String, String>> entryIterator = entries.iterator(); // 迭代器
        while (entryIterator.hasNext()) {
            Map.Entry entry = entryIterator.next();
            System.out.println(entry.getKey() + "-" + entry.getValue());
        }

        // 不使用泛型
        Set entries2 = treeMap.entrySet();
        for (Object o : entries2) { // 增强for
            Map.Entry entry = (Map.Entry)o;
            System.out.println(entry.getKey() + "-" + entry.getValue());
        }

        Iterator it2 = entries.iterator(); // 迭代器
        while (it2.hasNext()) {
            Map.Entry entry2 = (Map.Entry)it2.next();
            System.out.println(entry2.getKey() + "-" + entry2.getValue());
        }


    }
}

```



### Hashtable

Hashtable与HashMap的底层是一样的，数组+链表+红黑树。

与HashMap不同，Hashtable是线程安全的。

Hashtable与HashMap非常相像，操作也与HashMap一致，只不过Hashtable的key和value都不能使用null充当，而HashMap都可以。



### Properties

Hashtable的子类，操作与Hashtable非常相似。

Properties可以读取 xxx.properties 文件。
