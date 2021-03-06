# Linux笔记

## **一、Ubuntu安装**

### **1.1、Ubuntu的安装之后的操作**

apt-get update:更新安装列表 apt-get upgrade:升级软件 apt-get install software_name :安装软件 apt-get --purge remove software_name :卸载软件及其配置 apt-get autoremove software_name:卸载软件及其依赖的安装包 dpkg --list:罗列已安装软件 ctrl+C：中止命令 

**sudo：超级用户权限**

**隐藏文件前有个.**

获取密码：sudo passwd 替换到root用户：su 开通ssh：apt-get install openssh-server 安装net-tools：apt install net-tools 查看ip地址：ifconfig 

### **1.2、安装vmtools**

sudo apt update sudo apt install open-vm-tools-desktop fuse reboot 

### **1.3、安装搜狗输入法**

在目录下打开

sudo dpkg -i sogou...		安装 sudo apt install -f			修复依赖环境 sudo dpkg -i sogou...		重新安装 reboot					重启 

## **二、Linux基础教程**

![img](D:\软件\腾讯\有道云笔记\有道云笔记数据\qqCEF95AFC5CA0532DB9887F977262334B\c6562839ef254bf099394b7b8a3efe5c\14bcd4b2032.jpeg)

![img](D:\软件\腾讯\有道云笔记\有道云笔记数据\qqCEF95AFC5CA0532DB9887F977262334B\edadb09679b1422183ee35c9b27f87cc\8c974ff4f20.jpeg)

### **2.1、Linux目录结构：**

/ 根目录 /bin 存放必要的命令

 /boot 存放内核以及启动所需的文件 

/dev 存放设备文件 

/etc 存放系统配置文件

 /home 普通用户的宿主目录，用户数据存放在其主目录中 

/lib 存放必要的运行库 

/mnt 存放临时的映射文件系统，通常用来挂载使用。 

/proc 存放存储进程和系统信息,存放内存数据 

/root 超级用户的主目录 

/sbin 存放系统管理程序

 /tmp 存放临时文件 

/usr 存放应用程序，命令程序文件、程序库、手册和其它文档。 

/var 系统默认日志存放目录 /opt 普通用户安装的软件 

#### **三种网络连接方式：**

-bridged（桥接连接方式：默认使用vmnet0虚拟网卡）（主机有网就有网，不需配置）

-nat（网络地址转换模式，默认使用vmnet8虚拟网卡）（需要借助网络地址转换功能才能上网，和主机网络进行连接转换）

-host-noly（仅主机模式，默认使用vmnet1虚拟网卡）（需要使用专用网络，需要主机的共享网络才能连接外网）

配置网络 windows：window+R cmd     命令：ipconfig ifconfig 

**配置网络**

（Wired：无线网）设置IPV4   Method改为Marual    Add  （选择nat网络，找子网IP） 编辑nat设置 例：如果子网IP为：192.168.216.0,网关IP可以设置为：192.168.216.2 Add中的Address可以设置为192.168.216.111 Add中的NetMask（子网掩码）可以设置为24， Add中的Gateway（网关）设置为：192.168.216.2 查看：ifconfig：IPV4为inet addr ping：ping 192.168.216.111 ping 192.168.216.2 ctrl+c：停止 在window下ping：ping 192.168.216.111 

### **2.2、Linux必备命令：**

- 默认进入系统，我们会看到这样的字符: [root@localhost ~]#,其中#代表当前是root用户登录，如果是$表示当前为普通用户。

- 我们了解linux由很多目录文件构成，那我们来学习第一个Linux命令：

- cd命令， cd /home ；解析：进入/home目录

- cd /root 进入/root目录 ；cd ../返回上一级目录;cd ./当前目录；（.和..可以理解为相对路径；例如cd /hom/test ，cd加完整的路径，可以理解为绝对路径）

- 接下来继续学习更多的命令：

-   ls ./ 查看当前目录所有的文件和目录。

- ls -a 查看所有的文件，包括隐藏文件,以.开头的文件。

  

- pwd显示当前所在的目录。

- mkdir创建目录，用法mkdir test ，命令后接目录的名称。

- rmdir 删除空目录

- rm 删除文件或者目录，用法 rm –rf test.txt (-r表示递归，-f表示强制)。

- cp 拷贝文件，用法,cp old.txt /tmp/new.txt ，常用来备份；如果拷贝目录

- 需要加 –r参数。

- mv 重命名或者移动文件或者目录，用法, mv old.txt new.txt

- touch 创建文件，用法，touch test.txt，如果文件存在，则表示修改当前文件时间。

- Useradd创建用户，用法 useradd wugk ，userdel删除用户。

- Groupadd创建组，用法 groupadd wugk1 ，groupdel删除组。

  

- find查找文件或目录，用法 find /home -name “test.txt”,命令格式为:

- find 后接查找的目录，-name指定需要查找的文件名称，名称可以使用*表示所有。

- find /home -name “*.txt” ;查找/home目录下，所有以.txt结尾的文件或者目录。

- vi 修改某个文件，vi有三种模式：

- 命令行模式、文本输入模式、末行模式。

- 默认vi打开一个文件，首先是命令行模式，然后按i进入文本输入模式，可以在文件里写入字符等等信息。

- 写完后，按esc进入命令模式，然后输入:进入末行模式，例如输入:wq表示保存退出。

- 如果想直接退出，不保存，可以执行:q!， q!叹号表示强制退出。

- cat 查看文件内容，用法 cat test.txt 可以看到test.txt内容

- more 查看文件内容，分页查看，cat是全部查看，如果篇幅很多，只能看到最后的篇幅。可以使用cat和more同时使用,例如： cat test.txt |more 分页显示text内容，|符号是管道符，用于把|前的输出作为后面命令的输入。

- echo 回显，用法 echo ok，会显示ok，输入什么就打印什么。

- echo ok > test.txt ；把ok字符覆盖test.txt内容，>表示追加并覆盖的意思。

- \>>两个大于符号，表示追加，echo ok >> test.txt,表示向test.txt文件追加OK字符，不覆盖原文件里的内容。

### **2.3、Linux权限管理**

#### **2.3.1、可读，可写 、可执行**

Linux的文件和目录有以下三种方式：

r  、w 、x:可读，可写 、可执行

- r-可读(read)
- w-可写(write)
- x-可执行(execute)

硬连接数：表示到达某个文件的方式有多少种

#### **2.3.2所有者 、所属组 、其他人**

Linux的文件和目录又可以有三个所有者概念:

u、g 、o: 所有者 、所属组 、其他人

- u：所有者
- g：所属组
- o：其他人

![img](D:\软件\腾讯\有道云笔记\有道云笔记数据\qqCEF95AFC5CA0532DB9887F977262334B\f030cd6416354c66b01c4374e2293fc1\clipboard.png)

### **2.4、权限管理命令：chmod命令**

语法： chmod  【mode】 文件或目录    此处mode**是数字**

-  **r - 4**
-  **w - 2**
-  **x - 1**

**2.5、权限**

​		在Linux操作系统中，root的权限是最高的，相当于windows的administrator，拥有最高权限，能执行任何命令和操作。在系统中，通过UID来区分用户的权限级别，UID等于0，表示此用户具有最高权限，也就是管理员。其他的用户UID依次增加，通过/etc/passwd用户密码文件可以查看到每个用户的独立的UID。

​		每一个文件或者目录的权限，都包含一个用户权限、一个组的权限、其他人权限，例如下：

- 标红第一个root表示该文件所有者是root用户，第二个root代表该文件的所属的组为root组，其他用户这里默认不标出。

  ```
  -  [root@node1 ~]# ls -l monitor_log.sh
  - -rw-r--r-- 1 root root 91 May 7 20:21 monitor_log.sh
  - [root@node1 ~]#
  ```

  如果我们想改变某个文件的所有者或者所属的组，可以使用命令chown

  ```
  chown –R test:test monitor_log.sh即可。
  ```

- 每个Linux文件具有四种访问权限：可读(r)、可写(w)、可执行(x)和无权限(-)。

- 利用ls -l命令可以看到某个文件或目录的权限，它以显示数据的第一个字段为 准。第一个字段由10个字符组成，如下：

  ```
  [root@node1 ~]# ls -l monitor_log.sh
  
  -rw-r--r-- 1 root root 91 May 7 20:21 monitor_log.sh
  
  [root@node1 ~]#
  ```

-   第一位表示文件类型，-表示文件，d表示目录；后面每三位为一组。

-   第一组：2-4位表示文件所有者的权限，即用户user权限，简称u

-   第二组：5-7位表示文件所有者所属组成员的权限，group权限，简称g

-   第三组：8-10位表示所有者所属组之外的用户的权限，other权限，简称o

  从上面这个文件，我们可以看出，monito_log.sh文件对应的权限为：

- root用户具有读和写的权限，root组具有读的权限，其他人具有读的权限。

  为了能更简单快捷的使用和熟悉权限，rwx权限可以用数字来表示，分别表示为r（4）、w（2）、x（1）。

- Monitor_log.sh权限可以表示为：644

  ```
  //如果给某个文件授权，命令为:
  chmod：chmod 777 monitor_log.sh
  ```

### **2.5、**Linux网络配置管理

- 熟悉了常用的命令和Linux权限，那接下来如何让所在的Linux系统上网呢？管理linux服务器网络有哪些命令呢？
-   Linux服务器默认网卡配置文件在/etc/sysconfig/network-scripts/下，命名的名称一般为:ifcfg-eth0 ifcfg-eth1 ，eth0表示第一块网卡，eth1表示第二块网卡，依次类推。一般DELL R720标配有4块千兆网卡。
-   修改网卡的IP，可以使用命令: vi /etc/sysconfig/network-scripts/ifcfg-eth0 如果是DHCP获取的IP，默认配置如下：
- \# Advanced Micro Devices [AMD] 79c970 [PCnet32 LANCE]
- DEVICE=eth0
- BOOTPROTO=dhcp
- HWADDR=00:0c:29:52:c7:4e
- ONBOOT=yes
- TYPE=Ethernet
- 如果是静态配置的IP，ifcfg-eth0网卡配置内容如下：
- \# Advanced Micro Devices [AMD] 79c970 [PCnet32 LANCE]
- DEVICE=eth0
- BOOTPROTO=static
- HWADDR=00:0c:29:52:c7:4e
- ONBOOT=yes
- TYPE=Ethernet
- IPADDR=192.168.33.10
- NETMASK=255.255.255.0
- GATEWAY=192.168.33.1
- 网卡参数详解如下：
- DEVICE=eth0  #物理设备名
- ONBOOT=yes  # [yes|no]（重启网卡是否激活设备）
- BOOTPROTO=static #[none|static|bootp|dhcp]（不使用协议|静态分配|BOOTP协议|DHCP协议）
- TYPE=Ethernet #网卡类型
- IPADDR=192.168.33.10 #IP 地址
- NETMASK=255.255.255.0 #子网掩码
- GATEWAY=192.168.33.1 #网关地址
- 网卡配置完毕，重启网卡，命令: /etc/init.d/network restart 即可。
- 查看ip命令：ifconfig 查看当前服务器所有网卡的IP，可以单独指定，ifconfig eth0 查看eth0的IP地址。
- 网卡配置完毕，如果来配置DNS，首先要知道DNS配置在哪个目录文件下，vi /etc/resolv.conf 文件:
- 在该文件里面添加如下两条：
- nameserver 202.106.0.20
- nameserver 8.8.8.8
- 从上到下，分别表示主DNS，备DNS。配置完毕后，不需要重启网卡,DNS立即生效。
- 可以ping www.baidu.com 看看效果:
- IP配置完毕后，我们可以通过远程工具来连接Linux服务器，常见的Linux远程连接工具有:putty、secureCRT（主流）、xshell、xmanger等工具。
- 下载安装secureCRT，打开工具，然后如图配置：
- 点击左上角quick connect快速连接，弹出界面，然后输入IP，用户名，端口默认是22，然后点击下方的connect连接，会提示输入密码，输入即可。
- 弹出输入密码框：
- 进入远程界面，与服务器真实登录一样，然后可以执行命令：
- 通过这几章的学习，我们已经熟练了Linux常用命令的操作，权限网络、网络配置、远程连接等知识，那接下来我们还能做什么呢？我们已经差不多入门了，接下来就是更进一步的服务配置，Linux系统到底用来做什么呢？接下来的章节将跟大家一起来学习。 
- Linux系统的应用，我们最开始介绍的时候简单介绍过，目前大中型企业都用它来承载web网站、数据库、虚拟化平台等，那接下来我们将在Linux系统安装各种服务和软件来实现Linux真正的价值。

## **三、Linux基本使用**

![img](D:\软件\腾讯\有道云笔记\有道云笔记数据\qqCEF95AFC5CA0532DB9887F977262334B\32c0416da5754f45a83521824c224662\qq图片20200414203352.jpg)

删除文件夹命令：rm   -r   文件名

Linux区分大小写

**文件和目录常用命令**

![img](D:\软件\腾讯\有道云笔记\有道云笔记数据\qqCEF95AFC5CA0532DB9887F977262334B\7c84333edc21495f9b2dda158fef4b40\4972ef7ba4e.jpeg)

### **3.1、终端**

#### **3.1.1、终端命令格式：**

command   【-options】   【parameter】 命令名	      选项		     参数 

#### **3.1.2、帮助信息：**

1.command --help 2.man command 其中： 空格键：显示手册的下一屏 Enter：一次滚动手册的一行 b：回滚一屏 f：前滚一屏 q：退出 

#### **3.1.3、终端使用技巧**

tab键	补全 

上/下键	曾经用过的命令

![img](D:\软件\腾讯\有道云笔记\有道云笔记数据\qqCEF95AFC5CA0532DB9887F977262334B\f2d7756d00dd4ff097bbb9130ebd355b\5ee82d5e1dc.jpeg)

### **3.2、命令的使用**

#### **3.2.1、ls命令说明**

ll命令：统计个数

其中文件夹蓝色字体，文件白色字体

d		文件夹（目录） -		文件 

- list类似DOS下的dir命令
- Linux文件或目录名称最长可以有256个字符
- 以  .	开头的文件为隐藏文件，需要使用-a才能显示
- .  代表当前目录
- ..  代表上一级目录

#### **ls   常用选项**

| 参数                         | 含义                             |
| ---------------------------- | -------------------------------- |
| -a                           | 显示所有文件（包括隐藏文件）     |
| 以 . 开头的文件为隐藏文件 -l | 以列表方式显示文件的详细信息     |
| -h                           | 配合-l以人性化的方式显示文件大小 |

例如：ls   -l   -h,	ls   -lh,	ls   -lha 

#### **3.2.2、通配符的使用**

| 通配符 | 含义                                                         |
| ------ | ------------------------------------------------------------ |
| *      | 代表任意个数的字符（包括0个字符）                            |
| ？     | 代表任意一个字符，至少一个                                   |
| [ ]    | 表示可以匹配字符组中的任意一个 [ abc ]	匹配a、b、c中的任意一个 [ a-f ]	匹配从a到f范围内的任意一个字符 切换目录 |

#### **3.2.3、cd命令（cd后有空格）**

| 命令  | 含义                                     |
| ----- | ---------------------------------------- |
| cd    | 切换到当前用户的主目录（/home/用户目录） |
| cd ~  | 切换到当前用户的主目录（/home/用户目录） |
| cd .  | 保持到当前的目录不变                     |
| cd .. | 切换到上级目录                           |
| cd -  | 可以在最近两次工作目录之间切换           |

#### **3.2.4、相对路径和绝对路径**

```
相对路径：在输入路径时，最前面不是/或者~，表示相对当前目录所在的目录位置
绝对路径：在输入路径时，最前面是/或者~，表示从根目录/家目录开始的具体目录位置 
```

#### **3.2.5、mkdir：创建一个新目录**

-p		可以递归创建目录			

例如：mkdir   -p   a/b/c/d 

#### **3.2.6、rm：删除文件或目录**

| 选项 | 含义                                                         |
| ---- | ------------------------------------------------------------ |
| -f   | 强制删除，忽略不存在的文件，无需提示                         |
| -r   | 递归地删除目录下的内容，删除文件夹时必须写此参数	例如rm  -r  * |

#### **3.2.7、tree：可以以树状图列出文件目录结构**

~                   家目录 -d			只显示目录 

#### **3.2.8、cp：复制**

将给出的文件或目录复制到另一个文件或目录中，相当于DOS下的copy命令	

| 序号 | 选项 | 含义                                                         |
| ---- | ---- | ------------------------------------------------------------ |
| 01   | -f   | 已经存在的目标文件直接覆盖，不会提示                         |
| 02   | -i   | 覆盖文件前提示                                               |
| 03   | -r   | 若给出的源文件是目录文件，则cp将递归复制该目录下的所有子目录和文件，目标文件必须为一个目录名 |

#### **3.2.9、mv：移动**

| 序号 | 命令                 | 含义           |
| ---- | -------------------- | -------------- |
| 01   | mv  文件1  文件2     | 文件重命名     |
| 02   | mv  -i  文件1  文件2 | 覆盖文件的提示 |

### **3.3、查看文件内容**

####  **3.3.1、cat、more、grep 区别：**

| 序号 | 命令                 | 对应英文    | 作用                                             |
| ---- | -------------------- | ----------- | ------------------------------------------------ |
| 01   | cat   文件名         | concatenate | 查看文件内容，创建文件，文件合并，添加文件等功能 |
| 02   | more  文件名         | more        | 分屏显示文件内容                                 |
| 03   | grep  搜索文本文件名 | grep        | 搜素文本文件内容                                 |

#### **3.3.2、cat**

- cat：命令可用来查看文件，创建文件，文件合并，添加文件内容等功能
- cat：会一次显示所有的内容，适合查看内容缺少的文本文件

| 选项 | 含义             |
| ---- | ---------------- |
| -b   | 对有内容的行编号 |
| -n   | 对所有的行编号   |

Linux还有个：nl的命令和cat -b等价                       

#### **3.3.3、more**

- more：命令可以用于分屏显示文件内容，每次只显示一页内容
- 适用于查看内容较多的文本文件

| 操作符  | 功能                 |
| ------- | -------------------- |
| 空格键  | 显示手册页的下一屏   |
| Enter键 | 一次滚动手册页的一行 |
| b       | 回滚一屏             |
| f       | 前滚一屏             |
| q       | 退出                 |
| /word   | 搜索word字符串       |

#### **3.3.4、grep**

- grep：一种强大的文本搜索工具
- grep允许对文本文件进行模式查找，所谓模式查找，又被称为正则表达式

| 选项 | 含义                                     |
| ---- | ---------------------------------------- |
| -n   | 显示匹配行及行号                         |
| -v   | 显示不包含匹配文本的所有行（相当于求反） |
| -i   | 忽略大小写                               |

**例：grep -n as 123.txt：搜索as关键词，并制定行号**

- 常用的两种模式查找

| 选项 | 含义                   |
| ---- | ---------------------- |
| ^a   | 行首，搜寻以a开头的行  |
| ke$  | 行尾，搜寻以ke结束的行 |

**例：grep -n ke$ demo.txt**

#### **3.3.5、文本编辑vi**

在 Linux 中，可以使用 vi 编辑器创建一个文本文件，
例如： vi filename

若文件 filename 不存在会创建并打开，按下 i 键即可进入编辑模式，你可以向文件中写入内容。
在 Linux 下一般使用 vi 编辑器来编辑文件。

vi 既可以查看文件也可以编辑文件。

三种模式：命令行、插入、底行模式。

切换到命令行模式：按 Esc 键；

切换到插入模式：按 i 、o、a 键；

```
i 在当前位置生前插入

I 在当前行首插入

a 在当前位置后插入

A 在当前行尾插入

o 在当前行之后插入一行

O 在当前行之前插入一行

切换到底行模式：按 :（冒号）；

打开文件：vim file

退出：esc  :q

修改文件：输入 i 进入插入模式

保存并退出：esc:wq

不保存退出：esc:q!
```

**1、打开命令：**

vi+filename  （还有各种打开的姿势，只不过我比较顺手这个）

**2、退出命令：**

：q  退出而且不保存修改的内容

：q! 强制退出不保存修改的内容

：wq 退出并且保存修改的内容

：wq! 强制保存修改的内容然后退出（修改了只读文件会用到）

 ZZ  退出并且保存修改的内容，相当于：wq，看个人习惯 

**3、光标移动命令**

个人比较喜欢上下左右方向键，字母 h (左) ，j (下)， k(上)，l(右)也是可以的

^ 光标移到行首

$ 光标移到行尾

shift+g 光标移动到文件最后一行

gg 光标移动到文件第一行

**4、控制命令**

打开一个内容很多的文件的时候经常用到。

Ctrl+d  向下滚半屏

Ctrl+u  向上滚半屏

Ctrl+f   向下滚全屏

Ctrl+b  向上滚全屏

**5、编辑命令i**

主要是进入编辑状态，也就是insert状态

i 光标当前位置开始编辑

o 光标的下一行开始编辑

shift+o 光标的上一行开始编辑 

**6、删除命令d**

dd  删除一行，可以带个数字，如6dd，表示向下删除6行

d$  删除光标到行尾的内容（也可以使用ctrl+d）

d^  删除光标到行首的内容

x   删除光标位置的字符（向后删除）

shift+x  删除光标位置的字符（向前删除）

**7、替换命令r**

r  按esc退出insert状态再按个r,然后再输入一个字符，将会替换光标位置的字符

R 跟r一样，只不过是可以替换多个字符

：s/aa/bb/g   替换当前行的所有aa将会变成bb

：%s/aa/bb/g  替换整个文件的,所有aa将会变成bb

：n1,n2s/aa/bb/g  替换n1到n2行之间所有的aa变成bb

**8、查找命令**

/String 查找一个字符串（向下开始）

？String 查找一个字符串（向上开始）

n  向后查找下一个 

shift+n  向前查找下一个

**9、粘贴复制命令**

yw 复制一个单词

yy  复制一行，和删除dd一样可以带个数字，6yy复制六行（向下复制6行）

p  粘贴到光标位置的下一行

shift+p  粘贴到光标位置的上一行

**10、同时打开两个文件**

比如：aa.txt  ss.txt

打开第一个文件vi aa.txt然后输入下面的命令

：sp ss.txt    此时就在同一个窗口打开另外一个ss.txt 

 Ctrl+w  进行两个文件上下窗口切换（需要再按上下方向键）  

**11、其他常用命令**

：e!  重新加载文件，再查看日志文件的时候可以用，不断在变化的文件。

 shift+j  将下一行拼接到上一行

 u  撤销

：set nu  显示行号 

：n  跳转到第n行（按回车才会跳）

Ctrl+g 会在显示屏的底部显示文件名字和总的行数，当前光标的位置行号

~  这个将会改变光标位置的字符的大小写

Ctrl +a 跳到当前命令行里的首位，比如 cd /etc/profile ,这个是一个文件，我想改成vi /etc/profile 就可以按 ctrl+a 光标就会移到cd位置，如果碰到比较长的命令，这个还是非常的实用的

ctrl+e 跳到当前命令行的末尾。和ctrl+a 相反

**12、文本编辑器：gedit**

用记事本方式打开文本文件，语法：gedit  文件名称

#### **3.3.6、其它**

**3.3.5.1、echo：在终端中显示参数指定文字，通常和重定向联合使用**

​		echo ${JAVA_HOME}

**3.3.5.2、重定向>和>>**

exho 你好 >> a      ：将内容追加到文件a中 exho 你好 > a       ：将内容覆盖到文件a中 ls -lh > a          ：将ls -lh输出的结果显示到a文件里面中 

**3.3.5.3、管道|：**

**将一个命令的输出作为一个命令的输入**

例如：管子的一头塞东西，另一头取出来，这里|的左右分为两端，左端塞东西，右端取东西

**常用命令有：**

- more：分屏显示内容
- grep：在命令执行结果的基础上查询指定的文本

例如：ls -lha ~ | more：分屏显示家目录（包含隐藏文件）

   ls -lha ~ | grep vi：显示家目录中所有（包含隐藏文件）包含vi的文件

### **3.4、远程管理常用命令**

#### **3.4.1、关机/重启**

语法：shutdown  选项  时间

​			-r：重启

​			-c：取消关机计划

​			shutdown now：立刻关机

​		poweroff：关机

​		reboot：重启

#### **3.4.2、网卡和IP地址**

- 网卡：网卡是一个专门负责网络通讯的硬件设备
- IP地址是设置在网卡上的地址信息
- 可以将电脑比作电话，网卡相当于SIM卡，IP地址相当于电话号码

#### **3.4.3、ifconfig**

可以查看/配置计算机当前的网卡配置

| 序号 | 命令        | 作用                              |
| ---- | ----------- | --------------------------------- |
| 01   | ifconfig    | 查看/配置计算机当前的网卡配置信息 |
| 02   | ping ip地址 | 检测到目标IP地址的链接是否正常    |

例：ifconfig | grep inet：查看网卡队以ing的IP地址

#### **3.4.4、ping**

检测目标IP地址的链接是否正常

### **3.5、远程登陆和复制文件**

#### **3.5.1、SSH基础**

- 常见的端口号列表

| 序号 | 服务      | 端口号 |
| ---- | --------- | ------ |
| 01   | SSH服务器 | 22     |
| 02   | Web服务器 | 80     |
| 03   | HTTPS     | 445    |
| 04   | FTP服务器 | 21     |

- 通过IP地址找到网络上的计算机
- 通过端口号找到计算机上运行的应用程序
- 域名是IP地址的别命

#### **3.5.2、ssh：远程登录计算机**

​			ssh -p 端口 用户名@ip

​		scp：远程复制文件

​			用户名@ip地址:文件名或路径 用户名@ip地址:文件名或路径

​			例：

​			    scp -P 22 lj@192.168.1.77:/Desktop/a /Test/       远程>本机

scp -r demo @192.168.1.77:Desktop  		     将demo文件夹复制到桌面

​			    scp /Test/a.txt lj@192.168.1.77:/Desktop/a 	 本机>远程

​		免密码登录：

​			ssh-keygen：生成ssh钥匙

​			ssh-copy-id -p 端口 user@ip,将自己的公钥发送给服务器

添加到.ssh目录下（隐藏文件前有.） 本地使用私钥对数据进行加密/解密 服服务器使用公钥对数据进行加密/解密 

#### 	**3.5.3、设置计算机别名**

原因：每次都输入：ssh -p port user@remote，时间久了会麻烦

//在.ssh下创建config touch config gedit config 

在~/.ssh/config里面添加以下内容 在.ssh目录下创建config文件，里面添加 Host myserver    HostName 192.168.1.77    User lj(远程计算机用户名)    Point 22 

保存后就可以使用ssh myserver登录远程主机

ssh myserver 

例如：将桌面复制到demo文件夹下

scp -r ~/Desktop myserver:Desktop/demo 

windows下的SSH客户端安装

Putty，XShell，exut：退出登录

例如：scp -P 22 python@127.16.140.138:Desktop/01.py .

#### **3.5.3、scp**

| 选项 | 含义                                                         |
| ---- | ------------------------------------------------------------ |
| -r   | 若给出的源文件是目录，则scp将递归复制到该目录下的所有子目录和文件 |
| -P   | 若远程SSH服务器端口不是22，需要使用大写字母-P选项指定端口    |

- 注意：scp只能在Linux或UNIX系统下使用
- 如果在windows系统中，可以安装PuTTY，使用pscp命令行工具或者安装**FileZilla**使用FTP进行文件传输

### **3.6、权限管理**

#### **3.6.1、目录结构**

举例：当使用ls -l时输出如下信息：

​		-rw-rw-r-- 2 lj   dev  45 2月 9 05:56 c.txt

​		drwxrw---x 6 root root 45 2月 9 05:56 java1.8.0_241

​		其中权限后面2和文件夹的6表示硬链接数

​	其表示含义为：目录：d，文件：-

​		目录	拥有者权限	组权限	其他用户权限	拥有者	所属组	文件名

​		-	rw-		rw-	r--		lj	Dev	c.txt

​		d	rwx		rw-	--x		root	root	java1.8.0_241

​	Linux对每个文件目录的权限包括

​		权限	缩写	数字代号

​		读	r	4

​		写	w	2

​		执行	x	1

#### **3.6.2、用户权限**

​	为了方便用户管理，提出了组(grop)的概念

​	**超级用户roo**t：可对系统文件进行操作，不推荐直接使用root

​		如果安装系统时没有设置root密码，可以使用sudo passwd root  命令修改root密码

​	**标准账户**：只能在该用户home文件夹内操作的用户

**sudo （****Linux系统****管理指令）**：表示bai “superuser do”。 它允许已验证的用户du以其他用户的身份来运行命令。其他用户可以zhi是普通用户或者超级dao用户。然而，大部分时候我们用它来以提升的权限来运行命令。

​	---------------------------------现在正式开始学习权限------------------------------------

#### **3.6.3、修改文件权限：chmod**

​		chmod：修改用户/组对文件/目录的权限

​			-R：递归修改，修改目录下所有文件及子文件权限

​				+：增加权限 chmod +rwx a.txt	增加用户读写执行权限

​				-：减少权限 chmod -x a.txt	取消用户的执行权限

chmod +r 01.py  ：增加可读权限 

​			最常用的修改权限方式

​				chmod -R 755 dir

​				通过上面可以看到每一个权限都有数字代号则：

​					第一个数字7表示拥有者权限：读 r 4，写 w 2，执行 x 1

​					第二个数字5代表组权限：读 r 4，执行 x 1

​					第三个数字5代表其他用户权限：同理，4+1

​				

​		chown：修改拥有者

​		chgrp：修改所属组

​	上面说过不推荐使用root账户操作，如果要进行系统维护可以使用以下命令代替：

​		sudo 要执行的命令：借用root权限执行命令：

​			sudo poweroff

​			输入密码后5分钟之内使用sudo命令不需要在输入密码

#### 	**3.6.4、chmod组管理相关命令：**

| 序号 | 命令                 | 作用                |
| ---- | -------------------- | ------------------- |
| 01   | groupadd 组名        | 添加组              |
| 02   | groupdel 组名        | 删除组              |
| 03   | cat /etc/group       | 确认组信息          |
| 04   | chgrp 组名 文件/目录 | 修改文件/目录所属组 |

学习演示

创建目录：mkdir Python学习 添加组：sudo groupadd dev 确认组信息：cat /etc/group 查看：ls -l  （默认情况下组为当前用户） 修改组为dev：sudo chgrp -R dev Python学习/ 查看：li -l 

例：chgrp -R dev Linux学习/

​	组信息都保存在/etc/group文件中

#### **3.6.5、用户管理命令：**

​		useradd -m -g 组名 用户名：创建用户

​			-m：自动创建用户家目录

​			-g：指定用户所在组，否则会建一个和用户同名的组

​		passwd 用户名：设置/修改密码

​		userdel：删除用户

​			-r：自动删除用户家目录

​		cat /etc/passwd | grep 用户名：确认用户

​		用户信息都保存在/etc/passwd文件中

​		拓展：使用cat /etc/passwd或终端会显示如下内容

​			....

​			lj:x:1000:1000:lj,,,:/home/lj:/bin/bash

​			其由6个分号组成7个信息分别是：

​				用户名

​				密码，一般不会显示

​				UID(用户标识)

​				GID(主组标识)属于那个组

​				用户全名或本地账号

​				家目录

​				登录使用的shell。

​					默认使用shell：bash。同样还有dash，sh等终端

​					使用useradd创建的用户默认使用的dash

#### 	**3.6.6、查看用户信息**

​		id 用户名：查看用户UID和GID信息

​			以我的为例：

​				uid=1000(lj) gid=1000(lj) 组=1000(lj),4(adm),24(cdrom),

​					27(sudo),30(dip),46(plugdev),116(lpadmin),126(sambashare)

​			依次为UID GID 附加组(一个用户可存在多个附加组中)

​			主组和附加组下面马上会说

​		who：查看当前所有登录的用户列表

​		whoami：查看当前登录用户的账户名

#### 	**3.6.7、usermod：设置用户的主组**

可以用来设置用户的主组/附加组和登录shell

​		主组：在etc/passwd的第4列GID对应的组

​		附加组：在etc/group中最后一列表示该组的用户列表，用于指定用户的附加权限

​			还是不知道附加组的作用？

​				当你用新用户操作计算机时，如果想用sudo以root身份执行

​				命令时，会被告知没有权限执行sudo命令。

​				此时解决办法就是找可以使用root权限的用户将你的账户添加到你的sudo组。

​		设置附加组后，需要重新登录才能生效

​		-g 修改用户主组(passwd中的GID)

​		-G 修改用户附加组

​		-s 修改用户登录shell

例：sudo usermod -G sudo -s /bin/bash 用户名 

#### 	**3.6.8、which：查看命令的路径**

​		同理，看过上面的介绍我们知道在/etc/passwd下保存的是所有用户信息，

​		passwd user可修改密码，那么他们时一样的吗。

​		执行which passwd可以看到：passwd路径为：/usr/bin/passwd

​		并不是/etc/passwd，这个文件并不能执行。

#### 	**3.6.9、sh：切换用户**

​		sh - user：切换用户并回到指定用户的家目录，如果不加-则保持当前位置不变

#### **3.6.10、rz：上传文件**

# **Linux笔记2**

## **一、常用命令**

​	**ls：查看当前文件夹下内容**

​		-a：显示所有子目录和文件，包括隐藏文件

​		-l：以列表方式显示文件详细信息

​		与通配符使用：

​			ls ?.java

​	**pwd：查看当前所在文件夹**

​	**cd：切换文件夹**

​		.：当前目录

​		..：上级目录

​		~：用户主目录（/home/用户目录）

​		-：在最近两次目录之间切换

​	**touch：新建文件**

​		如果文件不存在则新建

​		如果存在修改文件末次修改日期

​	**mkdir：新建文件夹**

​		-p：可以递归创建目录，新建目录不能与已有目录重名

​	**rm：删除文件**

​		-r：递归删除目录下的内容，删除文件夹必须加此参数

​		-f：强制删除，忽略不存在文件，无提示

​	**tree：以树状图显示文件目录结构**

​		-d：只显示文件夹

​	**cp:拷贝文件**

​		-f：文件已存在直接覆盖，不提示

​		-i：覆盖文件时提示

​		-f：若给出的源文件是目录，则cp将递归复制该目录下的所有目录和文件

​	**mv：移动文件，如果目标路径指向当前目录，则可以改文件名**

​		-i：覆盖提示

​	**cat：查看文本内容、创建文件，文件合并，追加文件内容**

​		-b：对非空输出行编号

​		-n：输出所有行编号

​	**more：分屏显示文件内容，适应内容较多的文件**

​	**grep：搜索文件或结果内容，通常和管道命令|结合使用**

​		例：ls -a / | Python1.py

​		-n：显示匹配行及行号

​		-v：显示不包括匹配文本的所有行（相当求反）

​		-i：忽略大小写

​		常用两种查找模式：

​			^a：行首，搜索以a开头的行

​			ke$：行尾，搜索以ke结尾的行

​			例：grep -n ke$ demo.txt

​	**echo：在终端中显示参数指定文字，通常和重定向联合使用**

​		echo ${JAVA_HOME}

​		exho 你好 >> a.txt ,追加文件，内容

​		exho 你好 > a.txt ，覆盖文件内容

​	**clear：清屏**

​	**命令 --help：查阅简略命令帮助信息**

​	**man 命令：命令详细使用说明**

​	**|：管道命令，将一个命令的输出作为一个命令的输入**

​		例：ls -alh / | grep Do

​	**vi/vim：编辑文本文件，工具使用：自己会就行，不想写了**

## **二、远程管理常用命令**

**关机重启**

​		shutdown 选项 时间

​			-r：重启

​			-c：取消关机计划

​			shutdown now：立刻关机

​		poweroff：关机

​		reboot：重启

​	查看或配置网卡信息：

​		ifconfig：查看或配置计算机当前网卡配置信息

​			ifconfig | grep inet

​		ping：检查与目标ip连接是否正常

​	远程登录和赋值文件

​		ssh：远程登录计算机

​			ssh -p 端口 用户名@ip

​		scp：远程复制文件

​			用户名@ip地址:文件名或路径 用户名@ip地址:文件名或路径

​			例：

​			    scp -P 22 lj@192.168.1.77:/Desktop/a /Test/  远程>本机

​			    scp /Test/a.txt lj@192.168.1.77:/Desktop/a 	 本机>远程

​		免密码登录：

​			ssh-keygen：生成ssh钥匙

​			ssh-copy-id -p 端口 user@ip,将自己的公钥发送给服务器

​		设置计算机别名：

​			在.ssh目录下创建config文件，里面添加

​				Host myserver

​					HostName 192.168.1.77

​					User lj

​					Point 22

​			保存后就可以使用ssh myserver登录远程主机

## **三、权限操作**

​	举例：当使用ls -l时输出如下信息：

​		-rw-rw-r-- 2 lj   dev  45 2月 9 05:56 c.txt

​		drwxrw---x 6 root root 45 2月 9 05:56 java1.8.0_241

​		其中权限后面2和文件夹的6表示硬链接数

​	其表示含义为：目录：d，文件：-

​		目录	拥有者权限	组权限	其他用户权限	拥有者	所属组	文件名

​		-	rw-		rw-	r--		lj	Dev	c.txt

​		d	rwx		rw-	--x		root	root	java1.8.0_241

​	Linux对每个文件目录的权限包括

​		权限	缩写	数字代号

​		读	r	4

​		写	w	2

​		执行	x	1

​	为了方便用户管理，提出了组(grop)的概念

​	**超级用户root**：可对系统文件进行操作，不推荐直接使用root

​		如果安装系统时没有设置root密码，可以使用sudo passwd root  命令修改root密码

​	**标准账户**：只能在该用户home文件夹内操作的用户

​	---------------------------------现在正式开始学习权限------------------------------------

​	文件权限：

​		chmod：修改用户/组对文件/目录的权限

​			-R：递归修改，修改目录下所有文件及子文件权限

​				+：增加权限 chmod +rwx a.txt	增加用户读写执行权限

​				-：减少权限 chmod -x a.txt	取消用户的执行权限

​			最常用的修改权限方式

​				chmod -R 755 dir

​				通过上面可以看到每一个权限都有数字代号则：

​					第一个数字7表示拥有者权限：读 r 4，写 w 2，执行 x 1

​					第二个数字5代表组权限：读 r 4，执行 x 1

​					第三个数字5代表其他用户权限：同理，4+1

​				

​		chown：修改拥有者

​		chgrp：修改所属组

​	上面说过不推荐使用root账户操作，如果要进行系统维护可以使用以下命令代替：

​		sudo 要执行的命令：借用root权限执行命令：

​			sudo poweroff

​			输入密码后5分钟之内使用sudo命令不需要在输入密码

​	组管理相关命令：

​		groupadd 组名：添加组

​		groupdel 组名：删除组

​		cat /etc/group：确认组信息

​		chgrp：修改文件/目录所属组

​			例：chgrp -R dev Linux学习/

​		组信息都保存在/etc/group文件中

​	用户管理命令：

​		useradd -m -g 组名 用户名：创建用户

​			-m：自动创建用户家目录

​			-g：指定用户所在组，否则会建一个和用户同名的组

​		passwd 用户名：设置/修改密码

​		userdel：删除用户

​			-r：自动删除用户家目录

​		cat /etc/passwd | grep 用户名：确认用户

​		用户信息都保存在/etc/passwd文件中

​		拓展：使用cat /etc/passwd或终端会显示如下内容

​			....

​			lj:x:1000:1000:lj,,,:/home/lj:/bin/bash

​			其由6个分号组成7个信息分别是：

​				用户名

​				密码，一般不会显示

​				UID(用户标识)

​				GID(主组标识)属于那个组

​				用户全名或本地账号

​				家目录

​				登录使用的shell。

​					默认使用shell：bash。同样还有dash，sh等终端

​					使用useradd创建的用户默认使用的dash

​	查看用户信息：

​		id 用户名：查看用户UID和GID信息

​			以我的为例：

​				uid=1000(lj) gid=1000(lj) 组=1000(lj),4(adm),24(cdrom),

​					27(sudo),30(dip),46(plugdev),116(lpadmin),126(sambashare)

​			依次为UID GID 附加组(一个用户可存在多个附加组中)

​			主组和附加组下面马上会说

​		who：查看当前所有登录的用户列表

​		whoami：查看当前登录用户的账户名

​	usermod：可以用来设置用户的主组/附加组和登录shell

​		主组：在etc/passwd的第4列GID对应的组

​		附加组：在etc/group中最后一列表示该组的用户列表，用于指定用户的附加权限

​			还是不知道附加组的作用？

​				当你用新用户操作计算机时，如果想用sudo以root身份执行

​				命令时，会被告知没有权限执行sudo命令。

​				此时解决办法就是找可以使用root权限的用户将你的账户添加到你的sudo组。

​		设置附加组后，需要重新登录才能生效

​		-g 修改用户主组(passwd中的GID)

​		-G 修改用户附加组

​		-s 修改用户登录shell

​		例：

​			sudo usermod -G sudo -s /bin/bash 用户名

​	which：查看命令的路径

​		同理，看过上面的介绍我们知道在/etc/passwd下保存的是所有用户信息，

​		passwd user可修改密码，那么他们时一样的吗。

​		执行which passwd可以看到：passwd路径为：/usr/bin/passwd

​		并不是/etc/passwd，这个文件并不能执行。

​	sh：切换用户

​		sh - user：切换用户并回到指定用户的家目录，如果不加-则保持当前位置不变

​	--------------------------------权限讲完了，是不是很简单---------------------------------

## **四、系统信息相关命令**

​	**时间和日期：**

​		cal：查看日历

​			-y：可以查看一年的日历

​		date：查看系统时间

​	**磁盘和目录空间：**

​		df：disk free展示磁盘剩余空间

​			-h：以人性化展示磁盘剩余空间(K,M,G...)

​		du 目录名：disk usage显示目录下的文件大小

​			如果不加目录名，可查看当前目录占用情况

​			-h：以人性化展示磁盘剩余空间(K,M,G...)

​	**进程信息：**

​		ps：查看进程详细情况

​			-a：显示终端上的所有进程，包括其他用户进程

​			-u：显示进程的详细状态

​			-x：显示没有控制终端的进程

​		top：动态显示运行中的进程并且排序，目前我使用的时htop

​		kill [-9] 进程代号(PID)：终止指定代号的进程，-9表示强行终止。

​			不要终止其他用户开启的进程，尤其时root

​	其他命令：

​	**find：查找文件**

find Desktop/ -name "*.txt" 

​			find ～/Desktop/LibrarySys/ -name *.java：

​				查找指定目录下以.java结尾的文件，包括字目录

​		ln：创建软/硬链接

​			软连接：如果源文件删除则软连接失效，相当于win系统的快捷方式。创建软连接

​				时最好使用绝对路径，这样移动源文件都软连接地址也会跟踪改变。

​			硬链接：删除源文件不会影响文件访问，但文件硬链接数减1，如果文件硬连接数为0，则文件数据被删除。

​				文件硬连接数在哪里看？回到权限操作，刚开始时就说过。

​			硬链接原理：

​				Linux删除文件其实删除的时文件名。

​				在Linux中，文件名和文件数据是分开存储的，文件名指向文件数据。

​				软连接指向的是文件名，而硬链接指向的是文件数据，

​				所以删除文件后硬连接还可以访问。

​			-s：创建软连接，如果不加则是硬链接。

​				ln -s /usr/local/software/idea/bin/idea.sh ~/Desktop/"idea快捷方式"

​				在桌面创建idea快捷方式

​		**tar:打包/解包:**

​				-c：生成文档文件，创建打包文件

​				-x：解开档案文件

​				-v：显示详细过程，进度

​				-f：指定档案文件名称，f后面一定是.tar文件，所以必须放到选项最后

​				打包：

​					tar -cvf 打包文件.tar 被打包文件路径

​				解包

​					tar -xvf 打包文件

​					tar -cvf py.tar 01.py 02.py 03.py：将指定文件打包为py.tar

​					tar -xvf py.tar

​				但tar命令只负责打包，没有压缩功能，要想压缩需要使用gzip和bzip2

​			压缩/解压缩:

​				-z：可调用gzip

​					tar -zcvf py.tar.gz *.py 将所有以py结尾的文件压缩到指定压缩包

​					tar -zxvf py.tar.gz 解压缩

​				-j：可调用bzip2

​					tar -jcvf py.tar.bz2 *.py 压缩

​					tar -jxvf py.tar.bz2 解压缩

​		apt/apt-get：安装包管理工具

​			apt与apt-get作用一样，但apt除了有apt-get功能外，还有自动到参数

​			apt使用一般要使用root权限

​			首先需要更新源：apt update

​			更新已安装的包：apt upgrade

​			清理无用的包：apt clean && autoclean

​			安装软件：apt install

​			升级系统：apt dist-upgrade

​			删除包：apt-get remove

​			......

​			参数

​				-d：仅下载软件包，而不安装或解压

​				-f：修复系统中的软件包依懒性问题

​				-m：发现缺少软件包，仍视图执行

​				-q：将输出作为日志保留，不获取命令进度

​				--purge：与remove子命令一起使用，完全卸载软件包

​				--reinstall：与install子命令一起使用，重新安装软件包

​				-b：在下载完源码包后，编译生成相应的软件包

​				-s：不做实际操作，只是模拟命令执行结果

​				-y：对所有询问都作肯定的回答，apt-get不再进行任何提示

​				-u：获取已升级的软件包列表 

​				-h：获取帮助信息

​				-v：获取帮助信息