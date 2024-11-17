# 概述

## 1. 什么是hive

**hive简介：**Hive是由Facebook开源，基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张表，并提供类SQL查询功能。



**hive存在的意义：**将复杂的mr程序简化为hive sql，只使用一行代码就能实现与冗长的mr程序一样的效果，简单方便，易于理解。



**hive本质：**

Hive是一个Hadoop客户端，用于将HQL（Hive SQL）转化成MapReduce程序。

- Hive中每张表的数据存储在HDFS
- Hive分析数据底层的实现是MapReduce（也可配置为Spark或者Tez）
- 执行程序运行在Yarn上



## 2. hive架构原理

hive的架构包括：用户接口Client、元数据Metastore、驱动器Driver、和Hadoop。

![image-20230910194225252](D:\My tepora note\尚硅谷Hive.assets\image-20230910194225252.png)



1. 用户接口：Client

CLI（command-line interface）、JDBC/ODBC。



2. 元数据：Metastore

元数据包括：数据库（默认是default）、表名、表的拥有者、列/分区字段、表的类型（是否是外部表）、表的数据所在目录等。

默认存储在自带的derby数据库中，由于derby数据库只支持单客户端访问，生产环境中为了多人开发，推荐使用MySQL存储Metastore。



3. 驱动器：Driver

（1）解析器（SQLParser）：将SQL字符串转换成抽象语法树（AST）

（2）语义分析（Semantic Analyzer）：将AST进一步划分为QeuryBlock

（3）逻辑计划生成器（Logical Plan Gen）：将语法树生成逻辑计划

（4）逻辑优化器（Logical Optimizer）：对逻辑计划进行优化

（5）物理计划生成器（Physical Plan Gen）：根据优化后的逻辑计划生成物理计划

（6）物理优化器（Physical Optimizer）：对物理计划进行优化

（7）执行器（Execution）：执行该计划，得到查询结果并返回给客户端



4. Hadoop

使用HDFS进行存储，可以选择MapReduce/Tez/Spark进行计算。



## 3. 对于hive中一些概念的理解

### 元数据

元数据（metadata）是关于数据库、表、列、分区等数据结构和属性的描述性信息，包括字段名称、字段类型、表的结构、表的统计信息、数据访问权限等等。这些元数据信息存储在关系型数据库中，通常是Hive的Metastore数据库。

而实际的数据（表中的数据记录）存储在Hadoop分布式文件系统（如HDFS）或其他存储系统中，它们不被称为元数据。这些数据是用户实际操作和分析的内容，而元数据则是关于这些数据的描述信息，帮助用户管理和理解数据。

总而言之，元数据是关于数据的数据，它提供了有关数据结构、属性和管理信息，而实际的数据则存储在数据存储系统中。



常见的元数据：

1. **表元数据**：表元数据包括表的名称、列名、列的数据类型、分区键、表的存储格式等信息。这些信息描述了表的结构和属性。
2. **分区元数据**：如果表被分区，分区元数据包括有关分区键、分区值和分区存储位置的信息。这有助于Hive在查询时定位到正确的分区数据。
3. **列元数据**：列元数据包括有关每个列的信息，例如列名、数据类型、注释等。这有助于Hive执行列级别的操作和查询。
4. **表的存储位置元数据**：Hive记录了表的数据存储位置，通常是在Hadoop分布式文件系统（HDFS）上的文件路径。这使得Hive能够找到表的数据。
5. **权限元数据**：权限元数据包括有关哪些用户或角色有权访问表以及他们的访问权限的信息。这有助于Hive进行数据权限管理。
6. **表的统计信息元数据**：这些信息包括表的行数、数据大小、数据分布等统计信息，可用于查询优化和执行计划选择。
7. **数据源元数据**：如果数据是通过外部数据源（例如HBase、Kafka等）导入到Hive中，元数据可以包括有关数据源的连接信息、数据提供者和导入方式的信息。



### metastore和metastore数据库

metastore是管理元数据的组件，metastore数据库是存储着元数据的关系型数据库。

metastore不存储元数据，实际上元数据存储在如MySQL这样的关系型数据库中。



**metastore：**

Hive中的Metastore是用于管理和存储Hive元数据的关键组件。Hive是一个基于Hadoop的数据仓库和查询语言，它使用户能够通过类似SQL的查询语言来查询和分析大规模数据集。Metastore的主要作用如下：

1. 存储表和分区定义：Metastore存储了Hive中创建的所有表的元数据信息，包括表的结构（列名、数据类型、分区键等）、表的存储位置以及表的统计信息。它还管理了表的分区信息，允许用户将表的数据分成多个分区，以便更有效地管理和查询数据。
2. 元数据管理：Metastore还存储了有关数据库、表、列、分区和其他Hive对象的元数据信息。这些元数据对于Hive的查询优化、权限管理和元数据浏览非常重要。
3. 表和数据的关联：Metastore通过记录表的元数据信息和数据的物理位置之间的关系，使得Hive能够在查询时找到正确的数据。
4. 查询优化：Hive使用Metastore中的统计信息来执行查询优化。例如，它可以使用表的大小和分布信息来选择合适的查询计划，以提高查询性能。
5. 权限管理：Metastore还用于管理Hive的权限。它可以存储用户和角色的权限信息，并确保只有授权用户可以访问特定的表和数据。

总之，Hive中的Metastore是一个关键的组件，它使用户能够有效地管理和查询大规模数据集，并提供了元数据管理、查询优化和权限管理等功能。它将Hive表的定义和数据的物理位置联系起来，使得用户可以轻松地进行数据分析和查询操作。



**metastore数据库：**

"Metastore数据库" 这个术语指的是存储着Hive元数据的数据库，而不是说 "Metastore" 本身是一个数据库。Metastore 是一个Hive组件，用于管理和访问这个 "Metastore数据库" 中的元数据信息。"Metastore数据库" 的实际后端可以是关系型数据库，如MySQL或其他支持的存储系统，用于存储Hive表、列、分区等的元数据。这个术语确实有些令人混淆，但关键是理解 Metastore 是一个组件，而不是数据库本身。



### hive中的数据库和表

hive中的数据库和表都只是逻辑概念，并非真正的数据库和表。其中，数据库和表的元数据都存储在metastore数据库中，而实际的数据其实是存储在hdfs中。metastore数据库中的记录保证了hive中数据库和表的逻辑关系，hdfs保证了hive中的数据库和表可以存储数据，从而实现了逻辑意义上的数据库和表。

其中，通过navicat连接hadoop102上的MySQL可以看见，hive数据库（default或者其他我们自己创建的）都是merastore数据库中的DBS表的记录，而且由其中的location可以知道default数据库相当于母体，而其他创建的数据库则是在default之下。而在这些数据库中创建的所有的表都存在另一张表——TBLS表中，里面有着SD_ID这个字段。而将表与数据库之间联系起来的就是SD_ID这个字段，表与数据库之间的关系存在SDS表中，而且里面的InputFormat和OutputFormat字段也指定了对应的表。最后，在COLUMNS_V2表中记录了每一张表的字段的信息（名字、类型、注释等），通过CD_ID字段与SDS表联系起来，进而联系对应的表。（这段话中的“数据库”以及“表”都指的是他们的元数据，因为metastore数据库是将二者进行逻辑上的联系，实际上他们存储的数据存储在hdfs中）



### hive中元数据和hdfs的关系

- Hive元数据（Metastore）中的表定义了表的结构、列、分区等信息，并指定了表的数据存储位置（通常是HDFS上的路径）。
- HDFS存储了实际的数据文件，这些文件包含表中的数据记录。
- Hive使用Metastore中的元数据信息来理解和查询数据的结构，然后通过HDFS查找和访问实际的数据文件，以执行查询操作。
- 元数据与HDFS之间的关系是协同的，它们一起使Hive能够有效地管理和查询大规模数据集。

总之，Hive中的元数据（Metastore）用于描述和管理数据的结构和属性，而HDFS用于实际存储数据文件。这两者一起工作，使得Hive能够提供高效的数据管理和查询功能。



**对于hive中的表和hdfs**：

在Hive中，每个表通常对应着HDFS上的一个目录，而不是一个单独的文件。这个目录中包含了表的数据文件以及可能的分区子目录，这些数据文件包含了表中的数据记录。

具体来说：

1. **表对应目录**：每个Hive表通常对应着HDFS上的一个目录，该目录用于存储表的数据文件。这个目录的路径通常由用户定义，并且在创建表时指定。
2. **数据文件**：表的数据被存储在该目录下的一个或多个数据文件中，这些文件包含了表中的数据记录。数据文件的格式通常由表的存储格式（如Parquet、ORC、Text等）决定。
3. **分区子目录**：如果表被分区，表的数据目录下可能包含一个或多个分区子目录，每个分区子目录用于存储特定分区的数据。这些分区子目录可以帮助表的数据更好地组织和管理。

所以，表对应的是一个目录，而不是一个单独的文件。这个目录可以包含一个或多个数据文件，以及分区子目录（如果表被分区）。当查询Hive表时，Hive会根据表的定义和元数据信息在这些目录和文件中查找并读取数据。

总之，Hive中的表通常对应着HDFS上的一个目录，该目录包含了表的数据文件，而不是一个单独的文件。这种组织方式有助于管理和查询大规模数据集。



# Hive服务部署

hiveserver2和metastore的服务可以在不启动集群时部署，但是如果想要使用他们，那么必须先启动集群和MySQL服务。

我们这里的hiveserver2和metastore的服务都是部署在hadoop102上的，其他的节点可能被用来当做客户端，比如hadoop103。



## 1. hiveserver2服务

Hive的hiveserver2服务的作用是提供jdbc/odbc接口，为用户提供远程访问Hive数据的功能，例如用户期望在个人电脑中访问远程服务中的Hive数据，就需要用到Hiveserver2。

简而言之，hiveserver2就是用来远程连接hive的，不管是beeline还是DataGrip想要写hive sql，都需要连接hiveserver2。

![image-20230914233810899](D:\My tepora note\尚硅谷Hive.assets\image-20230914233810899.png)



**用户说明：**

这里有个关于用户的小问题，由Hivesever2代理访问时，访问Hadoop集群的用户身份是谁？是Hiveserver2的启动用户？还是客户端的登录用户？

答案是都有可能，具体是谁由Hiveserver2的hive.server2.enable.doAs参数决定，该参数的含义是是否启用Hiveserver2用户模拟的功能。若启用，则Hiveserver2会模拟成客户端的登录用户去访问Hadoop集群的数据，不启用，则Hivesever2会直接使用启动用户访问Hadoop集群数据。模拟用户的功能，默认是开启的。

未开启用户模拟功能：

![image-20230914234248532](D:\My tepora note\尚硅谷Hive.assets\image-20230914234248532.png)

开启用户模拟功能：

![image-20230914234353345](D:\My tepora note\尚硅谷Hive.assets\image-20230914234353345.png)



**hiveserver2部署：**

（1）Hadoop端配置：

hivesever2的模拟用户功能，依赖于Hadoop提供的proxy user（代理用户功能），只有Hadoop中的代理用户才能模拟其他用户的身份访问Hadoop集群。因此，需要将hiveserver2的启动用户设置为Hadoop的代理用户。

首先进入hadoop目录，在core-site.xml中增加下列内容：

```
<!--配置所有节点的root用户都可作为代理用户-->
<property>
    <name>hadoop.proxyuser.root.hosts</name>
    <value>*</value>
</property>

<!--配置root用户能够代理的用户组为任意组-->
<property>
    <name>hadoop.proxyuser.root.groups</name>
    <value>*</value>
</property>

<!--配置root用户能够代理的用户为任意用户-->
<property>
    <name>hadoop.proxyuser.root.users</name>
    <value>*</value>
</property>
```



（2）HIve端配置：

在hive-3.1.2目录里的conf目录中的hive-site.xml中增加下列内容：

```
<!-- 指定hiveserver2连接的host -->
<property>
	<name>hive.server2.thrift.bind.host</name>
	<value>hadoop102</value>
</property>

<!-- 指定hiveserver2连接的端口号 -->
<property>
	<name>hive.server2.thrift.port</name>
	<value>10000</value>
</property>
```



（3）启动hiveserver2服务：

在hive-3.1.2目录下使用命令`hive --service hiveserver2`就能启动hiveserver2服务。

实际上只要敲在hive-3.1.2目录下敲`hiveserver2`就能让hiveserver2启动起来，新开一个窗口`jps`一下可以发现多了给“RunJar”进程，该进程其实就是hiveserver2。若想看进程的详细信息，可以使用命令`jps -ml`。但是这样启动的话线程就直接堵塞了，而且一旦关闭了该窗口，那么后台的所有进程就都挂了，因此我们实际上在生产环境中不能直接使用这个命令将其启动。所以我们可以使用命令`nohup hiveserver2 1>/dev/null 2>/dev/null &`来让服务自动退到后台运行，不造成阻塞。其中，“nohup”的意思是免挂断，可以避免我们在关闭连接（如关闭该窗口）时进程被挂断了。通常配合nohup使用的还有‘&’，它可以让进程退到后台去运行，不会阻塞进程。双管齐下后就能保证我们的进程能够正常运行，不会被挂断了。此外，‘1’是文件描述符（0代表标准输入，1代表标准输出，2代表标准错误），代表输出的内容，“/dev/null”相当于黑洞，1重定向到/dev/null意思是输出的内容我不想要，直接丢掉，“2>/dev/null”同理。既然标准输出和标准错误重定向的位置是一样的，那么可以进行简化为`nohup hiveserver2 >/dev/null 2>&1 &`在2重定向的位置使用“&1”表示该位置与1重定向的位置相同；而不写文件描述符时写重定向的话系统会默认重定向的是标准输出，故1也可以不写。



（4）测试

使用beeline：

在hive-3.1.2目录下，使用命令`beeline -u jdbc:hive2://hadoop102:10000 -n root`即可。

若出现下列内容则表明使用beeline远程连接hadoop102的hiveserver2服务成功：

```
Connecting to jdbc:hive2://hadoop102:10000
Connected to: Apache Hive (version 3.1.2)
Driver: Hive JDBC (version 3.1.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
Beeline version 3.1.3 by Apache Hive
0: jdbc:hive2://hadoop102:10000>
```

注意，要先开启hiveserver2服务，而且就算开了也可能连不上，此时要等一会，或者新开个窗口。



使用DataGrip：

创建连接（主机名填hadoop102，用户为root，不需要输入密码）后直接测试几个简单的sql语句即可，若能正常运行，说明远程连接hadoop102的hiveserver2服务成功。



注意：这里需进行说明，**连接的到底是什么？**

我们只要仔细看看hiveserver2用户模拟的图示，就能明显看出。

![](D:\My tepora note\尚硅谷Hive.assets\image-20230914234353345.png)

Hive客户端通过网络连接到部署了HiveServer2服务的节点上的HiveServer2服务。HiveServer2充当了Hive的交互接口，客户端通过HiveServer2与Hive进行通信和操作。客户端通过HiveServer2模拟用户与Hive进行交互，发送SQL查询、数据操作请求等，然后HiveServer2会处理这些请求，与底层的Hive组件（如元数据存储、计算节点等）进行通信和协调，最终执行相应的操作并将结果返回给客户端。

这种架构允许多个客户端通过HiveServer2与Hive进行并发交互，并提供了查询协调和安全性的好处。因此，Hive客户端连接的是部署了HiveServer2服务的节点上的HiveServer2服务，而不是直接连接到底层的Hive组件。



记录一个连接hiveserver2的bug：

前几天我在部署并测试hiveserver2时第一次成功了，但是之后怎么都不行，永远报同一个错：`Error: Could not open client transport with JDBC Uri: jdbc:hive2://hadoop102:10000: java.net.ConnectException: 拒绝连接 (Connection refused) (state=08S01,code=0)`。

我很奇怪，首先使用`jps -ml`发现hiveserver2确实在正常运行，没有问题。

然后反复仔细检查了core-site.xml和hive-site.xml，没有发现有填错的地方。（这里很关键！！）

然后检查MySQL服务是否正常启动，结果也是正常的。

我非常奇怪，于是直接测试能不能使用hive，发现居然不行！！！

顿时我想到了原因，之前在部署metastore服务时我只部署到一半，没有弄完，有可能是这里出了问题。于是我再次查看core-site.xml和hive-site.xml，发现hive-site.xml中我粘贴连接MySQL的信息时忘记修改密码了。。。所以才会出现第一次连上了之后怎么都连不上，hiveserver2和MySQL服务都启动了，hiveserver2配置信息没填错等情况下依旧无法连接hadoop102的hiveserver2服务。

这个东东花了我好多时间，真是要仔细地修改配置文件，不然一个很小的地方会浪费非常多的时间。



## 2. metastore服务

metastore服务分为两种：嵌入式模式和独立服务模式

（1）嵌入式模式：

![image-20230915002555774](D:\My tepora note\尚硅谷Hive.assets\image-20230915002555774.png)

- 嵌入式 Metastore 服务是将 Metastore 组件直接嵌入到 Hive Server 进程中的一种模式。
- 在这种模式下，Metastore 与 Hive Server 在同一进程中运行（不需要单独启动服务，在启动 Hive Server 时，Metastore 也会启动），它们共享相同的 JVM（Java 虚拟机）。
- 这种模式适用于小型部署或单用户使用的情况，因为它相对简单，无需额外的 Metastore 服务器进程。
- 嵌入式模式通常用于开发和测试环境，不适合大规模、生产环境中的部署。



生产环境中，不推荐使用嵌入式模式。因为其存在以下两个问题：

1. 嵌入式模式下，每个Hive CLI都需要**直接连接**元数据库，当Hive CLI较多时，数据库压力会比较大。

2. 每个客户端都需要用户元数据库的读写权限，元数据库的安全得不到很好的保证。



因为嵌入式模式中，metastore和hiveserver2在同一节点，都读取hive-site.xml中的信息，所以我们只需要在hive-site.xml中添加连接MySQL数据库的信息即可：

```
<!-- jdbc连接的URL -->
    <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://hadoop102:3306/metastore?useSSL=false</value>
    </property>
    
    <!-- jdbc连接的Driver-->
    <property>
        <name>javax.jdo.option.ConnectionDriverName</name>
        <value>com.mysql.jdbc.Driver</value>
    </property>
    
	<!-- jdbc连接的username-->
    <property>
        <name>javax.jdo.option.ConnectionUserName</name>
        <value>root</value>
    </property>

    <!-- jdbc连接的password -->
    <property>
        <name>javax.jdo.option.ConnectionPassword</name>
        <value>@sun005222</value>
    </property>
```



但是该配置我们在安装MySQL后将元数据库修改为MySQL时就配置过了，所以其实在hadoop102节点我们一直都在运行嵌入式模式，毕竟我们之前可从未进行启动metastore服务的操作。



（2）独立服务模式：

![image-20230915003344326](D:\My tepora note\尚硅谷Hive.assets\image-20230915003344326.png)

- 独立 Metastore 服务是将 Metastore 组件作为一个独立的服务运行的模式。
- 在这种模式下，Metastore 作为一个单独的进程运行（需要单独开启进程），它可以独立于 Hive Server 和其他组件运行。
- 这种模式适用于生产环境和大规模部署，因为它提供了更好的可伸缩性和可管理性。
- 独立模式下，多个 Hive Server 可以共享同一个独立的 Metastore 服务，以实现更好的资源利用和管理。



在配置独立服务模式时，我们首先需要保证metastore服务的配置文件hive-site.xml中包含连接元数据库所需的以下参数：

```
<!-- jdbc连接的URL -->
    <property>
        <name>javax.jdo.option.ConnectionURL</name>
        <value>jdbc:mysql://hadoop102:3306/metastore?useSSL=false</value>
    </property>
    
    <!-- jdbc连接的Driver-->
    <property>
        <name>javax.jdo.option.ConnectionDriverName</name>
        <value>com.mysql.jdbc.Driver</value>
    </property>
    
	<!-- jdbc连接的username-->
    <property>
        <name>javax.jdo.option.ConnectionUserName</name>
        <value>root</value>
    </property>

    <!-- jdbc连接的password -->
    <property>
        <name>javax.jdo.option.ConnectionPassword</name>
        <value>@sun005222</value>
    </property>
```



然后在每一个需要连接metastore服务的节点（hiveserver2或hive client）的配置文件hive-site.xml中包含访问metastore服务所需的以下参数：

```
<!-- 指定metastore服务的地址 -->
<property>
	<name>hive.metastore.uris</name>
	<value>thrift://hadoop102:9083</value>
</property>
```

注意：主机名需要改为metastore服务所在节点，端口号无需修改，metastore服务的默认端口就是9083。



在部署了metastore服务的节点上启动metastore服务：

使用命令`hive --service metastore`可以将其启动，同样的这个命令会让进程阻塞，所以可以使用`nohup hive --service metastore >/dev/null 2>&1 &	`。



最后进行测试，看看是否部署成功：

启动hive client（我们让hadoop103为客户端，需要提前将hadoop102的hive发送给hadoop103），使用`show databases；`等简单的hive sql看看能否正常运行，若可以则说明部署成功。



需要说明的是，在我们配置独立运行模式的这个情形下，hadoop103为独立服务模式，通过连接hadoop102的metastore服务与数据库交互；hadoop102为嵌入式模式，metastore服务就在自己这个节点，所以是直接连接数据库。



注意：若客户端或者hiveserver2等连接metastore服务的节点中，hive-stie.xml文件里即包含连接metastore服务的参数，又包含直接连接MySQL的参数，那么只会看连接metastore服务的参数，即此时还是独立运行模式，假如部署了metastore服务的节点未开启metastore服务，那么客户端或者hiveserver2即使进入了hive也同样无法使用hive sql。



## 3. 群起脚本

此脚本是我配置云服务器时自己弄的，因为发现课程资料中的不好用所以自己弄了一个，好使无bug。

```shell
#!/bin/bash
if [ $# -lt 1 ]
then
    echo "No Args Input..."
    exit ;
fi
case $1 in
"start")
        echo " =================== 启动hiveserver2 ==================="
        nohup hiveserver2 >/dev/null 2>&1 &
        echo " =================== 启动metastore ==================="
        nohup hive --service metastore >/dev/null 2>&1 &
;;
"stop")
        echo " =================== 关闭metastore  ==================="

        SIGNAL=${SIGNAL:-TERM}
        PIDS=$(jps -lm | grep -i 'metastore.HiveMetaStore' | awk '{print $1}')

        if [ -z "$PIDS" ]; then
                 echo "No HiveMetaStore server to stop"
                 exit 1
        else
                 kill -s $SIGNAL $PIDS
        fi

        echo " =================== 关闭hiveserver2 ==================="
        pkill -f HiveServer2
;;
"restart")
                myhiveserver2.sh stop;myhiveserver2.sh start
;;
*)
    echo "Input Args Error..."
;;
esac
```





# Hive SQL（HQL）

很多语法和MySQL很像，难度不大。



## DDL

### 创建数据库

```
CREATE DATABASE [IF NOT EXISTS] database_name
[COMMENT database_comment]
[LOCATION hdfs_path]
[WITH DBPROPERTIES (property_name=property_value, ...)];
```



### 查询数据库

1. 展示所有数据库

```
SHOW DATABASES [LIKE 'identifier_with_wildcards'];
```

注：like通配表达式说明：*表示任意个任意字符，|表示或的关系。此处需要和MySQL区分开来，MySQL中模糊匹配是用`%`，这里是用` *`，而且MySQL有占位符`_`，hive中没有，但是有或`|`。



2. 查看数据库信息

```
DESC DATABASE [EXTENDED] db_name;
```

注：也可写describe，一样的。extended关键字可以查看更多信息，不加该关键字则只展示基本信息。



### 修改数据库

```
修改dbproperties
ALTER DATABASE database_name SET DBPROPERTIES (property_name=property_value, ...);

修改location
ALTER DATABASE database_name SET LOCATION hdfs_path;

修改owner user
ALTER DATABASE database_name SET OWNER USER user_name;
```



### 删除数据库

```
DROP DATABASE [IF EXISTS] database_name [RESTRICT|CASCADE];
```

注：restrict：严格模式，若数据库不为空，则会删除失败，默认为该模式。

  cascade：级联模式，若数据库不为空，则会将库中的表一并删除。



### 切换当前数据库

```
USE database_name;
```



### 创建表

建表语句有三种，只有第一种需要自己写字段信息，其他两种都不需要。



1. 普通建表

```
CREATE [TEMPORARY] [EXTERNAL] TABLE [IF NOT EXISTS] [db_name.]table_name   
[(col_name data_type [COMMENT col_comment], ...)]
[COMMENT table_comment]
[PARTITIONED BY (col_name data_type [COMMENT col_comment], ...)]
[CLUSTERED BY (col_name, col_name, ...) 
[SORTED BY (col_name [ASC|DESC], ...)] INTO num_buckets BUCKETS]
[ROW FORMAT row_format] 
[STORED AS file_format]
[LOCATION hdfs_path]
[TBLPROPERTIES (property_name=property_value, ...)]
```

关键字说明：

- temporary：临时表，该表只在当前会话可见，会话结束，表会被删除。

- external：外部表，与之相对应的是内部表（管理表）。管理表意味着Hive会完全接管该表，包括元数据和HDFS中的数据。而外部表则意味着Hive只接管元数据，而不完全接管HDFS中的数据。

- data_type：Hive中的字段类型可分为基本数据类型和复杂数据类型。

  

基本数据类型如下：

| **Hive**      | **说明**                                            | 定义          |
| ------------- | --------------------------------------------------- | ------------- |
| **tinyint**   | 1byte有符号整数                                     |               |
| **smallint**  | 2byte有符号整数                                     |               |
| **int**       | 4byte有符号整数                                     |               |
| **bigint**    | 8byte有符号整数                                     |               |
| **boolean**   | 布尔类型，true或者false                             |               |
| **float**     | 单精度浮点数                                        |               |
| **double**    | 双精度浮点数                                        |               |
| **decimal**   | 十进制精准数字类型                                  | decimal(16,2) |
| **varchar**   | 字符序列，需指定最大长度，最大长度的范围是[1,65535] | varchar(32)   |
| **string**    | 字符串，无需指定最大长度                            |               |
| **timestamp** | 时间类型                                            |               |
| **binary**    | 二进制数据                                          |               |



复杂数据类型如下：

| 类型   | 说明                                                     | 定义                         | 取值       |
| ------ | -------------------------------------------------------- | ---------------------------- | ---------- |
| array  | 数组是一组相同类型的值的集合                             | array<string>                | arr[0]     |
| map    | map是一组相同类型的键-值对集合                           | map<string, int>             | map['key'] |
| struct | 结构体由多个属性组成，每个属性都有自己的属性名和数据类型 | struct<id:int,  name:string> | struct.id  |

**注：类型转换**

Hive的基本数据类型可以做类型转换，转换的方式包括隐式转换以及显示转换。

**方式一：隐式转换**

具体规则如下：

a. 任何整数类型都可以隐式地转换为一个范围更广的类型，如tinyint可以转换成int，int可以转换成bigint。

b. 所有整数类型、float和string类型都可以隐式地转换成double。

c. tinyint、smallint、int都可以转换为float。

d. boolean类型不可以转换为任何其它的类型。

详情可参考Hive官方说明：[Allowed Implicit Conversions](https://cwiki.apache.org/confluence/display/hive/languagemanual+types#LanguageManualTypes-AllowedImplicitConversions)



**方式二：显示转换**

可以借助cast函数完成显示的类型转换

```
cast(expr as <type>) 
```



- partitioned by：创建分区表

- clustered by......sorted by ......into......buckets：创建分桶表

- row format：指定SERDE，SERDE是Serializer and Deserializer的简写。Hive使用SERDE序列化和反序列化每行数据。详情可参考 [Hive-Serde](https://cwiki.apache.org/confluence/display/Hive/DeveloperGuide#DeveloperGuide-HiveSerDe)。语法说明如下：

语法一：delimited关键字表示对文件中的每个字段按照特定分割符进行分割，其会使用默认的serde对每行数据进行序列化和反序列化。

```
ROW FORAMT DELIMITED 
[FIELDS TERMINATED BY char] 
[COLLECTION ITEMS TERMINATED BY char] 
[MAP KEYS TERMINATED BY char] 
[LINES TERMINATED BY char] 
[NULL DEFINED AS char]
```

注：

- fields terminated by：列分隔符

- collection items terminated by ： map、struct和array中每个元素之间的分隔符

- map keys terminated by：map中的key与value的分隔符

- lines terminated by ：行分隔符

这些可以选择性指定，也可以什么都不指定，直接写一个delimited。



语法二：serde关键字可用于指定其他内置的serde或者用户自定义的serde。例如JSON serde，可用于处理JSON字符串。

```
ROW FORMAT SERDE serde_name [WITH SERDEPROPERTIES (property_name=property_value,property_name=property_value, ...)] 
```

- stored as：指定文件格式，常用的文件格式有，textfile（默认值），sequence file，orc file、parquet file等等。

- location：指定表所对应的HDFS路径，若不指定路径，其默认值为：

${hive.metastore.warehouse.dir}/db_name.db/table_name

- tblproperties：用于配置表的一些KV键值对参数



2. Create Table As Select（CTAS）建表

该语法允许用户利用select查询语句返回的结果，直接建表，表的结构和查询语句的结构保持一致，且保证包含select查询语句放回的内容。

```
CREATE [TEMPORARY] TABLE [IF NOT EXISTS] table_name 
[COMMENT table_comment] 
[ROW FORMAT row_format] 
[STORED AS file_format] 
[LOCATION hdfs_path]
[TBLPROPERTIES (property_name=property_value, ...)]
[AS select_statement]
```



3. Create Table Like 语法

该语法允许用户复刻一张已经存在的表结构，与上述的CTAS语法不同，该语法创建出来的表中不包含数据。

```
CREATE [TEMPORARY] [EXTERNAL] TABLE [IF NOT EXISTS] [db_name.]table_name
[LIKE exist_table_name]
[ROW FORMAT row_format] 
[STORED AS file_format] 
[LOCATION hdfs_path]
[TBLPROPERTIES (property_name=property_value, ...)]
```



### 查看表

1. 展示所有表

```
SHOW TABLES [IN database_name] LIKE ['identifier_with_wildcards'];
```

注：like通配表达式说明：*表示任意个任意字符，|表示或的关系。



2. 查看表信息

```
DESC [EXTENDED | FORMATTED] [db_name.]table_name;
```

注：extended：展示详细信息

  formatted：对详细信息进行格式化的展示

不写任何关键字就是展示基本信息，想要看详细信息就加formatted。



### 修改表

1. 重命名表

```
ALTER TABLE table_name RENAME TO new_table_name;
```



2. 修改列信息

有三种，分为增加列、更新列和替换列。



**增加列：**该语句允许用户增加新的列，新增列的位置位于末尾。

```
ALTER TABLE table_name ADD COLUMNS (col_name data_type [COMMENT col_comment], ...);
```



**更新列：**该语句允许用户修改指定列的列名、数据类型、注释信息以及在表中的位置。

```
ALTER TABLE table_name CHANGE [COLUMN] col_old_name col_new_name column_type [COMMENT col_comment] [FIRST|AFTER column_name];
```

可以选择`first`或者`after 列名`从而修改字段位置。



**替换列：**该语句允许用户用新的列集替换表中原有的全部列。

```
ALTER TABLE table_name REPLACE COLUMNS (col_name data_type [COMMENT col_comment], ...);
```

替换列最为灵活，直接重新安排所有的信息，前两个能干的替换列都能干。



注：修改列信息都只是修改**元数据**！！！实际上的数据不会发生变化。



### 删除表

```
DROP TABLE [IF EXISTS] table_name;
```



### 清空表

```
TRUNCATE [TABLE] table_name;
```

注意：truncate只能清空管理表，不能删除外部表中数据。



## DML

### Load

Load语句可将文件导入到Hive表中。

```
LOAD DATA [LOCAL] INPATH 'filepath' [OVERWRITE] INTO TABLE tablename [PARTITION (partcol1=val1, partcol2=val2 ...)];
```

关键字说明：

- local：表示从本地加载数据到Hive表；不加则从HDFS加载数据到Hive表。

- overwrite：表示覆盖表中已有数据，不加则表示追加。

- partition：表示上传到指定分区，若目标是分区表，需指定分区。



注：local分两种情况，如果是在节点的命令行那么就指该节点；如果是像DataGrip一样远程登录，那么就是所连接的hiveserver2作为本地。

into也有两种，如果是local那么就相当于hdfs的put，将本地的文件复制一份上传到hdfs上文件对应的位置；如果没有加local，那么就是把hdfs上的文件剪切过去，操作完后原先的文件就不存在了。

加载后hdfs上路径里的文件的名字是被加载的文件名，而不是系统生成的一串数字作为名字。



### Insert

1. 新建一张表

```
INSERT (INTO | OVERWRITE) TABLE tablename [PARTITION (partcol1=val1, partcol2=val2 ...)] select_statement;
```

关键字说明：

- into：将结果追加到目标表

- overwrite：用结果覆盖原有数据



注：如果用的是into，那么可以不写table，直接`insert into 表名`即可，和MySQL一样。但是如果要overwrite，那么必须跟table，即`insert overwrite table 表名`。



2. 将给定Values插入表中

```
INSERT (INTO | OVERWRITE) TABLE tablename [PARTITION (partcol1[=val1], partcol2[=val2] ...)] VALUES values_row [, values_row ...];
```



3. 将查询结果写入目标路径

```
INSERT OVERWRITE [LOCAL] DIRECTORY directory
  [ROW FORMAT row_format] [STORED AS file_format] select_statement;
```



### Export&Import

Export导出语句可将表的数据和元数据信息一并到处的HDFS路径，Import可将Export导出的内容导入Hive，表的数据和元数据信息都会恢复。Export和Import可用于两个Hive实例之间的数据迁移。主要应用场景为两个不同的hadoop集群之间传输文件。

```
导出
EXPORT TABLE tablename TO 'export_target_path';

导入
IMPORT [EXTERNAL] TABLE new_or_original_tablename FROM 'source_path' [LOCATION 'import_target_path'];
```

注意：导入时末尾写分号DataGrip会报错，删掉分号就好了，但语句本身没问题，是DataGrip才会这样，如果在命令行的话是能正常执行的。



# 查询

这里的查询和MySQL中的很类似，就不过多赘述了。

## 基本查询

  ### limit

典型的查询会返回多行数据。limit子句用于限制返回的行数。

```
select * from emp limit 5; // 从第一行开始，向下抓取5行
select * from emp limit 2,3; // 表示从第2行开始，向下抓取3行
```

limit有两种用法，写一个数字和两个数字。两个数字相当于从第一个数字开始，向下抓取第二个数字的行数。同理，第一个数字相当于不写起始点，默认为1，所以只写一个数字就是直接抓取5行（从第一行开始）。



### 配置Hive的本地服务模式

```
set mapreduce.framwork.name = local;
```

`set mapreduce.framwork.name`命令可以查看目前该参数设置成了上面。

该参数实际上是hadoop目录中mapred-stie.xml上的一个参数，它的作用是设置mapreduce程序运行在什么上面，如果写的是yarn，那么mapreduce程序就会提交到yarn上面去运行，如果写的是local，那么就是本地模式。本地模式即所有的maptask和reducetask都在一个节点、一个进程中运行，此时每一个maptask和reducetask都是一个线程。

所以当数据量比较小的时候，使用本地模式可以加快速度，不用等那么久。但是在生产模式中慎用，只适合学习时使用。此外，由于是使用set在程序中设置，所以它是临时的，只在当前会话内有效，故hiveserver2要是断开连接了那么就需要重新设置一遍。



### 聚合函数

聚合函数，顾名思义就是把多个数据聚合在一起进行计算，最后返回一个数字。

```
count(*)，表示统计所有行数，包含null值；
count(某列)，表示该列一共有多少行，不包含null值；
max()，求最大值，不包含null，除非所有值都是null；
min()，求最小值，不包含null，除非所有值都是null；
sum()，求和，不包含null。
avg()，求平均值，不包含null。
```



count就是用来数行的，如果是null那么就不会包括在内，不是null那么就会记一行。

在Hive中，`count（*）`和 `count（1）`是一模一样的，底层逻辑也一模一样，执行效率也一毛一样，也就是说在Hive中，`count（*）`和`count（1）`是一样的，但是在某些关系型数据库如MySQL中，`count（1）`的效率比`count（*）`更高。

count（）括号里填1，2，3，4都是一样的效果，而count（1）和count（*）是一样的效果，所以count（）括号里不管填什么，效果都一样，没啥区别。

如果是空，那么这些聚合函数就都不会统计在内。



## 分组查询

分组后，待查询的字段名就只能是被分组的字段名和聚合函数。

分组就是所有数据根据某一个字段进行分组，即把一张表分成好几份，每一份返回一个数据，也就是说一共分成了几份最后就返回几个数据，分别对应该被分组字段。而聚合函数是把多个数据聚合在一起进行计算最后返回一个数据，所以在分组时聚合函数会对**每一个组**进行聚合并返回一个值，所以也是返回和组数相同的数据。

此时如果select了一个别的字段，该字段既不是聚合函数也不是被分组的字段，而我们输出的数据都是混和处理过的，那么该字段该放在生成的表中的哪里呢？很明显，它无处可去，所以使用分组后，select的字段就只能是被分组的字段以及聚合函数。



## Join语句

### 联合查询

联合查询分union和union all两种，区别只是union会对数据进行去重，而union all不进行去重。

union只需要保证字段数量和对应的字段类型一样就行，字段名无所谓的，因为union只是两张表简单粗暴地直接上下接在一起罢了，只要类型一样就行，哪还管名字呢。若真的名字不一样，那么返回的结果里字段名就是第一张表的，也就是上面那张表的。



## 排序

重点掌握全局排序，即order by，而sort by、distribute by和cluster by都不常用，了解即可。



### 全局排序（Order By）

order by用于排序，在待排序地的字段名后面写asc或desc用于指定是升序还是降序，其中asc为升序，desc为降序。此外，默认为升序，即什么都不写就是升序排序。

在hive中，我们最好不要直接使用order by，这样很容易出问题。因为在底层其实是一个mr程序，几个map分别对数据进行排序，然后汇入reduce，又由于order by是全局排序，所以最后处理的数据一定会只进入一个reduce中，此时如果数据量很大的话，reduce就很可能会出现问题，所以在使用排序时最好别直接用。

![image-20230922225701067](D:\My tepora note\尚硅谷Hive.assets\image-20230922225701067.png)

那么正确的操作是什么呢？那就是在普通的语法的末尾加上`limit 行数`，即指定输出排序后的前多少行。这样做的话底层的map就会只输出前指定行数的数据汇入reduce，然后reduce再最终挑选出前指定行数的数据。这样的话就算输入的数据量很大，由于指定了输出前多少行，最后汇入reduce的行数也只有指定行数的两倍而已，就能大概率保证reduce不会出现问题了。



### 每个Reduce内部排序（Sort By）

对于大规模的数据集，order by的效率非常低。在很多情况下，并不需要全局排序，此时可以使用**Sort by**。

Sort by为每个reduce产生一个排序文件。每个Reduce内部进行排序，对全局结果集来说不是排序。

使用sort by其实不是很适合用select语句查看结果，因为整体是无序的，我们应该用`insert overwrite local directory 目录名 查询语句; `将结果写入文件中。因为我们在使用sort by之前一定要使用命令`set mapreduce.job.reduces = 个数;`指定多少个reduce，而一共指定了多少个reduce，最后就会输出多少个文件，这些文件里每一个都是按照指定字段排好序的。



### 分区（Distribute By）

在有些情况下，我们需要控制某个特定行应该到哪个Reducer，通常是为了进行后续的聚集操作。**distribute by**子句可以做这件事。**distribute by**类似MapReduce中partition（自定义分区），进行分区，结合sort by进行排序。

说白了就是distribute by就是指定分区字段的，指定了之后mr程序就会按照该字段进行分区。

distribute by一般都和sort by一起使用，因为单独使用distribute by的话就只分区不排序，这样得到的每个文件都是无序的，而只使用sort by的话分区就不能指定，只能让输出的每一个文件都按照我们指定的字段进行排序。使用这两种方式的任意一种时万一我们有别的想法都无法实现，所以我们可以将这两种方式进行结合起来用， distribute by指定分区后sort by指定排序字段，这样就能让文件按照我们想的进行分区并且按我们的需要进行排序了。



### 分区排序（Cluster By）

分区排序，顾名思义，就是分区后进行排序。

由上面sort by可知，我们一般都会将distribute by一和sort by结合起来使用，此时如果我们希望的分区字段和排序字段相同，那么就可以直接使用cluster by，这样的话可以避免写`distribute by 字段`和`sort by 字段`，这样更加方便、简洁。

只不过，使用cluster by的话排序只能是升序排序，不能指定排序规则为asc或者desc。



# 函数

`round()`如果不指定保留的小数位数，就默认只保留整数，即没有小数。此外，四舍五入是只是对数值进行四舍五入，即如果是负数，相当于看不见负号，四舍五入后再把负号加回来。如round（-1.5）的结果是-2。



我们说的“日期”就是指年月日，“时间”指时分秒。实际上hive和MySQL也都是这么认为的。

时间戳都是零时区的，所以我们想将时间戳转换成某时区的时间字符串当然使用`from_utc_timestamp()`了，不然转换成的时间字符串还是零时区的。切记，时间戳都是指utc的，即零时区，咱门这是东八区，想要转换成时间字符串那叫转换成北京时间！时间戳都是指零时区的，没有什么东八区的时间戳！！！



`datediff()`的第一个参数是结束日趋，第二个参数是开始日期。该函数的值是由结束日期减开始日期得到的，也就是说，左边减右边。



需要注意，hive中的日期格式和Linux的不一样，hive中和java是一样的，都是`yyyy-MM-dd HH:mm:ss`，而Linux则是`+%Y-%m-%d %H:%M:%S`。



# 调优

## 分组聚合优化

主要使用map-side优化，只在map端进行聚合输出，不需要reduce，所以没有shuffle阶段，大大缩短了运行时间，提高了效率。



## join优化

Hive拥有多种join算法，包括Common Join，Map Join，Bucket Map Join，Sort Merge Buckt Map Join。

Common Join是最普通的，单纯是mr程序，也是默认的，我们一般需要对join使用优化即使用别的join方式。对于common join，sql语句中的join操作和执行计划中的Common Join任务并非一对一的关系，一个sql语句中的**相邻**的且**关联字段相同**的多个join操作可以合并为一个Common Join任务。

Map Join适合大表join小表，同样只有map没有reduce。map端先将小表缓存起来（维护hash表），然后再读取大表进行join。

Bucket Map Join适合大表join大表，解决了map join不适合缓存大表的问题。使用bucket map join需要满足一些条件：两张表都必须是分桶表且分区字段要相同，同时需满足两表分桶数是整数倍。

Sort Merge Buckt Map Join适合大表join大表，而且sort merge map join对分桶大小是没有要求的，因为它不基于内存，而前面三个都基于内存。使用sort merge map join也需要满足条件：两张表都必须是分桶表且分区字段要相同、每个桶的排序字段要相同且每个桶都是有序的、两表分桶数是整数倍。