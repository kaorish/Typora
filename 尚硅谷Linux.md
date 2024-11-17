# 基础篇

## Linux文件系统与目录结构

==**Linux系统中一切皆文件！！！**==

目前CentOs7默认使用xfs，即64位高性能日志文件系统。

Windows里有盘符且使用'\\'划分层级，如`C:\a\...`，而Linux中没有盘符且用'/'划分层级，如：`/root/Desktop/...`。



### 文件系统和挂载点

**文件系统：**

目录结构是层级式的，最顶端是根目录，即'/'。相比于Windows，Linux的文件管理的方式更加的扁平化且更加灵活高效。



**挂载点：**

顾名思义即被挂载的点，相当于让磁盘的某个区域从属于某个目录。假如磁盘某个区域挂载到了某个目录下面，那么存放在该目录下面的所以文件都会放到该磁盘区域内，从而与磁盘的其他分隔开，互不影响，不会搞混。

![image-20230601102045907](D:\My tepora note\尚硅谷Linux.assets\image-20230601102045907.png)



### 目录结构

介绍一下根目录下面所有的文件夹各是干什么用的。

**bin：**binary的缩写，存放的是二进制文件，里面也有所有的指令。是一个类似于超链接的映射，映射到/usr/bin。

**sbin：**system binary的缩写，即和系统有关的二进制文件。是一个类似于超链接的映射，映射到/usr/sbin。

**lib：**library的缩写，即库文件。是一个类似于超链接的映射，映射到/usr/lib。

**lib64：**与lib类似，只不过存放的是与64位系统有关的文件。是一个类似于超链接的映射，映射到/usr/lib64。

**boot：**存放的是引导程序。

**dev：**device的缩写，映射到所有的硬件设备，如cpu、disk，体现了Linux中万物皆对象的特点。

**etc：**存放的是配置文件。

**home：**存放的是与用户有关的，比如我创建了sunhao用户，里面就有一个叫sunhao的文件夹。

**media：**映射外接设备，即有U盘等外接设备连接，那么磁盘上的一部分区域就会挂载到media这个文件夹，用于专门存储外接设备的信息。

**mnt：**mout的缩写，作用与media类似。

**opt：**optional的缩写，即可选择的。顾名思义，里面存放的是可有可无的文件，即第三方应用都存放在这里。

**proc：**process的缩写，存放的是各自进程、程序的信息。

**root：**root用户的主文件夹，和桌面上的主文件夹一毛一样。也是当前用户为root时，终端上的'~'所指代的主文件夹。

**run：**有些类似于缓存，用于临时存放进程、程序信息，电脑重启后就清空了。

**srv：**service的缩写，存放的是电脑服务有关的文件。

**sys：**systems的缩写，存放与电脑系统有关的文件。

**tmp：**temporary的缩写，即临时的，也是临时存放应用程序信息。

**usr：**用于存放用户管理的各种文件，极其重要，上面的bin、sbin、lib和lib64都是链接到usr目录下的。

**var：**variable的缩写，即变量，用于存放经常改变的文件，如日志文件。



## VI/VIM编辑器

![image-20230605190401716](D:\My tepora note\尚硅谷Linux.assets\image-20230605190401716.png)

默认是一般模式，处于任何其他模式时都可以通过按`ESC`键回到一般模式，且处于编辑模式时想要进行保存等操作或者处于命令模式时想要执行编辑等操作，都需要先回到一般模式。



![image-20230605194333174](D:\My tepora note\尚硅谷Linux.assets\image-20230605194333174.png)



### 普通模式

常用语法：

![image-20230605190937564](D:\My tepora note\尚硅谷Linux.assets\image-20230605190937564.png)

![image-20230605191043137](D:\My tepora note\尚硅谷Linux.assets\image-20230605191043137.png)

不难看出，这些命令都是可以组合、有规律的。且在Linux中，^代表起始，$代表末尾。



### 编辑模式

从普通模式进入编辑模式的方法：

![image-20230605191546707](D:\My tepora note\尚硅谷Linux.assets\image-20230605191546707.png)

各种方法之间有区别，体现在进入编辑模式时光标的位置不同，视情况使用。



### 命令模式

基本语法：

![image-20230605193643261](D:\My tepora note\尚硅谷Linux.assets\image-20230605193643261.png)

注意：

- ！就表示强制的意思，也可以进行`:wq!`这个操作。

- %可以理解为所有行。`:s/old/new`只替换光标所在行的匹配的第一个单词，那么`:%s/old/new`则替换所有行的匹配的第一个单词`:s/old/new/g`替换光标所在行的匹配的所有单词，那么`:%s/old/new/g`则替换所有行的匹配的所有单词，即全局替换。



## 网络配置

![image-20230605201320368](D:\My tepora note\尚硅谷Linux.assets\image-20230605201320368.png)

查看ip地址：

```
Windows：ipconfig
Linux：ifconfig
```



测试两台主机/服务器能否互相通信的命令：`ping`

语法：

```
ping ip地址
```



**修改主机名：**

![image-20230606190848906](D:\My tepora note\尚硅谷Linux.assets\image-20230606190848906.png)



此外，`hostnamectl`命令可以查看该主机名的所有配置，`hostnamectl set-hostname 主机名`命令可以让修改的主机名立即生效，不用重启。



## 系统管理

### Linux中的进程和服务

![image-20230607213235557](D:\My tepora note\尚硅谷Linux.assets\image-20230607213235557.png)

**守护服务：**

从系统开始到关闭都存在执行的服务叫做守护服务，执行这些服务的进程就叫做守护进程。守护的英文为"daemon"，所以Linux中有很多服务是以'd'结尾的，代表他们是守护服务。实际上守护服务和守护进程是同一个东西，不用特意区分他们。



### service 服务

**查看服务后展现出来的各种服务颜色的含义：**

绿色：可执行文件，红色：压缩文件，蓝色：目录，浅蓝色：链接文件，灰色：其他文件



**CentOS 6（了解即可）：**

![image-20230607214053635](D:\My tepora note\尚硅谷Linux.assets\image-20230607214053635.png)

因为现在CentOS 7升级了，虽然还是向下兼容CentOS 6版本的服务，但是可以看到还存留着的服务很少了。



**CentOS 7（重点掌握）：**

![image-20230607214447627](D:\My tepora note\尚硅谷Linux.assets\image-20230607214447627.png)

注意：CentOS 7使用服务的语法和CentOS 6不一样！注意他们的去区别。



### 系统运行级别

**CentOS 6：**

![image-20230610200712086](D:\My tepora note\尚硅谷Linux.assets\image-20230610200712086.png)

级别0和6相当于没用；级别4是系统保留的，一般也不会去使用；级别1相当于安全模式，除非出现特殊情况，一般不会使用；平时能用的也就是2、3、5，而3把网络服务去掉后就和2没什么区别了，所以3和5是最关键的，所以CentOS 7直接将这些级别进行简化，即相当于只保留了3和5。



**CentOS 7：**

![image-20230610201337974](D:\My tepora note\尚硅谷Linux.assets\image-20230610201337974.png)



![image-20230610201708721](D:\My tepora note\尚硅谷Linux.assets\image-20230610201708721.png)



### 关闭防火墙







### 关机重启命令

**基本语法：**

![image-20230611151117547](D:\My tepora note\尚硅谷Linux.assets\image-20230611151117547.png)

![image-20230611151158577](D:\My tepora note\尚硅谷Linux.assets\image-20230611151158577.png)

![image-20230611151232829](D:\My tepora note\尚硅谷Linux.assets\image-20230611151232829.png)



**实际操作：**

![image-20230611151322522](D:\My tepora note\尚硅谷Linux.assets\image-20230611151322522.png)



# 实操篇

## 帮助命令

![image-20230611162051397](D:\My tepora note\尚硅谷Linux.assets\image-20230611162051397.png)

Linux系统中的命令分为内置命令和外部命令，`man`只能直接获取外部命令的帮助信息，`help`只能获取内置命令的帮助信息。可以只用`type`来查看某个命令是哪种命令，语法为：`type 命令名`。



### man获取帮助信息

基本语法：

![image-20230611160854570](D:\My tepora note\尚硅谷Linux.assets\image-20230611160854570.png)

进入文档后可以通过按空格键直接翻一页，也可以按上下箭头慢慢滑动，也可以按f向下、按b向上，或者按pageUp和pageDown。

注意：实际上man可以获取任何命令，只是不能简单地直接通过`man 命令`获取内置命令的帮助信息罢了。内置命令可以通过`man -f 命令`查看各种册中的解释，里面就有我们想要查看的内置命令。也可以再使用man查看其中特定某一个命令的解释。

如：

![image-20230611161951613](D:\My tepora note\尚硅谷Linux.assets\image-20230611161951613.png)



### help获取帮助信息

语法：

![image-20230611162119153](D:\My tepora note\尚硅谷Linux.assets\image-20230611162119153.png)

虽然help不能直接获取，但是外部命令里内置了相当于help的帮助命令，可使用`命令 --help`查看帮助信息。



### 常用快捷键

![image-20230611162310459](D:\My tepora note\尚硅谷Linux.assets\image-20230611162310459.png)

此外还有`ctrl + u`，用于清除当前未执行的命令。也就是清除目前写了的东西，就不需要自己一个一个删了。



## 文件目录类

### pwd 显示当前工作目录的绝对路径

![image-20230611162944155](D:\My tepora note\尚硅谷Linux.assets\image-20230611162944155.png)

语法：

![image-20230611162832871](D:\My tepora note\尚硅谷Linux.assets\image-20230611162832871.png)

绝对路径即从根目录开始一直走到该目录的唯一一条路径。绝对路径是以`/`开头的，相对路径不以`/`开头。绝对路径以根目录为起点，相对路径以自身所在目录为起点，`..`表示返回当前目录的上一层目录。



### ls 列出目录的内容

![image-20230611164149189](D:\My tepora note\尚硅谷Linux.assets\image-20230611164149189.png)

a即all，ls -a后会出现'.'和'..'，其中'..'表示切换到当前目录的上一级菜单，'.'指代当前目录。显然，'.'和'..'被当成特定的对象来看待了。除了这两个外，其余的文件和目录都以'.'开头，即表明他们都是隐藏文件。**以'.'开头的文件都是隐藏文件，以'.'开头的文件夹都是隐藏文件夹！**

l即long，即文件的完整版信息展示。

`ll`即`ls -l`的缩写，作用是一样的。

使用`ll`后展示的文件的首字母都具有不同的涵义，比如：'-'或者'd'或者'l'，其中'-'表示普通的文件，'d'表示文件目录，'l'表示链接文件。当然还有其他的，比如'c'和'b'，这两个都表示设备文件，其中'c'表示字符类型的设备文件，比如鼠标和键盘；'b'表示跨设备文件，比如硬盘。

此外，还可以将a和l组合起来使用，即`ls -al`，这样可以将文件的所有信息都列出来。还可以用`ls -lh`这样可以将显示出来的字节数转换成我们熟悉的多少kb和mb，让文件的大小更加直观（h即human readable，人们可以阅读的方式）。

ls后面还可以跟上路径（绝对路径或相对路径）。



### cd 切换目录

直接敲cd则返回主文件夹。（root用户的主文件夹就是"root"，其他用户的主文件夹则为"/home/用户名"。

tips：`cd -`可以直接跳回到上次的目录。每一行的开头都是'-'或'd'，'-'表示一般文件，'d'表示文件目录。



### mkdir 创建一个新的目录





### rmdir 删除一个空的目录





### touch 创建空文件





### cp 复制文件或目录





### rm 删除文件或目录







### cat 查看文件内容







### mv 移动文件与目录或重命名







### more 文件内容分屏查看器







### less 分屏显示文件内容







### echo







### head 显示文件头部内容







### tail 输出文件尾部内容



注意：使用**`tail -f`**可以实时监控文件信息，即如果文件 内容有追加，就能实时监控到。





### > 输出重定向 和 >> 追加







### ln 软链接







### history 查看已经执行过的历史命令







## 时间日期类

### date 显示当前时间







### date 显示非当前时间







### date 设置系统时间







### cal 查看日历







## 用户管理命令

### useradd 添加新用户

![image-20230623085819233](D:\My tepora note\尚硅谷Linux.assets\image-20230623085819233.png)

`useradd -d /home/文件目录名 用户名`可以为用户创建指定的文件目录。



### passwd 设置用户密码







### id 查看用户是否存在







### cat /etc/passwd 查看创建了哪些用户







### su 切换用户







### userdel 删除用户







### who 查看登录用户信息

`who am i`显示最外层，也就是最真实、最终的用户。

`whoami`显示目前所使用的用户，即第一个面具是谁。





### sudo 设置普通用户具有root权限

![image-20230623090204762](D:\My tepora note\尚硅谷Linux.assets\image-20230623090204762.png)



### usermod 修改用户

![image-20230623090511609](D:\My tepora note\尚硅谷Linux.assets\image-20230623090511609.png)



## 用户组管理命令

![image-20230623090257650](D:\My tepora note\尚硅谷Linux.assets\image-20230623090257650.png)



### groupadd 新增组

![image-20230623090314842](D:\My tepora note\尚硅谷Linux.assets\image-20230623090314842.png)



### groupdel 删除组







### groupmod 修改组







### cat /etc/group 查看创建了哪些组







## 文件权限类

### 文件属性







### chmod 改变权限







### chown 改变所有者

![image-20230623090946339](D:\My tepora note\尚硅谷Linux.assets\image-20230623090946339.png)





### chgrp 改变所属组

![image-20230623091031916](D:\My tepora note\尚硅谷Linux.assets\image-20230623091031916.png)





## 搜索查找类

### find 查找文件或者目录

![image-20230623091151724](D:\My tepora note\尚硅谷Linux.assets\image-20230623091151724.png)



![image-20230623091457906](D:\My tepora note\尚硅谷Linux.assets\image-20230623091457906.png)



不指定目录的话就默认主文件夹，而且会把隐藏文件也找出来。



### locate 快速定位文件路径







### grep 过滤查找 及 '|' 管道符







## 压缩解压类

### gzip/gunzip 压缩







### zip/unzip 压缩







### tar 打包







## 磁盘分区类

### du 查看文件和目录占用的磁盘空间

![image-20230623091721555](D:\My tepora note\尚硅谷Linux.assets\image-20230623091721555.png)



`du -sh`就是只统计总大小，以人类便于阅读的方式展现出来。



### df 查看磁盘空间使用情况

![image-20230623091921122](D:\My tepora note\尚硅谷Linux.assets\image-20230623091921122.png)



一般使用`df -h`查看磁盘使用情况。也可以用`free -h`查看内存使用情况。



### lsblk 查看设备挂载情况

![image-20230623092108330](D:\My tepora note\尚硅谷Linux.assets\image-20230623092108330.png)

一般用`lsblk`和`lsblk -f`，如果想看详细的设备挂载情况和文件系统信息，那么就用`lsblk -f`，否则就用`lsblk`。



### mount/unmount 挂载/卸载





### fdisk 分区







## 进程管理类

### ps 查看当前系统进程状态







### kill 终止进程







### pstree 查看进程树







### top 查看系统健康状态







### netstat 显示网络状态和端口占用信息







## 系统定时任务

### crontab 服务管理

![image-20230623091645898](D:\My tepora note\尚硅谷Linux.assets\image-20230623091645898.png)



### crontab 定时任务设置

![image-20230623091632940](D:\My tepora note\尚硅谷Linux.assets\image-20230623091632940.png)



