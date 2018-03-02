---
title: Linux-速查笔记
date: 2018-01-30 19:44:37
tags:
- linux
- 速查
categories:
- 编程
---

一份属于自己的快速查参笔记，这里参考的是Linuxcast的课程。

<!--more-->

## 日期时间

### date    查看操作系统时间

`-u` 查看UTC时间，默认是CST时间
`+%Y--%m--%d`   修正时间的表示方法，其中`-`符号可以随意替换，其余不可更改
`-s "20:20:20"` 修改24小时时间

### hwclock(clock)     查看硬件时钟时间

### cal 查看日历

### uptime 查看当前系统的运行时间

当前计算机已经启动了多长时间，多少个用户，平均负载是多少。

## 输出查看命令

### echo 显示输入的内容

### cat 用以显示文件内容

`cat somefile`  会显示文档的所有内容

以下命令语法均为 `cmd somefile`

### head 用以显示文件的头几行(默认10行)

`-n number` 指定显示的行数

### tail 用以显示文件的末尾几行(默认10行)

`-n number`指定显示的行数
`-f`追踪显示文件更新（一般用于查看日志，命令不会退出，而是持续显示新加入的内容）

-f (follow)，一旦使用该命令，则不会退出，而是持续显示文件内容。

### more 用于翻页显示文件内容(只能向下翻页)

### less 用于翻页显示文件内容(带上下翻页)

## 查看硬件信息

### lspci用以查看PCI类型的设备，如声卡网卡等等

输出的内容每一行是一个硬件信息

`-v`查看详细信息

### lsusb用以查看USB设备

`-v`查看详细信息

### lsmod用以查看加载的模块(驱动)

## 关机与重启

### shutdown

`-h` 关机
`-r` 重启


```
shutdown -h now
shutdown -h +10  十分钟后关机
shutdown -h 23:40
shutdown -r now  立即重启

poweroff 立即关机
reboot 立即重启计算机

```

## 归档与压缩

### zip 用以压缩文件

zip somefile.zip myfile

### unzip 解压缩文件

unzip somefile

### gzip 压缩文件

gzip somefile

生成文件的后缀为.gz

### tar 用以归档文件，打包命令

归档非压缩

`tar -cvf out.tar somefile`  将somefile文件归档至out.tar文件
`tar -xvf somefile.tar` 将somefile.tar 释放出来
`tar -cvzf backup.tar.gz somefile` 将somefile文件归档至backup.tar，再压缩为backup.tar.gz

## 查找

### locate 用以快速查找文件，文件夹

locate keyword 查找整个计算机内含有`keyword`关键字的文件or文件夹

`updatedb` 用以更新索引数据库

### find 用以高级查找文件与文件夹

`find 查找位置 查找参数`

```
find . -name *someword* 在当前目录下以文件名进行查找，查找 含有someword这个字样的文件名的文件。
find / -name *.conf  查找在根目录下以.conf结尾的文件
find / -perm 777    在根目录下查找所有权限为 777 的文件或文件夹
find / -type d      在根目录下查找所有文件类型为d的文件(d表示目录，l表示链接)
find . -name "a*" -exec ls -l {} \;   在当前目录下查找以a开头的文件，且把所有找出来的结果当成参数传给ls -l命令。其余都是固定格式。即将查找到的文件用ls -l再查看一遍

```

find的其它查找方式

- name
- perm 权限
- user  属于某个特定用户的文件
- group
- ctime 基于修改时间查找文件
- type
- size  基于大小查找文件

# vim 编辑器

vi 是一个命令行界面下的文本编辑工具，1976年开发，1991年基于vi发布了vim，加入了对GUI的支持。

vim被广泛地作为在文本编辑、文本处理、代码开发等用途。

Linux中知名的文本编辑器还有emacs，功能更加强大。

## vim的打开

`vim` 进入vim文本编辑器

使用vim path/file 来打开文件进行编辑，如果文件存在则打开，不存在则新建。

## vim 的模式

### 命令模式(常规模式)

- 任何模式都可以通过`esc`回到命令模式。
- 命令模式执行选择、复制、粘贴、撤销等操作

命令参数

- u 撤销上一个操作
- i 在光标前插入文本
- o 在当前行的下面插入一个新行
- dd 删除整行
- yy 将当前行的内容放入缓冲区，即复制该行
- n+yy 将n行的内容放入缓冲区(从光标所在行开始计数,+不用写)
- P 粘贴
- r 替换当前字符(先按r键再按需要替换的字符)
- / 查找关键字
    - 在此基础上按n进行光标跳转到匹配到的关键字

### 插入模式

在命令模式中按`i`或者`a`键就可进入，直接进行文本编辑。

### ex模式

在命令模式中按`:`进入ex模式，光标会移动到底部，在这里可以保存修改或退出vim。

- w 保存当前更改
- q 退出
- q! 强制退出不保存
- x 保存并退出，相当于wq
- set number 显示行号
- ! 执行一个系统命令并显示结果
- sh 切换到命令行，使用ctrl+d切换回vim

## 磁盘

所有硬件都会在Linux中使用文件的形式抽象化，分在/dev文件夹下，其名称有如下规律

IDE设备的名称为`hd[a-z]` SATA、SCSI、SAS、USB等设备名称为`sd[a-z]`

分区后的名称为`hda[123]`

### MBR分区

MBR(Master Boot Record主引导记录)是传统的分区机制，应用于绝大多数使用BIOS的PC设备。事实上，苹果电脑使用EFI引导模式，其它PC机使用BIOS引导，因此大多数计算机使用MBR分区机制。

- 支持32bit和64bit系统
- 支持分区数量有限
- 只支持不超过2T的硬盘，超过2T的硬盘只能使用2T空间。

#### 主分区、扩展分区、逻辑分区

1. 一块硬盘最多只能创建4个主分区，因为MBR结构中只有4个分区表
2. 一个扩展分区会占用一个主分区位置
3. 逻辑分区是使用扩展分区创建出来的，Linux最多支持63个IDE分区和15个SCSI分区。

### GPT分区

GPT是一个较新的分区机制

- 支持超过2T的磁盘(ZB级)
- 向后兼容MBR
- 必须在支持UEFI的硬件上才能使用
- 必须使用64bit系统
- Mac 和 Linux都能支持GPT分区格式
- Windows7 64bit、Windowsserver2008 64bit支持GPT

### fdisk磁盘管理

fdisk来自IBM的老牌分区工具，fdisk基于MBR分区。支持绝大多数操作系统

`常识备注`

- fdisk只能在root账户下使用
- fdisk -l 列出所有安装的磁盘及其分区信息
- fdisk /dev/sda 可以对目标磁盘进行分区操作
- 分区之后需要使用partprobe命令让内核更新分区信息，否则需要重启才能识别新的分区
- /proc/partitions文件也可用来查看分区信息




```linux

[root@VM_0_14_redhat dev]# fdisk -l

Disk /dev/vda: 53.7 GB, 53687091200 bytes, 104857600 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000c7a75

   Device Boot      Start         End      Blocks   Id  System
/dev/vda1   *        2048   104857599    52427776   83  Linux

//id 分区类型



```

`fdisk /dev/vda1` 对该硬盘进行分区

#### fdisk参数

- n 创建新分区
    - e 创建扩展分区
        - 输入分区号(1-4)
        - 如果last cylinder是缺省，则默认为一直到最后，就像first cylinder缺省是默认最开始
        - 在扩展分区上创建逻辑分区
            - 再次输入n后，如果硬盘内 存在扩展分区，则出现l表示创建逻辑分区(在该扩展分区的基础上)
            - 其余操作均和主分区方法一样
    - p 创建主分区
        - 输入分区号(1-4)
        - `+2G` 写在last cylinder上，即最后柱面在哪里
- p 打印出来当然分区表
- m 帮助文档
- t 修改分区id
    - 输入号码表示修改相应的分区id，
    - 再输入相应的要修改的hex code(type L to list codes)
- w 将分区表写入磁盘并退出(即将准备好的分区表确认，分区操作结束)

#### 验证

`cat /proc/partitions`

里面会列出当前分区信息

> 需要注意的是，我们需要在新分出的区内创建文件系统才可以开始使用。请移步下一节

## 文件管理系统

>操作系统通过文件系统管理文件及数据，磁盘或分区需要创建文件系统之后才能够为操作系统所用

常见文件系统

- fat32 WINDOWS
- NTFS  WINDOWS
- ext2\3\4  LINUX
- xfs   以性能和可扩展性著称
- HFS   

文件系统之间的区别：日志、支持的分区大小、支持的单个文件大小、性能等等。

### MKE2FS 命令

MKE2FS创建文件系统

`mke2fs -t ext4 fileurl` 对某个分区创建ext4文件系统

常用参数：

- b blocksize 指定文件系统块大小(每次读写文件的最小单位，如4096字节(4k))
- c          建立文件系统时检查坏损块
- L label    指定卷标
- j          建立文件系统日志(ext3,4是默认带日志的)

### MKFS 命令

MKFS 用于创建文件系统，相较于mke2fs简单，但是支持的参数少，不能精细化控制

`mkfs.ext3 /dev/sda3`

### DUMPE2FS 命令

dumpe2fs用来查看分区的文件系统信息

`dumpe2fs /dev/sda2`

### journal 文件系统日志

带日志的文件系统拥有较强的稳定性，在出现错误时可以进行修复。

带日志的文件系统在进行磁盘操作时，文件系统进行以下操作：

1. 文件系统将准备执行的事务的具体内容写入日志
2. 文件系统进行操作
3. 操作成功后，将事务的具体内容从日志删除

这样的好处是，如果有意外，可以通过查询日志进行恢复操作。

### E2LABEL

该命令可以为文件系统添加标签

- `e2label /dev/sdb1`  返回显示设备标签
- `e2label /dev/sdb1 labelname` 设置标签，建议大写

### FSCK

该命令用来检查并修复损坏的文件系统，检查文件系统时，分区必须是 **卸载**的

`fsck /dev/sda2`

参数参考

- y     不提示而直接进行修复
- 默认fsck 会自动判断文件系统类型，如果文件系统损坏较为严重，请使用-t参数指定文件系统类型
- 对于识别为文件的损坏数据(文件系统无记录)，fsck会将该文件放入lost+found目录
- 系统启动时会对磁盘进行fsck操作


### 挂在、使用文件系统

>挂载： 磁盘或分区创建好文件系统后，需要挂载到一个目录才能够使用。

windows和Mac是自动挂载，linux需要手动挂载到/mnt下，

#### mount

在Linux中，我们通过mount命令将格式化好的磁盘或分区挂载到一个目录上。

`mount /dev/sda3(要挂载的分区) /mnt(挂载点)`

常用参数

- 不带参数的mount命令会显示所有已挂载的文件系统
- t     指定文件系统的类型
- o     指定挂载选项
    - ro,rw   以只读或读写形式挂载，默认是rw
    - sync      代表不使用缓存，而是对所有操作直接写入磁盘
    - async     代表使用缓存，默认是async
    - noatime   代表每次访问文件时，不更新文件的访问时间
    - remount   重新挂载
    - atime     代表每次访问文件时更新文件的访问时间(默认选项)

>备注：当-o后需要跟多个参数时，用逗号分隔

#### umount

顾名思义，是取消挂载

`umount /dev/sda3 `
或者`umount /mnt(挂载点路径)`


>fuser -m /mnt 查看使用该文件系统的进程(后跟分区名亦可)

> lsof /mnt     查看哪些文件正在被使用

如果当前bash在目标文件系统内，则无法卸载，因为bash同样是一个进程。

#### 自动挂载

配置文件 /etc/fstab 用来定义需要自动挂载的文件系统，fstab中每一行代表一个挂载配置，格式如下：

>小贴士：/etc 一般用于放置系统配置文件

|/dev/sda3|/mnt|ext4|defaults|0 0|
|:---:|||||
|需要挂载的设备|挂载点|文件系统|挂载选项|dump、fsck相关选项|

> 每一列以tab键分隔


挂载的设备同样可以以label代替，同样识别有效
写法为 `LABEL=labelname`

## 获取帮助

>你没必要记住所有东西

>几乎所有的命令都可以使用 -h 或 --help参数获取使用方法、参数信息等

### man 更详细的使用方式

man(manual) 是Linux最常用的参数命令。

man手册有如下类型，可以加入参数来查阅相应的文档。如`man number cmd`

1. 用户命令
2. 内核系统调用
3. 库函数
4. 特殊文件和设备
5. 文件格式和规范
6. 游戏
7. 规范、标准和其它页面
8. 系统管理命令
9. Linux内核API

`man -k keyword` 查看包含keyword的文档有哪些

### info 更更详细的手册

语法与man相同

man 与 info 都可以通过 "/+关键字"来进行文档搜索，会对标中文字进行反向高亮，按空格翻页


### doc 最详细的手册

Linux的说明文档保存在 /usr/share/doc 目录中，这些文档是相应程序最为详尽的文档。

## Linux用户

常识备注：

1. 每个用户拥有一个userID，操作系统实际使用的是用户ID，而非用户名
2. 每个用户属于一个主组，属于一个或多个附属组
3. 每个组拥有一个groupID
4. 每个进程以一个用户身份运行，并受该用户可访问的资源限制
5. 每个可登陆用户拥有一个指定的shell

### 用户

用户ID为32位，从0开始，但为了兼容性，限制在60000以下。
用户分为以下三种

- root用户(ID为0的用户为root用户)
- 系统用户(1~499)(没有shell)
    - 每一个进程以一个用户的身份进行，系统用户为一个进程而创建并存在，只为这个进程而使用，故不需要shell
- 普通用户(500以上)

使用id 命令可以显示当前用户的信息，也可以`id username`
使用passwd命令可以修改当前用户密码

#### 相关文件

- /etc/passwd 保存用户信息
- /etc/shadow 保存用户密码(加密)
- /etc/group  保存组信息

passwd内的信息(顺序如下)，每个部分用冒号隔开

- 用户名
- 密码(x)
- id号
- 组id
- 用户描述信息
- 用户家目录
- 用户shell
    - 系统用户是 /sbin/nologin
    - 普通用户是 /bin/bash

shadow 内的文件

若两个感叹号，则说明是没有设置密码

#### 查看登陆的用户

>一个约定俗成的习惯：命令越长，显示的信息越短

whoami 显示当前用户
who     显示有哪些用户在登陆
w       显示所有已登陆用户正在干什么

#### 创建用户

`useradd username`

该命令执行以下操作

- 在/etc/passwd 中添加用户信息
- 如果使用passwd命令创建密码，则将密码加密保存在/etc/shadow中
- 为用户建立一个新的家目录，/home/username
- 将/etc/skel中的文件复制到用户家目录中，这是一些初始文件
- 建立一个与用户用户名相同的组，新建用户默认属于改组

`passwd username` 创建密码

useradd的参数

- d 指定家目录
- s 指定登陆shell
- u 指定userid
- g 主族
- G 附属组(最多31个)

也可以直接修改/etc/passwd实现，但不建议

#### 修改用户信息

`usermod 参数 username`

- I 新的用户名
- u 新的userid
- d 用户家目录位置
- g 用户所属主组
- G 用户所属附属组
- L 锁定用户使其不能登陆
- U 接触锁定

#### 删除用户

`userdel username` 保留用户家目录
`userdel -r username`同时删除用户家目录

### 组

组用于更加方便地管理用户，我们使用部门、职能或地理区域的分类方式来创建使用组

- 每个组有一个组ID
- 组信息保存在/etc/group中
- 每个用户拥有一个主组，同时拥有最多31个附属组

#### 创建，修改，删除组

`groupadd groupname` 创建组

`groupmod -n newname oldname` 修改组名

`groupmod -g newGid oldGid` 修改组id

`groupdel groupname` 删除组

## 权限

### 权限简介

Linux中，每个文件拥有三种权限

- r     读取      可读取文件内容     可列出目录内容
- w     写入      可以修改文件内容    可在目录中创建删除文件
- x     执行      可以作为命令执行    可访问目录内容

对于目录而言，单有r权限是没有意义的，必需有x

Linux权限基于UGO模式进行控制

U - user
G - group
O - other

命令 `ls -l`可以查看当前目录下文件的详细信息


```
ls -l 所列示的全部信息(自左向右)

文件类型    U权限     G权限     O权限     链接数量    U:所属用户  G:所属组   大小  时间      文件名

```

### 修改文件所属用户、组

**chown** 用以改变文件的所属用户：

`chown username filename/dirname`

-R 参数递归的修改目录下的所有文件的所属用户

**chgrp** 用以改变文件的所属组

`chgrp username filename/dirname`

-R 参数递归的修改目录下的所有文件的所属组

### chmod 修改文件权限

**第一种方式**

`chmod 模式 文件`

模式如下格式

- u、g、o分别代表用户、组和其它
- a可以代指ugo
- 加减号代表加入或删除对应权限
- rwx代表三种权限

> eg: 
> chmod u+rw filename
> chmod go+r filename
> chmod a-x filename

-R 递归对目录内文件产生效果

**第二种方式**

命令chmod支持以数字方式修改权限，三个权限分别由三个数字表示,421分别代表rwx。

使用数字表示权限时，每组权限分别为对应数字之和：

- rw = 6
- rwx = 7
- r-x = 5

`chmod 660 filename`

### 默认权限

用户创建新建文件与新建文件夹的权限称为默认权限，默认权限的计算方式为

新建目录  777 - umask
新建文件  666 - umask
一般，普通用户的默认umask 是002 ，root用户的默认umask是022

也就是说，对于普通用户来讲，新建文件的权限是664，新建目录的权限是775

>命令umask用以查看设置umask值
> umask 022


### 特殊权限

- suid  以文件的所属用户身份执行，而非执行文件的用户  该权限用于执行文件,所属用户的x位被改为了s
	- 以passwd命令为例，passwd命令用于将输入信息，写入shadow文件中，普通用户使用passwd命令，则自动使用root身份执行命令。
- sgid	对文件：以文件所属组身份执行，对文件夹：若对某个目录加入sgid权限，则在该目录以内的所有新创建文件将自动继承目录的所属组
	- 常规来说，当创建了一个文件夹，并赋予了一个所属组A，但在该目录内新建文件，则其所属组并未能继承所属组A
	- 使用sgid则产生了继承效果
	- sgid会显示在所属组的X位上(可执行位)
- sticky 

#### 设置特殊权限位

设置suid:	chmod u+s filename
设置sgid:	chmod g+s filename
设置sticky:	chmod o+t filename

与普通权限一样，也可以使用数字方式表示

- SUID = 4
- SGID = 2
- Sticky = 1

有 `	chmod 4755 filename`  4即为追加数字	

> 仍需详细理解！

## 网络基础

> 以太网接口被命令为 eth0、eth1...

> 通过lspci可以查看网卡硬件信息

### ifconfig

- ifconfig -a 查看所有接口
- ifconfig eth0 查看特定接口

### ifup|ifdown 接口号   启用禁用接口

### setup 配置命令

setup -> network configuartion


### 说明

网络相关配置文件

- 网卡配置文件	`/etc/sysconfig/network-scripts/ifcfg-eth0`
	- 该文件可以直接修改相关内容，而不用使用setup命令
- DNS配置文件	`/etc/resolv.conf`
- 主机名配置文件	`/etc/sysconfig/network`
- 静态主机名配置文件 `/etc/hosts`


### ping

测试网络通信

- `ping 193.234.23.13`
- `ping snowman.cn`

测试DNS 

- `host snowman.cn`
- `dig www.baidu.com`

显示路由表的相关信息

`ip route`


追踪到达目标地址的网络路径

`traceroute www.baidu.com`

使用mtr进行网络质量测试(结合了traceroute 和ping)(my trace route)

`mtr www.baidu.com`

### hostname 查看与修改主机名(实时)

- `hostname` 查看主机名
- `hostname newdoaminname` 修改新的主机名

### 永久性修改主机名

/etc/sysconfig/network

HOSTNAME = newdoaminname

> 问题：一个Linux系统内只能使用一个主机名吗？如果需要装载两个主机如何做。

> 经验：事实上，Linux的行内   `[root@training ~]`中，root是用户名，training是主机名，~是目录，而对于`git@187.129.1.1`而言，后者是主机名，git是用户。对于邮箱而言亦是。


## 管道与重定向

命令行shell数据流有以下定义

- STDIN 标准输入
- STDOUT 标准输出
- STDERR 标准错误

命令通过STDIN接收参数或数据，通过STDOUT输出结果或通过STDERR输出错误。

### 重定向

- > 将标准输出重定向到文件(覆盖)
- >> 将标准输出重定向到文件(追加)
- 2> 将标准错误重定向到文件(覆盖)
- 2>&1 将标准输出和标准错误相结合
- < 重定向STDIN

### 管道

`|` 将一个命令的STDOUT作为下一个命令的STDIN，执行之


## Linux文本处理工具

- 文件浏览
	- cat 	查看文件内容
	- more 	以翻页形式查看文件内容(只能向下翻页)
	- less	以翻页形式查看文件内容(可上下翻页)
	- head	查看文件的开始10行(或指定行数)
	- tail 	查看文件的结束10行(或指定行数)
- 基于关键字搜索(grep命令用以基于关键字搜索文本)
	- e.g.	grep 'linuxcast' /etc/passwd
	- e.g. 	find / -user linuxcast|grep Video  将根目录下所有用户为linuxcast的目录传输到grep命令下搜索video词
	- i 	在搜索的时候忽略大小写
	- n 	显示结果所在行数
	- v 	输出不带关键字的行
	- Ax 	在输出的时候包含结果所在行之后的指定行数(x就是数字)
	- Bx 	在输出的时候包含结果所在行之前的指定行数(x就是数字)
- 基于列处理文本(cut命令用以基于列处理文本内容)
	- e.g. 	cut -d: -f1 /etc/passwd
	- e.g. 	grep linuxcast /etc/passwd|cut -d: -f3 	grep搜索linuxcast的账户信息，cut切割出其第三列，即uid部分输出出来
	- d 	指定分割字符(默认是TAB)
	- f 	指定输出的列号
	- c 	基于字符进行切割(e.g. cut -c2-6 /etc/passwd 只显示第2到第6个字符)
- 文本统计(wc命令用以统计文本信息)
	- e.g. 	wc filename  对文件进行统计 
	- l 	只统计行数
	- w 	只统计单词数
	- c 	只统计字节数
	- m 	只统计字符数
- 文本排序(sort命令用以对文本内容进行排序)
	- e.g. 	sort filename 对文件进行排序(按每行首字母进行排序)
	- r 	倒序排序
	- n 	基于数字进行排序
	- f 	忽略大小写
	- u 	删除重复行(uniq命令同样可以删除重复的相邻行)
	- t c 	使用c作为分隔符分割为列进行排序
	- k x 	当分割为列进行排序时，指定基于哪个列进行排序
- 文本比较(diff命令用于比较两个文件的区别)
	- e.g. 	diff filename1 filename2
	- i 	忽略大小写
	- b 	忽略空格数量的改变
	- u 	统一显示比较信息(一般用以生成patch文件)(e.g. diff -u filename1 filenam2 > final.patch)
- 检查拼写
	- e.g. 	aspell check filename
	- e.g. 	aspell list < filename
- 处理文本内容(tr命令用以进行转换类型的功能)
	- e.g. 	tr -d 'apple' < linuxcast 	删除关键字
	- e.g. 	tr 'a-z''A-Z' < linuxcast  	转换大小写
- 搜索替换(sed命令用以搜索并替换文本)
	- e.g. 	sed 's/linux/unix/g' filename 	将linux词换为unix，g表示全部替换
	- e.g. 	sef '1,50s/linux/unix/g' filename 	1到50行进行替换


## 更加底层：Linux系统启动

### 系统启动流程

- BIOS
- MBR：Boot Code
- 执行引导程序 GRUB
- 加载内核
- 执行init
- runlevel

### BIOS

BIOS(basic input output system) 我们称之为基本输入输出系统，一般保存在主板上的BIOS芯片中。

BIOS负责检查硬件并且查找可启动设备

可启动设备在BIOS设置中进行定义，如USB，CDROM，HD

### MBR

BIOS找到可启动设备后执行其引导代码

引导代码为硬盘第一个扇区的前446字节，MBR其第一个扇区的最后部分一定是55aa

### GRUB

Grub是现在Linux使用的主流引导程序，并且可以用来引导现在几乎所有的操作系统

Grub的相关文件保存在/boot/grub目录中

MBR因为字节太小，往往执行的内容是寻路到一个更复杂的引导程序，即GRUB

流程

- stage1 (MBR)
- stage1_5 (驱动、文件系统)
- stage2
- 加载内核

### kernel

- MBR的引导代码用于找到并加载Linux内核
- Linux内核保存在/boot/vmlinuz-2.6.32-279.el6.i686
- 一般还会加载内核模块打包文件 /boot/initramfs-2.6.32-279.el6.i686.img
- linux将一些不常用的驱动与功能编译成模块，在需要的时候动态加载，而这些模块被打包保存为一个initramfs文件

### init

init 是Linux系统中运行的第一个进程

调用/etc/rc.d/rc.sysinit 负责对系统进行初始化、挂载文件系统，并且根据运行级别启动相应服务。


Linux运行级别

- 0 关机
- 1 单用户模式
- 2 不带网络的多用户模式
- 3 多用户模式
- 4 未使用
- 5 XII图形化模式
- 6 重新启动

### 单用户修改root密码

为内核传递参数1或者single可系统进入单用户模式

单用户模式下不启动任何服务

单用户模式直接以root用户登陆，并且不需要密码

再使用passwd修改密码

### rpm包管理

rpm(redhat package manager)

rpm通过将源代码基于特定平台系统编译为可执行文件，并保存依赖关系，来简化开源软件的安装管理。

设计目标如下

- 使用简单
- 使用单一软件包格式文件发布(.rpm)
- 可升级
- 追踪软件依赖关系
- 基本信息查询
- 软件验证功能
- 支持多平台

#### rpm命令规范

linuxcast-1.2.0-30.el6.i686.rpm

软件名+版本号分支号+平台型号+CPU型号.rpm

#### 基本命令

- 安装软件	rpm -i software.rpm
- 卸载软件	rpm -e software
- 升级形式安装	rpm -U software-new.rpm

rpm 支持通过http、ftp协议安装软件

e.g. 	rpm -ivh http://www.linuxcast.net/software.rpm

- v 	显示相关信息
- h 	显示进度条

#### rpm查询

- rpm -qa		列出所有已经安装的rpm软件
- rpm -qi SoftwareName	查询一个软件的基本信息
- rpm -ql SoftwareName 	查询某个软件有哪些目录
- rpm -qf filename 		查询某个文件是哪个软件安装进来的
- rpm -qlp software.rpm 查询一个rpm在安装以后会增加哪些文件目录，p代表未安装的意思
- rpm -V SoftwareName 	查询某个软件内的目录，是否存在不应该存在的变化
- rpm --import RPM-GPG-KEY-CentOS-6	导入密钥
- rpm -K SoftwareName.rpm 			验证rpm文件

### YUM机制

YUM(yellowdog updater,Modified)是一个RPM的前端程序，主要目的是设计用来自动解决RPM的依赖关系问题。其特点是

- 自动解决依赖关系
- 可以对RPM进行分组，并基于组进行安装操作
- 引入仓库概念，支持多个仓库
- 配置简单

YUM引入了仓库的概念，仓库用来存放所有现有的rpm软件包

仓库可以是本地的，也可以通过HTTP、FTP形式使用集中的，统一的网络仓库

#### YUM仓库

配置文件位于 `/etc/yum.repos.d/`目录下

格式如下

```
[LinuxCast]
name = ....
baseurl = ...
enable = 1
gpgcheck = 1 (1为启用，0为禁用)

```

- 仓库可以使用file、http、nfs方式
- yum配置文件必须以.repo结尾
- 一个配置文件内可以保存多个仓库的配置信息
- /etc/yum.repos.d/ 目录下可以存在多个配置文件

#### YUM基本命令

- yum install SoftwareName
- yum remove SoftwareName
- yum update SoftwareName

yum本质上是对rpm包的管理机制，因此安装也是安装rpm包

#### YUM 查询

- yum search keyword 	搜索
- yum list (all | installed | recent | updates)  列出所有仓库中已有的软件，已安装的软件，最近的，软件更新的
- yum info tigervnc		同rpm -qi tigervnc






