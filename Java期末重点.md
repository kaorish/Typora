# 一. 课上讲的



## 填空

阅读程序回答问题、内存模型、补充程序、找错





## 填空题

平常的作业多看看



## 问答题

1. 线程有哪些状态？说明何时有这种状态

五种状态：

jdk1.5之前，5种状态：新建（New）、就绪（Runnab）、运行（Running）、阻塞（Blocked）、死亡（Dead）

jdk1.5之后，6种状态：NEW、RUNNALBE、BLOCKED、WAITING、TIMED_WAITING、TERMINATED

NEW：线程刚被创建，但是未启动。还没调用start()方法。

RUNNABLE：



2. 有继承时，静态语句块和构造方法的调用顺序

假设父类和子类都具有静态代码块、构造代码块和构造方法。那么main方法中创建子类对象时，先调用父类的静态代码块，然后调用子类的静态代码块，再调用父类的构造代码块，再调用父类的构造方法，再调用子类的构造代码块，再调用子类的构造方法。即父类静态代码块 -> 子类静态代码块 -> 父类构造代码块 -> 父类构造方法 -> 子类构造代码块 -> 子类构造方法，各自的构造代码块分别位于各自的构造方法之前。





## 编程题

老师提到的：日期类

**打印当前日期的日历：**

```java
package com.hao.homework;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;
import java.util.Scanner;

public class Episode13 {

    public static void main(String[] args) throws ParseException {

        Scanner scanner = new Scanner(System.in);
        System.out.print("请输入日期(年月日，格式：2023-04-10)：");
        String str = scanner.next(); // 以字符串形式读入日期，因为可以按照格式输入
        SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd"); // 设置格式
        Date date = simpleDateFormat.parse(str); // 根据格式将字符串转换成Date对象
        Calendar calendar = Calendar.getInstance();
        calendar.setTime(date); // 将Date类的对象转换成Calendar类的对象
        System.out.println("日\t一\t二\t三\t四\t五\t六"); // 打印日历头
        int dateOfMonth = calendar.get(Calendar.DATE); // 获取当前日期在整个月份中的日子，即第几号
        calendar.set(Calendar.DATE, 1); // 将该时间设置为这个月的第一天
        int weekDay = calendar.get(Calendar.DAY_OF_WEEK); // 设置成第一天后调用get(Calendar.DAY_OF_WEEK)方法获得该日子是星期几
        for(int i=1;i<weekDay;i++) System.out.print('\t'); // 将该月第一天之前的日子用空白符占据，因为要打印的是这个月的日历，与上一个月无关
        int maxDay = calendar.getActualMaximum(Calendar.DATE); // 获取这个月的天数
        for(int i=1;i<=maxDay;i++){  // 只要没到最后一天就一直打印
            System.out.print(i);
            if(i==dateOfMonth) System.out.print("*"); // 如果当前打印的日子与当初输入的日子重合了，就在后面输出星号标识一下
            System.out.print("\t");
            weekDay = calendar.get(Calendar.DAY_OF_WEEK); // 获取当前日子是星期几，因为满一周后要换行
            if(weekDay==7) System.out.println(); // 因为令周日的weekDay为1，故当weekDay为7时表明打印周六了，即满了一周，需要换行了
            calendar.add(Calendar.DATE, 1); // 打印完后让当前日期+1，以便再次循环打印后面的日子
        }

    }
}

```





理工超市的会员积分



Set中TreeSet和TreeMap的自定义比较器

TreeSet自定义比较器：

```java
package com.hao.hspjava.gather;

import java.util.Comparator;
import java.util.Iterator;
import java.util.TreeSet;

@SuppressWarnings("all")
public class TreeSetTest {

    public static void main(String[] args) {

//        TreeSet<String> treeSet = new TreeSet<>(); // 自然排序，对于字符串来说就是自动调用String类型的compareTo()方法，若compareTo()返回0则说明两个对象相等，不添加；若不为0则按顺序添加进集合。
        TreeSet<String> treeSet = new TreeSet<>(new Comparator<String>() { // 根据传入的Comparator匿名内部类里的compare()方法进行排序，若compare()方法返回0则说明这两个对象相等，就不添加；若返回不为0，则按排序添加进集合中。
            @Override
            public int compare(String o1, String o2) {
                // 按字符串长度进行排序，o1是第一个参数，o2是第二个参数。
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





线程同步问题、封装、继承、多态、数据流、复制文件内容、网络编程



复制文件内容：

```java
package com.hao.homework.episode15;  
  
import java.io.File;  
import java.io.FileInputStream;  
import java.io.FileOutputStream;  
import java.io.IOException;  
  
@SuppressWarnings("all")  
public class Test4 {  
  
    public static void copyDir(File src, File des) {  
        if (!src.exists() || !src.isDirectory()) throw new RuntimeException("文件不存在或不是目录！");  
        if (!des.exists()) des.mkdir();  
          
        File[] files = src.listFiles();  
        for (File file : files) {  
            if (file.isDirectory()) copyDir(file, new File(des, file.getName()));  
            else copyFile(file, new File(des, file.getName()));  
        }  
    }  
  
    public static void copyFile(File src, File des) {  
        FileInputStream fis = null;  
        FileOutputStream fos = null;  
        try {  
            fis = new FileInputStream(src);  
            fos = new FileOutputStream(des);  
            byte[] cBuffer = new byte[1024];  
            int len;  
            while ((len = fis.read(cBuffer)) != -1) fos.write(cBuffer, 0, len);  
        } catch (IOException e) {  
            e.printStackTrace();  
        } finally {  
            try {  
                if (fos != null)  
                    fos.close();  
            } catch (IOException e) {  
                e.printStackTrace();  
            }  
            try {  
                if (fis != null)  
                     fis.close();  
            } catch (IOException e) {  
                e.printStackTrace();  
            }  
        }  
  
    }  
  
  
    public static void main(String[] args) {  
  
        File src = new File("C:\\Users\\孙皓\\Documents\\军训");  
        File des = new File("C:\\Users\\孙皓\\Documents\\军训_copy");  
  
        try {  
            copyDir(src, des);  
            System.out.println("文件复制成功！");  
        } catch (Exception e) {  
            System.out.println("文件复制失败！");  
        }  
    }  
}  
```



# 二. 老师给的

## 1. super关键字的作用、有继承结构时，静态语句块和构造方法的调用顺序

**super关键字的作用：**

`super`关键字在Java中有以下两个主要作用：

1. 调用父类的构造函数：当你创建一个子类对象时，子类的构造函数通常需要调用其父类的构造函数。这可以确保初始化顺序正确。为了实现这一点，你可以在子类构造函数中使用`super()`关键字来调用父类的构造函数。如果没有显式地调用`super()`，Java编译器会默认插入一个无参的`super()`调用。

示例：
```java
class Parent {
    public Parent() {
        System.out.println("Parent constructor");
    }
}

class Child extends Parent {
    public Child() {
        super(); // 调用父类的构造函数
        System.out.println("Child constructor");
    }
}
```

2. 子类访问父类的成员（方法或变量）：有时候，子类可能会覆盖父类的方法或隐藏父类的成员变量。在这些情况下，你可以使用`super`关键字在子类中访问被覆盖的父类方法或被隐藏的父类成员变量。

示例：
```java
class Parent {
    public void print() {
        System.out.println("Parent class method");
    }
}

class Child extends Parent {
    @Override
    public void print() {
        super.print(); // 调用父类的print方法
        System.out.println("Child class method");
    }
}
```
总之，`super`关键字是用于在子类中引用父类的成员或构造函数。这在继承关系中保持了代码的逻辑顺序和清晰性。



**有继承结构时，静态语句块和构造方法的调用顺序：**

假设父类和子类都具有静态代码块、构造代码块和构造方法。那么main方法中创建子类对象时，先调用父类的静态代码块，然后调用子类的静态代码块，再调用父类的构造代码块，再调用父类的构造方法，再调用子类的构造代码块，再调用子类的构造方法。即父类静态代码块 -> 子类静态代码块 -> 父类构造代码块 -> 父类构造方法 -> 子类构造代码块 -> 子类构造方法，各自的构造代码块分别位于各自的构造方法之前。





## 2. 线程的生命周期、产生死锁的条件、创建线程的两种方式

**线程的生命周期：**

Java中线程的生命周期包含以下几个状态：

1. 新建（New）：当使用`new Thread()`创建一个线程对象时，它处于新建状态。此时，线程对象已经被创建，但尚未启动。
2. 可运行（Runnable）：当调用线程对象的`start()`方法后，线程进入可运行状态。这意味着线程已经准备好运行，但实际执行时间取决于操作系统的调度。在可运行状态下，一旦操作系统分配给线程CPU时间片，线程就开始执行。
3. 阻塞（Blocked）：阻塞状态表示线程因等待某种资源或完成特定操作而暂停执行。例如，当线程等待I/O完成、获取锁或调用`sleep()`、`wait()`、`join()`等方法时，都可能导致线程进入阻塞状态。
4. 等待（Waiting）：线程在等待其他线程执行特定操作的过程中。例如，当一个线程调用了另一个线程的`join()`方法，它会进入等待状态，直到被调用`join()`的线程执行完毕。
5. 超时等待（Timed Waiting）：与等待状态类似，不同之处在于超时等待状态有一个时间限制。例如，当线程调用`sleep(long millis)`、`wait(long timeout)`或`join(long millis)`等方法时，线程会进入超时等待状态。当超时期满或条件得到满足，线程将转为其他状态。
7. 终止（Terminated）：线程已完成任务或因异常而终止。一旦线程达到终止状态，它不能重新启动。

在Java线程的生命周期中，线程可能会在这些状态之间转换。理解这些状态及其转换对于更好地管理和控制多线程程序非常重要。



**产生死锁的条件：**

Java中线程产生死锁的条件包括以下四个：

1. 互斥：一个资源同时只能被一个线程占用，如果其他线程想要占用该资源，需要等待当前线程释放。

2. 持有和等待：一个线程持有一个资源，并且还在等待另一个线程正在持有的资源。

3. 不可抢占性：一个线程已经获取了一个资源，其他线程无法强制剥夺它所持有的资源。

4. 循环等待：一组线程之间形成一种头尾相接的循环等待，每个线程都在等待下一个线程所持有的资源。

当以上四个条件同时满足时，就可能出现死锁情况。为了避免死锁的发生，我们可以采取一些措施比如使用非阻塞算法、破坏循环等待、加锁顺序等来避免上述条件的发生。



**创建线程的两种方式：**

1. 继承Thread类： 创建一个新类继承Thread类，并重写run()方法，该方法包含需要在新线程中执行的代码。之后可实例化该类的对象，并调用start()方法启动新线程。
2. 实现Runnable接口： 创建一个实现了Runnable接口的类，并重写run()方法，也包含需要在新线程中执行的代码。之后可实例化该类的对象，并将其传入Thread类的构造方法中，最后调用start()方法启动新线程

 

**这两种方式的优缺点：**

1. 继承Thread类：

- 优点：直观易懂、使用简单，在run()方法中直接编写任务代码即可。

- 缺点：继承了Thread类后无法再继承其他类；如果要执行的任务已经有了基类，则需要重新创建一个新类，不方便程序的复用。



2. 实现Runnable接口：

- 优点：与其他类进行继承关系，更易于复用，也更能体现面向对象设计的思想。另外，多个线程可以共享同一个目标对象，适合于多线程处理同一资源的情况。

- 缺点：实现多个方法，其中只有一个run()方法是线程执行的任务入口，较为麻烦；同时也需要新建一个Thread类来调用Runnable对象。



## 3. ArrayList和Set的遍历方式、HashSet与TreeSet的区别

**ArrayList的遍历方式：**

遍历方式：使用普通for循环、使用增强for循环、使用迭代器

如：

```java
package com.hao.hspjava.gather;

import java.util.ArrayList;
import java.util.Iterator;

public class ArrayListTest {

    public static void main(String[] args) {

        ArrayList arrayList = new ArrayList();
        arrayList.add(1);
        arrayList.add(2);
        arrayList.add(3);

        // 三种遍历方式：使用普通for循环、使用增强for循环、使用迭代器
        // 普通for循环：
        for (int i = 0; i < arrayList.size(); i++) {
            System.out.println(arrayList.get(i));
        }

        // 增强for循环
        for (Object o : arrayList) {
            System.out.println(o);
        }

        // 迭代器
        Iterator iterator = arrayList.iterator();
        while (iterator.hasNext()) {
            Object next =  iterator.next();
            System.out.println(next);
        }


    }
}

```



**Set的遍历方式：**

 Set集合的两种遍历方式：增强for循环和迭代器。

因为Set是无序的，所以没有索引，也没有size() 方法，所以Set不能使用普通for循环进行遍历。

如：

```java
package com.hao.hspjava.gather;

import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class HashSetTest {

    public static void main(String[] args) {

        Set<String> set = new HashSet<>();
        set.add("Java");
        set.add("Python");
        set.add("C++");

        // Set集合的两种遍历方式：增强for循环和迭代器
        // 增强for：
        for (String element : set) {
            System.out.println(element);
        }

        // 迭代器：
        Iterator<String> it = set.iterator();
        while (it.hasNext()) {
            String element = it.next();
            System.out.println(element);
        }

    }
}

```



**HashSet和TreeSet的区别：**

Java 中的 HashSet 和 TreeSet 分别是基于哈希表和红黑树实现的 Set 集合类。

1. 存储方式不同

- HashSet 内部采用哈希表 (实际上是数组+链表+红黑树)，元素的存放位置依赖于它的 hashCode() 方法；
- TreeSet 内部采用红黑树，按照元素值的大小进行排序。


2. 判重方式不同

- HashSet 中判定两个元素是否相同是根据元素的 hashCode 和 equals 方法是否相等来决定的；
- TreeSet 中判定两个元素是否相同是根据元素的 compareTo 或 compare 方法的返回值是否为 0 来决定的。


3. 元素顺序不同

- HashSet 不关心元素之间的顺序，因此遍历出元素的顺序是不可预测的；
- TreeSet 存储时候会把元素排序，因此遍历出的元素是按升序排列的。


4. 数据结构性能不同

- 哈希表容量大时插入删除、查找速率仅次于数组，平均时间复杂度为 O(1);
- 红黑树插入、删除、查找都保持 O(logN) 的较高效率。

综上所述，HashSet 更适合在无序、唯一性要求较高的场合使用；TreeSet 更适合于需要排序，同时去重的场合使用。



## 4. 何时使用自定义异常、如何创建自定义异常类

**使用场景：**

在开发、写代码的过程中中总是有些异常情况是SUN没有定义好的,此时我们根据自己业务的异常情况来定义异常类。例如年龄负数问题,考试成绩负数问题等等。



**如何创建自定义异常类：**

创建一个类并继承Java已有的异常类（比如Exception或者RuntimeException），并且需要编写构造方法来初始化异常对象。

如：

```java
public class MyException extends Exception {

  private int code; // 自定义异常的错误码
  private String message; // 异常信息

  public MyException(int code, String message) {
    super(message);
    this.code = code;
    this.message = message;
  }

  public int getCode() {
    return code;
  }

  public String getMessage() {
    return message;
  }
}
```

作业的例子：

```java
// MyException自定义异常类
package com.hao.homework.episode12;

public class MyException extends Exception {

    private int score;

    public MyException(int score) {
        this.score = score;
    }

    @Override
    public String toString() {
        return "当前输入的成绩" + this.score + "不合法，" + "分数必须在0—100之间";
    }
}

// Test测试类
package com.hao.homework.episode12;

import java.util.Scanner;

public class Test {

    public static void check(int score) throws MyException {
        if (score < 0 || score > 100) throw new MyException(score);
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int score = scanner.nextInt();
        try {
            check(score);
            System.out.println(score);
        } catch (MyException e) {
            e.printStackTrace();
        }
    }
}

```

输出结果：

![image-20230519212217352](D:\My tepora note\Java期末重点.assets\image-20230519212217352.png)



## 5. 字节流和字符流的区别，文件输入输出流

**字节流和字符流的区别：**

字节流(Byte Stream)：字节流处理的是二进制数据，每次从流中读取或写入一个字节，适用于处理图像、音频等二进制文件，如InputStream、OutputStream、FileInputStream、FileOutputStream类。

 字符流(Character Stream)：字符流则处理的是字符数据，每次从流中读取或写入一个字符，适用于处理文本文件，如Reader、Writer、FileReader、FileWriter类。



**文件输入输出流：**

Java 中常用的文件输入和输出流有如下几种：

1. FileInputStream 和 FileOutputStream：用于读写字节数据，在读取和写入时一次读写一个字节或指定长度的字节数组，可以用于操作所有类型的文件。

2. FileReader 和 FileWriter：用于读写字符数据，是基于字符集的，可以处理 Unicode 编码的字符。使用时要注意字符编码的问题。

3. BufferedReader 和 BufferedWriter：用于缓存读写数据流，提高读写效率。BufferedReader 可以一次读取较多字节的内容放到缓存中，InputStreamReader 组合 BufferedReader 更易于解决乱码问题；BufferedWriter 可以将多个小块的数据写入一个大块中，OutputStreamWriter 组合 BufferedWriter 更易于提供自动换行和定位功能，同时也比较易于解决乱码问题。

4. ObjectInputStream 和 ObjectOutputStream：用于读写对象数据，可以将 Java 对象转化为字节序列并进行持久化存储，也支持将其读取出来后还原成原来的 Java 对象。

5. DataInputStream 和 DataOutputStream：用于读写基本数据类型和字符串类型，可以方便地将数据以二进制形式串行化或反串行化，但不支持处理 Unicode 编码的字符。

需要注意的是，在使用完以上 IO 流之后，一定要记得关闭资源，否则会导致资源泄漏以及占用过多系统资源，甚至导致程序崩溃。可以采用 try-catch-finally 或者 try-with-resources 方式来处理关闭资源的操作。



## 6. TCP和UDP的异同点、TCP服务端和客户端的通信

**TCP和UDP的异同点：**

TCP（传输控制协议，Transmission Control Protocol）和UDP（用户数据报协议，User Datagram Protocol）都是基于IP协议的网络编程模型。它们在Java中实现的方式主要有Socket类（TCP）和DatagramSocket类（UDP）。以下是它们之间的相同点和不同点：

相同点：

1. 都基于IP协议：TCP和UDP都使用IP地址和端口号来标识通信的两个端点，提供计算机之间的通信功能。
2. 数据传输方式：无论是TCP还是UDP，它们都通过发送和接收数据包进行数据交换。

不同点：

1. 连接性：TCP是面向连接的协议，通信双方在数据传输前需要建立可靠的连接。而UDP是无连接的协议，发送数据时不需要建立连接。
2. 可靠性：TCP提供了可靠的数据传输，通过确认和重传机制确保数据完整无误地到达目的地。而UDP则不保证可靠性，数据可能会丢失或者乱序，但传输速度更快。
3. 顺序性：TCP保证数据包按照发送顺序到达接收端，因此可以处理复杂的数据流。UDP无法保证



**TCP服务端和客户端的通信：**

```java
// Client端
package com.hao.homework.episode17_TCP;

import java.io.IOException;
import java.io.OutputStream;
import java.net.InetAddress;
import java.net.Socket;
import java.util.Scanner;

@SuppressWarnings("all")
public class Client {

    public static void main(String[] args) {

        Socket socket = null;
        OutputStream outputStream = null;
        try {
            InetAddress inetAddress = InetAddress.getLocalHost();
            int port = 8989;
            socket = new Socket(inetAddress, port);
            outputStream = socket.getOutputStream();
            Scanner scanner = new Scanner(System.in);
            String str = scanner.nextLine();
            outputStream.write(str.getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (outputStream != null)
                    outputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if (socket != null)
                    socket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }


    }
}

// Consultant端
package com.hao.homework.episode17_TCP;

import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.net.ServerSocket;
import java.net.Socket;
import java.nio.charset.Charset;

@SuppressWarnings("all")
public class Consultant {

    public static void main(String[] args) {

        ServerSocket serverSocket = null;
        Socket socket = null;
        InputStream inputStream = null;
        try {
            int port = 8989;
            serverSocket = new ServerSocket(port); // 创建ServerSocket对象时千万要记得传port！！！
            socket = serverSocket.accept();
            System.out.println("连接成功！");

            inputStream = socket.getInputStream();
            InputStreamReader isr = new InputStreamReader(inputStream);
            char[] buffer = new char[1024];
            isr.read(buffer);
            System.out.println(buffer);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (inputStream != null)
                inputStream.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if (socket != null)
                    socket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if (serverSocket != null)
                    serverSocket.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }


    }
}

```





## 7. 类和对象，属性和方法（面向对象）

**面向对象的三大特征：**

封装、继承、多态。



**各特性待解决的问题：**

封装：

1. 防止误操作：通过将成员变量私有化，人为地限制对数据的访问和修改。这样可以避免在其他部分意外地改变数据从而导致系统出现不必要的错误。
2. 提高代码复用性和可维护性：可以将一些与具体实现无关的逻辑封装在类中，在其他地方使用时只需要调用相应的方法即可，而不需要重复编写代码并细节处理每个属性。
3. 实现信息隐藏：通过封装，可以将内部细节隐藏起来，暴露给外界只是接口，而不影响内部实现。这样可以减少耦合，提高安全性，同时也便于更改内部实现。



继承：

1. 提高代码复用性：通过继承，子类可以直接使用父类的属性和方法，避免了代码的重复编写。
2. 实现多态性: 继承机制使得子类可以重写自己的父类的方法并提供不同的实现。当一个对象被声明为父类类型时，它可以引用子类的对象，从而在运行时动态地确定调用哪个方法，实现多态性。
3. 简化程序结构：通过将通用属性和方法定义在父类中，从而简化了程序的结构，并且代码易于维护。



多态：

1. 简化了代码：通过将通用的父类属性和方法定义在父类中，避免了代码过于复杂的情况发生。
2. 增加了程序的灵活性：基于多态实现时，程序在编译期间并不知道具体调用哪个子类方法，在运行时才会动态地确定调用哪个实现。这样就增加了程序的灵活性。
3. 提高了代码的可扩展性：当需要增加新的子类时，只要他符合原先定义的接口或继承结构，就可以无需更改原有代码而进行扩展。这种特性对软件工程师来说是极其重要的。



## 8. 文件的拷贝

**非目录文件的拷贝：**

```java
package com.hao.io;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

@SuppressWarnings("all")
public class FileCopy {

    public static void copyFile(File src, File des) {
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream(src);
            fos = new FileOutputStream(des);
            byte[] cBuffer = new byte[1024];
            int len;
            while ((len = fis.read(cBuffer)) != -1) fos.write(cBuffer, 0, len);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fos != null)
                    fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if (fis != null)
                    fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }

    public static void main(String[] args) {

        File src = new File("C:\\Users\\孙皓\\Desktop\\review.txt");
        File des = new File("C:\\Users\\孙皓\\Desktop\\review_copy.txt");

        copyFile(src, des);
        System.out.println("文件复制完成！");
    }
}

```



**目录的拷贝：**

```java
package com.hao.io;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

@SuppressWarnings("all")
public class DirectoryCopy {

    public static void copyDir(File src, File des) {
        if (!src.exists() || !src.isDirectory()) throw new RuntimeException("文件不存在或不是目录！");
        if (!des.exists()) des.mkdir();

        File[] files = src.listFiles();
        for (File file : files) {
            if (file.isDirectory()) copyDir(file, new File(des, file.getName()));
            else copyFile(file, new File(des, file.getName()));
        }
    }

    public static void copyFile(File src, File des) {
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream(src);
            fos = new FileOutputStream(des);
            byte[] cBuffer = new byte[1024];
            int len;
            while ((len = fis.read(cBuffer)) != -1) fos.write(cBuffer, 0, len);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fos != null)
                    fos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if (fis != null)
                    fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

    }


    public static void main(String[] args) {

        File src = new File("C:\\Users\\孙皓\\Documents\\军训");
        File des = new File("C:\\Users\\孙皓\\Documents\\军训_copy");

        try {
            copyDir(src, des);
            System.out.println("文件复制成功！");
        } catch (Exception e) {
            System.out.println("文件复制失败！");
        }
    }
}
```



## 9.  聊天室，BufferedReader与BufferedWriter的应用



## 10. 理工超市支付

switch-case中：

```java
case 7:
                    println("结算");
                    if(!isLogin) println("请先登录！");
                    else{
                        shoppingCart = cartDada.get(currUser.getName()); // 获取当前用户的购物车对象
                        if (shoppingCart == null) shoppingCart = new ShoppingCart(); // 防止购物车变量为空
                        if (shoppingCart.isEmpty()) println("当前购物车为空，请先添加宝贝到购物车中再进行结算吧！");  // 购物车为空则只输出提示信息
                        else pay(shoppingCart, input); // 购物车不为空则进行结算
                        cartDada.put(currUser.getName(), shoppingCart); // 更新购物车集合中当前用户购物车的状态，原理是Map存入的数据在key已存在的情况下就更新value
                    }

                    continue;
```



pay()方法：

```java
public static void pay(ShoppingCart shoppingCart, Scanner input) {
        if (!shoppingCart.canPay()) { // 判断购物车中是否所有商品都可以支付
            println("您的购物车部分商品库存不足，现为你清空库存不足的商品。");
            shoppingCart.getValidCart(); // 去除购物车中数量不足的商品，让剩余商品都可以支付
        }
        if (shoppingCart == null) {
            println("当前购物车为空，先去添加宝贝进来吧。");
            return;
        }
        println("您购买了以下物品：");
        shoppingCart.printCart(); // 展示购物车中含有的东西
        println("确定支付？输入1表示确认，输入0表示取消");
        int confirm = input.nextInt();
        while (confirm != 1 && confirm != 0)
        {
            print("输入不合法，请重新输入：");
            confirm = input.nextInt();
        }
        if (confirm == 0) return;
        double totalPrice = shoppingCart.getPayPrice(); // 获取购物车中商品的总价

        double reducePrice = getReducePrice(input, totalPrice); // 获取抵扣金额
        if (reducePrice > 0) {    // 只有抵扣金额大于0才说明用户选择了使用积分进行抵扣
            println("您共消费" + totalPrice + "元，现积分抵扣" + reducePrice + "元，实需付" + (totalPrice - reducePrice) + "元");
            totalPrice = totalPrice - reducePrice;
        } else {  // 抵扣金额等于0说明未进行任何抵扣
            println("您需支付：" + totalPrice + "元");
        }
        print("请输入支付金额：");
        double userPay = input.nextDouble();
        while (userPay < totalPrice)
        {
            print("您支付的金额不足，请重新输入：");
            userPay = input.nextDouble();
        }

        println("支付成功！找零" + (userPay - totalPrice) + "元");

        shoppingCart.clean();

        // 会员用户购买完商品后会获取一定的会员积分
        if (currUser instanceof CustomerMember) {
            ((CustomerMember)currUser).addMemberPoint(totalPrice);
            System.out.println("总积分为：" + ((CustomerMember)userManage.getUserByName(currUser.getName())).getMemberPoint());
        }

        // 记录下交易时间
        OrderStatistic.add(new Date());
    }
```



pay方法中调用的shoppingCart.canPay()方法：

```java
// 判断购物车中是否所有商品都支持支付，即判断是否有商品数量不足，若有则返回false
    public boolean canPay() {
        boolean flag = true;
        for (ShopItem item : items) {
            Goods goods = item.getGoods();
            if (goods.getCount() < item.getAmount()) { // 只要存在某一个商品的数量不足，那么就返回false，说明此购物车不能直接结算
                flag = false;
                break;
            }
        }
        return flag;
    }
```



pay方法中调用的shoppingCart.getValidCart()方法：

```java
// 去除购物车中所有数量不足的商品
    public void getValidCart() {
        Iterator<ShopItem> iterator = items.iterator();
        while (iterator.hasNext()) {
            ShopItem item =  iterator.next();
            if (item.getAmount() > item.getGoods().getCount()) {
                iterator.remove();
            }
        }
    }
```



pay方法中调用的shoppingCart.printCart()方法：

```java
public void printCart() {
        double totalPrice = 0;
        for (ShopItem item : items) {
            item.show();
            totalPrice += item.payPrice();
        }
        println("总计：" + totalPrice + "元");
    }
```



pay方法中调用的shoppingCart.getPayPrice()方法：

```java
// 获取购物车中所有商品的总价
    public double getPayPrice() {
        double totalPrice = 0;
        for (ShopItem item : items) {
            totalPrice += item.payPrice();
        }
        return totalPrice;
    }
```



pay方法中调用的getReducePrice()方法：

```java
// 获取抵扣价格（使用会员积分进行抵扣），若为0则说明未进行任何抵扣
    public static double getReducePrice(Scanner input, double totalPrice) {
        int reducePrice = 0;
        if (!(currUser instanceof CustomerMember)) return reducePrice;
        int menberPoint = ((CustomerMember)currUser).getMemberPoint();
        if (menberPoint < 1000) return reducePrice;
        print("您是会员，现有积分：" + menberPoint + "积分，每1000积分可抵扣10元，是否使用？输入1表示是，0表示否：");
        int confirm = input.nextInt();
        while (confirm != 1 && confirm != 0)
        {
            print("输入错误，请重新输入：");
            confirm = input.nextInt();
        }
        if (confirm == 0) return reducePrice;
        int maxPoint = (int)totalPrice * 100;
        print("请输入需要使用的积分数，应为1000的整数倍：");
        int point = input.nextInt();
        while (point < 1 || (point % 1000 != 0) || point > maxPoint || point > menberPoint)
        {
            print("输入不合法，请重新输入：");
            point = input.nextInt();
        }
        reducePrice = point / 100;
        ((CustomerMember)currUser).userMemberPoint(point); // 使用积分抵扣金额后需要更新积分数量
        return reducePrice;
    }
```



## 11.  Treeset遍历，实现 Comparable 接口并重写compareTo()方法

**TreeSet自定义构造器及遍历：**

```java
package com.hao.hspjava.gather;

import java.util.Comparator;
import java.util.Iterator;
import java.util.TreeSet;

@SuppressWarnings("all")
public class TreeSetTest {

    public static void main(String[] args) {

//        TreeSet<String> treeSet = new TreeSet<>(); // 自然排序，对于字符串来说就是自动调用String类型的compareTo()方法，若compareTo()返回0则说明两个对象相等，不添加；若不为0则按顺序添加进集合。
        TreeSet<String> treeSet = new TreeSet<>(new Comparator<String>() { // 根据传入的Comparator匿名内部类里的compare()方法进行排序，若compare()方法返回0则说明这两个对象相等，就不添加；若返回不为0，则按排序添加进集合中。
            @Override
            public int compare(String o1, String o2) {
                // 按字符串长度进行排序，o1是第一个参数，o2是第二个参数。
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



**实现Comparable接口并重写compareTo方法：**

```java
package com.hao.review;

public class ComparableTest {

    public static void main(String[] args) {

        Test t1 = new Test("java");
        Test t2 = new Test("c");
        System.out.println(t1.compareTo(t2));

    }
}

class Test implements Comparable {

    private String str;

    public Test(String str) {
        this.str = str;
    }

    @Override
    public int compareTo(Object o) {
        return this.str.length() - ((Test)o).str.length();
    }
}

```



## 12. 多线程卖票

三个窗口同时卖票，票数由构造器传入。

```java
package com.hao.review;

@SuppressWarnings("all")
public class SaleTickets {

    public static void main(String[] args) {
        Sale sale = new Sale(100); // 票数由构造器传入
        Thread t1 = new Thread(sale);
        Thread t2 = new Thread(sale);
        Thread t3 = new Thread(sale);

        t1.start();
        t2.start();
        t3.start();
        try {
            t3.join();
        } catch (InterruptedException e) {  // 调用t3.join()方法是为了让main方法在启动了t3线程后进入阻塞状态，直到t3运行完毕，这样就能保证在最后卖完所有的票了再输出语句
            e.printStackTrace();
        }

        System.out.println("票数已告罄");

    }


}

class Sale implements Runnable {

    int num;

    public Sale(int num) {
        this.num = num;
    }

    @Override
    public void run() {
//        synchronized (this) { // 错误的，这样某一个线程得到执行权后就一直在while中出不来了，直到卖完了所有的票
            while (true) {
                synchronized (this) { // 正确的，synchronized只包住了卖票和更新票数的操作，执行完后别的线程也可以进入while循环卖票
                    if (num > 0) {
                        System.out.println(Thread.currentThread().getName() + "卖出编号为" + num + "的票");
                        num--;
                    } else break;
                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {  // 无关紧要，sleep只是让其他线程得到CPU调度的机会更大而已
                        e.printStackTrace();
                    }
                }
            }
//        }
    }
}
```



## 13. 到期日计算

```java
	Date validPeriod = new Date(); // 获取当前系统时间
    Calendar calendar = Calendar.getInstance();
    calendar.setTime(validPeriod); // 将当前存储的系统时间赋给calendar变量
    calendar.add(Calendar.DAY_OF_MONTH, 3); // 将目前的时间添加保质期天数，所得即是有效期。此处第一个参数为Calendar.DAY_OF_MONTH，表明第二个参数的数字指增加的天数
    //calendar.add(Calendar.DAY_OF_MONTH, -1); // 令面包过期
    validPeriod = calendar.getTime(); //返回有效期到validPeriod对象变量中，实际上单位是毫秒
```

