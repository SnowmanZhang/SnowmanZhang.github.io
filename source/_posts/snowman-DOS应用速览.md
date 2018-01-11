---
title: '[snowman]DOS应用速览'
date: 2017-12-28 15:47:01
tags:
- DOS
categories:
- 编程
---

>虽然Windows已经迭代出了许多优秀的桌面操作系统，但每一个人都知道，真正提高效率的方式永远是神秘又有趣的命令行。


-----


### cls (clear screen)

作用：     清屏

### dir (directory)

列出当前文件夹下的所有文件和文件夹，当然不包括隐藏的和系统文件。

#### 举例

在本机的用户文件夹`C:\Users\Machenike`下操作

1. `dir`

```
 驱动器 C 中的卷没有标签。
 卷的序列号是 A4DF-CE39

 C:\Users\Machenike 的目录

2018/01/09  11:05    <DIR>          .
2018/01/09  11:05    <DIR>          ..
2017/11/14  12:57    <DIR>          .anaconda
2018/01/09  11:05            11,168 .bash_history
2017/11/14  14:29    <DIR>          .conda
2017/11/14  12:57                43 .condarc
2017/09/06  12:13               126 .defaults-0.1.0.ini
2017/10/02  16:28               183 .gitconfig
2017/09/13  08:19    <DIR>          .idlerc
2017/11/14  13:00    <DIR>          .ipynb_checkpoints
2017/11/14  13:01    <DIR>          .ipython
2017/11/22  02:57    <DIR>          .jupyter
2018/01/09  13:07    <DIR>          .matplotlib
2017/10/18  21:15                58 .minttyrc
2017/10/31  18:31               106 .node_repl_history
2017/09/02  21:14    <DIR>          .oracle_jre_usage
2017/09/02  21:24    <DIR>          .PyCharmCE2017.2
2018/01/09  13:07    <DIR>          .spyder-py3
2017/10/14  13:12    <DIR>          .ssh
2017/09/25  15:35             9,488 .v8flags.5.1.281.107.cbfbe083657fef000920ceab40466420.json
2017/12/04  11:07             7,559 .viminfo
2018/01/05  14:53    <DIR>          Anaconda3
2018/01/05  23:16    <DIR>          Contacts
2018/01/06  11:10    <DIR>          Desktop
2018/01/05  23:16    <DIR>          Documents
2018/01/05  23:16    <DIR>          Downloads
2017/10/19  19:02                 0 exam.html
2018/01/05  23:16    <DIR>          Favorites
2017/10/28  14:27             8,351 huhuan.md
2017/05/04  11:29    <DIR>          Intel
2018/01/05  23:16    <DIR>          Links
2018/01/05  23:16    <DIR>          Music
2017/09/03  19:18         1,408,775 mygettext.html
2017/12/15  15:32    <DIR>          OneDrive
2018/01/05  23:16    <DIR>          Pictures
2017/09/05  19:33    <DIR>          PycharmProjects
2017/05/04  11:35    <DIR>          Roaming
2018/01/05  23:16    <DIR>          Saved Games
2018/01/05  23:16    <DIR>          Searches
2017/11/14  13:09             5,739 Untitled.ipynb
2017/11/14  12:58                 0 untitled.txt
2018/01/09  14:49    <DIR>          Videos
2017/09/19  20:08    <DIR>          新建文件夹
              13 个文件      1,451,596 字节
              30 个目录 27,367,956,480 可用字节
```

2. `dir /ad /od /ta /b`

- /ad 表示只显示目录，其中a表示属性，d表示dir，更多具体的参数详见参数参考一节
- /od 表示按日期顺序排列列表，o表示order，d表示datetime
- /ta 表示日期使用上次访问时间，t表示time，a表示access
- /b 表示使用空格式，也就是只显示文件名，其它的一概不现实

```
PrintHood
Application Data
NetHood
SendTo
My Documents
Recent
「开始」菜单
Cookies
Templates
AppData
Local Settings
Contacts
Music
Saved Games
Pictures
OneDrive
Intel
Roaming
Favorites
Searches
.oracle_jre_usage
.PyCharmCE2017.2
PycharmProjects
.idlerc
新建文件夹
.ssh
wc
Documents
.anaconda
.ipynb_checkpoints
.ipython
.conda
.jupyter
Links
Anaconda3
Downloads
Desktop
IntelGraphicsProfiles
.spyder-py3
.matplotlib
Videos

```

3. `dir E:/pythonabc /-c /q`

- /-c 表示去除掉文件大小中的逗号，因给字节加上千分位符号为默认设置，故-c以关闭，事实上所有的参数都可以用-表示反向开关
- /q 表示文件所有者

```
2018/01/04  15:14    <DIR>          DESKTOP-BRRT43V\Macheni.
2018/01/04  15:14    <DIR>          NT AUTHORITY\SYSTEM    ..
2017/08/30  13:19    <DIR>          DESKTOP-BRRT43V\Macheni.idea
2017/11/28  14:24    <DIR>          DESKTOP-BRRT43V\Machenibiotree
2017/12/05  21:33    <DIR>          DESKTOP-BRRT43V\Machenichuangyebang
2017/11/12  20:58              4531 DESKTOP-BRRT43V\Machenichuangyebang.zip
2017/12/26  18:05    <DIR>          DESKTOP-BRRT43V\Machenicompany1109
2017/11/09  21:48           3427352 DESKTOP-BRRT43V\Machenicorp.csv
2017/11/01  21:09    <DIR>          DESKTOP-BRRT43V\Machenieluosifangkuai
2017/08/30  18:41    <DIR>          DESKTOP-BRRT43V\MacheniEXERCISE
2017/08/30  13:40    <DIR>          DESKTOP-BRRT43V\Machenijiebapro
2017/11/08  09:57    <DIR>          DESKTOP-BRRT43V\Machenilibrary
2018/01/04  15:13    <DIR>          DESKTOP-BRRT43V\Machenilistdir_exe
2018/01/04  15:14          18674398 DESKTOP-BRRT43V\Machenilistdir_exe.rar
2017/11/04  00:09    <DIR>          DESKTOP-BRRT43V\Machenimachinelearning
2017/10/23  09:13          39355559 DESKTOP-BRRT43V\MacheniML.pdf
2017/08/31  15:56    <DIR>          DESKTOP-BRRT43V\Macheninlp
2018/01/09  13:52    <DIR>          DESKTOP-BRRT43V\MacheniPPPpro
2017/09/08  20:57    <DIR>          DESKTOP-BRRT43V\Machenipynlpir
2017/10/31  16:51    <DIR>          DESKTOP-BRRT43V\Machenipython_standard
2017/04/19  10:31          29809397 DESKTOP-BRRT43V\MacheniPython核心编程（第3版）.epub
2017/08/30  11:45           4255871 DESKTOP-BRRT43V\MacheniPYTHON自然语言处理_中文版.pdf
2017/07/02  18:33          49789379 DESKTOP-BRRT43V\MacheniPython金融大数据分析.pdf
2017/11/08  10:14    <DIR>          DESKTOP-BRRT43V\Machenipy_mysql
2017/11/08  10:00    <DIR>          DESKTOP-BRRT43V\Machenireference
2017/12/02  19:14    <DIR>          DESKTOP-BRRT43V\Machenissecom
2017/11/12  20:32    <DIR>          DESKTOP-BRRT43V\Machenitemp
2017/12/29  11:15    <DIR>          DESKTOP-BRRT43V\Machenizhengxuan1229
2017/09/24  10:56           8709214 DESKTOP-BRRT43V\Macheni《Python标准库》-迷你书.pdf
2018/01/05  01:25    <DIR>          DESKTOP-BRRT43V\Macheni细碎程序
               8 个文件      154025701 字节
              22 个目录   209876656128 可用字节
```

#### 参数一览表

```
e:\>dir /?
显示目录中的文件和子目录列表。

DIR [drive:][path][filename] [/A[[:]attributes]] [/B] [/C] [/D] [/L] [/N]
  [/O[[:]sortorder]] [/P] [/Q] [/R] [/S] [/T[[:]timefield]] [/W] [/X] [/4]

  [drive:][path][filename]
              指定要列出的驱动器、目录和/或文件。

  /A          显示具有指定属性的文件。
  属性         D  目录                R  只读文件
               H  隐藏文件            A  准备存档的文件
               S  系统文件            I  无内容索引文件
               L  解析点             -  表示“否”的前缀
  /B          使用空格式(没有标题信息或摘要)。
  /C          在文件大小中显示千位数分隔符。这是默认值。用 /-C 来
              禁用分隔符显示。
  /D          跟宽式相同，但文件是按栏分类列出的。
  /L          用小写。
  /N          新的长列表格式，其中文件名在最右边。
  /O          用分类顺序列出文件。
  排列顺序     N  按名称(字母顺序)     S  按大小(从小到大)
               E  按扩展名(字母顺序)   D  按日期/时间(从先到后)
               G  组目录优先           -  反转顺序的前缀
  /P          在每个信息屏幕后暂停。
  /Q          显示文件所有者。
  /R          显示文件的备用数据流。
  /S          显示指定目录和所有子目录中的文件。
  /T          控制显示或用来分类的时间字符域。
  时间段      C  创建时间
              A  上次访问时间
              W  上次写入的时间
  /W          用宽列表格式。
  /X          显示为非 8.3 文件名产生的短名称。格式是 /N 的格式，
              短名称插在长名称前面。如果没有短名称，在其位置则
              显示空白。
  /4          用四位数字显示年

可以在 DIRCMD 环境变量中预先设定开关。通过添加前缀 - (破折号)
来替代预先设定的开关。例如，/-W。

```

### cd (change directory)

#### 举例

1. cd ./.ssh

./ 表示当前目录，在前面dir命令中也可以看到，每一个地址下都有两个目录，一个是`.`，一个是`..`，前者表示当前目录本身，后者表示上一级目录地址，该命令将跳转至当前目录的子目录`.ssh`中。

2. cd ../..

该命令将跳至父目录的父目录(如果有的话)，如果已到驱动器根目录上，如`C:\>`，则会不变

3. cd /d f:/dirname

该命令可以跨驱动器跳转，如从C盘跳至F盘中的某目录，/d即表示启动跨驱动器开关

#### 参数一览表

```
e:\>cd /?
显示当前目录名或改变当前目录。

CHDIR [/D] [drive:][path]
CHDIR [..]
CD [/D] [drive:][path]
CD [..]

  ..   指定要改成父目录。

键入 CD drive: 显示指定驱动器中的当前目录。
不带参数只键入 CD，则显示当前驱动器和目录。

使用 /D 开关，除了改变驱动器的当前目录之外，
还可改变当前驱动器。

如果命令扩展被启用，CHDIR 会如下改变:

当前的目录字符串会被转换成使用磁盘名上的大小写。所以，
如果磁盘上的大小写如此，CD C:\TEMP 会将当前目录设为
C:\Temp。

CHDIR 命令不把空格当作分隔符，因此有可能将目录名改为一个
带有空格但不带有引号的子目录名。例如:

     cd \winnt\profiles\username\programs\start menu

与下列相同:

     cd "\winnt\profiles\username\programs\start menu"

在扩展停用的情况下，你必须键入以上命令。
```

### move 

#### 举例

#### 参数一览表

```
d:\>move /?
移动文件并重命名文件和目录。

要移动至少一个文件:
MOVE [/Y | /-Y] [drive:][path]filename1[,...] destination

要重命名一个目录:
MOVE [/Y | /-Y] [drive:][path]dirname1 dirname2

  [drive:][path]filename1 指定你想移动的文件位置和名称。
  destination             指定文件的新位置。目标可包含一个驱动器号
                          和冒号、一个目录名或组合。如果只移动一个文件
                          并在移动时将其重命名，你还可以包括文件名。
  [drive:][path]dirname1  指定要重命名的目录。
  dirname2                指定目录的新名称。

  /Y                      取消确认覆盖一个现有目标文件的提示。
  /-Y                     对确认覆盖一个现有目标文件发出提示。

命令行开关 /Y 可以出现在 COPYCMD 环境变量中。这可以用命令行上
的 /-Y 替代。默认值是-Y，除非 MOVE 命令是从一个批脚本内
执行的，覆盖时都发出提示。

```

### copy

```
d:\>copy /?
将一份或多份文件复制到另一个位置。

COPY [/D] [/V] [/N] [/Y | /-Y] [/Z] [/L] [/A | /B ] source [/A | /B]
     [+ source [/A | /B] [+ ...]] [destination [/A | /B]]

  source       指定要复制的文件。
  /A           表示一个 ASCII 文本文件。
  /B           表示一个二进位文件。
  /D           允许解密要创建的目标文件
  destination  为新文件指定目录和/或文件名。
  /V           验证新文件写入是否正确。
  /N           复制带有非 8dot3 名称的文件时，
               尽可能使用短文件名。
  /Y           不使用确认是否要覆盖现有目标文件
               的提示。
  /-Y          使用确认是否要覆盖现有目标文件
               的提示。
  /Z           用可重新启动模式复制已联网的文件。
  /L           如果源是符号链接，请将链接复制
               到目标而不是源链接指向的实际文件。

命令行开关 /Y 可以在 COPYCMD 环境变量中预先设定。
这可能会被命令行上的 /-Y 替代。除非 COPY
命令是在一个批处理脚本中执行的，默认值应为
在覆盖时进行提示。

要附加文件，请为目标指定一个文件，为源指定
数个文件(用通配符或 file1+file2+file3 格式)。
```

### del (delete)

```
d:\>del /?
删除一个或数个文件。

DEL [/P] [/F] [/S] [/Q] [/A[[:]attributes]] names
ERASE [/P] [/F] [/S] [/Q] [/A[[:]attributes]] names

  names         指定一个或多个文件或者目录列表。
                通配符可用来删除多个文件。
                如果指定了一个目录，该目录中的所
                有文件都会被删除。

  /P            删除每一个文件之前提示确认。
  /F            强制删除只读文件。
  /S            删除所有子目录中的指定的文件。
  /Q            安静模式。删除全局通配符时，不要求确认
  /A            根据属性选择要删除的文件
  属性          R  只读文件                     S  系统文件
                H  隐藏文件                     A  存档文件
                I  无内容索引文件               L  重分析点
                -  表示“否”的前缀

如果命令扩展被启用，DEL 和 ERASE 更改如下:

/S 开关的显示句法会颠倒，即只显示已经
删除的文件，而不显示找不到的文件。

```

### md\mkdir  (makedirectory)

需注意的是，mkdir的迭代生成是自动开启的，故若你在python中调用os进行文件夹创建时，若字符串中含有`/`或`\\`，则多层文件夹会自动生成不会报错。

```
d:\>md /?
创建目录。

MKDIR [drive:]path
MD [drive:]path

如果命令扩展被启用，MKDIR 会如下改变:

如果需要，MKDIR 会在路径中创建中级目录。例如: 假设 \a 不
存在，那么:

    mkdir \a\b\c\d

与:

    mkdir \a
    chdir \a
    mkdir b
    chdir b
    mkdir c
    chdir c
    mkdir d

相同。如果扩展被停用，则需要键入 mkdir \a\b\c\d。

```

### rd/rmdir (removedirectory)

#### 举例

`rmdir E:/pythonabc /s /q`

代表将E盘下名为`pythonabc`的文件夹及其所有内容均删除

/s 表示删除文件夹下的所有内容
/q 表示在/s开启后，不对删除每个文件进行 \[y/n\]? 的确认

#### 参数一览表

```
d:\>rd /?
删除一个目录。

RMDIR [/S] [/Q] [drive:]path
RD [/S] [/Q] [drive:]path

    /S      除目录本身外，还将删除指定目录下的所有子目录和
            文件。用于删除目录树。

    /Q      安静模式，带 /S 删除目录树时不要求确认

```

### type

```
d:\>help type
显示文本文件的内容。

TYPE [drive:][path]filename
```


### fc


```
d:\>help fc
比较两个文件或两个文件集并显示它们之间
的不同


FC [/A] [/C] [/L] [/LBn] [/N] [/OFF[LINE]] [/T] [/U] [/W] [/nnnn]
   [drive1:][path1]filename1 [drive2:][path2]filename2
FC /B [drive1:][path1]filename1 [drive2:][path2]filename2

  /A         只显示每个不同处的第一行和最后一行。
  /B         执行二进制比较。
  /C         不分大小写。
  /L         将文件作为 ASCII 文字比较。
  /LBn       将连续不匹配的最大值设置为指定
             的行数。
  /N         在 ASCII 比较上显示行数。
  /OFF[LINE] 不要跳过带有脱机属性集的文件。
  /T         不要将制表符扩充到空格。
  /U         将文件作为 UNICODE 文本文件比较。
  /W         为了比较而压缩空白(制表符和空格)。
  /nnnn      指定不匹配处后必须连续
             匹配的行数。
  [drive1:][path1]filename1
             指定要比较的第一个文件或第一个文件集。
  [drive2:][path2]filename2
             指定要比较的第二个文件或第二个文件集。


```

### find

```
d:\>find /?
在文件中搜索字符串。

FIND [/V] [/C] [/N] [/I] [/OFF[LINE]] "string" [[drive:][path]filename[ ...]]

  /V         显示所有未包含指定字符串的行。
  /C         仅显示包含字符串的行数。
  /N         显示行号。
  /I         搜索字符串时忽略大小写。
  /OFF[LINE] 不要跳过具有脱机属性集的文件。
  "string" 指定要搜索的文本字符串。
  [drive:][path]filename
             指定要搜索的文件。

如果没有指定路径，FIND 将搜索在提示符处键入
的文本或者由另一命令产生的文本。


```

### findstr

```
d:\>findstr /?
在文件中寻找字符串。

FINDSTR [/B] [/E] [/L] [/R] [/S] [/I] [/X] [/V] [/N] [/M] [/O] [/P] [/F:file]
        [/C:string] [/G:file] [/D:dir list] [/A:color attributes] [/OFF[LINE]]
        strings [[drive:][path]filename[ ...]]

  /B         在一行的开始配对模式。
  /E         在一行的结尾配对模式。
  /L         按字使用搜索字符串。
  /R         将搜索字符串作为一般表达式使用。
  /S         在当前目录和所有子目录中搜索匹配文件。
  /I         指定搜索不分大小写。
  /X         打印完全匹配的行。
  /V         只打印不包含匹配的行。
  /N         在匹配的每行前打印行数。
  /M         如果文件含有匹配项，只打印其文件名。
  /O         在每个匹配行前打印字符偏移量。
  /P         忽略有不可打印字符的文件。
  /OFF[LINE] 不跳过带有脱机属性集的文件。
  /A:attr    指定有十六进位数字的颜色属性。请见 "color /?"
  /F:file    从指定文件读文件列表 (/ 代表控制台)。
  /C:string  使用指定字符串作为文字搜索字符串。
  /G:file    从指定的文件获得搜索字符串。 (/ 代表控制台)。
  /D:dir     查找以分号为分隔符的目录列表
  strings    要查找的文字。
  [drive:][path]filename
             指定要查找的文件。

除非参数有 /C 前缀，请使用空格隔开搜索字符串。
例如: 'FINDSTR "hello there" x.y' 在文件 x.y 中寻找 "hello" 或
"there"。'FINDSTR /C:"hello there" x.y' 文件 x.y  寻找
"hello there"。

一般表达式的快速参考:
  .        通配符: 任何字符
  *        重复: 以前字符或类出现零或零以上次数
  ^        行位置: 行的开始
  $        行位置: 行的终点
  [class]  字符类: 任何在字符集中的字符
  [^class] 补字符类: 任何不在字符集中的字符
  [x-y]    范围: 在指定范围内的任何字符
  \x       Escape: 元字符 x 的文字用法
  \<xyz    字位置: 字的开始
  xyz\>    字位置: 字的结束

有关 FINDSTR 常见表达法的详细情况，请见联机命令参考。
```

### color

```
d:\>color /?
设置默认的控制台前景和背景颜色。

COLOR [attr]

  attr        指定控制台输出的颜色属性。

颜色属性由两个十六进制数字指定 -- 第一个
对应于背景，第二个对应于前景。每个数字
可以为以下任何值:

    0 = 黑色       8 = 灰色
    1 = 蓝色       9 = 淡蓝色
    2 = 绿色       A = 淡绿色
    3 = 浅绿色     B = 淡浅绿色
    4 = 红色       C = 淡红色
    5 = 紫色       D = 淡紫色
    6 = 黄色       E = 淡黄色
    7 = 白色       F = 亮白色

如果没有给定任何参数，此命令会将颜色还原到 CMD.EXE 启动时
的颜色。这个值来自当前控制台
窗口、/T 命令行开关或 DefaultColor 注册表
值。

如果尝试使用相同的
前景和背景颜色来执行
 COLOR 命令，COLOR 命令会将 ERRORLEVEL 设置为 1。

示例: "COLOR fc" 在亮白色上产生淡红色

```

### more

```
d:\>more /?
逐屏显示输出。

MORE [/E [/C] [/P] [/S] [/Tn] [+n]] < [drive:][path]filename
command-name | MORE [/E [/C] [/P] [/S] [/Tn] [+n]]
MORE /E [/C] [/P] [/S] [/Tn] [+n] [files]

    [drive:][path]filename  指定要逐屏显示的文件。

    command-name            指定要显示其输出的命令。

    /E      启用扩展功能
    /C      显示页面前先清除屏幕
    /P      扩展 FormFeed 字符
    /S      将多个空白行缩成一行
    /Tn     将制表符扩展为 n 个空格(默认值为 8)

            开关可以出现在 MORE 环境变量中。
    +n      从第 n 行开始显示第一个文件

    files   要显示的文件列表。使用空格分隔列表中的文件。
            如果已启用扩展功能，则在 -- More -- 提示处 接受下列命令:
    P n 显示下 n 行
    S n 跳过下 n 行
    F 显示下个文件
    Q 退出
    = 显示行号
    ? 显示帮助行
    <space> 显示下一页
    <ret> 显示下一行

```

### help


```
d:\>help
有关某个命令的详细信息，请键入 HELP 命令名
ASSOC          显示或修改文件扩展名关联。
ATTRIB         显示或更改文件属性。
BREAK          设置或清除扩展式 CTRL+C 检查。
BCDEDIT        设置启动数据库中的属性以控制启动加载。
CACLS          显示或修改文件的访问控制列表(ACL)。
CALL           从另一个批处理程序调用这一个。
CD             显示当前目录的名称或将其更改。
CHCP           显示或设置活动代码页数。
CHDIR          显示当前目录的名称或将其更改。
CHKDSK         检查磁盘并显示状态报告。
CHKNTFS        显示或修改启动时间磁盘检查。
CLS            清除屏幕。
CMD            打开另一个 Windows 命令解释程序窗口。
COLOR          设置默认控制台前景和背景颜色。
COMP           比较两个或两套文件的内容。
COMPACT        显示或更改 NTFS 分区上文件的压缩。
CONVERT        将 FAT 卷转换成 NTFS。你不能转换
               当前驱动器。
COPY           将至少一个文件复制到另一个位置。
DATE           显示或设置日期。
DEL            删除至少一个文件。
DIR            显示一个目录中的文件和子目录。
DISKPART       显示或配置磁盘分区属性。
DOSKEY         编辑命令行、撤回 Windows 命令并
               创建宏。
DRIVERQUERY    显示当前设备驱动程序状态和属性。
ECHO           显示消息，或将命令回显打开或关闭。
ENDLOCAL       结束批文件中环境更改的本地化。
ERASE          删除一个或多个文件。
EXIT           退出 CMD.EXE 程序(命令解释程序)。
FC             比较两个文件或两个文件集并显示
               它们之间的不同。
FIND           在一个或多个文件中搜索一个文本字符串。
FINDSTR        在多个文件中搜索字符串。
FOR            为一组文件中的每个文件运行一个指定的命令。
FORMAT         格式化磁盘，以便用于 Windows。
FSUTIL         显示或配置文件系统属性。
FTYPE          显示或修改在文件扩展名关联中使用的文件
               类型。
GOTO           将 Windows 命令解释程序定向到批处理程序
               中某个带标签的行。
GPRESULT       显示计算机或用户的组策略信息。
GRAFTABL       使 Windows 在图形模式下显示扩展
               字符集。
HELP           提供 Windows 命令的帮助信息。
ICACLS         显示、修改、备份或还原文件和
               目录的 ACL。
IF             在批处理程序中执行有条件的处理操作。
LABEL          创建、更改或删除磁盘的卷标。
MD             创建一个目录。
MKDIR          创建一个目录。
MKLINK         创建符号链接和硬链接
MODE           配置系统设备。
MORE           逐屏显示输出。
MOVE           将一个或多个文件从一个目录移动到另一个
               目录。
OPENFILES      显示远程用户为了文件共享而打开的文件。
PATH           为可执行文件显示或设置搜索路径。
PAUSE          暂停批处理文件的处理并显示消息。
POPD           还原通过 PUSHD 保存的当前目录的上一个
               值。
PRINT          打印一个文本文件。
PROMPT         更改 Windows 命令提示。
PUSHD          保存当前目录，然后对其进行更改。
RD             删除目录。
RECOVER        从损坏的或有缺陷的磁盘中恢复可读信息。
REM            记录批处理文件或 CONFIG.SYS 中的注释(批注)。
REN            重命名文件。
RENAME         重命名文件。
REPLACE        替换文件。
RMDIR          删除目录。
ROBOCOPY       复制文件和目录树的高级实用工具
SET            显示、设置或删除 Windows 环境变量。
SETLOCAL       开始本地化批处理文件中的环境更改。
SC             显示或配置服务(后台进程)。
SCHTASKS       安排在一台计算机上运行命令和程序。
SHIFT          调整批处理文件中可替换参数的位置。
SHUTDOWN       允许通过本地或远程方式正确关闭计算机。
SORT           对输入排序。
START          启动单独的窗口以运行指定的程序或命令。
SUBST          将路径与驱动器号关联。
SYSTEMINFO     显示计算机的特定属性和配置。
TASKLIST       显示包括服务在内的所有当前运行的任务。
TASKKILL       中止或停止正在运行的进程或应用程序。
TIME           显示或设置系统时间。
TITLE          设置 CMD.EXE 会话的窗口标题。
TREE           以图形方式显示驱动程序或路径的目录
               结构。
TYPE           显示文本文件的内容。
VER            显示 Windows 的版本。
VERIFY         告诉 Windows 是否进行验证，以确保文件
               正确写入磁盘。
VOL            显示磁盘卷标和序列号。
XCOPY          复制文件和目录树。
WMIC           在交互式命令 shell 中显示 WMI 信息。

有关工具的详细信息，请参阅联机帮助中的命令行参考。

```

### 管道命令

管道命令总共有“|”、“>”、“>>”、“<”、“<<”五种。

“|”：可叫它为竖管道，随便吧，我也不知道他叫什么，你可以查查。使用的话，最多是搭配more命令使用的，也可以这样，当某个文件夹下的文件比较多时，dir命令不能一屏显示完，可以使用dir| more来显示。

“>”：以覆盖的方式输出重定向。

“<”：以覆盖的方式输入重定向。

“>>”：以追加的形式输出重定向。

“<<”：以追加的形式输入重定向。

### 通配符

通配符一般用于描述filename上

```
一般表达式的快速参考:
  .        通配符: 任何字符
  *        重复: 以前字符或类出现零或零以上次数
  ^        行位置: 行的开始
  $        行位置: 行的终点
  [class]  字符类: 任何在字符集中的字符
  [^class] 补字符类: 任何不在字符集中的字符
  [x-y]    范围: 在指定范围内的任何字符
  \x       Escape: 元字符 x 的文字用法
  \<xyz    字位置: 字的开始
  xyz\>    字位置: 字的结束
```

