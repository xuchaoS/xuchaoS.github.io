---
layout: post
title:  "Linux基础学习"
date:   2017-03-30 17:48:00 +0800
categories: Linux
---
## 1. 启动级别

* **0：停机**
* **1：单用户形式，只root进行维护**
* 2：多用户，不能使用net file system
* **3：完全多用户**
* 4：安全模式
* **5：图形化**
* **6：重启**

其实，可以通过查看`/etc/rc.d/`中的`rc*.d`的文件来对比理解，不同的目录中包含的命令是不同的  
`init 0`，对应的系统会运行，`/etc/rc.d/rc0.d`里指定的程序
S00killall、 S01halt  
这两个都表示为终止进程。故init 0是用于表示关机的

## 2. Shell终端

如同其他UNIX类系统，Linux本身也是基于命令行的。试试 `Ctrl+Alt+F1`。这就是控制台，算是Linux的本来面目。至于使用方法，除了多出登录注销外，和前面章节所提到的“终端”差不多。 在X-Window出问题或不运行X-Window的时候，操作主要在这里完成。

在控制台终端下登录时，“login: ”处输入帐号，“password: ”处输入口令。同样为安全起见，输入的口令不显示。控制台终端注销用命令“logout”。在控制台终端启动的后台程序不会因为注销而终止。

前面说过，控制台终端“算是Linux的本来面目”。也只能“算是”，因为各终端里运行的命令行程序并不是终端本身，更不是Linux本身。像X-Window一样，那个命令行程序实质上也是个外围程序，叫“shell”。

“shell”，壳。从名字看，也许你已经猜到一些东西。不错，它包含了用户界面功能，负责接收使用者输入的东西，翻译后发送给Linux内核处理。如果有输出信息，它也会把输出信息显示出来。相对DOS而言，shell就相当于“command.com”。

shell同样能进行由几个命令串成的“批处理”。与“command.com”不同，shell的功能要强大许多。一个功能稍强的shell脚本，已经具备高级语言的语法结构，因此编写shell脚本在很多情况下也被看作是编程。

一般情况下，在控制台终端登录或在图形界面下开启“终端”，默认都会启动一个shell来接待使用者。

可以在shell的命令行里启动另外一个shell。退出当前shell的通用命令是“exit”。如果当前使用的shell正是控制台登录后启动的，则`exit`等效于`logout`。

shell有很多种，各有特色。目前使用比较广泛的是shell是“bash”，主要的Linux发行版均以其作为默认的shell。 “bash”和其他主流shell都支持一次输入多个命令，支持启动后台程序。如果要依次执行多个命令，命令间用“;”隔开；如果要让这个程序在后台运 行，在命令后面加“&”。

`/etc/passwd`中记录了每个用户登录使用的默认shell终端，通常shell脚本中第一行中的`#!/bin/bash`即为直接执行此脚本所调用的默认shell。

## 3. 环境变量

### 1) 什么是环境变量

环境变量是一个具有特定名字的对象，它包含了一个或者多个应用程序所将使用到的信息。许多用户（特别是那些刚接触Linux的新手）发现这些变量有些怪异或者难以控制。其实，这是个误会：通过使用环境变量，你可以很容易的修改一个牵涉到一个或多个应用程序的配置信息。

简单的说，环境变量就是在操作系统层面供程序间记录、读取状态的变量。

### 2) 环境变量文件和配置

Linux中环境变量包括系统级和用户级，系统级的环境变量是每个登录到系统的用户都要读取的系统变量，而用户级的环境变量则是该用户使用系统时加载的环境变量。
所以管理环境变量的文件也分为系统级和用户级的，下面贴一个网上找到的讲的比较明白的文件介绍：

1. 系统级：  
**`/etc/profile`：该文件是用户登录时，操作系统定制用户环境时使用的第一个文件，应用于登录到系统的每一个用户。该文件一般是调用`/etc/bash.bashrc`文件。**  
`/etc/bash.bashrc`：系统级的bashrc文件。  
`/etc/environment`:在登录时操作系统使用的第二个文件,系统在读取你自己的profile前,设置环境文件的环境变量。

2. 用户级（这些文件处于家目录下）：  
`~/.profile`:每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次!默认情况下,他设置一些环境变量,执行用户的.bashrc文件。这里是推荐放置个人设置的地方  
**`~/.bashrc`:该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该文件被读取。不推荐放到这儿，因为每开一个shell，这个文件会读取一次，效率肯定有影响。**  
`~/.bash_profile` or `~./bash_login`：这里没有引用作者的，下面会提到  
`~/.pam_environment`:用户级的环境变量设置文件，没有做测试，不知道管不管用。  

### 3) 相关命令

`PATH=/usr/local/bin:$PATH`: 变量赋值
`export PATH`: 导出为环境变量（脚本中为永久，命令行中为临时）  
`source ~/.bashrc`: 使环境变量文件立即生效  
`echo $PATH`: 打印当前环境变量  
`env`: 查看当前所有环境变量

## 4. 常用命令

### 1) 常用指令

**`ls`　　        显示文件或目录**

* **`-l`           列出文件详细信息l(list)**

* **`-a`          列出当前目录下所有文件及目录，包括隐藏的a(all)**

**`mkdir`         创建目录**

* **`-p`           创建目录，若无父目录，则创建p(parent)**

**`cd`               切换目录**

**`touch`          创建空文件**

**`cat`              查看文件内容**

**`cp`                拷贝**

**`mv`               移动或重命名**

**`rm`               删除文件**

* **`-r`            递归删除，可删除子目录及文件**

* **`-f`            强制删除**

**`find`              在文件系统中搜索某文件**

**`wc`                统计文本中行数、字数、字符数**

**`grep`             在文本文件中查找某个字符串**

`rmdir`           删除空目录

`tree`             树形结构显示目录，需要安装tree包

**`pwd`              显示当前目录**

`ln`                  创建链接文件

**`more`、`less`  分页显示文本文件内容**

**`head`、`tail`    显示文件头、尾内容**


### 2) 系统管理命令

`stat`              显示指定文件的详细信息，比ls更详细

`who`               显示在线登陆用户

`whoami`          显示当前操作用户

`hostname`      显示主机名

`uname`           显示系统信息

**`top`                动态显示当前耗费资源最多进程信息**

**`ps`                  显示瞬间进程状态 `ps aux` 或 `ps -ef`**

**`du`                  查看目录大小 `du -h /home`带有单位显示目录信息**

**`df`                  查看磁盘大小 `df -h` 带有单位显示磁盘信息**

**`ifconfig`          查看网络情况**

**`ping`                测试网络连通**

**`netstat`          显示网络状态信息**

**`man`                命令不会用了，找男人  如：`man ls`**

`clear`              清屏

`alias`               对命令重命名 如：`alias showmeit="ps -aux"` ，另外解除使用`unaliax showmeit`

**`kill`                 杀死进程，可以先用`ps` 或 `top`命令查看进程的id，然后再用`kill`命令杀死进程。**

### 3) 打包压缩相关命令

`gzip`：

`bzip2`：

**`tar`:                打包压缩**

* **`-c`              归档文件**

* **`-x`              压缩文件**

* **`-z`              `gzip`压缩文件**

* **`-j`              `bzip2`压缩文件**

* **`-v`              显示压缩或解压缩过程 `v`(view)**

* **`-f`              使用档名**

例：

`tar -cvf /home/abc.tar /home/abc`              只打包，不压缩

`tar -zcvf /home/abc.tar.gz /home/abc`        打包，并用gzip压缩

`tar -jcvf /home/abc.tar.bz2 /home/abc`      打包，并用bzip2压缩

当然，如果想解压缩，就直接替换上面的命令  `tar -cvf  /` `tar -zcvf  /` `tar -jcvf` 中的`c` 换成`x` 就可以了。

 

### 4) 关机/重启机器

`shutdown`

* `-r`             关机重启

* `-h`             关机不重启

* `now`          立刻关机

`halt`               关机

**`reboot`          重启**

 

### 5) Linux管道

**将一个命令的标准输出作为另一个命令的标准输入。也就是把几个命令组合起来使用，后一个命令除以前一个命令的结果。**

**例：`grep -r "close" /home/* | more`       在home目录下所有文件中查找，包括close的文件，并分页输出。**

 

### 6) Linux软件包管理(Ubuntu)

dpkg (Debian Package)管理工具，软件包名以.deb后缀。这种方法适合系统不能联网的情况下。

比如安装tree命令的安装包，先将tree.deb传到Linux系统中。再使用如下命令安装。

`sudo dpkg -i tree_1.5.3-1_i386.deb`         安装软件

`sudo dpkg -r tree`                                     卸载软件

 

注：将tree.deb传到Linux系统中，有多种方式。VMwareTool，使用挂载方式；使用winSCP工具等；

APT（Advanced Packaging Tool）高级软件工具。这种方法适合系统能够连接互联网的情况。

依然以tree为例

`sudo apt-get install tree`                         安装tree

`sudo apt-get remove tree`                       卸载tree

`sudo apt-get update`                                 更新软件

`sudo apt-get upgrade`        

 

将.rpm文件转为.deb文件

.rpm为RedHat使用的软件格式。在Ubuntu下不能直接使用，所以需要转换一下。

`sudo alien abc.rpm`

 

### 7) vim使用

vim三种模式：命令模式、插入模式、编辑模式。使用ESC或i或：来切换模式。

命令模式下：

`:q`                      退出

`:q!`                     强制退出

`:wq`                   保存并退出

`:set number`     显示行号

`:set nonumber`  隐藏行号

`/apache`            在文档中查找apache 按n跳到下一个，shift+n上一个

`yyp`                   复制光标所在行，并粘贴

`h`(左移一个字符←)、`j`(下一行↓)、`k`(上一行↑)、`l`(右移一个字符→)

 

### 8) 用户及用户组管理

**`/etc/passwd`    存储用户账号**

`/etc/group`       存储组账号

`/etc/shadow`    存储用户账号的密码

`/etc/gshadow`  存储用户组账号的密码

`useradd 用户名`

`userdel 用户名`

`adduser 用户名`

`groupadd 组名`

`groupdel 组名`

`passwd root`     给root设置密码

`su root`

`su - root` 

`/etc/profile`     系统环境变量

`bash_profile`     用户环境变量

`.bashrc`              用户环境变量

**`su user`              切换用户，加载配置文件.bashrc**

**`su - user`            切换用户，加载配置文件/etc/profile ，加载bash_profile**

#### 更改文件的用户及用户组

`sudo chown [-R] owner[:group] {File|Directory}`

例如：还以jdk-7u21-linux-i586.tar.gz为例。属于用户hadoop，组hadoop

要想切换此文件所属的用户及组。可以使用命令。

`sudo chown root:root jdk-7u21-linux-i586.tar.gz`

 

### 9) 文件权限管理

#### 三种基本权限

`R`           读         数值表示为4

`W`          写         数值表示为2

`X`           可执行  数值表示为1



如图所示，jdk-7u21-linux-i586.tar.gz文件的权限为-rw-rw-r--

-rw-rw-r--一共十个字符，分成四段。

第一个字符“-”表示普通文件；这个位置还可能会出现“l”链接；“d”表示目录

第二三四个字符“rw-”表示当前所属用户的权限。   所以用数值表示为4+2=6

第五六七个字符“rw-”表示当前所属组的权限。      所以用数值表示为4+2=6

第八九十个字符“r--”表示其他用户权限。              所以用数值表示为2

所以操作此文件的权限用数值表示为662 

#### 更改权限

`sudo chmod [u所属用户  g所属组  o其他用户  a所有用户]  [+增加权限  -减少权限]  [r  w  x]   目录名` 

例如：有一个文件filename，权限为“-rw-r----x” ,将权限值改为"-rwxrw-r-x"，用数值表示为765

`sudo chmod u+x g+w o+r  filename`

上面的例子可以用数值表示

`sudo chmod 765 filename`

 

