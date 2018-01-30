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















































