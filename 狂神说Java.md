说明：此笔记是看《狂神说Java》时记录的，但是却不仅仅有视频里的内容，还融合了我自己的思想以及上网寻找的解释。



# 一. Java入门

只记不熟悉的内容。



## 常见的Dos命令







# 二. Java基础

一些很基础的我就不记笔记了，就记一些小细节的东西，学到后面之后再认真记笔记。



## 数据类型

long类型要在数据后面加个'L'或'l'。因为不加的话会默认为int类型的，所以必须加。

float类型要在数据后面加个'F'或'f'。因为不加的话会默认为double类型的，所以必须加。

最好都写大写的，因为'l'很容易看成数字'1'，所以最好两个都写大写的，养成良好的习惯。

任何数据类型的变量在定义时都要初始化，不然编译器会报错！



查看数据类型范围的小技巧：

在idea里打出要查看的数据类型，第一个字母要大写。如：Integer、Byte。这些会自动提示，只需要选择即可。然后ctrl+鼠标左击，就能进入`java.doc`页面查看了，只不过是以指数形式出现的，如：Integer为`-2<sup>31</sup>`。



## 数据类型拓展

整数拓展：

进制：二进制以`0b`开头，十进制没有前缀，直接敲数字，八进制以`0`开头，十六进制以`0x`开头。



浮点数拓展：

浮点数的能表现的字长是有限的，也是离散的，存在舍入误差，结果只是个大约数，接近但不等于。所以**最好完全避免使用浮点数进行比较！！！**比如银行存钱，应该使用`BigDecimal`这个数学工具类。



字符拓展：

所有的字符本质还是数字。重要的ASCII码：48->0  	 65->A 	 97->a

注：**\u不能出现在除了转义字符以外的任何地方！**只能用于如：`char c='\u0061'`这样的。



## 类型转换

强制类型转换：（类型）变量名	高->低	eg: int -> byte

自动类型转换：低->高	eg: byte -> int

这里的"高"与"低"指的是数据范围的高低。且浮点数一定高于整数。

自动类型转换：(低) byte,short,char -> int -> long -> float -> double  (高)



注意点：

1. 不能对布尔值进行转换
2. 不能把对象类型转换为不相干的类型
3. 在把高容量转换为低容量的时候，要用强制转换
4. 转换的时候可能存在内存溢出，或者精度问题



小技巧：JDK7新特性，数字之间可以用下划线分割，不会对数字本身造成任何影响

eg：`int money = 10_0000_0000;`与`int money = 1000000000;`是一模一样的



操作比较大的数的时候，注意溢出问题

```java
public class Demo {
    public static void main (String[] args) {
        int money = 10_0000_0000;
        int years = 20;
        int total = money * years; //-1474836480,计算的时候溢出了 
        long total2 = money * years; //输出也是-1474836480，因为自动类型转换要先按照原先的类型计算，算完了再转换。原先money * years是int类型，计算会溢出，溢出了之后再转成long，结果当然不变了。
        long total3 = (long)money * years;//正确输出20000000000，因为money先被强制转换成long，long和int在一起计算会自动将那个int先转成long后再进行计算。写成money * ((long)years)也一样。
    }
}
```



## 变量作用域

- 类变量
- 实例变量
- 局部变量

```java
public class Variable {
    
    //class下边和方法上边这块区域都在类里面、方法外面，而不是在方法里面 
    
    
    static int allclicks = 0; //类变量
    String str = "hello world"; //实例变量
    
    public void method() {
        
        //类似这里才是方法里面
        
        
        int i = 0; //局部变量
    }
}
```



局部变量：必须声明和初始化。作用域和生存期只在定义该变量的**方法**内，即两个大括号括起来的区域。

实例变量：从属于对象；声明后可以不进行初始化，如果不进行初始化，它的值会变成这个类型的默认值。

​	除了基本类型（byte, short, int , long, float, double, char, boolean），其余的默认值都是null。基本类型一般都与0有关，eg：整数为0，浮点数为0.0，字符串变量为十六位的0：u0000，布尔类型为false。

类变量：用static关键词修饰

```java
public class Demo {

    //类变量：static
    static double salary = 2500;

    //类   属性：变量

    //实例变量：从属于对象，在类里面，方法外面；如果不进行初始化，这个类型的默认值 0 0.0
    //布尔值：默认是false
    //除了基本类型，其余的默认值都是null
    String name;
    int age;

    static  {

        // 代码块
    }

    //main方法
    public static void main(String[] args) {
        //局部变量：必须声明和初始化值
        int i = 10;
        System.out.println(i);

        //变量类型   变量名字 = new Demo();
        Demo demo = new Demo();
        System.out.println(demo.age);
        System.out.println(demo.name);

        //类变量  static
        System.out.println(salary);

    }

    //其他方法
    public  void add() {

    }
}

```



## 命名规范

- 所有变量、方法、类名：**见名知义**。使用英文，不能用中文（因为太low了）
- 类成员变量：首字母小写和驼峰原则（除了第一个单词以外，后面的单词首字母大写）：monthSalary
- 局部变量：首字母小写和驼峰规则
- 常量：大写字母和下划线：MAX_VALUE
- 类名：首字母大写和驼峰原则：Man，GoodMan
- 方法名：首字母小写和驼峰原则：run()，runRun()
- 项目名：全小写和下划线：jxust_supermarket_system



## 基本运算符

- 整数运算中只要有一个变量的类型时long， 那么所有变量都会先自动转换成long然后再进行运算。double同理，只要有一个为double，那么所有变量都会先自动转换成double然后再进行运算。
- 除了long类型的整数运算，都会自动转换成int，如int, short, byte混合在一起做运算会先全部转换成int型再进行运算。

```java
public class Demo02 {
    public static void main(String[] args) {
        long a = 123123123123123123L;
        int b = 123;
        short c = 10;
        byte d = 8;

        System.out.println(a+b+c+d); // long
        System.out.println(b+c+d); // int
        System.out.println(c+d); // int

        double e = 3.14;
        System.out.println(b+e); // double
    }
}

```



## 字符串连接符

字符串连接符：`+`

只要`+`前面出现了`String`类型也就是字符串类型，那么它就会把剩下的也就是右边的按照字符串进行处理，如和前面拼接在一起。除非给后面要参与运算的式子打上括号，因为括号的优先级最高。

如果字符串在后面，那么前面就会正常进行运算，然后再把后面的字符串和前面拼接起来。

```java
package base;

public class Demo04 {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;
        System.out.println(""+a+b); // 1020
        System.out.println(a+b+""); // 30
        System.out.println(""+(a+b)); // 30
    }
}

```

总而言之，字符串后面再跟的加号就会把后面前面计算的结果以及所有东西直接当作字符串连接起来，变成一个字符串。

```Java
package com.hao.test;

public class casual {

    public static void main(String[] args) {
        int a = 1;
        int b = 2;
        int c = 3;
        int d = 4;
        System.out.println(a + b + "and" + c + d); //输出3and34 因为a+b在"and"前面，会计算为3，然后遇到"and"，就变为字符串”3and“了，然后后面的3和4都被当做字符串直接拼接起来了，所以最终输出3and34。
    }
}

```







## 包机制

为了更好地组织类，Java提供了包机制，用于区别类名的命名空间。

包语句的语法格式为：

```java
package pkg1[.pkg2[.pkg3...]];
```

**包语句一定要位于代码的开头，不然会立马报错！**



**一般利用公司域名倒置作为包名。**

如：百度为com.baidu.www

相同公司的不同产品开发也能更好地分类管理

如：

![image-20230228213921422](狂神说Java.assets/image-20230228213921422.png)

我个人的包就可以取名为：com.hao，然后再在hao里建包用来记录各种东西，比如homework、kuangstudy、test，这样就可以很好地管理代码。而且虽然在不同的包里可以建同样名字的类，但是最好不要这样做，因为可能以后导包的时候会出问题。



为了能够使用某一个包的成员，我们需要在Java程序中明确导入该包。使用"import"语句可完成此功能。

```java
import package1[package2...].(classname|*);
```

如:

```java
import java.util.Scanner;
```



包的本质就是文件夹！

通配符：`.*`。意思是导入这个包下所有的类

如果一个包里的类有很多要用的，一个一个导又太麻烦，那么可以选择用通配符。

如：

```java
import java.util.*
```



## JavaDoc

javadoc命令是用来生成自己API文档的

参数信息：

- @autho 作者名
- @vers 版本号
- @since 指明最早使用的JDK版本（开发时用的JDK版本）
- @param 参数名
- @return 返回值情况
- @throws 异常抛出情况

这些东西是写在文档注释里的。

文档注释可以加在类上，也能加在方法上。写在方法上时，如果写在实例变量下面就会自动生成东西。

文档注释语法：

```java
/**+回车
完整版：
/**
 * @author kuangshen
 * @version 1.0
 * @since 1.8
 *...
 */
```

使用命令行生成JavaDoc文档：

```
javadoc -encoding UTF-8 -charset UTF-8 类名
```

**注意空格！！！**



# 三. 面向对象

面向对象（OOP）的本质就是：**以类的方式组织代码，以对象的方式组装（封装）数据**。

面向对象的三大特性：封装、继承、多态。

对象，是具体的事物；类，是抽象的，是对对象的抽象。



## 静态与非静态方法的相互调用

使用同一个包下的不同的类中的方法可以不导包，若是一个类中静态的方法，可以在另一个类中通过`类名.方法名`的方法来调用。若是非静态的，则需要先创建一个对象，再通过对象调用，同样不需要导包，因为是同一个包。



在同一个类下的两个方法若都是非静态的和都是非静态的，则可以相互调用。

```java
package com.hao.test;

public class Demo03 {
    public static void main(String[] args) {
        ...
    }
    
    public void a() {
        b();
    }
    
    public void b() {
        a();
    }
    
}

以及：
    package com.hao.test;

public class Demo03 {
    public static void main(String[] args) {
        ...
    }
    
    public static void a() {
        b();
    }
    
    public static void b() {
        a();
    }
    
}
上面两种情况都是合法的
```

若其中一个是静态，另一个非静态，那么不能同时互相调用，只能非静态的调用静态的，静态的不能调用非静态的。因为**==被static修饰的内容会随着类的加载而加载，所以静态化的内容可以不实例化就直接调用！==**static和类一起加载，非实例化的要实例化之后才存在，也就是创建了对象。故被static修饰的方法早早地出现，但是非实例化的还没出现，静态的自然不能调用非静态的了。

```java
package com.hao.test;

public class Demo03 {
    public static void main(String[] args) {
        ...
    }
    
    public  void a() {
        b();
    }
    
    public static void b() {
        a(); // 报错
    }
    
}

```



## 类与对象的创建



从现在开始养成一个习惯，不要在每一个类中都加上main方法了，一个项目中只应该有一个main方法，可以创建一个大的启动类或者测试类`Application`，里面包含main方法，是唯一的入口。

类里面只有两个东西：属性和方法。

类是抽象的，不能直接使用，需要实例化。通过`new`关键词来实例化，类实例化后会产生一个自己的实例化对象，对象就是类的具体的实例。创建了对象了说明该抽象的类落实了，就可以使用其中的方法与属性了。

类就相当于制造飞机的图纸，是一个模板，是负责创建对象的。对象就相当于用图纸制造出来的飞机，是类的实例化，即从图纸变为了现实。



`this`指代当前这个类，用于本类中的方法使用本类中的属性。



使用`new`关键字创建对象：使用`new`关键字创建的时候，除了分配内存空间外，还会给创建好的对象进行默认的初始化，以及对类中构造器的调用。初始化是因为类是一个模板，模板里的东西如果被写死了那还叫模板嘛，难不成所有的名字都叫"小明"？



## 构造器详解

类中的构造器也称为构造方法，是在进行创建对象的时候必须要掉用的。并且构造器有以下两个特点：

1. 必须和类的名字相同
2. 必须没有反回类型，也**不能**写`void`！写了就不是构造方法了！



一个类即使什么都不写，它也会存在一个方法，即''默认构造方法"，也是默认的构造器。



有参构造：一旦定义了有参构造，无参就必须显示定义。



构造器的作用：

1. 使用`new`关键字，本质是在调用构造器（构造方法）。
2. 用来初始化值，若有this语句赋值那就会赋由this赋的值，不然就赋默认值。



`this.啥啥啥`是代表当前类的，等号右边的值一般是参数传进来的。



快捷键：`Alt + Insert`   会生成构造器（constructor），有参无参自己选择。



## 封装

高内聚，低耦合。

高内聚：类的内部数据操作细节自己完成，不允许外部干涉     									低耦合：仅暴露少量的方法给外部使用。



封装（数据的隐藏）：

通常，应禁止直接访问一个对象中数据的实际表示，而应通过操作接口来访问，这称为"信息隐藏"。



**属性私有，get/set**



封装的意义：

1. 提高程序的安全性，保护数据
2. 隐藏代码的实现细节
3. 统一接口
4. 系统可维护性增加了

```java
Student类：
package com.hao.oop.Demo02;

//类 private:私有
//一般来说，私有是对于属性而言，方法一般不设置私有
public class Student {
    //属性私有
    private String name; //名字
    private int id; //学号
    private char gender; //性别
    private int age; //年龄

    //提供一些可以操作这个属性的方法！
    //提供一些public的get、set方法

    //get 获得这个属性
    public String getName() {
        return this.name;
    }
    //set 给这个属性赋值
    public void setName(String name) {
        this.name = name;
    }

    //Alt + insert 可以选择自动生成get和set的方法

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public char getGender() {
        return gender;
    }

    public void setGender(char gender) {
        this.gender = gender;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        if (age > 120 || age < 0) this.age = 3; //不合法
        else this.age = age;
    }
}

Application主启动类：
package com.hao.oop.Demo02;

public class Application {

    public static void main(String[] args) {
        Student s1 = new Student();
        String name = s1.getName();
        s1.setName("sunhao");
        System.out.println(name); //null -> 初始值
        System.out.println(s1.getName()); //sunhao -> 传入值

        s1.setAge(999); //不合法的
        System.out.println(s1.getAge());
    }
}

```



## 继承

继承的本质是对某一批类的抽象，从而实现对现实世界更好的建模。



**extends**的意思是"扩展"。子类是父类的扩展。



继承是类和类之间的一种关系。除此之外，类和类之间的关系还有依赖、组合、聚合等。

继承关系的两个类，一个为子类（派生类），一个为父类（基类）。子类继承父类，使用关键字`extends`来表示。

子类和父类之间，从意义上讲应该具有"is a"的关系。如：父类：Person 子类：Student ，则 Student is a Person。

```java
Person类：
package com.hao.oop.demo03;

//Person  人 ：父类
public class Person {

}

Student类：
package com.hao.oop.demo03;

//Student is 人 : 派生类、子类
public class Student extends Person {
    
}

Teacher类：
package com.hao.oop.demo03;

//Teacher is 人 : 派生类、子类
public class Teacher extends Person{

}


```



```java
Person类：
package com.hao.oop.demo03;

//Person  is  人
public class Person {

    //public 可以直接使用
    public int money = 10_0000_0000;

    //private 不能直接使用  一般属性才是私有的
    private String wife = "yyc";  // 父亲私有的东西凭什么给儿子用啊

    //default 不写就是默认的，也可以直接使用
    int home;

    //protected
    protected int carNumber;

    public void say () {
        System.out.println("说了一句话");
    }
}

Student类：
package com.hao.oop.demo03;

//Student is 人 : 派生类、子类
//子类继承了父类，就会拥有父类的全部方法和可直接调用的东西。
public class Student extends Person {
    
    //Ctrl + H 一键打开族谱
}
```

快捷键：`Ctrl + H ` 一键打开族谱。



Java中类只有单继承，没有多继承！即一个儿子只能有一个爸爸，但是一个爸爸可以有多个儿子。



Object类：

在Java中，所有的类，都默认直接或间接继承Object类。所以不需要显式继承Object类。



this 与 super 的语法：

```
成员变量：this.本类成员变量			super.父类成员变量
成员方法：this.本类成员方法()			super.父类成员方法()
构造方法：this(参数)				   super.(参数)
```



super：

1. 使用super调用父类构造方法时，必须放在构造方法的第一个，也就是最上面，不然会有红线报错。
2. super必须只能出现在子类的方法或者构造方法中。
3. super和this不能同时调用构造方法！因为他们俩都要求放在第一个，但是又不能同时放在第一个，所以必然有一个会报错。并且`this();`表示调用本类的构造方法，也就是说这句话相当于`public void Student() {}` ，若在构造方法里放`this();` 那么就会变成“递归构造器调用”调用，并且构造方法下会有红线，提示`Recursive constructor invocation` 。所以这个地方无论如何都不能有`this();` 所以要么不写`super();` 要么就写在第一行。



```java
Person类：
package com.hao.oop.demo03;

//Person  is  人 : 父类
public class Person {

    public Person() {
        System.out.println("Person无参执行了");
    }

    protected String name = "sunhao";

    //私有的东西无法被直接使用，super也不行！
    public void print () {  //这里若写成private就立马报错
        System.out.println("Person");
    }

}

Student类：
package com.hao.oop.demo03;

//Student is 人 : 派生类、子类
//子类继承了父类，就会拥有父类的全部方法！
public class Student extends Person {

    public Student() {
        //通过主启动类构造student对象的输出：先输出“Person无参执行了”后输出“Student无参执行了”可知Student的无参构造器内有一句隐藏代码，并且该隐藏代码默认调用了父类的无参构造 --> super();
        super(); //调用父类的构造器，若显示地定义出来了，那就必须要在子类构造器的第一行。
        System.out.println("Student无参执行了");
    }

    private String name = "haoge";

    public void print () {
        System.out.println("Student");
    }

    public void test1 () {
        print(); //Student
        this.print(); //Student
        super.print(); //Person
    }

    public void test (String name) {
        System.out.println(name); //孙皓
        System.out.println(this.name); //haoge
        System.out.println(super.name); //sunhao
    }

}

Application类：
package com.hao.oop.demo03;

public class Application {

    public static void main(String[] args) {

        Student student = new Student(); //依次输出：Person无参执行了  Student无参执行了
        student.test("孙皓");
        student.test1();
    }
}

```







super VS this：

1. 代表的对象不同：

- this：代表本身调用者这个对象，谁调用了this，那么就表示谁。
- super：代表父类对象的引用

2. 使用前提不同：

- this：没有继承也可以使用
- super：只能在继承条件下才可以使用

3. 调用构造方法不同：

- this：`this();` 调用的是本类的构造
- super：`super();` 调用的是父类的构造



## 重写

重写：子父类中出现”方法声明一模一样的两个方法“，则这两个方法之间的关系是”方法重写（Override）“



特点：

1. 必须是子父类关系
2. 子类方法和父类方法一样



应用：当子类对父类方法不满意时，进行重写，可以覆盖从父类继承过来的方法。



注：重写都是方法的重写，与属性无关！



创建一个对象的方法为：`类名 对象名 = new 类名();` 。此过程体现了对象是哪个类定义的、是由哪个类为模板创建的对象。对象是哪个类定义的就说明该对象属于哪个类，说明该对象可以使用该类的方法和属性。对象是由哪个类为模板创建的，说明该对象可以调用该模板类的非静态方法，也就是未被static修饰的方法。如：`A a = new A();` 表明a属于A类，是以A类为模板创建的对象。而当A类继承B类时，`B a = new A();` 说明a虽然是由A为模板模板 new 出来的对象，当时a其实是属于B类的，a能用B里所有的属性和方法，却只能用A类中对B类重写的方法。

注：子类中重写父类方法时，要么都加 static 要么都不加，即 static 只能用 static 覆盖。



```java
B类：
package com.hao.test01;

public class B {

    public int money = 100;
    public String name = "b";

    public  B() {
        System.out.println("this is B constructor");
    }

    public void bprintv() {
        System.out.println("this is B void print");
    }

    public static void bprints() {
        System.out.println("this is B static print");
    }

    public  static void print() {
        System.out.println("this is B print");
    }

    public void printn() {
        System.out.println("this is B printn");
    }
}

A类：
package com.hao.test01;

public class A extends B {

    public int money = 1000;
    public String name = "a";

    public A() {
        System.out.println("this is A constructor");
    }

    public static void print() {
        System.out.println("this is A print");
    }

    @Override
    public void printn() {
        System.out.println("this is A printn");
    }

}

Application类：
package com.hao.test01;

public class Application {

    public static void main(String[] args) {

        //父类的引用指向了子类
        B a = new A(); //先输出 “this is B constructor” 后输出 “this is A constructor” 因为A类继承了B类，A类的构造方法里第一句默认了super();这句话，且构造方法也是和方法一样按顺序从上往下走，所以先输出“this is B constructor”，然后回到A类的构造方法执行super();的下一句，再输出 “this is A constructor”
        System.out.println(a.name); //b  a属于B类
        System.out.println(a.money); //100  a属于B类
        a.bprintv(); //this is B void print  a属于B类
        a.bprints(); //this is B static print  a属于B类，只会调用B类的静态方法
        a.print(); //this is B print  a属于B类，即使A类也有同名的print静态方法，也只会调用B类的static方法，且不属于重写，因为有static修饰 
        a.printn(); //this is A printn @Override，重写是只调用子类中重写的方法，父类的方法被子类的覆盖了

        A a1 = new A(); ////先输出 “this is B constructor” 后输出 “this is A constructor” 因为A类继承了B类，A类的构造方法里第一句默认了super();这句话，且构造方法也是和方法一样按顺序从上往下走，所以先输出“this is B constructor”，然后回到A类的构造方法执行super();的下一句，再输出 “this is A constructor”
        System.out.println(a1.money); //1000  a1属于A类
        System.out.println(a1.name); //a   a1属于A类
        a1.print(); //this is A print  因为a是A类的，所以调用A类的static方法
        a1.printn(); //this is A printn @Override，重写是只调用子类中重写的方法，父类的方法被子类的覆盖了
        a1.bprintv(); //this is B void print 继承来的方法
        a1.bprints(); //this is B static print 继承来的方法
        
        
    }
}

```



**当父类的方法被static修饰时，子类必须也写static才能将其覆盖，不然会报错，并且这不算重写。而且父类和子类都不能一方写static而另一方不写，因为这样子类无法重写父类。所以要么都写static，并且这不算重写；要么都不写static，算重写。**



重写的条件：

1. 需要有继承关系，子类重写父类的方法
2. 方法名、参数列表和返回类型（除子类中方法的返回值是父类方法返回值的子类时）必须相同
3. 修饰符：范围可以扩大但不能缩小          public > protected > default > private
4. 抛出的异常：范围可以被缩小但不能扩大         ClassNotFoundException --> Exception(大)



重写与重载：

- 重载（Overload）：方法名、返回类型必须一样，参数列表必须不同。而且即使两方法方法名一样，参数列表不同，但返回类型不同，这也不构成重载，只是两个名字一样的不同方法罢了。此外，若方法名和参数列表都相同，返回类型不同，这也不构成重载，而且会报错，说该方法已经定义过了。重载发生在同一个类中。重载体现的是编译时的多态性。 
- 重写（Override）：方法名、参数列表、返回类型（除子类中方法的返回值是父类方法返回值的子类时）都相同的情况下，对方法体进行重写，而且必须满足重写的条件。重写发生在具有子父类关系的子类中。重写体现的是运行时的多态性。



## 多态

多态，即同一方法可以根据发送对象的不同而采用多种不同的行为方式。



一个对象的实际类型是确定的，但可以指向对象的引用的类型有很多。



多态注意事项：

1. 多态是方法的多态，和重写一样，属性没有多态。
2. 多态发生在子类上，故必须具有父子类关系。不然会类型转换异常：ClassCastException
3. 多态存在的条件：具有继承关系、方法需要被重写、父类引用指向子类对象 如：`Father f1 = new Son();`



不能被重写的情况：

1. static 是静态方法，属于类，不属于实例，只与类相关。
2. final 是常量的，在常量池里，是不能被修改的。
3. private 是私有的，不能被重写



父类引用指向子类对象：

 在CSDN看关于这个的文章时，一直都没什么触动，直到看见了这两句话：“那是什么动物？”“那是大熊猫！”这两句话让我精神一震，瞬间明白了前面提到的一句话“Human man = new Man(); 这里是把man当做Human来看了。” 原来父类引用指向子类，就是把这个类的对象当做另一个类来看，而这两个类本身也是父子关系，所以这样子看待完全没问题。就好比将猫看做动物，将男人看做人，完全说得过去，而且日常生活中一直是这样干的。

所以父类引用指向子类对象后，这个对象就被当做父类看待了，所以它可以用父类的所有东西，但是它只是被看成父类，并不真正是父类，它有自己独特的东西，所以如果它重写了父类的方法那就只会调用重写后的。而且不能调用子类有而父类没有的方法，因为这样运用多态只是让子类在某些方法的具体实现上与父类不同。就好比Animal cat = new Cat(); 不能通过cat调用造火箭这个方法，因为父类里没有，但是可以调用吃、跑、叫等方法，因为这些方法是所以动物都有的方法。



## instanceof 和类型转换

`instanceof` 关键字用于测试等号左边的对象是否是等号右边的类的实例，返回Boolean类型的值。

`instanceof`是Java中的二元[运算符](https://so.csdn.net/so/search?q=运算符&spm=1001.2101.3001.7020)，等号左边是对象，等号右边是类；当对象是右边类或子类所创建对象时，返回true；否则，返回false。

用法：

```java
boolean result = object instanceof class;
```

说明：

- 类的实例包含本身的实例，以及所有直接或间接子类的实例
- instanceof左边显式声明的类型与右边操作元必须是同种类或存在继承关系，也就是说需要位于同一个继承树，否则会编译错误

也就是说instanceof



## static关键字详解







## 抽象类







## 接口的定义与实现







## 内部类









## 异常







   
