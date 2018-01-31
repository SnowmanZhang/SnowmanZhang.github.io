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

