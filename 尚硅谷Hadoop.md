# 入门

## hadoop概述

### 一. hadoop是什么

1. Hadoop是一个由 Apache基金会所开发的分布式系统基础架构

2. 主要解决海量数据的存储和海量数据的分析计算问题

3. 广义上来说，Hadoop通常是指一个更广泛的概念——Hadoop生态圈



### 二. hadoop三大发行版本

Hadoop三大发行版本：Apache、Cloudera、Hortonworks。

- Apache：最原始（最基础）的版本，对于入门学习最好。2006
- Cloudera：内部集成了很多大数据框架，对应产品CDH。2008
- Hortonworks：文档较好，对应产品HDP。2011

Hortonworks现在已经被Cloudera公司收购，推出新的品牌CDP。



### 三. hadoop的优势

hadoop的优势有四个（四高）：高可靠性、高扩展性、高效性、高容错性。



**高可靠性：**hadoop底层维护多个数据副本，所以即使hadoop某个计算单元或存储出现故障，也不会导致数据的丢失。

**高扩展性：**在集群间分配任务数据，可方便的扩展数以千计的节点。

**高效性：**在mapreduce的思想下，hadoop是并行工作的，以加快任务处理速度。

**高容错性：**能够自动将失败的任务重新分配。



### 四. hadoop组成

hadoop1.x、2.x和3.x之间的区别：

![image-20230709164526968](D:\My tepora note\尚硅谷Hadoop.assets\image-20230709164526968.png)



#### 1. HDFS架构概述

HDFS，Hadoop Distributed File System，即[Hadoop](https://baike.baidu.com/item/Hadoop/3526507?fromModule=lemma_inlink)[分布式文件系统](https://baike.baidu.com/item/分布式文件系统/1250388?fromModule=lemma_inlink)。

- NameNode (nn) ：存储文件的**元数据**，如**文件名**，**文件目录结构**，**文件属性**(生成时间、副本数、文件权限)，以及每个文件的**块列表**和**块所在的DataNode**等。
- DataNode(dn)：在本地文件系统**存储文件块数据**，以及**块数据的校验和**。
- Secondary NameNode(2nn)：**每隔一段时间对NameNode元数据备份**。



总而言之：Datanode是用来存放数据的，而Namenode就是类似于一个目录，记录了数据存放位置的信息。所以说，Namenode可以帮助我们快速查找和访问存储在Datanode上的数据。至于Secondarynamenode嘛，它是Namenode的副本，负责辅助Namenode的工作。这样一来，即使Namenode出现故障，Secondarynamenode也可以帮助恢复并保证数据的稳定性。



#### 2. YARN架构概述

YARN，Yet Another Resource Negotiator，即另一种资源协调者。

- ResourceManager（资源管理器）：它负责整个集群的资源调度和管理。它知道集群中有多少资源可用，并根据应用程序的需求进行资源分配，确保集群资源的高效利用。

- NodeManager（节点管理器）：每台机器上都会有一个NodeManager，它负责管理该机器上的资源。NodeManager会向ResourceManager报告自己上面的资源和运行的应用程序的状态，同时接收来自ResourceManager的指令，如分配资源给应用程序或者杀死应用程序。

- ApplicationMaster（应用程序管理器）：每个运行的应用程序都会有一个对应的ApplicationMaster。它负责与ResourceManager通信，向其请求资源，并监控和管理应用程序的执行。如果需要更多资源或者有任务失败，ApplicationMaster会与ResourceManager进行交互。

- Container（容器）：它是YARN中的基本执行单位。每个应用程序都会被分配一个或多个Container，用来运行应用程序的任务。Container提供了一定的资源，如内存和CPU，供应用程序使用。

  

  这样一来，YARN就可以更好地管理集群中的资源，实现作业的高效调度和执行。



#### 3. MapReduce架构概述

MapReduce，是一种编程模型，用于大规模数据集（大于1TB）的并行运算。概念"Map（映射）"和"Reduce（归约）"，是它们的主要思想。

MapReduce将计算过程分为两阶段：Map和Reduce。

- Map阶段：并行处理输入数据

- Redeuce阶段：对Map结果进行汇总



#### 4. HDFS、YARN和MapReduce三者的关系

HDFS（Hadoop分布式文件系统）是Hadoop提供的分布式文件系统，用于存储和管理大规模数据。HDFS提供了数据的可靠性、高容错性和高可扩展性，它是MapReduce作业的主要数据存储和读取源。

YARN（Yet Another Resource Negotiator，又称为MapReduce 2.0）是Hadoop的资源管理器和作业调度系统。YARN主要负责集群资源的分配和管理，让不同的应用程序可以共享集群的资源。在YARN的架构下，除了MapReduce，还可以运行其他类型的分布式处理框架，如Spark、Storm等。

MapReduce是一种编程模型和计算框架，用于大规模数据的分布式处理和计算。MapReduce将任务分解为若干个Mapper和Reducer任务，通过并行处理和归约，在分布式环境下高效地处理大量数据。MapReduce执行过程中依赖于HDFS存储数据，并借助YARN进行资源调度和管理。



简单来说：HDFS提供了数据存储，YARN负责资源管理和作业调度，而MapReduce则是一种分布式计算框架。



### 五. 大数据技术生态体系

![image-20230709171316789](D:\My tepora note\尚硅谷Hadoop.assets\image-20230709171316789.png)





## 配置集群

总的来说，配置集群的步骤为：配置模板机 -> 克隆虚拟机 -> 配置完全分布式运行模式

其中：

配置模板机：新建虚拟机并配置root用户和主机名 -> 配置IP地址、设置hosts文件 -> 安装epel-release -> 测试网络 -> 关闭防火墙及其自启动 -> 安装jdk和hadoop 

克隆虚拟机：克隆虚拟机 -> 更改主机名和IP地址

配置完全分布式运行模式：配置ssh免密登录 -> 编写集群分发脚本xsync -> 集群配置 -> 群起集群 -> 测试集群 -> 配置历史服务器 -> 配置日志的聚集 -> 编写群起集群脚本myhadoop.sh和查看所有节点jps脚本jpsall



以上内容是配置集群的大致步骤，接下来是详细的配置过程。



### 一. 配置模板机

#### 1. 新建虚拟机

学习hadoop之前我们应该具备了一定的Linux基础，创建虚拟机其实可以沿用之前的配置。所以这部分没啥好说的，只需注意别安装最小系统版的Linux，选择一个图形化界面即可。然后在创建的过程中设置好root用户的密码，不用创建别的用户了，因为我们将全程使用root用户，这将为以后避免许多不必要的麻烦。同时配置好主机名，我将这台模板机命名为hadoop102，102将设置成这台虚拟机ip地址的尾号，这样我们可以做到看见机器名字就能知道它的ip地址是什么，将为我们带来很大的便利。



#### 2. 配置IP地址、hosts文件

**配置IP地址：**

使用命令`vim /etc/sysconfig/network-scripts/ifcfg-ens33`，将其中的`BOOTPROTO`改为`static`，如果不这样设置则会出现ifcfg-ens33文件中的ip地址与使用`ifconfig`命令查看的ip地址不一样的情况。

然后在最下面添加以下内容：

```
#IP地址
IPADDR=192.168.17.102
#网关
GATEWAY=192.168.17.2
#域名解析器
DNS1=192.168.17.2
```

然后保存退出即可。



**配置hosts文件：**模板机和本地机器都要配置hosts文件，这样就可以直接使用主机名来访问别的节点了，不再需要敲冗长且容易记错的IP地址。而本地机器也需要配置hosts的原因是，配置好后我们就可以在使用xshell、xftp以及浏览器时可以直接输入主机名代替输入ip地址，更便利且不易出错。



模板机：

使用命令`vim /etc/hosts`打开hosts文件后，添加以下内容：

```
192.168.17.100 hadoop100
192.168.17.101 hadoop101
192.168.17.102 hadoop102
192.168.17.103 hadoop103
192.168.17.104 hadoop104
```

然后保存退出。

我打算用hadoop102、hadoop103和hadoop104这三台虚拟机搭建集群，所以hosts文件只写到了hadoop104，以后若是有别的机器加入那么只需要再添加对于的IP地址和主机名即可。 



本地机器：

Windows11系统的hosts文件的路径为：`C:\Windows\System32\drivers\etc\hosts`，找到后直接复制到桌面上，然后重命名在后面加上".txt"后缀方便我们用打开并修改其中的内容。用文本编辑器打开后再最下面加上与模板机相同的内容，保存退出。再将后缀删去，复制到原地址中，选择"替换目标中的文件"即可。



#### 3. 安装epel-release

在联网的情况下输入命令`yum install -y epel-release`就能安装好了。

注：Extra Packages for Enterprise Linux是为“红帽系”的操作系统提供额外的软件包，适用于RHEL、CentOS和Scientific Linux。相当于是一个软件仓库，大多数rpm包在官方 repository 中是找不到的。



#### 4. 测试网络

我们需要测试虚拟机能否连接外网和本地机器。

我们需要使用伟大的命令：`ping`

外网：输入`ping www.baidu.com`看看能不能ping通。

本地机器：在自己电脑的命令行上输入`ipconfig`查看ip地址，然后在模板机上ping一下，看看能不能ping通。



如果都能ping通那就没问题，否则需要上网寻找相关方法来解决这个问题。



#### 5. 关闭防火墙及其自启动

为了我们能正常访问集群配置好后的各种web页面，我们需要先将防火墙关闭。

关闭防火墙：`systemctl stop firewalld`

关闭防火墙自启动：`systemctl disable firewalld.service`



注：在企业开发时，通常单个服务器的防火墙是关闭的，公司整体对外会设置非常安全的防火墙。



#### 6. 安装jdk和hadoop

在安装jdk和hadoop前，我们需要先将虚拟机自带的jdk删除干净，然后再安装自己的jdk和hadoop。最后我们需要配置环境变量，以便我们在任何情况下都能直接使用java和hadoop命令。



**卸载虚拟机自带的jdk：**

使用命令`rpm -qa | grep -i java | xargs -n1 rpm -e --nodeps `。

其中各项参数为：

- rpm -qa：查询所安装的所有rpm软件包

- grep -i：忽略大小写

- xargs -n1：表示每次只传递一个参数

- rpm -e –nodeps：强制卸载软件



注：卸载完后记得使用`reboot`命令重启虚拟机。



**安装jdk和hadoop：**

先在/opt目录下创建module和software目录，其中software用来存放jdk和hadoop的压缩包，module用来存放jdk和hadoop。

先进入software目录，然后使用xftp连接模板机并将jdk和hadoop的压缩包传输给模板机。接着使用命令`tar -zxvf jdk-8u371-linux-x64.tar.gz -C /opt/module/`将jdk解压至module目录，再使用命令`tar -zxvf hadoop-3.1.3.tar.gz -C /opt/module/`将jdk解压至module目录。



**配置环境变量：**

首先新建my_env.sh文件`vim /etc/profile.d/my_env.sh`

然后添加以下内容：

```
#JAVA_HOME
export JAVA_HOME=/opt/module/jdk1.8.0_212
export PATH=$PATH:$JAVA_HOME/bin

#HADOOP_HOME
export HADOOP_HOME=/opt/module/hadoop-3.1.3

export PATH=$PATH:$HADOOP_HOME/bin
export PATH=$PATH:$HADOOP_HOME/sbin
```

保存退出后用命令`source /etc/profile`，让新的环境变量PATH生效。

接着回到主目录分别使用`java -version`和`hadoop -version`测试环境变量是否配置成功，有结果、能看见他们的信息就是配置成功了，且配置成功后输入开头的几个字母可以按tab键自动补全。



此外，可以通过运行本地运行模式（官方WordCount）来检测是否成功安装jdk和hadoop：

在hadoop-3.1.3下创建文件夹input，再在里面创建word.txt文本文件并输入几个单词，然后退回hadoop-3.1.3目录，使用命令：`hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.3.jar wordcount input/ output`。

运行成功的情况下可以看见多了output目录，进入后发现里面有两个文件：part-r-00000和_SUCCESS，part-r-00000就是对word.txt进行单词数统计得出的结果。



### 二. 克隆虚拟机

#### 1. 克隆虚拟机

将模板机关机后（注意一定要先关机！）克隆出剩下的虚拟机。我打算用三台虚拟机搭建集群，所以我克隆了另外两台。



#### 2. 更改主机名和IP地址

分别对每一台克隆出来的虚拟机进行更改对应的主机名和ip地址的操作。

更改主机名：`vim /etc/sysconfig/network-scripts/ifcfg-ens33`，只需要将inet这一行的ip地址修改就行了，其余不用动。

更改ip地址：`vim /etc/hostname`



操作完后直接`reboot`重启。



### 三. 配置完全分布式运行模式

#### 1. 配置ssh免密登录

配置ssh免密登录是后面所有脚本运行的基础（指方便快捷，因为不再需要输一堆密码），所以我们一上来就先将其配置好。



**免密登录的原理：**

![image-20230709133118416](D:\My tepora note\尚硅谷Hadoop.assets\image-20230709133118416.png)

总而言之，我将我的公钥发给别人后就代表：我要免密登录你了。所以我们接下来需要生成公钥和私钥，再拷贝给其他的所有节点。



**使用ssh登录随便另一个节点：**

`ssh hadoop103`之后直接`exit`退出，这样主目录就出现了.ssh目录了。

注意：一定要先用ssh连别人一次！不然是不会出现.ssh目录的！！！



**生成公钥和私钥：**

按`cd`回到主目录然后`cd .ssh`进入.ssh目录，再输入`ssh-keygen -t rsa`后连续按三次回车，就会生成两个文件id_rsa（私钥）、id_rsa.pub（公钥）。



**将公钥拷贝到要免密登录的目标机器上：**

使用命令`ssh-copy-id 主机名`将公钥发给每一个节点，**包括自己！！！**对我来说就是`ssh-copy-id hadoop102`、`ssh-copy-id hadoop103`、`ssh-copy-id hadoop104`各输一遍，然后挨个输入密码。

然后对剩余的每一个节点都这么操作（此时就是拼手速的时候了）。

==**千万千万别落下了自己！！！这很重要！！**==



注：.ssh目录中的 known_hosts 记录ssh访问过计算机的公钥（public  key），id_rsa 是生成的私钥， id_rsa.pub 是生成的公钥，authorized_keys 存放授权过的无密登录服务器公钥。



#### 2. 编写集群分发脚本xsync

在编写xsync脚本前我们需要先学习两个命令：scp和rsync。



**scp（secure copy）安全拷贝：**

定义：scp可以实现服务器与服务器之间的数据拷贝。（from server1 to server2）

基本语法：scp      -r              $pdir/$fname                $user@$host:$pdir/$fname

​                  命令  递归   要拷贝的文件路径/名称  目的地用户@主机:目的地路径/名称

例：`scp -r /opt/module/jdk1.8.0_212 root@hadoop103:/opt/module`可以将本节点上的jdk拷贝给登录root用户的hadoop103的/opt/module。



**rsync远程同步工具**：

rsync主要用于备份和镜像。具有速度快、避免复制相同内容和支持符号链接的优点。

基本语法：rsync     -av                  $pdir/$fname                 $user@$host:$pdir/$fname

​                    命令  选项参数  要拷贝的文件路径/名称  目的地用户@主机:目的地路径/名称

选项参数说明：

- -a：归档拷贝
- -v：显示复制过程



rsync和scp区别：用rsync做文件的复制要比scp的速度快，rsync只对差异文件做更新，scp是把所有文件都复制过去。



注意：rsync是同步！！什么是同步，若我的机器有别的机器没有的，那么rsync给它后他也就有了一份一模一样的，连文件路径都一样。即如果我rsync对方没有的文件，就相当于scp，即拷贝。若我rsync对面有的文件且进行了修改，那么只会更新对方的该文件至和我相同，此时是更新！！若完全一样那就不进行任何操作。所以假如我删了某个目录中的某些文件，再rsync这一整个目录，不会让对方的目录的目录减少相同的文件。即更新只会令文件数量增多或保持不变，不可能让文件数量变少。



**xsync集群分发脚本：**

xsync相当于对其他所有节点依次使用rsync命令。因为rsync即具备scp的复制文件作用，又具备更新相关文件的作用，所以我们没有必要使用scp命令了，它的功能已经完全被rsync所代替。故我们使用xsync就能将本节点的某些文件直接同步至其余所有节点（无论对方是有还是没有），且由于我们前面配置好了ssh免密登录，故分发文件只需要使用xsync这一个命令即可，非常的好用，非常的便捷。

此外我们希望随时随地都能使用这个命令，所以应该将脚本放在声明了全局环境变量的路径。

我们使用`echo $PATH`就能看见所有声明了全局环境变量的路径：/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/module/jdk1.8.0_212/bin:/opt/module/hadoop-3.1.3/bin:/opt/module/hadoop-3.1.3/sbin:/home/sunhao/bin:/root/bin。可以发现路径/root/bin是其中之一，故我们决定将脚本放在这个路径。

在主目录中用`mkdir bin`新建bin目录，然后进去，再`vim xsync`新建xysnc文件，输入以下内容：

```
#!/bin/bash

#1. 判断参数个数
if [ $# -lt 1 ]
then
    echo Not Enough Arguement!
    exit;
fi

#2. 遍历集群所有机器
for host in hadoop102 hadoop103 hadoop104
do
    echo ====================  $host  ====================
    #3. 遍历所有目录，挨个发送

    for file in $@
    do
        #4. 判断文件是否存在
        if [ -e $file ]
            then
                #5. 获取父目录
                pdir=$(cd -P $(dirname $file); pwd)

                #6. 获取当前文件的名称
                fname=$(basename $file)
                ssh $host "mkdir -p $pdir"
                rsync -av $pdir/$fname $host:$pdir
            else
                echo $file does not exists!
        fi
    done
done

```

注意：要先按 i 进入编辑模式再粘贴，不能直接粘贴！在普通模式下粘贴会出错！



保存退出后，使用`chmod 777 xsync`修改脚本权限，再分发脚本`xsync xsync`。



#### 3. 集群配置

**集群部署规划：**

- NameNode和SecondaryNameNode很消耗内存，不要安装在同一台服务器上。

- ResourceManager也很消耗内存，不要和NameNode、SecondaryNameNode配置在同一台机器上。

![image-20230709141926760](D:\My tepora note\尚硅谷Hadoop.assets\image-20230709141926760.png)



**配置文件：**

我们需要先配置存放在$HADOOP_HOME/etc/hadoop这个路径上的**core-site.xml、hdfs-site.xml、yarn-site.xml、mapred-site.xml**、**workers**五个配置文件。



core-site.xml：

`cd /opt/module/hadoop-3.1.3/etc/hadoop/`进入hadoop目录后，`vim core-site.xml `进入core-site.xml，在`<configuration>`和`</configuration>`之间插入以下内容：

```
    <!-- 指定NameNode的地址 -->
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://hadoop102:8020</value>
    </property>

    <!-- 指定hadoop数据的存储目录 -->
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/opt/module/hadoop-3.1.3/data</value>
    </property>

    <!-- 配置HDFS网页登录使用的静态用户为root -->
    <property>
        <name>hadoop.http.staticuser.user</name>
        <value>root</value>
    </property>
```



hdfs-site.xml：

`vim hdfs-site.xml `进入hdfs-site.xml，在`<configuration>`和`</configuration>`之间插入以下内容：

```
        <!-- nn web端访问地址-->
    <property>
        <name>dfs.namenode.http-address</name>
        <value>hadoop102:9870</value>
    </property>
        <!-- 2nn web端访问地址-->
    <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>hadoop104:9868</value>
    </property>
```



yarn-site.xml：

`vim yarn-site.xml `进入yarn-site.xml，在`<configuration>`和`</configuration>`之间插入以下内容：

```
    <!-- 指定MR走shuffle -->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>

    <!-- 指定ResourceManager的地址-->
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>hadoop103</value>
    </property>

    <!-- 环境变量的继承 -->
    <property>
        <name>yarn.nodemanager.env-whitelist</name>
                <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value>
    </property>
```



mapred-site.xml：

`vim mapred-site.xml `进入mapred-site.xml，在`<configuration>`和`</configuration>`之间插入以下内容：

```
<!-- 指定MapReduce程序运行在Yarn上 -->
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
```



workers：

`vim workers`进入workers，删除原本的那一行话，再添加以下内容：

```
hadoop102
hadoop103
hadoop104
```

注意：这里是将每一行都当做主机名了，所以名字的后面不能留有空格，最后一行不能有单独的空行，否则会将这个空行当做主机名称，从而出现错误。

五个文件全都配置好后，在集群上分发hadoop配置文件：`cd ..`回退至上一级目录etc，然后`xsync hadoop`。



#### 4. 群起集群

如果是第一次启动集群，那么需要先在hadoop102节点格式化NameNode，然后再启动hdfs和yarn。之后则不需要进行初始化操作。



**初始化：**

使用命令：`hdfs namenode -format`

正常情况下会多出data和logs这两个目录，若缺少其中任何一个则说明有问题，此时需要检查core-site.xml文件有没有复制错。

注意：格式化NameNode，会产生新的集群id，导致NameNode和DataNode的集群id不一致，集群找不到已往数据。如果集群在运行过程中报错，需要重新格式化NameNode的话，一定要先停止namenode和datanode进程，并且要删除所有机器的data和logs目录，然后再进行格式化。



**启动集群：**

首先，我们使用root用户不能直接调用`start-dfs.sh`，会报错。因为这是hdfs的保护机制，root的权力太大了，用root启动集群可能造成某些不可挽回的问题，所以hdfs默认禁止使用root用户启动集群。

此时我们需要修改hadoop安装目录下的 etc/hadoop下的 hadoop-env.sh文件，直接在开头添加以下内容：

```
export HDFS_NAMENODE_USER=root
export HDFS_DATANODE_USER=root
export HDFS_JOURNALNODE_USER=root
export HDFS_ZKFC_USER=root
export YARN_RESOURCEMANAGER_USER=root
export YARN_NODEMANAGER_USER=root
export HDFS_SECONDARYNAMENODE_USER=root
```

保存退出再分发：`xsync hadoop-env.sh`

然后就可以在**hadoop102**使用命令：`start-dfs.sh`。正常情况下，在hadoop102使用jps会出现jps、datanode和namenode，在hadoop103使用jps会出现datanode和jps，在hadoop104使用jps会出现secondarynamenode、datanode和jps。

接着在**hadoop103**使用命令：`start-yarn.sh`。正常情况下，在每个节点使用jps都会出现四个进程，其中有三个是一样的：jps、datanode和nodemanager。然后剩下的一个，hadoop102为namenode，hadoop103为resourcemanager，hadoop104为secondarynamenode。

至此，集群启动成功。

注意：千万要记住，dfs是在hadoop102上启停的，yarn是在hadoop103上启停的！！！不然会报错！



#### 5. 测试集群

首先打开浏览器在地址栏分别尝试进入：hadoop102：9870、hadoop103：8088。其中hadoop102：9870为Namenode information，hadoo103：8088为All Applications。

接着在集群创建文件夹：`hadoop fs -mkdir /input`

上传小文件：`hadoop fs -put $HADOOP_HOME/input/word.txt /input`

上传大文件：`hadoop fs -put  /opt/software/jdk-8u371-linux-x64.tar.gz /`



由于yarn是管理资源的分配和调度的，所以需要计算时才会显示任务。

我们可以采用官方的wordcount案例来切身体会一下：

使用命令：`hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.3.jar wordcount /input /output`，在计算过程中我们切换至hadoo103：8088页面，可以发现多了一个任务，而这个任务正是wordcount。



HDFS文件的存储路径为：`/opt/module/hadoop-3.1.3/data/dfs/data/current/BP-697859437-192.168.17.102-1688800644759/current/finalized/subdir0/subdir0`

且一共有三个备份，每个节点都存放着相同的文件。



查看的文件内容：`cat blk_...`可以使用t查看没有后缀的文件。

拼接文件：`cat blk_... >> temp.tar.gz`可以将多个文件追加进同一个压缩包中，解压后就能看见存放的是什么了。



#### 6. 配置历史服务器

为了查看程序的历史运行情况，所以需要配置一下历史服务器。



**配置mapred-site.xml：**

`vim mapred-site.xml`打开mapred-site.xml文件后，在`<configuration>`和`</configuration>`之间插入以下内容：

```
<!-- 历史服务器端地址 -->
<property>
    <name>mapreduce.jobhistory.address</name>
    <value>hadoop102:10020</value>
</property>

<!-- 历史服务器web端地址 -->
<property>
    <name>mapreduce.jobhistory.webapp.address</name>
    <value>hadoop102:19888</value>
</property>
```



之后分发配置：`xsync mapred-site.xml`



**启动历史服务器：**

在hadoop102上使用命令：`mapred --daemon start historyserver`

之后用jps查看是否成功启动，若成功启动则会增加一个进程：JobHistoryServer。

我们也可以在网页上进行查看：在浏览器进入hadoop102：19888，即JobHistory。之后的所有任务都会在历史服务器上保存，只要开启了。



#### 7. 配置日志的聚集

日志聚集概念：应用运行完成以后，将程序运行日志信息上传到HDFS系统上。

日志聚集功能好处：可以方便的查看到程序运行详情，方便开发调试。



**配置yarn-site.xml：**

`vim yarn-site.xml`打开yarn-site.xml文件后，在`<configuration>`和`</configuration>`之间插入以下内容：

```
<!-- 开启日志聚集功能 -->
<property>
    <name>yarn.log-aggregation-enable</name>
    <value>true</value>
</property>
<!-- 设置日志聚集服务器地址 -->
<property>
    <name>yarn.log.server.url</name>
    <value>http://hadoop102:19888/jobhistory/logs</value>
</property>
<!-- 设置日志保留时间为7天 -->
<property>
    <name>yarn.log-aggregation.retain-seconds</name>
    <value>604800</value>
</property>
```

之后分发配置：`xsync yarn-site.xml`



**关闭NodeManager 、ResourceManager和HistoryServer：**

在hadoop103上调用：`stop-yarn.sh`

在hadoop102上调用： `mapred --daemon stop historyserver`



**启动NodeManager 、ResourceManager和HistoryServer：**

在hadoop103上调用：`start-yarn.sh`

在hadoop102上调用： `mapred --daemon start historyserver`



**测试：**

此时我们再次执行wordcount：`hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-3.1.3.jar wordcount /input /output2`

运行完毕后就能在hadoop102：19888上查看历史任务，然后点击logs查看日志。



#### 8. 编写群起集群脚本myhadoop.sh和查看所有节点jps脚本jpsall

我们可以采用整体启动/停止hdfs和yarn，也可以单独启动/停止hdfs和yarn的组件（hdfs的组件是namenode、datanode和secondarynamenode，yarn的组件是resourcemanager和nodemanager）。

整体启动/停止HDFS：`start-dfs.sh/stop-dfs.sh`

整体启动/停止：`start-yarn.sh/stop-yarn.sh`

单独启动/停止HDFS组件：`hdfs --daemon start/stop namenode/datanode/secondarynamenode`

单独启动/停止YARN组件：`yarn --daemon start/stop resourcemanager/nodemanager`



我们在启动集群时需要在不同的节点上使用不同的命令，这样就很麻烦。我们更希望在某一台机器上使用一个命令就能将集群启动起来，所以我们来编写集群脚本myhadoop.sh。

我们将脚本放在主目录下的bin目录中，`cd /root/bin`然后`vim myhadoop.sh`新建myhadoop.sh文件，输入以下内容：

```
#!/bin/bash

if [ $# -lt 1 ]
then
    echo "No Args Input..."
    exit ;
fi

case $1 in
"start")
        echo " =================== 启动 hadoop集群 ==================="

        echo " --------------- 启动 hdfs ---------------"
        ssh hadoop102 "/opt/module/hadoop-3.1.3/sbin/start-dfs.sh"
        echo " --------------- 启动 yarn ---------------"
        ssh hadoop103 "/opt/module/hadoop-3.1.3/sbin/start-yarn.sh"
        echo " --------------- 启动 historyserver ---------------"
        ssh hadoop102 "/opt/module/hadoop-3.1.3/bin/mapred --daemon start historyserver"
;;
"stop")
        echo " =================== 关闭 hadoop集群 ==================="

        echo " --------------- 关闭 historyserver ---------------"
        ssh hadoop102 "/opt/module/hadoop-3.1.3/bin/mapred --daemon stop historyserver"
        echo " --------------- 关闭 yarn ---------------"
        ssh hadoop103 "/opt/module/hadoop-3.1.3/sbin/stop-yarn.sh"
        echo " --------------- 关闭 hdfs ---------------"
        ssh hadoop102 "/opt/module/hadoop-3.1.3/sbin/stop-dfs.sh"
;;
*)
    echo "Input Args Error..."
;;
esac
```

保存退出后，给予脚本权限：`chmod 777 myhadoop.sh`

之后分发脚本：`xsync myhadoop.sh`

然后分别使用`myhadoop.sh stop` 和 `myhadoop.sh start`看看能否正常停止和启动集群。



我们查看jps时需要切换到每一个节点上挨个调用jps，这样就很麻烦，我们希望能在任意一个节点上调用一个命令就能查看所有节点的jps，所以我们来编写查看所有节点jps脚本jpsall。

同样在/root/bin目录中创建jpsall文件，然后输入以下内容：

```
#!/bin/bash

for host in hadoop102 hadoop103 hadoop104
do
        echo =============== $host ===============
        ssh $host jps
done
```

保存退出后，给予脚本权限：`chmod 777 jpsall`

之后分发脚本：`xsync jpsall`

之后再每个节点上各调用一次看看有没有问题。



# HDFS

## HDFS概述

### 1. HDFS组成架构

<img src="D:\My tepora note\尚硅谷Hadoop.assets\image-20230716112911892.png" alt="image-20230716112911892"  />

1. NameNode（nn）：就是Master，它 是一个主管、管理者。

   NameNode是HDFS中的主节点，负责管理整个文件系统的命名空间和元数据。它记录了文件和数据块的映射关系，并维护了文件系统的目录结构。NameNode还负责处理客户端的读写请求，指导数据节点进行数据的读写操作。由于NameNode存储了整个文件系统的元数据，因此它需要足够的内存和计算资源来支持大规模数据集。

   

2. DateNode（dn）：就是Slave。NameNode 下达命令，DataNode执行实际的操作。

   DataNode是HDFS中的工作节点，负责存储实际的数据块。每个数据节点上都会存储一部分数据块，并定期向NameNode发送心跳信号，报告自己的存活状态和数据块的信息。DataNode主要执行数据的读写操作，根据NameNode的指导，将数据块复制到其他数据节点上以实现数据的冗余和容错性。

   

3. Client：就是客户端。

   客户端是与HDFS交互的应用程序或用户界面。客户端通过与NameNode通信来获取文件的元数据信息，包括文件的大小、分块信息等。然后客户端可以直接与数据节点交互来读取或写入数据。客户端还可以通过HDFS提供的API来进行文件操作，例如创建、删除、移动和重命名文件等。

   

4. Secondary NameNode：并非NameNode的热备。当NameNode挂掉的时候，它并不 能马上替换NameNode并提供服务。

   Secondary NameNode并不是NameNode的备份，它是一个辅助性的节点，用于帮助NameNode管理元数据。Secondary NameNode定期从NameNode获取元数据的快照，并将其保存在本地磁盘上。它还可以协助NameNode进行元数据的合并和清理工作，以提高系统的性能和稳定性。



### 2. HDFS文件块大小

![image-20230716193812331](D:\My tepora note\尚硅谷Hadoop.assets\image-20230716193812331.png)



寻址时间除以1%（即0.01）得到传输时间，然后传输时间乘100MB/s所得结果即为块大小。但是在计算机领域，1024才是整数，所以看求出的块大小接近128MB还是256MB，接近哪个，块大小就设置成哪个。



**为什么块的大小不能设置太小，也不能设置太大？**

1. HDFS的块设置太小，会增加寻址时间，程序一直在找块的开始位置。
2. 如果块设置的太大，从磁盘传输数据的时间会明显大于定位这个块开 始位置所需的时间。导致程序在处理这块数据时，会非常慢。



总结：：**HDFS块的大小设置主要取决于磁盘传输速率**（即磁盘读写速度）。在企业开发中，常见的为128MB或256MB，一般大公司用256MB。中小公司用128MB。





## HDFS的操作

HDFS的操作分为Shell操作和API操作，当然也包括在网页上直接操作。



### HDFS的Shell操作

**基本语法：**

```
方式一：hadoop fs 具体命令
方式二：hdfs dfs 具体命令
```

方式一和方式二的效果是完全相同的，且命令的前面都有一个杠`-`。



**命令大全：**

输入`hadoop fs`就能看见所以的命令：

```
[root@hadoop102 ~]# hadoop fs
Usage: hadoop fs [generic options]
	[-appendToFile [-n] <localsrc> ... <dst>]
	[-cat [-ignoreCrc] <src> ...]
	[-checksum [-v] <src> ...]
	[-chgrp [-R] GROUP PATH...]
	[-chmod [-R] <MODE[,MODE]... | OCTALMODE> PATH...]
	[-chown [-R] [OWNER][:[GROUP]] PATH...]
	[-concat <target path> <src path> <src path> ...]
	[-copyFromLocal [-f] [-p] [-l] [-d] [-t <thread count>] [-q <thread pool queue size>] <localsrc> ... <dst>]
	[-copyToLocal [-f] [-p] [-crc] [-ignoreCrc] [-t <thread count>] [-q <thread pool queue size>] <src> ... <localdst>]
	[-count [-q] [-h] [-v] [-t [<storage type>]] [-u] [-x] [-e] [-s] <path> ...]
	[-cp [-f] [-p | -p[topax]] [-d] [-t <thread count>] [-q <thread pool queue size>] <src> ... <dst>]
	[-createSnapshot <snapshotDir> [<snapshotName>]]
	[-deleteSnapshot <snapshotDir> <snapshotName>]
	[-df [-h] [<path> ...]]
	[-du [-s] [-h] [-v] [-x] <path> ...]
	[-expunge [-immediate] [-fs <path>]]
	[-find <path> ... <expression> ...]
	[-get [-f] [-p] [-crc] [-ignoreCrc] [-t <thread count>] [-q <thread pool queue size>] <src> ... <localdst>]
	[-getfacl [-R] <path>]
	[-getfattr [-R] {-n name | -d} [-e en] <path>]
	[-getmerge [-nl] [-skip-empty-file] <src> <localdst>]
	[-head <file>]
	[-help [cmd ...]]
	[-ls [-C] [-d] [-h] [-q] [-R] [-t] [-S] [-r] [-u] [-e] [<path> ...]]
	[-mkdir [-p] <path> ...]
	[-moveFromLocal [-f] [-p] [-l] [-d] <localsrc> ... <dst>]
	[-moveToLocal <src> <localdst>]
	[-mv <src> ... <dst>]
	[-put [-f] [-p] [-l] [-d] [-t <thread count>] [-q <thread pool queue size>] <localsrc> ... <dst>]
	[-renameSnapshot <snapshotDir> <oldName> <newName>]
	[-rm [-f] [-r|-R] [-skipTrash] [-safely] <src> ...]
	[-rmdir [--ignore-fail-on-non-empty] <dir> ...]
	[-setfacl [-R] [{-b|-k} {-m|-x <acl_spec>} <path>]|[--set <acl_spec> <path>]]
	[-setfattr {-n name [-v value] | -x name} <path>]
	[-setrep [-R] [-w] <rep> <path> ...]
	[-stat [format] <path> ...]
	[-tail [-f] [-s <sleep interval>] <file>]
	[-test -[defswrz] <path>]
	[-text [-ignoreCrc] <src> ...]
	[-touch [-a] [-m] [-t TIMESTAMP (yyyyMMdd:HHmmss) ] [-c] <path> ...]
	[-touchz <path> ...]
	[-truncate [-w] <length> <path> ...]
	[-usage [cmd ...]]

Generic options supported are:
-conf <configuration file>        specify an application configuration file
-D <property=value>               define a value for a given property
-fs <file:///|hdfs://namenode:port> specify default filesystem URL to use, overrides 'fs.defaultFS' property from configurations.
-jt <local|resourcemanager:port>  specify a ResourceManager
-files <file1,...>                specify a comma-separated list of files to be copied to the map reduce cluster
-libjars <jar1,...>               specify a comma-separated list of jar files to be included in the classpath
-archives <archive1,...>          specify a comma-separated list of archives to be unarchived on the compute machines

The general command line syntax is:
command [genericOptions] [commandOptions]
```



此外，还可以通过-help来获得某个命令的具体用法：

-help：输出这个命令参数

```
[root@hadoop102 ~]# hadoop fs -help setrep
-setrep [-R] [-w] <rep> <path> ... :
  Set the replication level of a file. If <path> is a directory then the command
  recursively changes the replication factor of all files under the directory tree
  rooted at <path>. The EC files will be ignored here.
                                                                                 
  -w  It requests that the command waits for the replication to complete. This   
      can potentially take a very long time.                                     
  -R  It is accepted for backwards compatibility. It has no effect.   
```



#### 1. 上传

-moveFromLocal：从本地剪切粘贴到HDFS

```
hadoop fs  -moveFromLocal  ./shuguo.txt  /sanguo
```



-copyFromLocal：从本地文件系统中拷贝文件到HDFS路径去

```
hadoop fs -copyFromLocal weiguo.txt /sanguo
```



-put：等同于copyFromLocal，生产环境更习惯用put

```
hadoop fs -put ./wuguo.txt /sanguo
```



-appendToFile：追加一个文件到已经存在的文件末尾

```
hadoop fs -appendToFile liubei.txt /sanguo/shuguo.txt
```



#### 2. 下载

-copyToLocal：从HDFS拷贝到本地

```
hadoop fs -copyToLocal /sanguo/shuguo.txt ./
```



-get：等同于copyToLocal，生产环境更习惯用get

```
hadoop fs -get /sanguo/shuguo.txt ./shuguo2.txt
```



#### 3. HDFS直接操作

与Linux命令非常相像，若命令后面还要跟上参数的话，则必须**空一格**再加参数！！千万不能像Linux一样将两个参数合在一起写！！！

如：`rm`后可接两个参数`-r`和`-f`，在Linux中我们可以写`rm -rf`，但是在Shell中我们不能这样写`hadoop fs -rm -rf` ，得写成`hadoop fs -rm -r -f`。

```
[root@hadoop102 ~]# hadoop fs -rm -r -f /xiyou
Deleted /xiyou
```

当然，`-f`表示强制删除，不需要询问我们。而hdfs不会问我们，所以敲`hadoop fs -rm -r`就行了，只不过敲`-f`也不会报错啦。



-ls: 显示目录信息

```
hadoop fs -ls /sanguo
```



-cat：显示文件内容

```
hadoop fs -cat /sanguo/shuguo.txt
```



-chgrp、-chmod、-chown：Linux文件系统中的用法一样，修改文件所属权限

```
hadoop fs  -chgrp  root /sanguo/shuguo.txt
hadoop fs  -chmod  666  /sanguo/shuguo.txt
hadoop fs  -chown  root:root   /sanguo/shuguo.txt
```



-mkdir：创建路径

```
hadoop fs -mkdir /jinguo
```



-cp：从HDFS的一个路径拷贝到HDFS的另一个路径

```
hadoop fs -cp /sanguo/shuguo.txt /jinguo
```



-mv：在HDFS目录中移动文件

```
hadoop fs -mv /sanguo/weiguo.txt /jinguo
hadoop fs -mv /Lecture1.pdf /guoting.pdf
```

注：当目的地是目录时，就是单纯的移动文件。若目的地是文件，则相当于剪切过去再重命名。没错，给文件或目录重命名的实质就是剪切并重命名，只不过路径没有发生变化只更改名字罢了。



-tail：显示一个文件的末尾1kb的数据

```
hadoop fs -tail /jinguo/shuguo.txt
```



-rm：删除文件或文件夹

```
hadoop fs -rm /sanguo/shuguo.txt
```



-rm -r：递归删除目录及目录里面内容

```
hadoop fs -rm -r /sanguo
```



-du：统计文件夹的大小信息

```
hadoop fs -du /jinguo
```

当然，我们一般使用命令`-du`会加上`-h`，这样文件大小更容易读。此外，更常用的命令是`hadoop fs -du -s -h`，只显示总和，和Linux一样。只不过需要注意别把`-s -h`写成`-sh`就行。



-setrep：设置HDFS中文件的副本数量

```
hadoop fs -setrep 10 /jinguo/shuguo.txt
```

注意：这里设置的副本数只是记录在NameNode的元数据中，是否真的会有这么多副本，还得看DataNode的数量。因为目前只有3台设备，最多也就3个副本，只有节点数的增加到10台时，副本数才能达到10。



### HDFS的API操作

#### 1. 客户端环境准备

**配置依赖：**

首先需要下载与hadoop**同版本**的**Windows依赖文件**，并拷贝到非中文路径。

然后配置系统**HADOOP_HOME**环境变量。

接着配置**Path**环境变量。（**注意：如果环境变量不起作用，可以重启电脑试试。**）

最后验证Hadoop环境变量是否正常：双击winutils.exe，若出现一个一闪而过的窗口则说明正常。



**配置Maven：**

可以按照一位大神写的教程（[传送门](https://blog.csdn.net/weixin_44630012/article/details/122279002)）一步步配。只需要注意正确输入**自己的hadoop和jdk版本**即可，除非版本一模一样，否则千万别直接无脑复制粘贴！



#### 2. IDEA操作

**新建工程HDFSClient：**

点击`new project`，输入名字`HDFSClient`，将`Build system`选择为`Maven`，最后修改`Advanced Settings`中的`Groupld`即可（这个选项相当于设置包名，一般采用公司域名的倒置作为名字）。



**修改IDEA中的Maven配置：**

点开设置，搜索"maven"，然后点击"Maven"（直接点，别将它展开），再将系统自带的Maven其修改为自己配置的，需要修改的地方有三处，按照自己的Maven进行相应的设置即可。

![image-20230716205139204](D:\My tepora note\尚硅谷Hadoop.assets\image-20230716205139204.png)



然后点击设置中目前所在的"Maven"下面的"Importing"，找到"VM options for importer"，输入下面这行话：

```
-Dmaven.wagon.http.ssl.insecure=true -Dmaven.wagon.http.ssl.allowall=true 
```

![image-20230716205502797](D:\My tepora note\尚硅谷Hadoop.assets\image-20230716205502797.png)

这样设置是：忽略SSL证书验证，防止连不到阿里云的服务器无法自动下载依赖，而出现找不到依赖的错误 。



**导入相应的依赖坐标+日志添加：**

创建完Maven工程后会自动生成一些代码，在其中的`</properties>`与`</project>`之间添加以下内容（注意自己的hadoop版本）：

```
<dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>3.1.3</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>2.0.5</version>
        </dependency>
    </dependencies>
```

因为是首次添加，所以需要让idea联网下载一些文件：点击右边侧边栏的`Maven`，然后点击页面顶端的刷新图标，之后耐心等待即可。



**新建文件log4j.properties：**

在项目的src/main/resources目录下，新建一个文件，命名为“log4j.properties”，在文件中填入以下内容：

```
log4j.rootLogger=INFO, stdout  
log4j.appender.stdout=org.apache.log4j.ConsoleAppender  
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout  
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m%n  
log4j.appender.logfile=org.apache.log4j.FileAppender  
log4j.appender.logfile.File=target/spring.log  
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout  
log4j.appender.logfile.layout.ConversionPattern=%d %p [%c] - %m%n
```



**创建包com.sunhao名.hdfs并创建Java类HdfsClient：**

之后就可以在这个类中调用API控制hdfs了。

```java
package com.sunhao.hdfs;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.*;
import org.junit.After;
import org.junit.Before;
import org.junit.Test;

import java.io.IOException;
import java.net.URI;
import java.net.URISyntaxException;
import java.util.Arrays;

/**
 * 客户端代码常用套路：
 * 1. 获取一个客户端对象
 * 2. 执行相关的操作命令
 * 3. 关闭资源
 */

public class HdfsClient {

    private FileSystem fs;

    @Before
    public void init() throws IOException, InterruptedException, URISyntaxException {

        // 连接集群的nn地址
        URI uri = new URI("hdfs://hadoop102:8020");
        // 创建一个配置文件
        Configuration configuration = new Configuration();
        configuration.set("dfs.replication", "2"); // 修改副本数
        // 用户
        String user = "root";

        // 1. 获取到了客户端对象
        fs = FileSystem.get(uri, configuration, user);
    }

    @After
    public void close() throws IOException {

        // 3. 关闭资源
        fs.close();

    }

    @Test
    public void testMkdir() throws URISyntaxException, IOException, InterruptedException {
        
        // 2. 创建一个文件夹 
        fs.mkdirs(new Path("/xiyou/huaguoshang"));
        
    }

    /**
     * 参数优先级：
     * hdfs-default.xml -> hdfs-site.xml -> 在项目资源目录下的配置文件 -> 代码里面的配置
     *
     * @throws IOException
     */
    @Test
    public void testPut() throws IOException {
        fs.copyFromLocalFile(false, true, new Path("D:\\testapi\\sunwukong.txt"), new Path("/xiyou/huaguoshang/"));
    }

    @Test
    public void testRm() throws IOException {
        fs.delete(new Path("/xiyou/huaguoshang/sunwukong.txt"), false);
    }

    @Test
    public void testMv() throws IOException {

        // 移动
        fs.rename(new Path("/sunwukong.txt"), new Path("/xiyou/huaguoshang/sunwukong.txt"));

        // 重命名
        //fs.rename(new Path("/xiyou/huaguoshang/sunwukong.txt"), new Path("/xiyou/huaguoshang/wukong.txt"));
    }

    @Test
    public void testGet() throws IOException {
        fs.copyToLocalFile(false, new Path("/xiyou/huaguoshang"), new Path("D:\\testapi"), false);
    }

    @Test
    public void testListFiles() throws IOException {

        RemoteIterator<LocatedFileStatus> listFiles = fs.listFiles(new Path("/"), true);
        while (listFiles.hasNext()) {
            LocatedFileStatus fileStatus =  listFiles.next();
            System.out.println("==========" + fileStatus.getPath() + "==========");
            System.out.println(fileStatus.getPermission());
            System.out.println(fileStatus.getOwner());
            System.out.println(fileStatus.getGroup());
            System.out.println(fileStatus.getLen());
            System.out.println(fileStatus.getAccessTime());
            System.out.println(fileStatus.getReplication());
            System.out.println(fileStatus.getBlockSize());
            System.out.println(fileStatus.getPath().getName());

            // 获取块信息
            BlockLocation[] blockLocations = fileStatus.getBlockLocations();
            System.out.println(Arrays.toString(blockLocations));
        }
    }

    @Test
    public void testFiles() throws IOException {

        FileStatus[] fileStatuses = fs.listStatus(new Path("/"));
        for (FileStatus fileStatus: fileStatuses) {
            if (fileStatus.isFile()) System.out.println("文件：" + fileStatus.getPath().getName());
            else System.out.println("文件目录：" + fileStatus.getPath().getName());
        }

    }

}
```



**客户端代码常用套路：**

1. 获取一个客户端对象
2. 执行相关的操作命令
3. 关闭资源



**参数优先级：**

hdfs-default.xml -> hdfs-site.xml -> 在项目资源目录下的配置文件 -> 代码里面的配置



## HDFS的读写流程

### 1. HDFS写数据流程

![image-20230716211105716](D:\My tepora note\尚硅谷Hadoop.assets\image-20230716211105716.png)



具体步骤为：

1. 客户端创建Distributed FileSystem对象，然后通过它和大哥NameNode谈判：请求上传文件。
2. NameNode检查是否可以创建：检查它是否有权限创建；检查目标文件是否已存在，父目录是否存在。然后进行响应：返回是否可以上传。
3. 客户端请求第一个 Block上传到哪几个DataNode服务器上。
4. NameNode根据相应的规则（本地节点，其他机架上的一个节点，与第二个节点同机架上的另一个节点）返回3个DataNode节点，分别为dn1、dn2、dn3。
5. 客户端通过FSDataOutputStream模块请求dn1上传数据，dn1收到请求会继续调用dn2，然后dn2调用dn3，将这个通信管道建立完成。
6. dn1、dn2、dn3逐级应答客户端。
7. 客户端开始往dn1上传第一个Block（先从磁盘读取数据放到一个本地内存缓存），以Packet为单位，dn1收到一个Packet就会传给dn2，dn2传给dn3；dn1每传一个packet会放入一个应答队列等待应答。
8. 当一个Block传输完成之后，客户端再次请求NameNode上传第二个Block的服务器（重复执行3-7步）。



注意：hdfs写数据时是将全部块给dn1，然后再由dn1依次传给剩余的节点。同时会产生一个ack队列，用于接收下一端是否应答成功。都应答成功后其存储的数据才会删除，否则把未应答的数据的重新塞给发送数据的packet队列。



### 2. 网络拓扑-节点距离计算

在HDFS写数据的过程中，NameNode会选择距离待上传数据最近距离的DataNode接收数据。

**节点距离：两个节点到达最近的共同祖先的距离总和。**

注意：若这两个节点是同一个节点，那么它自己就是他们的共同祖先，所以此时距离和为0，这两个节点之间的节点距离为0。且同一个机架上的节点距离该机架的距离都是一样的，都是1。

![image-20230716212659782](D:\My tepora note\尚硅谷Hadoop.assets\image-20230716212659782.png)

若两个节点不是同一个节点，那么其实可以直接将这两个节点形成的一条线拉成一条直线，然后数距离，即为这两个节点之间的节点距离。



### 3. 副本节点选择

![image-20230716212912742](D:\My tepora note\尚硅谷Hadoop.assets\image-20230716212912742.png)



副本节点选择策略包含了就近原则和负载均衡。



### 4. HDFS读数据流程

![image-20230716213122720](D:\My tepora note\尚硅谷Hadoop.assets\image-20230716213122720.png)

具体步骤为：

1. 客户端通过DistributedFileSystem向NameNode请求下载文件，NameNode通过查询元数据，找到文件块所在的DataNode地址。
2. 挑选一台DataNode（就近原则，然后随机）服务器，请求读取数据。
3. DataNode开始传输数据给客户端（从磁盘里面读取数据输入流，以Packet为单位来做校验）。
4. 客户端以Packet为单位接收，先在本地缓存，然后写入目标文件。



注意：hdfs读数据时是串行的，读完一个块再读下一个块。



# MapReduce

## MapReduce 概述

### 1. MapReduce优缺点

**优点：**

1. 易于编程

   它简单的实现一些接口，就可以完成一个分布式程序，这个分布式程序可以分布到大量廉价的PC机器上运行。也就是说你写一个分布式程序，跟写一个简单的串行程序是一模一样的。就是因为这个特点使得MapReduce编程变得非常流行。

2. 良好的扩展性

   当你的计算资源不能得到满足的时候，你可以通过简单的增加机器来扩展它的计算能力。

3. 高容错性

   MapReduce 设计的初衷就是使程序能够部署在廉价的 PC 机器上，这就要求它具有很高 的容错性。比如其中一台机器挂了，它可以把上面的计算任务转移到另外一个节点上运行， 不至于这个任务运行失败，而且这个过程不需要人工参与，而完全是由 Hadoop 内部完成的。

4. 适合 PB 级以上海量数据的离线处理

   可以实现上千台服务器集群并发工作，提供数据处理能力。



**缺点：**

1. 不擅长实时计算

   MapReduce 无法像 MySQL 一样，在毫秒或者秒级内返回结果。

2. 不擅长流式计算

   流式计算的输入数据是动态的，而 MapReduce 的输入数据集是静态的，不能动态变化。 这是因为 MapReduce 自身的设计特点决定了数据源必须是静态的。

3. 不擅长 DAG（有向无环图）计算

   多个应用程序存在依赖关系，后一个应用程序的输入为前一个的输出。在这种情况下， MapReduce 并不是不能做，而是使用后，每个 MapReduce 作业的输出结果都会写入到磁盘， 会造成大量的磁盘 IO，导致性能非常的低下。



### 2. MapReduce 核心思想

![image-20230723211045681](D:\My tepora note\尚硅谷Hadoop.assets\image-20230723211045681.png)



分布式的运算程序往往需要分成至少2个阶段：

- 第一个阶段的MapTask并发实例，完全并行运行，互不相干。
- 第二个阶段的 ReduceTask 并发实例互不相干，但是他们的数据依赖于上一个阶段 的所有 MapTask 并发实例的输出。



MapReduce 编程模型只能包含一个 Map 阶段和一个 Reduce 阶段，如果用户的业 务逻辑非常复杂，那就只能多个 MapReduce 程序，串行运行。



### 3. MapReduce 进程

一个完整的 MapReduce 程序在分布式运行时有三类实例进程：

1. MrAppMaster：负责整个程序的过程调度及状态协调。
2. MapTask：负责 Map 阶段的整个数据处理流程。
3. ReduceTask：负责 Reduce 阶段的整个数据处理流程。



### 4. 官方 WordCount 

采用反编译工具反编译源码，发现 WordCount 案例有 Map 类、Reduce 类和驱动类。且 数据的类型是 Hadoop 自身封装的序列化类型。



**常用数据序列化类型：**

![image-20230723211419223](D:\My tepora note\尚硅谷Hadoop.assets\image-20230723211419223.png)



### 5. MapReduce 编程规范

用户编写的程序分成三个部分：Mapper、Reducer 和 Driver。

![image-20230723211503184](D:\My tepora note\尚硅谷Hadoop.assets\image-20230723211503184.png)

![image-20230723211516446](D:\My tepora note\尚硅谷Hadoop.assets\image-20230723211516446.png)



### 6. WordCount 案例实操

#### 本地测试

需求：统计一堆文件中单词出现的个数（WordCount案例）

![image-20230723211742776](D:\My tepora note\尚硅谷Hadoop.assets\image-20230723211742776.png)



**环境准备：**

1. 在idea新建Maven工程MapReduceDemo后，在pom.xml文件`</project>`前添加下列内容：

```
<dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>3.1.3</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>2.0.5</version>
        </dependency>
    </dependencies>
```



2. 在项目的 src/main/resources 目录下，新建一个文件，命名为“log4j.properties”，再填入以下内容：

```
log4j.rootLogger=INFO, stdout  
log4j.appender.stdout=org.apache.log4j.ConsoleAppender  
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout  
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m%n  
log4j.appender.logfile=org.apache.log4j.FileAppender  
log4j.appender.logfile.File=target/spring.log  
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout  
log4j.appender.logfile.layout.ConversionPattern=%d %p [%c] - %m%n
```



3. 创建包名com.sunhao.mapreduce.wordcount



**编写程序：**

Mapper类：

```java
package com.sunhao.mapreduce.wordcount;

import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;

import java.io.IOException;

public class WordCountMapper extends Mapper<LongWritable, Text, Text, IntWritable> {

    Text k = new Text();
    IntWritable v = new IntWritable(1);

    @Override
    protected void map(LongWritable key, Text value, Mapper<LongWritable, Text, Text, IntWritable>.Context context) throws IOException, InterruptedException {
        // 1. 获取一行
        String line = value.toString();

        // 2. 切割
        String[] words = line.split(" ");

        // 3. 输出
        for (String word : words) {
            k.set(word);
            context.write(k, v);
        }
    }
}
```



Reducer类：

```java
package com.sunhao.mapreduce.wordcount;

import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

import java.io.IOException;

public class WordCountReducer extends Reducer<Text, IntWritable, Text, IntWritable> {

    int sum;
    IntWritable v = new IntWritable();

    @Override
    protected void reduce(Text key, Iterable<IntWritable> values, Reducer<Text, IntWritable, Text, IntWritable>.Context context) throws IOException, InterruptedException {

        // 1. 累加求和
        sum = 0;
        for (IntWritable value : values) {
            sum += value.get();
        }

        // 2. 输出
        v.set(sum);
        context.write(key, v);
    }
}
```



Driver驱动类：

```java
package com.sunhao.mapreduce.wordcount;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

import java.io.IOException;

public class WordCountDriver {

    public static void main(String[] args) throws IOException, InterruptedException, ClassNotFoundException {

        // 1. 获取配置信息以及获取job对象
        Configuration conf = new Configuration();
        Job job = Job.getInstance(conf);

        // 2. 关联本Driver程序的jar
        job.setJarByClass(WordCountDriver.class);

        // 3. 关联Mapper和Reducer的jar
        job.setMapperClass(WordCountMapper.class);
        job.setReducerClass(WordCountReducer.class);

        // 4. 设置Mapper输出的key和value类型
        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(IntWritable.class);

        // 5. 设置最终输出的key和value类型
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);

        // 6. 设置输入和输出路径
        FileInputFormat.setInputPaths(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));

        // 7. 提交job
        boolean result = job.waitForCompletion(true);
        System.exit(result ? 0 : 1);
    }
}
```



#### 提交到集群测试

1. 用 maven 打 jar 包，在pom.xml文件添加下列打包插件依赖：

```
<build>
        <plugins>
            <plugin>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.6.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <configuration>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
```

注：有三处地方会爆红，但是可以不用管，可以正常运行，我也不知道为什么。



2. 将程序打成 jar 包

   成功后会出现两个jar包，一个是不带依赖的，很小，另一个带了依赖，大很多。但是我们只需要将不带依赖的上传到集群。



3. 修改不带依赖的 jar 包名称为 wc.jar，并拷贝该 jar 包到 Hadoop 集群的 /opt/module/hadoop-3.1.3 路径。



4. 启动集群并执行WordCount程序

```
hadoop jar  wc.jar
 com.sunhao.mapreduce.wordcount.WordCountDriver /input /output
```

注意：此处在wc.jar后面需要跟上Driver类的全类名，因为我们的程序中Driver类有main方法，是程序的入口。

只需在Driver类页面右击鼠标，光标移到Copy/Paste Special，选中Copy Reference即可。

![image-20230723213723216](D:\My tepora note\尚硅谷Hadoop.assets\image-20230723213723216.png)



## Hadoop序列化

### 1. 序列化概述

**序列化：**序列化就是把内存中的对象，转换成字节序列（或其他数据传输协议）以便于存储到磁 盘（持久化）和网络传输。

**反序列化：**反序列化就是将收到字节序列（或其他数据传输协议）或者是磁盘的持久化数据，转换 成内存中的对象。



**为什么要用序列化：**

一般来说，“活的”对象只生存在内存里，关机断电就没有了。而且“活的”对象只能 由本地的进程使用，不能被发送到网络上的另外一台计算机。 然而序列化可以存储“活的” 对象，可以将“活的”对象发送到远程计算机。



**为什么不用Java的序列化：**

Java 的序列化是一个重量级序列化框架（Serializable），一个对象被序列化后，会附带 很多额外的信息（各种校验信息，Header，继承体系等），不便于在网络中高效传输。所以， Hadoop 自己开发了一套序列化机制（Writable）。



**Hadoop 序列化特点：**

- 紧凑 ：高效使用存储空间
- 快速：读写数据的额外开销小。
- 互操作：支持多语言的交互



### 2. 自定义 bean 对象实现序列化接口（Writable）

在企业开发中往往常用的基本序列化类型不能满足所有需求，比如在 Hadoop 框架内部 传递一个 bean 对象，那么该对象就需要实现序列化接口。



具体实现 bean 对象序列化步骤如下 7 步：

1. 必须实现 Writable 接口
2. 反序列化时，需要反射调用空参构造函数，所以必须有空参构造

```java
public FlowBean() {
    }
```

3. 重写序列化方法

```
@Override
    public void write(DataOutput dataOutput) throws IOException {

        dataOutput.writeLong(upFlow);
        dataOutput.writeLong(downFlow);
        dataOutput.writeLong(sumFlow);
    }
```

4. 重写反序列化方法

```java
@Override
    public void readFields(DataInput dataInput) throws IOException {

        this.upFlow = dataInput.readLong();
        this.downFlow = dataInput.readLong();
        this.sumFlow = dataInput.readLong();
    }
```

5. **注意反序列化的顺序和序列化的顺序完全一致**（upFlow、downFlow和sunFlow的顺序）！！！
6. 要想把结果显示在文件中，需要重写 toString()，可用"\t"分开，方便后续用。

```java
@Override
    public String toString() {
        return upFlow + "\t" + downFlow + "\t" + sumFlow;
    }
```

7. 如果需要将自定义的 bean 放在 key 中传输，则还需要实现 Comparable 接口，因为 MapReduce 框中的 Shuffle 过程要求对 key 必须能排序。详见后面排序案例。

```java
@Override
public int compareTo(FlowBean o) {
	// 倒序排列，从大到小
	return this.sumFlow > o.getSumFlow() ? -1 : 1;
}
```



### 3. 序列化案例实操

**需求分析：**

![image-20230723220511066](D:\My tepora note\尚硅谷Hadoop.assets\image-20230723220511066.png)



注意：有的数据网络ip后面没有域名，有的数据后面跟上了域名。

![image-20230723220417634](D:\My tepora note\尚硅谷Hadoop.assets\image-20230723220417634.png)



**编写程序：**

FlowBean对象：

```java
package com.sunhao.mapreduce.writable;

import org.apache.hadoop.io.Writable;

import java.io.DataInput;
import java.io.DataOutput;
import java.io.IOException;

// 1. 继承Writable接口
public class FlowBean implements Writable {

    private long upFlow; // 上行流量
    private long downFlow; // 下行流量
    private long sumFlow; // 总流量

    // 2. 提供无参构造
    public FlowBean() {
    }

    // 3. 提供getter/setter
    public long getUpFlow() {
        return upFlow;
    }

    public void setUpFlow(long upFlow) {
        this.upFlow = upFlow;
    }

    public long getDownFlow() {
        return downFlow;
    }

    public void setDownFlow(long downFlow) {
        this.downFlow = downFlow;
    }

    public long getSumFlow() {
        return sumFlow;
    }

    public void setSumFlow(long sumFlow) {
        this.sumFlow = sumFlow;
    }

    public void setSunFlow() {
        this.sumFlow = this.upFlow + this.downFlow;
    }

    // 4. 实现序列化和反序列化方法时，注意顺序一定要保持一致（upFlow、downFlow和sunFlow的顺序）！！！
    @Override
    public void write(DataOutput dataOutput) throws IOException {

        dataOutput.writeLong(upFlow);
        dataOutput.writeLong(downFlow);
        dataOutput.writeLong(sumFlow);
    }

    @Override
    public void readFields(DataInput dataInput) throws IOException {

        this.upFlow = dataInput.readLong();
        this.downFlow = dataInput.readLong();
        this.sumFlow = dataInput.readLong();
    }

    // 5. 重写toString方法
    @Override
    public String toString() {
        return upFlow + "\t" + downFlow + "\t" + sumFlow;
    }
}
```



Mapper类：

```java
package com.sunhao.mapreduce.writable;

import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;

import java.io.IOException;

public class FlowMapper extends Mapper<LongWritable, Text, Text, FlowBean> {

    private Text k = new Text();
    private FlowBean v = new FlowBean();

    @Override
    protected void map(LongWritable key, Text value, Mapper<LongWritable, Text, Text, FlowBean>.Context context) throws IOException, InterruptedException {

        // 1. 获取一行数据
        String line = value.toString();

        // 2. 切割
        String[] split = line.split("\t");

        // 3. 获取我们需要的数据：手机号、上行流量和下行流量
        String phone = split[1];
        String up = split[split.length - 3];
        String down = split[split.length - 2];

        // 4. 封装k和v
        k.set(phone);
        v.setUpFlow(Long.parseLong(up));
        v.setDownFlow(Long.parseLong(down));
        v.setSunFlow();

        // 5. 写出k和v
        context.write(k, v);
    }
}
```



Reducer类：

```java
import org.apache.hadoop.mapreduce.Reducer;

import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

import java.io.IOException;

public class FlowReducer extends Reducer<Text, FlowBean, Text, FlowBean> {

    private FlowBean value = new FlowBean();

    @Override
    protected void reduce(Text key, Iterable<FlowBean> values, Reducer<Text, FlowBean, Text, FlowBean>.Context context) throws IOException, InterruptedException {

        long sumUp = 0;
        long sumDown = 0;

        // 1. 遍历values，将其中的上行流量和下行流量分别加起来
        for (FlowBean flowBean : values) {
            sumUp += flowBean.getUpFlow();
            sumDown += flowBean.getDownFlow();
        }

        // 2. 封装value
        value.setUpFlow(sumUp);
        value.setDownFlow(sumDown);
        value.setSunFlow();

        // 3. 写出key和value
        context.write(key, value);
    }
}
```



Driver驱动类：

```java
package com.sunhao.mapreduce.writable;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

import java.io.IOException;

public class FlowDriver {

    public static void main(String[] args) throws IOException, InterruptedException, ClassNotFoundException {

        // 1. 获取job对象
        Configuration conf = new Configuration();
        Job job = Job.getInstance(conf);

        // 2. 关联Driver类
        job.setJarByClass(FlowDriver.class);

        // 3. 关联Mapper和Reducer
        job.setMapperClass(FlowMapper.class);
        job.setReducerClass(FlowReducer.class);

        // 4. 设置Map端输出kv类型
        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(FlowBean.class);

        // 5. 设置程序最终输出的kv类型
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(FlowBean.class);

        // 6. 设置程序的输入输出路径
        FileInputFormat.setInputPaths(job, new Path("D:\\hadoop\\mapreduce\\input\\flow"));
        FileOutputFormat.setOutputPath(job, new Path("D:\\hadoop\\mapreduce\\output\\flow"));

        // 7. 提交job
        boolean result = job.waitForCompletion(true);
        System.exit(result ? 0 : 1);
    }
}
```



## MapReduce 框架原理

![image-20230723221645100](D:\My tepora note\尚硅谷Hadoop.assets\image-20230723221645100.png)

MapReduce包括Input、Mapper、Reducer和Output四个阶段，其中Mapper和Reducer之间的数据混洗的阶段称作Shuffle，且Shuffle包括分区、排序和Combiner。



### 1. InputFormat

InputFormat包含了很多子类，如FileInputFormat、TextInputFormat和CombinerTextInputFormat。



#### 1.0 MapTask并行度决定机制

**数据块：**Block是HDFS物理上把数据分成一块一块。数据块是HDFS存储数据单位。

**数据切片：**数据切片只是在逻辑上对输入进行分片，并不会在磁盘上将其切分成片进行存储。数据切片是MapReduce程序计算输入数据的单位，一个切片会对应启动一个MapTask。



![image-20230730214059989](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730214059989.png)

即有多少个切片就有多少个MapTask，且默认情况下切片大小等于**块大小**。此外，由于默认情况下InputFormat为TextInputFormat，而TextInputFormat又是FileInputFormat的实现类，所以默认情况下hadoop会对每一个文件进行单独的切片，不会将他们合在一起进行切片。



#### 1.1 FileInputFormat

FileInputFormat让Hadoop给每一个文件单独进行切片，不管文件到底有多小，也是默认情况下hadoop的做法。若想要令多个文件合在一起进行切片，则需使用CombinerTextInputFormat。

![image-20230730214454121](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730214454121.png)

![image-20230730214833905](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730214833905.png)



FileInputFormat常见的接口实现类包括：TextInputFormat、KeyValueTextInputFormat、NLineInputFormat、CombineTextInputFormat和自定义InputFormat等。



**切片源码解析：**

![image-20230730215555148](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730215555148.png)



#### 1.2 TextInputFormat

故名思意，TextInputFormat是一行一行读取文件内容的。

TextInputFormat是默认的FileInputFormat实现类，为每一个文件单独进行切片。

键是存储该行在整个文件中的起始字节偏移量， LongWritable类型。值是这行的内容，不包括任何行终止符（换行符和回车符），为Text类型。



#### 1.3 CombineTextInputFormat

CombineTextInputFormat用于小文件过多的场景，它可以将多个小文件从逻辑上规划到一个切片中，这样，多个小文件就可以交给一个MapTask处理。

CombineTextInputFormat使用虚拟存储，可以认为进行设置：`CombineTextInputFormat.setMaxInputSplitSize(job, 4194304);// 4m`。

注意：虚拟存储切片最大值设置最好根据实际的小文件大小情况来设置具体的值。



**切片机制：**

生成切片过程包括：虚拟存储过程和切片过程两部分：

![image-20230730215408451](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730215408451.png)



**案例实操：**

需求：将输入的大量小文件合并成一个切片统一处理。

只需要在WordCount案例的Driver驱动类上做一点小小的修改即可。

```java
// 设置CombineTextInputFormat  若不设置那么默认是TextInputFormat，number of splits:4
        job.setInputFormatClass(CombineTextInputFormat.class);

        // 设置虚拟存储切片最大值设置4m  number of splits:3
        CombineTextInputFormat.setMaxInputSplitSize(job, 4194304);

        // 设置虚拟存储切片最大值设置20m  number of splits:1
//        CombineTextInputFormat.setMaxInputSplitSize(job, 20971520);
```

不同设置的区别写在注释上了。



### 2. MapReduce工作流程

![image-20230730220230751](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730220230751.png)

![image-20230730220329310](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730220329310.png)



注意：ReduceTask是主动向MapTask**拉取**自己负责的分区的数据，不是MapTask主动给它的！！！



### 3. Shuffle

Map方法之后，Reduce方法之前的数据处理过程称之为Shuffle。

![image-20230730220547817](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730220547817.png)





### 4. Partition分区

**默认Partitioner分区：**

![image-20230730220725042](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730220725042.png)



**自定义Parititioner分区：**

![image-20230730220817507](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730220817507.png)



**分区总结：**

![image-20230730220916625](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730220916625.png)



![image-20230730220943379](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730220943379.png)



**案例实操：**

需求：将统计结果按照手机归属地不同省份输出到不同文件中（分区），即：手机号136、137、138、139开头都分别放到一个独立的4个文件中，其他开头的放到一个文件中。



我们只需在统计流量的案例的基础上进行修改即可。

Driver驱动类：

```java
package com.sunhao.mapreduce.partitioner;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

import java.io.IOException;

public class FlowDriver {

    public static void main(String[] args) throws IOException, InterruptedException, ClassNotFoundException {

        // 1. 获取job对象
        Configuration conf = new Configuration();
        Job job = Job.getInstance(conf);

        // 2. 关联Driver类
        job.setJarByClass(FlowDriver.class);

        // 3. 关联Mapper和Reducer
        job.setMapperClass(FlowMapper.class);
        job.setReducerClass(FlowReducer.class);

        // 4. 设置Map端输出kv类型
        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(FlowBean.class);

        // 5. 设置程序最终输出的kv类型
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(FlowBean.class);

        // 指定自定义分区器
        job.setPartitionerClass(ProvencePartitioner.class);

        // 指定相应数量的ReduceTask
        job.setNumReduceTasks(5);

        // 6. 设置程序的输入输出路径
        FileInputFormat.setInputPaths(job, new Path("D:\\hadoop\\mapreduce\\input\\partitioner"));
        FileOutputFormat.setOutputPath(job, new Path("D:\\hadoop\\mapreduce\\output\\partitioner"));

        // 7. 提交job
        boolean result = job.waitForCompletion(true);
        System.exit(result ? 0 : 1);
    }
}
```



新增ProvencePartitioner分区类：

```java
package com.sunhao.mapreduce.partitioner;

import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Partitioner;

public class ProvencePartitioner extends Partitioner<Text, FlowBean> {

    @Override
    public int getPartition(Text text, FlowBean flowBean, int i) {

        // 分区号
        int partition;

        // 获取手机号前三位
        String phone = text.toString();
        String subPhone = phone.substring(0, 3);

        // 根据手机号前三位进行分区
        if ("136".equals(subPhone)) partition = 0;
        else if ("137".equals(subPhone)) partition = 1;
        else if ("138".equals(subPhone)) partition = 2;
        else if ("139".equals(subPhone)) partition = 3;
        else partition = 4;

        return partition;
    }
}
```



其余代码保持不变。



### 5. 排序

排序分为全排序、区内排序、二次（多次）排序和Combiner部分排序。

![image-20230730221746709](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730221746709.png)

注意：key一定要支持排序，比如实现WritableComparable接口，重写compareTo方法。



#### 5.1 全排序案例

需求：对序列化案例产生的**结果**再次对总流量进行倒序排序。

注：这个需求不能使用一个MapReduce程序独立完成，需要两个，一个输出目标格式的文件，另一个进行排序。所以我们可以直接使用之前的序列化案例的输出文件，相当于一个MapReduce程序已经执行完了，我们对这个输出文件再使用MapReduce程序进行排序，以达到目的。



FlowBean：

```java
package com.sunhao.mapreduce.writablecomparable;

import org.apache.hadoop.io.Writable;
import org.apache.hadoop.io.WritableComparable;

import java.io.DataInput;
import java.io.DataOutput;
import java.io.IOException;

// 1. 继承Writable接口
public class FlowBean implements WritableComparable<FlowBean> {

    private long upFlow; // 上行流量
    private long downFlow; // 下行流量
    private long sumFlow; // 总流量

    // 2. 提供无参构造
    public FlowBean() {
    }

    // 3. 提供getter/setter
    public long getUpFlow() {
        return upFlow;
    }

    public void setUpFlow(long upFlow) {
        this.upFlow = upFlow;
    }

    public long getDownFlow() {
        return downFlow;
    }

    public void setDownFlow(long downFlow) {
        this.downFlow = downFlow;
    }

    public long getSumFlow() {
        return sumFlow;
    }

    public void setSumFlow(long sumFlow) {
        this.sumFlow = sumFlow;
    }

    public void setSumFlow() {
        this.sumFlow = this.upFlow + this.downFlow;
    }

    // 4. 实现序列化和反序列化方法时，注意顺序一定要保持一致（upFlow、downFlow和sunFlow的顺序）！！！
    @Override
    public void write(DataOutput dataOutput) throws IOException {

        dataOutput.writeLong(upFlow);
        dataOutput.writeLong(downFlow);
        dataOutput.writeLong(sumFlow);
    }

    @Override
    public void readFields(DataInput dataInput) throws IOException {

        this.upFlow = dataInput.readLong();
        this.downFlow = dataInput.readLong();
        this.sumFlow = dataInput.readLong();
    }

    // 5. 重写toString方法
    @Override
    public String toString() {
        return upFlow + "\t" + downFlow + "\t" + sumFlow;
    }

    @Override
    public int compareTo(FlowBean o) {

        //按照总流量比较,倒序排列
        if (this.sumFlow < o.sumFlow) return 1;
        else if (this.sumFlow == o.sumFlow) {

            // 全排序只需当总流量相同时返回0即可
//            return 0;

            // 二次排序 总流量相同时令上行流量高的在前
            if (this.upFlow < o.upFlow) return -1;
            else if (this.upFlow == o.upFlow) return 0;
            else return 1;
        }
        else return -1;
    }
}
```



FlowMapper：

```java
package com.sunhao.mapreduce.writablecomparable;

import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;

import java.io.IOException;

public class FlowMapper extends Mapper<LongWritable, Text, FlowBean, Text> {

    private Text v = new Text();
    private FlowBean k = new FlowBean();

    @Override
    protected void map(LongWritable key, Text value, Mapper<LongWritable, Text, FlowBean, Text>.Context context) throws IOException, InterruptedException {

        //1 获取一行数据
        String line = value.toString();

        //2 按照"\t",切割数据
        String[] split = line.split("\t");

        //3 封装outK outV
        k.setUpFlow(Long.parseLong(split[1]));
        k.setDownFlow(Long.parseLong(split[2]));
        k.setSumFlow();
        v.set(split[0]);

        //4 写出outK outV
        context.write(k,v);

    }
}
```



FlowReducer：

```java
package com.sunhao.mapreduce.writablecomparable;

import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

import java.io.IOException;

public class FlowReducer extends Reducer<FlowBean, Text, Text, FlowBean> {

    private FlowBean value = new FlowBean();

    @Override
    protected void reduce(FlowBean key, Iterable<Text> values, Reducer<FlowBean, Text, Text, FlowBean>.Context context) throws IOException, InterruptedException {

        //遍历values集合,循环写出,避免总流量相同的情况
        for (Text value : values) {

            //调换KV位置,反向写出
            context.write(value,key);
        }
    }
}
```



FlowDriver：

```java
package com.sunhao.mapreduce.writablecomparable;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

import java.io.IOException;

public class FlowDriver {

    public static void main(String[] args) throws IOException, InterruptedException, ClassNotFoundException {

        // 1. 获取job对象
        Configuration conf = new Configuration();
        Job job = Job.getInstance(conf);

        // 2. 关联Driver类
        job.setJarByClass(FlowDriver.class);

        // 3. 关联Mapper和Reducer
        job.setMapperClass(FlowMapper.class);
        job.setReducerClass(FlowReducer.class);

        // 4. 设置Map端输出kv类型
        job.setMapOutputKeyClass(FlowBean.class);
        job.setMapOutputValueClass(Text.class);

        // 5. 设置程序最终输出的kv类型
        job.setOutputKeyClass(FlowBean.class);
        job.setOutputValueClass(Text.class);

        // 6. 设置程序的输入输出路径
        FileInputFormat.setInputPaths(job, new Path("D:\\hadoop\\mapreduce\\input\\writablecomparable"));
        FileOutputFormat.setOutputPath(job, new Path("D:\\hadoop\\mapreduce\\output\\writablecomparable2"));

        // 7. 提交job
        boolean result = job.waitForCompletion(true);
        System.exit(result ? 0 : 1);
    }
}
```



#### 5.2 区内排序案例

需求：要求每个省份手机号输出的文件中按照总流量内部排序。

我们只需要在上面的全排序案例中添加自定义Partitioner分区即可。



ProvencePartitoner：

```java
package com.sunhao.mapreduce.partitionercomparable;

import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Partitioner;

public class ProvencePartitioner extends Partitioner<FlowBean, Text> {
    @Override
    public int getPartition(FlowBean flowBean, Text text, int i) {

        // 分区号
        int partition;

        // 获取手机号前三位
        String phone = text.toString();
        String subPhone = phone.substring(0, 3);

        // 根据手机号前三位进行分区
        if ("136".equals(subPhone)) partition = 0;
        else if ("137".equals(subPhone)) partition = 1;
        else if ("138".equals(subPhone)) partition = 2;
        else if ("139".equals(subPhone)) partition = 3;
        else partition = 4;

        return partition;
    }
}
```



在Driver驱动类中添加下列代码：

```java
		// 自定义分区器
        job.setPartitionerClass(ProvencePartitioner.class);

        // 设置ReduceTask数
        job.setNumReduceTasks(5);
```



#### 5.3 Combiner合并

![image-20230730222945974](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730222945974.png)



注意：Combier不是什么情况下都可以用的，只能在不影响最终的业务逻辑的情况下使用。



**自定义Combiner：**

1. 自定义一个Combiner继承Reducer，重写Reduce方法

```java
public class WordCountCombiner extends Reducer<Text, IntWritable, Text, IntWritable> {

    private int sum;
    private IntWritable v = new IntWritable();

    @Override
    protected void reduce(Text key, Iterable<IntWritable> values, Reducer<Text, IntWritable, Text, IntWritable>.Context context) throws IOException, InterruptedException {

        // 1. 累加求和
        sum = 0;
        for (IntWritable value : values) {
            sum += value.get();
        }

        // 2. 输出
        v.set(sum);
        context.write(key, v);
    }
}
```

2. 在Job驱动类中设置：`job.setCombinerClass(WordCountCombiner.class);`



**案例实操：**

需求：统计过程中对每一个MapTask的输出进行局部汇总，以减小网络传输量即采用Combiner功能。

只需在WordCount案例的基础上自定义并使用Combiner即可。



WordCountCombiner：

```java
package com.sunhao.mapreduce.combiner;

import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

import java.io.IOException;

public class WordCountCombiner extends Reducer<Text, IntWritable, Text, IntWritable> {

    private int sum;
    private IntWritable v = new IntWritable();

    @Override
    protected void reduce(Text key, Iterable<IntWritable> values, Reducer<Text, IntWritable, Text, IntWritable>.Context context) throws IOException, InterruptedException {

        // 1. 累加求和
        sum = 0;
        for (IntWritable value : values) {
            sum += value.get();
        }

        // 2. 输出
        v.set(sum);
        context.write(key, v);
    }
}
```



在WordcountDriver驱动类中指定Combiner：`job.setCombinerClass(WordCountCombiner.class);`



可以观察到，在未使用Combiner合并时，日志的 `Combine input records` 和 `Combine output records` 都为0，而使用了之后，这两处地方都不为0，说明Combiner在Reduce之前就预先对数据进行合并了，减少了Reduce的计算量，加快了效率。



### 6. OutputFormat

OutputFormat是MapReduce输出的基类，所有实现MapReduce输出都实现了 OutputFormat 接口。而默认的则是TextOutputFormat，与TextInputFormat一样，TextOutputFormat也是一行一行地写数据的。



**自定义序列化：**

1. 自定义一个类继承FileOutputFormat
2. 自定义一个类继承RecordWriter
3. 重写所有的方法



**案例实操：**

需求：过滤输入的 log 日志，包含 atguigu 的网站输出到 e:/atguigu.log，不包含 atguigu的网站输出到 e:/other.log。

我们只需要简单地自定义OutputFormat，创建两个流，一个用来输出含atguigu的，另一个用来输出不含atguigu的即可。



LogOutputFormat：

```java
package com.sunhao.mapreduce.outputformat;

import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.*;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

import java.io.IOException;

public class LogOutputFormat extends FileOutputFormat<Text, NullWritable> {

    @Override
    public RecordWriter<Text, NullWritable> getRecordWriter(TaskAttemptContext job) throws IOException, InterruptedException {

        // 返回一个自定义的RecordWriter
        LogRecordWriter logRecordWriter = new LogRecordWriter(job);

        return logRecordWriter;
    }
}
```



LogRecordWriter：

```java
package com.sunhao.mapreduce.outputformat;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.*;
import org.apache.hadoop.io.IOUtils;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.RecordWriter;
import org.apache.hadoop.mapreduce.TaskAttemptContext;

import java.io.IOException;

public class LogRecordWriter extends RecordWriter<Text, NullWritable> {

    private FSDataOutputStream atguiguOut;
    private FSDataOutputStream otherOut;

    public LogRecordWriter(TaskAttemptContext job) throws IOException {

        //获取文件系统对象
        FileSystem fs = FileSystem.get(new Configuration());

        //用文件系统对象创建两个输出流对应不同的目录
        atguiguOut = fs.create(new Path("D:\\hadoop\\mapreduce\\output\\log\\atguigu.log"));
        otherOut = fs.create(new Path("D:\\hadoop\\mapreduce\\output\\log\\other.log"));
    }

    @Override
    public void write(Text text, NullWritable nullWritable) throws IOException, InterruptedException {

        String log = text.toString();

        // 根据一行的log数据是否包含atguigu,判断两条输出流输出的内容
        if (log.contains("atguigu")) atguiguOut.writeBytes(log + "\n");
        else otherOut.writeBytes(log + "\n");
    }

    @Override
    public void close(TaskAttemptContext taskAttemptContext) throws IOException, InterruptedException {

        // 关流
        IOUtils.closeStream(atguiguOut);
        IOUtils.closeStream(otherOut);
    }
}
```



LogMapper：

```java
package com.sunhao.mapreduce.outputformat;

import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;

import java.io.IOException;

public class LogMapper extends Mapper<LongWritable, Text, Text, NullWritable> {

    @Override
    protected void map(LongWritable key, Text value, Mapper<LongWritable, Text, Text, NullWritable>.Context context) throws IOException, InterruptedException {

        // 直接写出即可
        context.write(value, NullWritable.get());
    }
}
```



LogReducer：

```java
package com.sunhao.mapreduce.outputformat;

import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

import java.io.IOException;

public class LogReducer extends Reducer<Text, NullWritable, Text, NullWritable> {

    @Override
    protected void reduce(Text key, Iterable<NullWritable> values, Reducer<Text, NullWritable, Text, NullWritable>.Context context) throws IOException, InterruptedException {

        // 同样是直接写出，只不过要防止有相同的key，所以要遍历
        for (NullWritable value : values) {
            context.write(key, NullWritable.get());
        }
    }
}
```



LogDriver：

```java
package com.sunhao.mapreduce.outputformat;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.InputFormat;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

import java.io.IOException;

public class LogDriver {

    public static void main(String[] args) throws IOException, InterruptedException, ClassNotFoundException {

        // 1. 获取job
        Job job = Job.getInstance(new Configuration());

        // 2. 关联Driver、Mapper和Driver的jar
        job.setJarByClass(LogDriver.class);
        job.setMapperClass(LogMapper.class);
        job.setReducerClass(LogReducer.class);

        // 3. 设置Mapper输出的k和v
        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(NullWritable.class);

        // 4. 设置最终输出的k和v
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(NullWritable.class);

        // 设置自定义的outputformat
        job.setOutputFormatClass(LogOutputFormat.class);

        // 5. 设置输入和输出路径
        FileInputFormat.setInputPaths(job, new Path("D:\\hadoop\\mapreduce\\input\\log"));
        /*
         虽然我们自定义了outputformat，但是因为我们的outputformat继承自fileoutputformat
         但是fileoutputformat要输出一个_SUCCESS文件，所以在这还得指定一个输出目录
         */
        FileOutputFormat.setOutputPath(job, new Path("D:\\hadoop\\mapreduce\\output\\log"));

        // 6. 提交job
        boolean result = job.waitForCompletion(true);
        System.exit(result ? 0 : 1);
    }
}
```



## Hadoop数据压缩

### 1. 概述

**压缩的好处和坏处：**

压缩的优点：以减少磁盘IO、减少磁盘存储空间。

压缩的缺点：增加CPU开销。



**压缩原则：**

- 运算密集型的Job，少用压缩

- IO密集型的Job，多用压缩



### 2. 压缩方式的选择

压缩算法的比较：

| 压缩格式 | Hadoop自带？ | 算法    | 文件扩展名 | 是否可切片 | 换成压缩格式后，原来的程序是否需要修改 |
| -------- | ------------ | ------- | ---------- | ---------- | -------------------------------------- |
| DEFLATE  | 是，直接使用 | DEFLATE | .deflate   | 否         | 和文本处理一样，不需要修改             |
| Gzip     | 是，直接使用 | DEFLATE | .gz        | 否         | 和文本处理一样，不需要修改             |
| bzip2    | 是，直接使用 | bzip2   | .bz2       | 是         | 和文本处理一样，不需要修改             |
| LZO      | 否，需要安装 | LZO     | .lzo       | 是         | 需要建索引，还需要指定输入格式         |
| Snappy   | 是，直接使用 | Snappy  | .snappy    | 否         | 和文本处理一样，不需要修改             |



压缩性能的比较：

| 压缩算法 | 原始文件大小 | 压缩文件大小 | 压缩速度 | 解压速度 |
| -------- | ------------ | ------------ | -------- | -------- |
| gzip     | 8.3GB        | 1.8GB        | 17.5MB/s | 58MB/s   |
| bzip2    | 8.3GB        | 1.1GB        | 2.4MB/s  | 9.5MB/s  |
| LZO      | 8.3GB        | 2.9GB        | 49.3MB/s | 74.6MB/s |



### 3. 压缩位置的选择

可以单独开启map端，可以单独开启reduce端，也可以map端和reduce端同时开启。

![image-20230730230229298](D:\My tepora note\尚硅谷Hadoop.assets\image-20230730230229298.png)



### 4. 压缩参数配置

要想使用压缩/解压缩算法，需要指定编码/解码器：

| 压缩格式 | 对应的编码/解码器                          |
| -------- | ------------------------------------------ |
| DEFLATE  | org.apache.hadoop.io.compress.DefaultCodec |
| gzip     | org.apache.hadoop.io.compress.GzipCodec    |
| bzip2    | org.apache.hadoop.io.compress.BZip2Codec   |
| LZO      | com.hadoop.compression.lzo.LzopCodec       |
| Snappy   | org.apache.hadoop.io.compress.SnappyCodec  |



要在Hadoop中启用压缩，可以配置如下参数：

| 参数                                                         | 默认值                                         | 阶段        | 建议                                          |
| ------------------------------------------------------------ | ---------------------------------------------- | ----------- | --------------------------------------------- |
| io.compression.codecs    （在core-site.xml中配置）           | 无，这个需要在命令行输入hadoop checknative查看 | 输入压缩    | Hadoop使用文件扩展名判断是否支持某种编解码器  |
| mapreduce.map.output.compress（在mapred-site.xml中配置）     | false                                          | mapper输出  | 这个参数设为true启用压缩                      |
| mapreduce.map.output.compress.codec（在mapred-site.xml中配置） | org.apache.hadoop.io.compress.DefaultCodec     | mapper输出  | 企业多使用LZO或Snappy编解码器在此阶段压缩数据 |
| mapreduce.output.fileoutputformat.compress（在mapred-site.xml中配置） | false                                          | reducer输出 | 这个参数设为true启用压缩                      |
| mapreduce.output.fileoutputformat.compress.codec（在mapred-site.xml中配置） | org.apache.hadoop.io.compress.DefaultCodec     | reducer输出 | 使用标准工具或者编解码器，如gzip和bzip2       |



### 5. 案例实操

即使你的MapReduce的输入输出文件都是未压缩的文件，你仍然可以对Map任务的中间结果输出做压缩，因为它要写在硬盘并且通过网络传输到Reduce节点，对其压缩可以提高很多性能，这些工作只要设置两个属性即可。

基于WordCount案例，在Map端、Reduce端、Map和Reduce端使用压缩，只需要在Driver驱动类上进行相应设置即可。



Driver：

```java
package com.sunhao.mapreduce.compression;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.io.compress.BZip2Codec;
import org.apache.hadoop.io.compress.CompressionCodec;
import org.apache.hadoop.io.compress.DefaultCodec;
import org.apache.hadoop.io.compress.GzipCodec;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

import java.io.IOException;

public class WordCountDriver {

    public static void main(String[] args) throws IOException, InterruptedException, ClassNotFoundException {

        // 1. 获取配置信息以及获取job对象
        Configuration conf = new Configuration();

        // 可以单独开启map端，可以单独开启reduce端，也可以map端和reduce端同时开启
        // 开启map端输出压缩
        conf.setBoolean("mapreduce.map.output.compress", true);

        // 设置map端输出压缩方式
        conf.setClass("mapreduce.map.output.compress.codec", BZip2Codec.class, CompressionCodec.class);

        Job job = Job.getInstance(conf);

        // 2. 关联本Driver程序的jar
        job.setJarByClass(WordCountDriver.class);

        // 3. 关联Mapper和Reducer的jar
        job.setMapperClass(WordCountMapper.class);
        job.setReducerClass(WordCountReducer.class);

        // 4. 设置Mapper输出的key和value类型
        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(IntWritable.class);

        // 5. 设置最终输出的key和value类型
        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);

        // 6. 设置输入和输出路径
        FileInputFormat.setInputPaths(job, new Path("D:\\hadoop\\mapreduce\\input\\compression"));
        FileOutputFormat.setOutputPath(job, new Path("D:\\hadoop\\mapreduce\\output\\compression4"));

        // 设置reduce端输出压缩开启
        FileOutputFormat.setCompressOutput(job, true);

        // 设置压缩的方式 选择一个即可，不同的压缩方式产生的文件的后缀名不同
//        FileOutputFormat.setOutputCompressorClass(job, BZip2Codec.class);
	    FileOutputFormat.setOutputCompressorClass(job, GzipCodec.class);
	    FileOutputFormat.setOutputCompressorClass(job, DefaultCodec.class);


        // 7. 提交job
        boolean result = job.waitForCompletion(true);
        System.exit(result ? 0 : 1);
    }
}
```



# Yarn

## Yarn资源调度器

### 1. Yarn基础架构

YARN主要由ResourceManager、NodeManager、ApplicationMaster和Container等组件构成。



![image-20230806110847058](D:\My tepora note\尚硅谷Hadoop.assets\image-20230806110847058.png)



### 2. Yarn工作机制

![image-20230806111333652](D:\My tepora note\尚硅谷Hadoop.assets\image-20230806111333652.png)



当程序运行到job.waitForCOmpletion()方法时，会启动YarnRunner。YarnRunner向ResourceManager申请一个Application，然后RM将该应用程序的资源路径返回给YarnRunner，接着程序向HDFS提交资源，即Job.split、Job.xml和jar包。资源提交完成后YarnRunner向RM申请运行MrApplicationMaster，RM便将用户的请求转化成一个task并放入队列里。根据先进先出原则，当轮到自己且自己有足够的资源，NodeManager便领取到任务。之后该NodeManager创建一个容器Container，里面放MRAppMaster。然后MRAppmaster读取Job.split知道切片数量，便向RM申请对应数量的MapTask。RM将运行MapTask任务分配给另外两个NodeManager，另两个NodeManager分别领取任务并创建容器（也可以是同一个），里面放MapTask。接着MrAppMaster向那两个接收到任务的NodeManager（或者是一个）发送job.xml和jar包，这两个MapTask便启动了起来，对数据进行分区排序最后写入磁盘。当所有MapTask运行完毕后，即将信息写入磁盘后，MrAppMaster便向RM申请运行ReduceTask，同样是先创建容器Container再创建ReduceTask。ReduceTask从MapTask拉取对应分区的数据开始处理，当所有的ReduceTask都运行完毕，MrAppMaster便会向RM申请释放资源，即刚刚产生的所有东西包括自己全都释放掉。



### 3. 作业提交全过程

上面的Yarn工作机制相当于是Yarn和MapReduce的关系，而这里的作业提交过程就是Yarn和HDFS与MapReduce三者的关系。



![image-20230806114113220](D:\My tepora note\尚硅谷Hadoop.assets\image-20230806114113220.png)



实际上大体和上面的Yarn工作机制差不多，之不多在开始和结束多了与HDFS的交互，即输入输出。



### 4. Yarn调度器和调度算法

目前，Hadoop作业调度器主要有三种：先进先出调度器（FIFO）、容量调度器（Capacity Scheduler）和公平调度器（Fair Scheduler）。Apache Hadoop3.1.3默认的资源调度器是Capacity Scheduler。



#### 4.1先进先出调度器（FIFO）

FIFO调度器（First In First Out）：单队列，根据提交作业的先后顺序，先来先服务。

![image-20230806115016558](D:\My tepora note\尚硅谷Hadoop.assets\image-20230806115016558.png)



没啥好说的，先到先得嘛，很容易。但是由于它不支持多队列，所以在生产环境中用的少。



#### 4.2 容量调度器（Capacity Scheduler）

Capacity Scheduler是Yahoo开发的多用户调度器，默认队列的资源分配方式为FIFO。

![image-20230806115339271](D:\My tepora note\尚硅谷Hadoop.assets\image-20230806115339271.png)



**调度算法：**

![image-20230806115438649](D:\My tepora note\尚硅谷Hadoop.assets\image-20230806115438649.png)



#### 4.3 公平调度器（Fair Scheduler）

Fair Schedulere是Facebook开发的多用户调度器，默认队列的资源分配方式为FAIR。

![image-20230806115616174](D:\My tepora note\尚硅谷Hadoop.assets\image-20230806115616174.png)



可以发现，容量调度器有的公平调度器全都有，而且公平调度器还有容量调度器没有的。由于容量调度器的队列的资源分配方式默认是FIFO，如果将公平资源调度器的队列的资源分配方式设置成FIFO，那么此时公平调度器就相当于是容量调度器。



**缺额：**某一 时刻一个作业应获资源和实际获取资源的差距。



**调度算法：**

![image-20230806120529093](D:\My tepora note\尚硅谷Hadoop.assets\image-20230806120529093.png)

![image-20230806120622528](D:\My tepora note\尚硅谷Hadoop.assets\image-20230806120622528.png)



总之，公平调度器只办三件事：**公平，公平，还是tmd公平！ **



### 5. Yarn常用命令

Yarn状态的查询，除了可以在hadoop103:8088页面查看外，还可以通过命令操作。



#### 5.1 yarn application查看任务

列出所有Application：

```
yarn application -list
```



据Application状态过滤（状态可选择ALL、NEW、NEW_SAVING、SUBMITTED、ACCEPTED、RUNNING、FINISHED、FAILED、KILLED）：

```
yarn application -list -appStates 状态
```



Kill掉Application：

```
yarn application -kill 填Application-Id
```



#### 5.2 yarn logs查看日志

查询Application日志：

```
yarn logs -applicationId 填Application-Id
```



查询Container日志：

```
yarn logs -applicationId 填Application-Id -containerId 填Container-Id
```



#### 5.3 yarn applicationattempt查看尝试运行的任务

列出所有Application尝试的列表：

```
yarn applicationattempt -list 填Application-Id
```



打印ApplicationAttemp状态：

```
yarn applicationattempt -status 填ApplicationAttempt-Id
```



#### 5.4 yarn container查看容器

列出所有Container:：

```
yarn container -list 填ApplicationAttempt-Id
```



打印Container状态：

```
yarn container -status 填Container-Id
```

**注：只有在任务跑的途中才能看到container的状态**



#### 5.5 yarn node查看节点状态

列出所有节点：

```
yarn node -list -all
```



#### 5.6 yarn rmadmin更新配置

加载队列配置：

```
yarn rmadmin -refreshQueues
```



#### 5.7 yarn queue查看队列

打印队列信息：

```
yarn queue -status 队列名称
```



### 6. Yarn生产环境核心参数

![image-20230806122545092](D:\My tepora note\尚硅谷Hadoop.assets\image-20230806122545092.png)



## Yarn案例实操

### Yarn的Tool接口案例

之前我们将自己写的wordcount打包成jar包并上传到集群，在hdfs上使用。但是那有一个问题，所有的参数都固定死了，不能动态更改参数，很不方便。所以我们需要让自己写的程序也可以动态修改参数。

于是，它来了——Yarn的Tool接口。



**新建Maven项目YarnDemo并增加下列代码到pom.xml：**

```
	<dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>3.1.3</version>
        </dependency>
    </dependencies>
```



**创建类WordCount并实现Tool接口：**

```
package com.sunhao.yarn;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.Tool;

import java.io.IOException;

public class WordCount implements Tool {

    private Configuration conf;

    @Override
    public int run(String[] strings) throws Exception {

        Job job = Job.getInstance(conf);

        job.setJarByClass(WordCountDriver.class);
        job.setMapperClass(WordCountMapper.class);
        job.setReducerClass(WordCountReducer.class);

        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(IntWritable.class);

        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);

        FileInputFormat.setInputPaths(job, new Path(strings[0]));
        FileOutputFormat.setOutputPath(job, new Path(strings[1]));

        return job.waitForCompletion(true) ? 0 : 1;
    }

    @Override
    public void setConf(Configuration configuration) {
        this.conf = configuration;
    }

    @Override
    public Configuration getConf() {
        return conf;
    }

    public static class WordCountMapper extends Mapper<LongWritable, Text, Text, IntWritable> {

        private Text k = new Text();
        private IntWritable v = new IntWritable(1);

        @Override
        protected void map(LongWritable key, Text value, Mapper<LongWritable, Text, Text, IntWritable>.Context context) throws IOException, InterruptedException {

            String line = value.toString();

            String[] words = line.split(" ");

            for (String word : words) {
                k.set(word);
                context.write(k, v);
            }
        }
    }

    public static class WordCountReducer extends Reducer<Text, IntWritable, Text, IntWritable> {

        private IntWritable v = new IntWritable();

        @Override
        protected void reduce(Text key, Iterable<IntWritable> values, Reducer<Text, IntWritable, Text, IntWritable>.Context context) throws IOException, InterruptedException {

            int sum = 0;

            for (IntWritable value : values) {
                sum += value.get();
            }

            v.set(sum);
            context.write(key, v);

        }
    }
}
```



**新建WordCountDriver：**

```
package com.sunhao.yarn;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

import java.util.Arrays;

public class WordCountDriver {

    private static Tool tool;

    public static void main(String[] args) throws Exception {

        // 1. 创建配置文件
        Configuration conf = new Configuration();

        // 2. 判断是否有tool接口
        switch (args[0]) {
            case "wordcount":
                tool = new WordCount();
                break;
            default:
                throw new RuntimeException("No such tool：" + args[0]);
        }

        // 3. 用Tool执行程序
        // Arrays.copyOfRange 将老数组的元素放到新数组里面 因为第三个参数要求是string[]所以只能使用拷贝的方法传一个过去
        // 为什么这里从1开始复制，因为程序能走到这里说明已经通过了前面的检验，所以args[0]一定是wordcount，所以后面两个分别是输入输出路径
        // 此时问题又来了，如果在wordcount后面加上-D参数呢，这个参数不会被当做输入路径吗？
        // 因为ToolRunner.run方法会自动识别-D参数并进行相应处理，所以相当于吸收了，故args[1]和args[2]一定是输入输出路径
        // Arrays.copyOfRange是左闭右开的，所以length为3那么只会拷贝到2，不会数组越界
        int run = ToolRunner.run(conf, tool, Arrays.copyOfRange(args, 1, args.length));

        System.exit(run);
    }
}
```

之后打包成jar包，上传至集群。

这样便实现了动态传参，此外若写的不是wordcount也会报错。



