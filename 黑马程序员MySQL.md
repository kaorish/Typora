# 基础篇

## MySQL概述

### MySQL启动及客户端连接

**MySQL启动：**

因为在安装MySQL时勾选了开机自启，所以会平时会自动启动，不需要我们自己手动启动。

但是也可以手动在控制台上开启或关闭MySQL：

首先按Windows + R，再按CTRL + shift + enter以使用管理员身份运行，然后再输入相关的命令：

```
启动：net start mysql80
关闭：net stop mysql80
```



**客户端连接：**

方式一：使用MySQL提供的客户端命令行工具，即在开始菜单找到MySQL 8.0 Commend Line Client，打开后输入密码即可连接MySQL。

方式二：使用Windows的命令行，即win + R后输入cmd打开命令行窗口，然后输入以下指令：

```
mysql [-h 127.0.0.1] [-P3306] -u root -p
```

回车后输入密码再回车即可连接MySQL。

注：-h指定连接ip地址，-P指定连接端口号，-u指定连接用户名，-p指定连接用户密码。实际上打了中括号的部分可以不用敲，不写的话就会默认使用本地回路地址和默认端口号3306，且-u和root之间也可以不需要空格，-p后面也可以直接输入密码，只不过会提示这样做不安全罢了。（使用方式二需要先配置环境变量，不过我已经配好了）



## SQL

SQL语言是不区分大小写的，可以任意使用大小写，但是建议对于关键字全部使用大写，符合编程规范。

SQL语言的空格和缩进也是和其他编程语言一样，可以随便打多少个，可以为了美观而控制缩进、增加空格。

SQL语句也要以分号结尾。

![image-20230522140658039](D:\My tepora note\黑马程序员MySQL.assets\image-20230522140658039.png)

SQL分为四类，分别是：DDL、DML、DQL和DCL。

| 分类 |            全称            |                          说明                          |
| :--: | :------------------------: | :----------------------------------------------------: |
| DDL  |  Data Definition Language  |  数据定义语言，用来定义数据库对象（数据库、表、字段）  |
| DML  | Data Manipulation Language |     数据操作语言，用来对数据库表中的数据进行增删改     |
| DQL  |    Data Query Language     |         数据查询语言，用来查询数据库中表的记录         |
| DCL  |   Data Control Language    | 数据控制语言，用来创建数据库用户、控制数据库的访问权限 |



### DDL

DDL，全称Data Definition Language，数据定义语言，用来定义数据库对象（数据库、表、字段）。



#### DDL-数据库操作

有四种操作：查询、创建、删除和使用。

**查询：**

```
查询所有数据库：SHOW DATABASES;
查询当前数据库：SELECT DATABASE();
```

注意：查询所有数据库的`SHOW DATABASES;`中，DATABASES是复数，要加S！查询当前数据库的`SELECT DATABASE();`中，要加小括号！



**创建：**

```
CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则];
```



**删除：**

```
DROP DATABASE [IF EXISTS] 数据库名;
```



**使用：**

```
USE 数据库名;
```

注意：若没有使用任何数据库，则显示NULL。



#### DDL-表操作-查询

**查询当前数据库所有表：**

```
SHOW TABLES;
```



**查询表结构：**

```
DESC 表名;
```



**查询指定表的建表语句：**

```
SHOW CREATE TABLE 表名;
```



#### DDL-表操作-创建

```
CREATE TABLE 表名(
	字段1 字段1类型 [COMMENT 字段1注释],
	字段2 字段2类型 [COMMENT 字段2注释],
	字段3 字段3类型 [COMMENT 字段3注释],
	...
	字段n 字段n类型 [COMMENT 字段n注释],

) [COMMENT 表注释];
```

注意：在小括号里填写字段，字段之间用逗号分隔，最后一个字段不需要加逗号。注释要用单引号括起来。最后使用分号结束该SQL语句。（都是英文状态下的！）



#### DDL-表操作-数据类型

表的数据类型有三大种：数值类型、字符串类型和日期时间类型。其中每一大种都包含很多种数据类型。

**数值类型：**

![image-20230521221031566](D:\My tepora note\黑马程序员MySQL.assets\image-20230521221031566.png)

注意：使用浮点数时需要在后面跟上一个小括号，里面指定精度和标度。其中，精度指整个小数的总长度，不包括小数点，只是数字的个数；标度指小数部分的数字的长度。如：1234.56，精度是6，标度是2，故应使用语句`num DOUBLE(6, 2) COMMENT '小数';`。



**字符串类型：**

![image-20230521220750578](D:\My tepora note\黑马程序员MySQL.assets\image-20230521220750578.png)

注意：char的效率更高，varchar的效率较低。



**日期时间类型：**

![image-20230521220937019](D:\My tepora note\黑马程序员MySQL.assets\image-20230521220937019.png)



#### DDL-表操作-修改

**添加字段：**

```
ALTER TABLE 表名 ADD 字段名 类型（长度） [COMMENT 注释] [约束];
```



**修改数据类型：**

```
ALTER TABLE 表名 MODIFY 字段名 新数据类型（长度）;
```



**修改字段名和字段类型：**

```
ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型（长度） [COMMENT 注释][约束];
```

注：如果想要修改字段的名字，那么应该使用change。



**删除字段：**

```
ALTER TABLE 表名 DROP 字段名;
```



**修改表名：**

```
ALTER TABLE 表名 RENAME TO 新表名;
```



**删除表：**

```
DROP TABLE [IF EXISTS] 表名;
```

注意：在删除表时，表中的全部数据也会被删除。



**删除指定表，并重新创建该表：**

```
TRUNCATE TABLE 表名;
```

注意：使用这种方法后，相当于将表中的数据清空了，最后得到一张没有填东西的表格（那些表头什么的还在，里面的东西没了），而且会重置自增计数器，如自增的主键。



### DML

DML，全称Data Manipulation Language，数据操作语言，用来对数据库表中的数据进行增删改。



#### DML-添加数据

**给指定字段添加数据：**

![image-20230522142659897](D:\My tepora note\黑马程序员MySQL.assets\image-20230522142659897.png)



**给全部字段添加数据：**

![image-20230522142858720](D:\My tepora note\黑马程序员MySQL.assets\image-20230522142858720.png)

未指定字段名则给所有字段添加值。



**批量添加数据：**

![image-20230522142823058](D:\My tepora note\黑马程序员MySQL.assets\image-20230522142823058.png)



![image-20230522143009028](D:\My tepora note\黑马程序员MySQL.assets\image-20230522143009028.png)

此处的引号指的是单引号！



![image-20230622172116979](D:\My tepora note\黑马程序员MySQL.assets\image-20230622172116979.png)

![image-20230622172745188](D:\My tepora note\黑马程序员MySQL.assets\image-20230622172745188.png)



#### DML-修改数据

![image-20230522145402685](D:\My tepora note\黑马程序员MySQL.assets\image-20230522145402685.png)

![image-20230522145420883](D:\My tepora note\黑马程序员MySQL.assets\image-20230522145420883.png)



#### DML-删除数据

```
DELETE FROM 表名 [WHERE 条件];
```

![image-20230522150000260](D:\My tepora note\黑马程序员MySQL.assets\image-20230522150000260.png)

注意：此处的删除，删除的是一整行的所有数据，不是某一个单元格。要删除单元格可以使用update设置为null。



### DQL

DQL，全称Data Query Language，数据查询语言，用来查询数据库中表的记录。

语法结构：

![image-20230522151357775](D:\My tepora note\黑马程序员MySQL.assets\image-20230522151357775.png)



#### DQL-基础查询

**查询多个字段：**

![image-20230522151521471](D:\My tepora note\黑马程序员MySQL.assets\image-20230522151521471.png)



**设置别名：**

![image-20230522151553533](D:\My tepora note\黑马程序员MySQL.assets\image-20230522151553533.png)

注意：起别名后，返回的数据中该字段的名字就被别名代替了。而且起别名时as关键字可以不写，只写别名即可。别名如果是英文的，那么可以用单引号括起来，也可以不用；如果是中文的，那么就一定要用单引号括起来。



**去除重复记录：**

![image-20230522151652577](D:\My tepora note\黑马程序员MySQL.assets\image-20230522151652577.png)

注意：去除重复记录时也可起别名，一样可以省略as关键字。



#### **DQL-条件查询**

语法：

![image-20230522152722907](D:\My tepora note\黑马程序员MySQL.assets\image-20230522152722907.png)



条件：

![image-20230522152909256](D:\My tepora note\黑马程序员MySQL.assets\image-20230522152909256.png)

注意：

使用`like 占位符`时，占位符要用单引号括起来！`_`指这个位置已经被占用了，是什么无所谓，意思是固定了长度，如`select * from emp where name like '__';`指查找emp表中名字为两个字的所有员工的信息。`%`指前面的东西都不重要，只要最后是以%后面跟着的结尾就行，如`select * from emp where idcard like '%X';`指查询身份证号以X结尾的所有员工的信息。假设要查询中间有个值为X的员工信息，那么就写成`like '%X%'`。假设要查姓氏为X的，那么就写`like 'X%'`，如`like '孙%'`。

`between...and...`是**左闭右闭**的，且使用`between...and...`时顺序不能写反了，between后面的数字更小，and后面的更大。



#### DQL-聚合函数

介绍：

将一列数据作为一个整体，进行纵向计算。



常见聚合函数：

![image-20230522160145440](D:\My tepora note\黑马程序员MySQL.assets\image-20230522160145440.png)

注意：这些聚合函数都是作用于某一列数据的。



语法：

![image-20230522160302456](D:\My tepora note\黑马程序员MySQL.assets\image-20230522160302456.png)

将字段写在聚合函数后面的括号内。

聚合函数也可以和基础查询一起使用，如`select sum(age) from emp where workaddress = '西安';`

注意：null值不参与所有聚合函数运算。



#### DQL-分组查询

语法：

![image-20230522161139485](D:\My tepora note\黑马程序员MySQL.assets\image-20230522161139485.png)



where和having的区别：

![image-20230522161300706](D:\My tepora note\黑马程序员MySQL.assets\image-20230522161300706.png)



如果需要对分组之后的数据再次进行过滤，那么此时就应该使用having。如：

![image-20230522161945848](D:\My tepora note\黑马程序员MySQL.assets\image-20230522161945848.png)



![image-20230522162151014](D:\My tepora note\黑马程序员MySQL.assets\image-20230522162151014.png)

where是在分组之前过滤的，而分组的时候会执行聚合函数，而having是在分组聚合之后过滤的。所以执行顺序为where > 聚合函数 > having。



#### DQL-排序查询

语法：

![image-20230522162750255](D:\My tepora note\黑马程序员MySQL.assets\image-20230522162750255.png)



排序方式（只有两种）：

![image-20230522162838464](D:\My tepora note\黑马程序员MySQL.assets\image-20230522162838464.png)

asc指ascend，vi.&vt. 上升；desc指descend，vi.& vt. 下降。

若省略排序方式，那么默认是升序排序。所以若要用降序排序，desc必须写。



#### DQL-分页查询

语法：

![image-20230522163501002](D:\My tepora note\黑马程序员MySQL.assets\image-20230522163501002.png)



![image-20230522163632196](D:\My tepora note\黑马程序员MySQL.assets\image-20230522163632196.png)

记录数即行数，对于表而言，每一行都被称为一条记录。

假设起始索引是从0开始的，那么可以省略0，直接写第二个参数，即查询记录数就行。



例：

![image-20230522163931556](D:\My tepora note\黑马程序员MySQL.assets\image-20230522163931556.png)



#### DQL-执行顺序

编写顺序：

<img src="D:\My tepora note\黑马程序员MySQL.assets\image-20230522170326220.png" alt="image-20230522170326220" style="zoom:67%;" />



执行顺序：

![image-20230522170511292](D:\My tepora note\黑马程序员MySQL.assets\image-20230522170511292.png)



### DCL

DCL，全称Data Control Language，数据控制语言，用来创建数据库用户、控制数据库的访问权限。



#### DCL-用户管理

主要是用来管理哪些用户可以访问当前MySQL数据库。



**查询用户：**

![image-20230522171526539](D:\My tepora note\黑马程序员MySQL.assets\image-20230522171526539.png)

数据库中的所有用户的信息都存储在mysql数据库中的user表中。



**创建用户：**

![image-20230522171555985](D:\My tepora note\黑马程序员MySQL.assets\image-20230522171555985.png)

若想让该用户可以在任意主机访问该数据库，那么主机名就用%代替。如：`create user 'sunhao' @ '%' identified by '123456';`



**修改用户密码：**

![image-20230522171622891](D:\My tepora note\黑马程序员MySQL.assets\image-20230522171622891.png)



**删除用户：**

![image-20230522171644709](D:\My tepora note\黑马程序员MySQL.assets\image-20230522171644709.png)



![image-20230522172559950](D:\My tepora note\黑马程序员MySQL.assets\image-20230522172559950.png)



#### DCL-权限控制

主要控制的是当前用户连接上MySQL后，他可以访问当前MySQL数据库中的哪些数据库、哪些表，可以对这些表执行什么样的操作。



![image-20230522180335282](D:\My tepora note\黑马程序员MySQL.assets\image-20230522180335282.png)

**查询权限：**

![image-20230522180443732](D:\My tepora note\黑马程序员MySQL.assets\image-20230522180443732.png)



**授予权限：**

![image-20230522180518754](D:\My tepora note\黑马程序员MySQL.assets\image-20230522180518754.png)

授予的是具体的库里的表，但是可以使用通配符，就能授予所有库或表的权限了。



**撤销权限：**

![image-20230522180554374](D:\My tepora note\黑马程序员MySQL.assets\image-20230522180554374.png)

![image-20230522181222678](D:\My tepora note\黑马程序员MySQL.assets\image-20230522181222678.png)

还可以授予权限时on后面写`*.*`，就把所有数据库所有表的权限都给了，相当于超级管理员。



## 函数

函数，指一段可以直接被另一段程序调用的程序或代码。

函数是内置的，我们只需要调用即可。



各函数的语法都是一样的：

<img src="D:\My tepora note\黑马程序员MySQL.assets\image-20230522193206641.png" alt="image-20230522193206641" style="zoom:100%;" />



### 字符串函数

![image-20230522193019388](D:\My tepora note\黑马程序员MySQL.assets\image-20230522193019388.png)

注：`TRIM(str)`函数只能去掉头部和尾部的空格，中间的空格不会去掉。

  	 `SUBSTRING(str, start, len)`函数的索引值是从1开始的！

![image-20230615124253095](D:\My tepora note\黑马程序员MySQL.assets\image-20230615124253095.png)



### 数值函数

![image-20230522194236993](D:\My tepora note\黑马程序员MySQL.assets\image-20230522194236993.png)

注：`RAND()`函数不需要传参数。

​	   `ROUND(X, Y)`函数意思是将x进行四舍五入到y位小数。如：`select round(2.344, 2);`的结果为2.34，`select round(2.345, 2);`的结果为2.35。



### 日期函数

![image-20230522195321484](D:\My tepora note\黑马程序员MySQL.assets\image-20230522195321484.png)

注：`DATADIFF(date1, date2);`函数算出的时间（天数）是第一个日期减第二个日期得出的，前减后，即date1-date2。



时间差：

- TIMESTAMPDIFF(interval, time_start, time_end)可计算time_end-time_start的时间差（即第二个减第一个），单位以指定的interval为准，常用可选：
  - SECOND 秒
  - MINUTE 分钟（返回秒数差除以60的整数部分）
  - HOUR 小时（返回秒数差除以3600的整数部分）
  - DAY 天数（返回秒数差除以3600*24的整数部分）
  - MONTH 月数
  - YEAR 年数



删除时还能限制删除的记录数，只需要在最后加上limit value即可，这样就能删除value个符合记录的记录。



### 流程函数

![image-20230522200830156](D:\My tepora note\黑马程序员MySQL.assets\image-20230522200830156.png)

注：`IFNULL(value1, value2)`函数的value1不为空是指不为null，就算是空串那也不为空。

`CASE [expr] WHEN [val1] THEN [res1] ... ELSE [default] END`函数应用举例：

![image-20230522201453949](D:\My tepora note\黑马程序员MySQL.assets\image-20230522201453949.png)

该函数的when...then...可以重复写多次，以end表示该函数调用结束。



`CASE WHEN [val1] THEN [res1] ... ELSE [default] END`函数应用举例：

![image-20230522202157786](D:\My tepora note\黑马程序员MySQL.assets\image-20230522202157786.png)



### 窗口函数

窗口函数适用场景: `对分组统计结果中的每一条记录进行计算`的场景下, 使用窗口函数更好, 注意, 是**每一条**!! 因为MySQL的普通聚合函数的结果(如 group by)是`每一组只有一条记录`!!!

![在这里插入图片描述](https://img-blog.csdnimg.cn/4e98d1f734da4e80abbd94c91a5d139e.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA6I-c6I-c55qE5aSn5pWw5o2u5byA5Y-R5LmL6Lev,size_20,color_FFFFFF,t_70,g_se,x_16#pic_center)



## 约束

### 概述

![image-20230522203120934](D:\My tepora note\黑马程序员MySQL.assets\image-20230522203120934.png)



### 演示

![image-20230522203432668](D:\My tepora note\黑马程序员MySQL.assets\image-20230522203432668.png)

从上图可得知，对于字段，可以添加多个约束，且多个约束之间不用加任何符号，空格就行。

![image-20230522212520187](D:\My tepora note\黑马程序员MySQL.assets\image-20230522212520187.png)



若要修改某个字段的约束：

```sql
ALTER TABLE 表名 MODIFY COLUMN 字段名 字段类型 新约束;
```



### 外键约束

![image-20230522212923437](D:\My tepora note\黑马程序员MySQL.assets\image-20230522212923437.png)

语法：

![image-20230522213220592](D:\My tepora note\黑马程序员MySQL.assets\image-20230522213220592.png)

![image-20230522214224945](D:\My tepora note\黑马程序员MySQL.assets\image-20230522214224945.png)

若存在外键约束，那么就不允许删除父表中相应的主键。



### 外键删除/更新行为

![image-20230522234433148](D:\My tepora note\黑马程序员MySQL.assets\image-20230522234433148.png)

语法：

![image-20230522234551043](D:\My tepora note\黑马程序员MySQL.assets\image-20230522234551043.png)

注：此处是以`CASCADE`（级联）为例。实际上的语法是在添加外键的语法后面将删除和更新的行为分别写在`ON UPDATE`和`ON DELETE`后面。

​        `NO ACTION`和`RESTRICT`是一样的，而且也是外键约束中的默认行为。



## 多表查询

### 多表关系介绍

![image-20230523145201463](D:\My tepora note\黑马程序员MySQL.assets\image-20230523145201463.png)

![image-20230523145302389](D:\My tepora note\黑马程序员MySQL.assets\image-20230523145302389.png)

![image-20230523145350600](D:\My tepora note\黑马程序员MySQL.assets\image-20230523145350600.png)

![image-20230523145453919](D:\My tepora note\黑马程序员MySQL.assets\image-20230523145453919.png)



### 多表查询概述

![image-20230523145657345](D:\My tepora note\黑马程序员MySQL.assets\image-20230523145657345.png)

学过离散数学都知道，两个集合做乘法时，会进行笛卡尔积，得到一个有序对集合，其中的每一个元素都是一个有序对，有序对中的第一元素来自第一个集合，第二元素来自第二集合，有序对总个数为两相乘集合的元素之积。

所以这里的多表查询也是一样，两张表进行笛卡尔积生成一张新的长表，里面的每一条记录都包含了原先两个表的字段，内容是进行排列组合的，所以总记录数为原先两表的记录数之积。我们如果直接将所有内容显示出来，就会多出很多无用的信息，即无效的笛卡尔积，所以我们需要对查找条件进行限制以消除无效的笛卡尔积，而这个限制就是：连接条件，连接条件通常是两表中相同的字段要相等，即不是两表共有的字段不能作为连接条件放在on后面。通过连接条件，在笛卡尔积后的新表中查询想要的数据。通过on消除了无效的笛卡尔积后还可以使用where进一步限制搜索范围，因为left join on后相当于产生了一张新表，这张新表当然可以使用where进行限制了。



![image-20230523150414907](D:\My tepora note\黑马程序员MySQL.assets\image-20230523150414907.png)



### 内连接

![image-20230523150540770](D:\My tepora note\黑马程序员MySQL.assets\image-20230523150540770.png)

语法：

![image-20230523150559820](D:\My tepora note\黑马程序员MySQL.assets\image-20230523150559820.png)

注意：隐式内连接就是直接用逗号区分两张表，条件前面用where！而显示内连接要用inner join关键字区分两张表，连接条件前面用on！不要搞错了，尤其是用where还是on！



### 外连接

外连接分为两种，左外连接和右外连接。不过我们一般使用左外连接，因为右外连接可以写成左外连接的形式。



语法：

![image-20230523151255462](D:\My tepora note\黑马程序员MySQL.assets\image-20230523151255462.png)



### 自连接

自连接就是自己连接自己，但是我们要将两个自己看成是两张表，因为要起别名。直接将两个不同的别名看成是两张表，这样更好理解。

![image-20230523151536318](D:\My tepora note\黑马程序员MySQL.assets\image-20230523151536318.png)

注意：自连接可以是内连接，说明可以是显式也可以是隐式。同时自连接可以是外连接，说明可以是左外连接也可以是右外连接。比较灵活，更具需求选择用哪种方式。切记，一定一定要起别名！不然怎么知道该字段是哪张表的。而且join可以省略不写。条件前面是on！



### 联合查询

关键字：union、union all

![image-20230523151911852](D:\My tepora note\黑马程序员MySQL.assets\image-20230523151911852.png)



语法：

![image-20230523151943530](D:\My tepora note\黑马程序员MySQL.assets\image-20230523151943530.png)



注意：

![image-20230523152405851](D:\My tepora note\黑马程序员MySQL.assets\image-20230523152405851.png)

实际上联合查询是用于多表查询的！



### 子查询介绍







#### 标量子查询







#### 列子查询





#### 行子查询







#### 表子查询



