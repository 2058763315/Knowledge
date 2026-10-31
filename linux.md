# Linux

## Linux发行版

Linux发行版指的是一些机构或者公司封装好的、针对某种指定用途的、基于Linux的操作系统。常见的发行版有以下几种：

Ubuntu ：乌班图是一个面向一般用户的Linux发行版，也是目前使用最广泛的Linux发行版。这个发行版每年会发布两个版本，这两个版本会在每年的4月和10月发布，其中偶数年份的4月会发布一个长期维护版本，这个版本一般称为 LTS 版本，通常会维护至少五年。

CentOS ：这个发行版是过去服务器上最常见的发行版，早期围绕这个发行版的教程也非常丰富，但它后来被红帽组织收购，先后将 CentOS 8 和 CentOS 7 停止维护，改为维护 CentOS Stream ，从而逐渐式微。

RHEL ：这个发行版是红帽（Red Hat）组织开发的一款商用的Linux发行版，全称： Red Hat Enterprise Linux 。

其他的发行版还有 Debian 、 Arch Linux 、 Kali Linux 等等，其在各自的领域都是比较有名的发行版。

## Linux基础

### 终端（Terminal）

在 `ubuntu` 下，按 `Ctrl + Alt + T` 可以呼出终端，终端默认路径为当前用户的主目录。

1、按上下键可以回溯命令，可回溯的命令可以用 `history` 指令查看。

2、按 `Tab` 可以补全命令，如果有多种可能的命令，则多按一次 `Tab` 键就可以显示所有可能的结果。

### 路径

※使用 pwd 指令可以获取当前目录的绝对路径

1、在Linux下，路径会用正斜杠（ / ）来表示不同层级。（Windows下是反斜杠）。

2、Linux下没有盘符的概念，所有的文件都会挂载在根目录（ / ）下。（Windows下是分盘符挂载的）。

3、Linux下存在主目录（ ~ ）的概念，每一个用户都拥有自己的主目录，互相之间不可见。一般用户的主目录都在 `/home` 下，用户名就是目录名（如： `/home/cheng` 就是 cheng 用户的主目录）。而 root 用户的主目录是 `/root` 。在用户进行操作时，可以用 ` ~` 来代指自己的主目录。

4、在Linux下，以 `.` 开头的文件是隐藏文件。

5、Linux下，每个目录下都有 `.` 和 `..` 两个特殊目录。 `.` 指的是当前路径， `..` 指的是上一级路径。

🌟在日常处理中，文件名其实包含路径。而文件名的长度通常不超过256个字符，所以路径不宜过深。

### 其他命令

**df (disk free)**

查看磁盘分区占用情况：总容量、已用、可用、挂载点、文件系统。

```bash
df -h      #可以查看磁盘状况
```

**常用参数：**

```bash
df         # 默认输出，单位 KB
df -h      # 最常用！human-readable 人性化单位(G/M)
df -H      # 1000进制(厂商硬盘标准)，-h 是1024进制
df -i      # 查看 inode 使用量（磁盘满但空间充足大概率inode耗尽）
df -T      # 显示文件系统类型 ext4/xfs/tmpfs
df -lh     # 只列出本地磁盘，排除NFS等网络挂载
```

```bash
ps -e      #可以静态查看进程信息
top        #可以动态查看进程信息
reboot     #重新启动
init 0     #关机
kill -9 进程编号 #杀死进程
```

## 目录命令

使用 `man + 命令名` 的格式可以查看命令的帮助菜单。所有命令也都有 `--help` 参数作为帮助菜单。

### cd (change directory)

更改当前所在的目录，基本语句逻辑：

```bash
cd 目录名
```

`cd`后可以跟一个路径（绝对路径或相对路径都可），表示切换当前的工作目录。如：

```bash
cd /home/cheng/Desktop #绝对路径
cd Desktop #相对路径
```

特殊用法：

`cd` 后什么也不跟，表示回到当前用户的主目录

`cd -` 表示回退到上一次所在的工作目录，但只能回溯一次

### ls (list)

列表显示目录下的所有文件和子目录，基本语句逻辑：

```bash
ls 目录名
```

`ls`后可以跟一个路径（绝对路径或相对路径都可），表示展示该路径下的所有内容，如果不给目录名，则表示展示当前所在目录的内容。

**常用参数：**

`-a` 显示所有文件/目录（包括隐藏文件）

`-l` 列表显示所有的文件/目录的详细信息

`-F` 往特殊文件的末尾加上符号，其中目录后会加上 `/` ，可执行文件后会加上 `*`

🌟在Ubuntu下，默认支持快捷指令 `ll` ，其相当于 `ls -alF`

### mkdir (make directory)

创建空目录，基本语句逻辑：

```bash
mkdir 目录名 [目录名2 [...]]
```

可以在后面跟上无穷多个目录名，所有的空目录都会被创建。但不支持多层级的创建，如 `mkdir a/b` ，则必须保证 a 存在，才能在目录 a 下创建目录 b

### rmdir (remove directory)

删除**空**目录，基本语句逻辑：

```bash
rmdir 目录名 [目录名2 [...]]
```

可以在后面跟上无数个目录名，只要这些目录是空的就会被删除。

## 文件命令

### touch

创建空文件，基本语句逻辑：

```bash
touch 文件名 [文件名2 [...]]
```

### cp (copy)

复制文件，基本语句逻辑：

```bash
cp 文件名1 文件名2  #格式1
cp 文件名  目录     #格式2
   	 A      B
```

两种格式的A都必须是一个已存在的文件，否则语句直接失效。

格式1中， `cp` 会将A拷贝为B，即创建了A的备份B。如果B是已存在的文件，则会直接覆盖。

格式2中，目录名必须存在（若不存在则等同于格式1），表示将文件拷贝到这个目录中。

**常用参数：**

`-r` ：可以让字段1是一个已存在的目录，复制时，会递归复制目录中所有的内容，这种拷贝同样同样有两种格式，参考 `cp` 基本逻辑即可。

`-f` ：如果待拷贝的文件中存在系统文件或隐藏文件等特殊文件， `cp` 会进行询问是否拷贝，加上参数后就不再询问直接拷贝了。

### mv (move)

移动文件，基本语句逻辑：

```bash
mv A B
```

A可以是一个目录或者一个文件，但必须存在，B同样有两种格式：

格式1：会将A移动为B，即将A重命名为B了。

格式2：会将A移动到B中。

`mv` 不需要参数就可以直接移动目录，所以相当于 `cp -r` ，只是这次是剪切粘贴。

🌟 `mv` 的本质是重命名，所以其速度只和文件数量成正比。所以相对 `cp` 要快。

### rm（remove）

删除文件，基本语句逻辑：

```bash
rm 文件名 [文件名2 [...]]
```

**常用参数：**

`-r` 可以递归删除目录，即即使目录非空，也能递归删掉目录。

`-f` 如果待删除的文件中存在系统文件或隐藏文件等特殊文件，会询问是否删除。

## 管道

管道允许将上一个命令的处理结果作为文件输入给下一个命令。如：

```bash
ls --help | more
```

这个命令的意思是将 `ls --help` 的输出结果交给 `more` 来处理，这样 `more` 就不再需要输入文件了。

**所有查看文件的命令全部都可以用于管道**，而且管道可以连续使用，如：

```bash
ls --help | cat -n | more
```

表示先把结果打上行号再用 `more` 来浏览。

## 重定向

重定向分为输入重定向和输出重定向，查看文件的命令都可以用输入重定向来给予文件，如：

```bash
more < 1.txt
```

这种写法由于和 `more 1.txt` 没有区别， 所以很少出现。

输出重定向可以将原本要输出到屏幕上的内容转而输出到文件中去。如：

```bash
echo "Hello world!" > 1.txt
```

这样原本会打印到屏幕上的 `Hello world!` 就流入了1.txt。此时文件不存在的话就会创建，存在则会覆盖。

输入重定向还可以用 `>>` ，表示不存在则创建，存在则追加。如：

```bash
echo "Goodbye, World!" >> 1.txt
```

🌟双引号内 `$3` 被解析成**位置参数**，单引号内部所有符号原样输出，如：

```bash
echo 'Product A: $10' > price_list.txt
```

## 查看文件命令

### cat (catch)

抓取文件内容，基本语句逻辑：

```bash
cat 文件名
```

**常用参数：**

`-n` 显示行号。

### more

```bash
more 1.txt
```

抓取文件内容，如果文件内容过长，则先打印第一屏（文件的开头），然后可以根据需要按回车向下浏览。按q退出浏览。

### less

`less` 工具和 `more` 不同，不只是更加复杂，而且其查看的内容不会留在终端中。打开后其支持很多操作。

**基本操作**

```bash
↑↓ 		   : 向上或向下走一行
←→ 		   : 用来查看长行的内容
空格/PGDN   : 向下翻页
b/PGUP 	   : 向上翻页
%g 		   : 跳转到第几行（类似vim）
G 		   : 跳转到最后一行
/字段       : 查找某个字段，按n查找下一个，N查找上一个
```

**常用参数：**

`-N` 显示行号

`-S` 切行长行（长行不折行）

### head/tail

这两个指令可以显示文件的头10行/尾10行。

```bash
head 1.txt
ls -help | head
```

**常用参数：**

`-n` 后跟一个数字字段表示要显示几行，如 `head -n 5 3.txt` 表示显示 `3.txt` 文件的前5行。

### grep

这个指令可以筛选文件中符合条件的行。后跟一个字段表示筛选出包含这个字段的行。

```bash
grep 12 2.txt
grep -v newdomain.com email.txt
```

**常用参数：**

`-v` 反选，即筛选出不包含这个字段的行。

`-i` 匹配字段时忽略大小写。

### wc

这个命令是专门来统计行数、单词数和字节数的命令。

```bash
wc 2.txt
```

```bash
你好 001
世界 002
```

打印结果：

```bash
2  4 22 3.txt
```

可以看出直接用 `wc 1.txt` 执行命令时会打印出文件名，如果不想要文件名可以选择在管道中使用，也可以选择输入重定向，如 `cat 1.txt | wc` 或者 `wc < 1.txt` ，后者更常用。

### sort

按照字典序升序来排序整理整个文件的各个行，默认不识别数字。

`-n` 将字段当作数字进行排序

`-r` 排序改为降序

`-k` 后跟一个数字，表示排第几列

`-t` 后跟一个字符，表示分隔符，用这个分隔符来划分列

```bash
sort -nrk3  1.txt  
sort -t, -nrk1,1 num.txt #只使用第1字段作为排序依据，不受行后面内容干扰
```

```
cheng@ubuntu:~/b$ sort -t, -nrk3,3 4.txt
24,94,77,2,14
60,74,66,86,66
90,81,57,78,45
2,36,56,72,3
5,66,52,9,42
0,19,49,7,92
86,53,48,93,54
64,47,46,20,39
26,42,34,28,45
23,76,23,32,47
```

### uniq

这个命令可以过滤掉重复的相邻行，

```bash
uniq [参数] [输入文件] [输出文件]
```

输入文件如果不给表示由管道或输入重定向提供，输出文件不给则将结果直接打印到屏幕上。

**常用参数：**

`-c` 在每行前显示这一行出现了多少次（统计用）

`-d` 只显示重复的行

`-u` 只显示不重复的行

`-i` 忽略大小写

`-f n` 跳过每行的前n个**字段**再进行比较

`-s n` 跳过每行的前n个**字符**再进行比较

### sed (stream editor)

这个命令是一个非交互的文本编辑工具，它按行处理文本，可以执行查找、替换、删除、插入、筛选等等功能，基本逻辑为：

```bash
sed [参数] '命令' [文件名]
```

**常用参数：**

`-n` 关闭原文件的默认输出，只打印由 `sed` 命令补充的行。

`-i[后缀]` 让命令的结果不再显示到屏幕上，而是直接覆盖到文件中。如果提供了后缀，则会将原文件加上后缀后备份起来。

`-e` 允许连续执行多个 `sed` 指令，在执行过程中，行号始终保持一开始的行号。

**sed中的命令**

sed中的命令格式为：`[地址] 命令 [内容]`

**地址**

空地址：表示作用于所有行

行号：5表示第五行，1,3表示1~3行，$表示最后一行

正则表达式：`/test/` 指包含字段test的行，`/^10/` 表示行首为10的行

**命令**

`a` 在行后追加，如：

```bash
sed '1,3a, xxx 11 22 33' 2.txt #前三行后都追加这一行
sed '$a, 哈哈 11 22 33' 2.txt   #最后一行追加
```

`d` 删除这些行，如：

```bash
sed '2,4d' 2.txt  #删除2到4行
sed '/2/d' 2.txt  #删除含有2的行
```

`i` 在行前追加，如：

```bash
sed '2i, 哈哈 11 22 33' 2.txt
```

`c` 替换某些行，如：

```bash
sed '3c, 哈哈 11 22 33' 2.txt  #将第三行换成这样一行
```

`p` 打印某些行，打印某些行，由于原本就会打印，这个命令会导致对应行打印两次。此时可以配合 `-n` 参数关闭原本文件的打印，这样这个命令就成了筛选。

```bash
sed -n '1,3p' 2.txt  #筛选前三行
sed -n '/newdomain.com/!p' email.txt #匹配结果取反
```

`s` 查找替换某些字段，基本用法：

```bash
sed 's/example/foxmail/' email.txt  #将每行中的第一个example替换为foxmail
```

`s///` 默认只替换每行的第一个匹配到的字段，如果要更换，则需要：

`数字` 替换第几次匹配，如 `s/kun/fan/2` 就是将匹配到的第二个 `kun` 替换为 `fan`

`g` 替换所有的匹配

在上面两项后还可以额外加上 `i`，表示匹配时不区分大小写。

`s///` 中的 `/` 可以替换为 `#` 或者 `|` ，以规避待匹配字符串中包含这三个字符中的某个字符的情况。

```bash
sed 's/kun/fan/2' 3.txt  #将每行匹配到的第二个kun替换为fan
sed 's/kun/fan/g' 3.txt  #将全部的kun替换为fan
sed 's/kun/fan/gi' 3.txt #匹配时不区分大小写
```

**sed的多重命令**

当有多个命令时，每个命令前面都要上一个 `-e`，这些命令会**按照先后次序依次执行**。如：

```bash
sed -e 's/kun/cai/' -e 's/kun/xu/' 3.txt
```

即这条命令先后执行了两个功能，先将每行的第一个 `kun` 替换成了 `cai`，再将刚刚替换完后的结果中的每行的第一个 `kun`（即原来的第二个）替换成了 `xu`。

`-e` 可以有无穷多个，在连续使用 `-e` 时，如果要判断行号，则行号不会因为之前指令的影响而发生变动，即始终保持一开始的行号。如:

```bash
sed -e '1,3d' -e '4a lily:lily@qq.com' email.txt
```

可以看出插入的位置依然是一开始的行号，而不是删除改变后的行号。

**sed的备份功能**

`-i` 参数会让原本显示到屏幕上的输出直接修改到源文件中。如:

```bash
sed -i '1i kunkun' text.txt
```

此时屏幕上没有回显，而文件已经被改了。

如果不希望文件直接被修改，也可以选择对文件做备份，可以直接在 `-i` 参数后跟上一个后缀，表示备份文件额外加上一个什么后缀。如:

```bash
sed -i.bak '1i kunkunkun' text.txt
```

此时，`text.txt` 还是被修改了，但同时会多生成一个 `text.txt.bak` 文件，这个文件中的内容是修改前的 `text.text`。

🌟建议先用不带 `-i` 的语句观察结果后再执行。

### awk

这个命令是一种按列处理文本的编程语句，尤其适合结构化文本的处理。其名字来源于其三个作者名字的首字母。基本语句逻辑：

```bash
awk [参数] '命令' [文件名]
```

基本逻辑和 `sed` 差不多。但 `awk` 的命令语法非常复杂，在这个命令中，有两个概念，即**记录**（文本中的一行就是一条记录）和**字段**（默认由空格隔开的一段内容就是一个字段）

**常用的内置变量**

`NR` 当前的行号（如果有多个文件作为输入，则累加）

`FNR` 当前文件中的行号（如果有多个文件作为输入，则各自独立）

`NF` 当前行的字段个数

`$NF` 最后一个字段的值

`FS` 字段分隔符（默认空格），可以由用户手动指定一个或多个分隔符。

`RS` 记录分隔符（默认回车），可以由用户手动指定一个或多个分隔符。

**awk中的命令**

和 `sed` 类似，`awk` 中也有命令格式，只是更加复杂，基本逻辑为：

```bash
'模式 {代码}''
```

其中，代码块可以有三个：

```bash
'BEGIN{代码} 模式 {代码} END{代码}'
```

这三个代码块之间的关系是：

`BEGIN` 块只在处理前执行一次，之后对每行都执行模式{代码}的部分，而 `END` 块只在全部处理完毕后执行一次。

模式包括以下几种:

**空模式**：模式部分留空，表示无条件执行

**关系表达式模式**：可以用比较/逻辑运算符表示这一行是否要进行处理，如:

```bash
awk 'NR == 3 {print}' email.txt #筛选出第三行
awk '$2 >= 60 {print}' 2.txt  #筛选出第二列大于60
awk '$2 >= 60 {print $1"及格"}' 2.txt  #第二列大于60
```

**正则模式：**模式中也可以用正则来匹配，如：

```bash
awk '/^192.168/{print $1" "$NF}' access.log  #提取以 192.168 开头的行，打印该行第一个字段和最后一个字段
```

**BEGIN模式和END模式：**输出每一门课的总分:

```bash
awk 'BEGIN{count1 = 0; count2 = 0; count3 = 0} {count1 += $2; count2 +=$3; count3 += $4; print} END{print "总分 "count1" "count2" "count3}' score.txt
```

**awk中常见的动作语句**

`print` 打印内容，会自动连接各个字段，可以按照需求进行连接

`printf` 类似C语言的格式化输出

`exit` 退出程序，可以跟上退出码，默认退出码为0

**其他部分**

1、`awk` 支持各种算数运算，包括 `+ - / %` ，甚至支持 `^` 的次方运算（也可以用 `a ** b` 求 `a` 的 `b` 次方）

2、支持字符串直接拼接，无需任何操作符，只需要将两个字符串写到一起就行了

3、支持条件判断，即 `if-else` 体系，也支持循环，如 `while、for` 等，其写法和C语言基本一致

4、变量在使用前无需声明，可以直接使用，默认是0或空字符串。

**awk中的关联数组统计**

`awk` 可以像STL中的 `map` 那样统计数据，可以用如下命令统计：

```bash
awk '{count[$1]++} END{for(i in count) print i" "count[i]}' num.txt
```

这里面有两个常用用法：

`for(i in count)` 用 `i` 遍历数组 `count` ，此时的 `i` 就是每一个 `key`

`delete count[key]` 删除数组中的某个元素

**常用参数：**

`-F` 指定分隔符，后面可以跟一个字符，如 `-F:` ，表示分隔符是 `:` ，也可以跟一个中括号，里面是一个正则，表示多个分隔符，如 `-F[:,]` ，或者也可以借用 `-` 表示一个范围，如 `F[a-z]` 。

1、提取 `/etc/passwd` 中UID大于等于1000的用户（第三列是UID）

```bash
awk -F: '$3 >= 1000{print}' /etc/passwd
```

2、分析 `access.log` 文件，找出访问次数最多的5个IP。

```bash
awk '{print $1" "$NF}' access.log | sort -nrk2 | head -n 5 | awk '{print $1}'
```

## 其他常用命令

### echo

打印一个字符串，基本语句逻辑:

```bash
echo 字符串
```

打印出的字符串会自动在末尾补一个回车。

**常用参数：**

`-n` 不补末尾的回车

### find

在磁盘中查找文件，基本语句逻辑：

```bash
find 查找范围 -查找方式 字段
```

查找范围指的是要在范围中查找，需要给一个具体路径，它会在该路径及其子路径中查找。

查找方式:

`-name` 按文件名查找

`-type` 按文件类型查找

`-size` 按大小查找

`-mtime` 按修改时间查找

**示例**

```bash
#全盘搜索名字中包含1.txt的文件
find / -name "1.txt"
#在主目录中查找首字母是D的文件夹
find ~ -type d -name "D*"
#找总大小大于80M的文件
find / -size +80M
#查找最近7天内有改动的文件
find ~ -mtime -7
```

### ln

创建链接文件，基本语句逻辑：

```bash
ln 源文件的绝对路径 链接文件的路径
```

链接文件分为硬链接和软链接，其中硬链接相当于备份了文件，所以并不依赖源文件，但通常不这么做。直接用上述语句创建的链接文件就是硬链接。

**常用参数：**

`-s` 创建软链接，加上这个参数后，创建出的就是软链接文件，即通常说的“快捷方式”，通过 `1s -1` 查看时，快捷方式会显示其指向的文件。

### tar

这个命令用于Linux下对文件的打包/解包操作，也支持一些压缩包类型。基本语句逻辑：

```bash
tar 参数 包名 [文件列表]
```

通常参数 `-f` 是不可少的，用它来指定包名

```bash
tar -zcvf test.tar.gz 待打包文件
```

**常用参数：**

`-f` 指定包名

`-c` 打包，需要给出要被打包的文件列表

`-v` 显示细节(打包/解包了哪些文件)

`-x` 解包

`-z` 打成格式为 `.gz` 的压缩包

`-j` 打成格式为 `.bz2` 的压缩包

## vim编辑器

### 普通模式

进入vim后，默认进入这个模式，在这个模式下不能直接对文本做修改，但可以对文本进行一些宏观的修改，如：

```bash
yy  : 复制一行
%yy : 复制几行
dd  : 剪切一行
%dd : 剪切几行
p   : 粘贴
u   : 撤销
gg  : 跳转到文件开头
%gg : 跳转到第几行
G   : 跳转到文件末尾
```

### 末行模式

```bash
:w  ： 保存
:q  ： 退出，如果没保存则不能直接退出
:wq ： 保存并退出
:q! ： 强制退出，不保存
:wq!： 强制保存退出（必须要有权限才可以完成操作）
:set nu ： 显示行号
:set nonu ： 隐藏行号
/字段 ：查找该字段，按n查找下一个
:s/字段1/字段2/g ：将当前行的字段1替换为字段2
:n1,n2s/字段1/字段2/g ：将行数范围为[n1,n2]的区间中的字段1替换为字段2
:%s/字段1/字段2/g ：将全文的字段1替换为字段2
:sp 文件名 ： 可以同时打开另一个文件。打开的形式为上下分栏，按Ctrl + W可以让光标切换上下栏显
示，两个文件之间的管理是各自独立的，但可以互相粘贴内容。
```

### 编辑模式

在编辑模式下可以直接编辑文件内容，切换到编辑模式虽然有很多种方法，但主要的方式是按 `i` 。如果要从编辑模式退出到普通模式下，则按下 `Esc` 即可。

🌟在Linux的终端环境下， `Ctrl + S` 快捷键的意思是锁屏，即锁定当前屏幕的显示，按 `Ctrl + Q` 即可恢复。

## 软件安装命令

`sudo dpkg -i deb包名` 安装deb包

`sudo dpkg -r deb包名` 卸载软件（保留配置文件）

`sudo dpkg -P deb包名` 彻底卸载（删除程序 + 所有配置）

`sudo dpkg -L deb包名` 查看某个软件安装后所有文件路径

`sudo dpkg -l` 列出系统全部已安装软件

## Linux下的代码的编译和运行

```
main.c -> main.i -> mian.s -> main.o -> mainapp
      预处理     编译       汇编       链接
```

**编译参数：**

-o ：给可执行文件指定文件名。如 `gcc test.c -o test`

-S ：编译成汇编代码（一个 .s 文件）。如 `gcc -S test.c`

-c ：将单个或部分 .c 文件编译成静态库（一个 .o 文件）。 如 `gcc -c test.c`

-g ：采用 Debug 模式编译。如 `gcc -g test.c`

参数可以连用，如： `gcc -c test.c -o test.o`

🌟如果不指定文件名，则输出的可执行文件默认叫 `a.out` ，而其他文件只会改后缀（如 `1.c` 编译为 `1.s/1.o` ）

## 权限和用户

### 权限的查看和含义

在使用 `ls -l` 命令时，可以看到最前面有一个特别的字段，如下是某个 `ls -l` 的结果的一部分:

```
-rw-rw-r-- 1 cheng cheng 8267 7月  22 09:50 1.txt
-rw-rw-r-- 1 cheng cheng   84 7月  22 10:51 2.txt
drwxrwxr-x 3 cheng cheng 4096 7月  21 18:51 c
```

这些字段分别表示 `文件类型及权限 目录深度 文件所有人 文件所在的用户组 文件大小 修改时间 文件名` 

第一个字段的10个字符解释如下：

```bash
文件类型 所有人权限 组权限 其他权限
   d      rwx     r-x    r-x
```

文件类型在Linux中一共有7种，分别是：

```
-    ：普通文件
d    ：目录文件
l    ：链接文件
p    ：管道文件
c    ：字符设备文件
s    ：socket设备文件
b    ：块设备文件
```

剩下9个字符为文件权限

所有人（owner）：指的是这个文件的所有者，通常是创建者。

组用户：指的是和所有人所在组同组的其他用户。

其他用户：组外的用户

每组的三个权限符号，有对应字符则表示有对应权限，反之则没有权限。

```bash
r  ：读权限
w  ：写权限
x  ：执行权限
```

对于一般的文件而言，r表示文件是否可读（即是否可以使用 `cat` 等命令获取内容），w表示文件的内容是否可以被修改，x只针对可执行文件，表示文件是否可以被运行。

对于目录而言，r表示目录内的文件列表是否可见（是否可以 `ls` ），w表示是否能更改目录内的文件/目录结构（是否可以在目录内增删文件/目录），x表示目录是否可以进入（是否可以cd进去）。

### 权限修改命令

#### chomd (change mode)

这个指令有两种模式，修改的方式完全不同。基本语句逻辑：

```bash
chmod 权限字段 文件名/目录名
```

**模式1**

用三组字符来进行管理，如：

```bash
chmod a+x 1.sh
```

中间有三个字符，每一个字符都是一组字符。

第一组：

```bash
u   所有人(user)
g   组用户(group)
o   其他用户(others)
a   所有用户(all)
```

第二组：

```bash
+   增加权限
-   取掉权限
```

第三组：

```bash
r   读权限
w   写权限
x   执行权限
```

其中，有三个特殊规则：

1、第一组、第三组的字符可以多选，如：

```bash
chmod go-wx 1.sh
```

2、第一组的字符可以省略，表示默认写了 `a`，如：

```bash
chmod +x 1.sh #相当于a+x
```

3、中间的三组字段可以有多个，互相之间用逗号隔开，如：

```bash
chmod g+wx,o-w 1.sh
```

**模式2**

这种模式中用三个八进制数来表示权限

| 八进制 | 二进制 | 对应权限 |
| :----: | :----: | :------: |
|   0    |  000   |   ---    |
|   1    |  001   |   --x    |
|   2    |  010   |   -w-    |
|   3    |  011   |   -wx    |
|   4    |  100   |   r--    |
|   5    |  101   |   r-x    |
|   6    |  110   |   rw-    |
|   7    |  111   |   rwx    |

每个八进制数都能表示一个用户的权限，那么第一位表示所有人权限，第二位表示组用户权限，第三位表示其他用户权限，如:

```bash
chmod 755 1.sh
```

对应的权限就是 `rwxr-xr-x` 。

### 用户管理

在Linux中，每一个用户在使用该系统时都会创建自己的主目录，这些主目录默认互相不可见。其中存在一个特殊的用户，即 `root` 用户，它是管理员用户，拥有对系统的所有权限，一般用户可以通过在命令前添加 `sudo` 来尝试获取 `root` 用户权限。在 `Ubuntu` 下，用户可以使用 `sudo -i` 或 `sudo -s` 指令直接切换到 `root` 用户。（`-i` 会一并切换到 `root` 用户的主目录 `/root` 下，`-s` 则保持当前路径不变）。

#### 用户管理的相关命令

##### su

切换到某个用户，基本语句逻辑：

```bash
su 用户名
```

在 Ubuntu 下，这个命令不能用于切换到 root 用户，而是用于在普通用户间切换，或者是 root 用户切换到一般用户。

##### passwd

修改用户密码，基本语句逻辑:

```bash
passwd 用户名
```

如果是一般用户敲击，则不需要带用户名，表示修改自己的密码。如果是root用户使用，则表示直接修改某个用户名的密码。

##### useradd

添加用户，基本语句逻辑：

```bash
useradd 用户名
```

**常用参数：**

`-m` 绑定、创建主目录

`-d` 指定主目录

这两个参数通常连用，后跟主目录的路径

`-g` 给用户指定主组，如果不指定则会创建一个和用户同名的主组，让该用户属于该组

`-s` 用于指定 `shell` 的版本。如果不指定默认使用 `/bin/sh`，通常要指定为 `/bin/bash`

```bash
useradd -md /home/qianquan -s /bin/bash qianquan
```

##### userdel

删除用户，基本语句逻辑：

```bash
userdel 用户名
```

常用参数：

`-r` 删除用户时，连带主目录、邮件池一起删除。如果该用户所属的，和该用户同名的主组中只有这一个用户，则这个主组也会被一并删除。

##### usermod 

修改用户信息，使用方式和 `useradd` 几乎一致，但是必须作用于一个已存在的用户。

**常用参数：**

`-aG` ：`-a` 表示添加一个附加组，`-G` 表示指定附加组的组名，如果不加 `-a`，则表示直接设置附加组，而 `-a` 的意思是在原有附加组的基础上添加，所以这两个通常连用。

```bash
usermod -aG sudo qianquan  #给qianquan的附加组中额外加上一个sudo组，以保证其可以使用sudo权限
usermod -g sales alice     #将alice用户的主组更改为sales组
```

组相关命令

```bash
groupadd #添加新组
groupdel #删除主组
groupmod #修改组信息
```

## makefile

`makefile` 是一个Linux下的一个常见的项目管理工具，主要用于管理中大型项目的编译流程。这里以一个简单的例子做以说明：

首先创建如下三个文件：

`main.c`

```c++
#include <stdio.h>
#include "add.h"

int main()
{
	printf("%d\n", add(4, 7));
	return 0;
}
```

`add.h`

```c++
#idndef _ADD_H_
#define _ADD_H_

int add(int, int);
#endif //_ADD_H_
```

`add.c`

```c++
#include "add.h"

int add(int a,int b){
	return a + b;
}
```

随后要将这三个文件整理成一个项目，此时可以编写这样一个 `makefile` 文件：

```makefile
main: add.o main.o
	gcc add.o main.o -o main
add.o: add.c add.h
	gcc -c add.c -o add.o
main.o: main.c add.h
	gcc -c main.c -o main.o
clean:
	rm *.o main
```

注意，这个文件的名称只能是 `makefile` 或者 `Makefile` 。

此时就可以使用 `make` 指令编译整个项目了。

`make` 指令如果后面不跟标签名，则默认编译第一个标签，如果跟了标签名，就会只执行某个标签。如 `make clean` 就可以清理掉项目中已经编好的所有内容。

`make` 的标签后存在依赖，即 `main` 标签依赖 `add.o` 、 `main.o` 标签，而 `add.o` 标签又依赖 `add.c` 、`add.h` 两个文件，依次类推。标签依赖决定了编译的次序，会先编译 `add.o` 、 `main.o` ，最后编译 `main` 标签，得到可执行文件 `main` 。

在编译前，工具会比较编出的文件和代码文件之间的修改日期，如果编出的文件更新，则会跳过该步的编译。这就是 `makefile` 工具管理工程的思路。实际上现在常用的 `CMake` 等工具在思想上也是差不多的。

## shell语言

shell语言是一种**解释性语言**，解释性语言也叫脚本语言，和编译型语言有很多区别。

|   比较项目   |                          编译型语言                          |                          解释性语言                          |
| :----------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|   基本操作   |        编辑代码，通过编辑器编译成可执行文件，然后运行        |                  编辑代码，直接用解释器运行                  |
| 运行时的区别 |                    直接运行二进制文件即可                    |                    直接以行为单位运行代码                    |
|   可移植性   | 编译好的二进制代码可以直接在相似的系统环境下运行，跨平台时需要重新编译 | 需要平台上已经安装了解释器，只要多个平台上都安装了解释器，则可以直接移植代码。 |
|   运行速度   | 编译时可以统筹资源，部分语言可以直接访问内存空间，所以运行时可以更快、更节省空间 | 运行时以行为单位运行，所以无法对资源做统筹，也不能直接访问内存，所以慢一些 |
|  出错的情况  | 分为编译时报错和运行时报错，由于编译时已经查过错了，所以一般不会在运行时产生严重错误，也很少出现无法回退的错误 | 因为是按行执行的，所以某行出错时，之前已经执行的部分难以回退，且没有编译器把关，更容易出现语法错误 |

在不同的平台上，解释型语言需要分别安装解释器才能让代码通用，所以很多解释性语言都是平台依赖的，`shell` 语言就是 `Unix` 下所常用的脚本语言。而 `windows` 下则是批处理脚本更常见。各个平台上交流的脚本一般都有着操作系统依赖性。

### shell基础

`shell` 语言是Linux下编写脚本（Script）最常用的语言，而一个简单的脚本其实就是语句的集合。

```bash
cp $1 .tmp.txt.bak
grep -v '#'.tmp.txt.bak > $1
rm .tmp.txt.bak
```

这个脚本的功能是清理掉某个文件中带有#的行，其中，$1的含义是这样的:

```bash$0
							 $0		 $1
clairad@ubuntu:~/Desktop$ ./test.sh 1.txt
这里在运行脚本，需要先给脚本执行权限，这些执行字段也天然形成了传给脚本的变量
```

这样 `1.txt` 中带有 `#` 的行就被删除了。

可以看出脚本中就是几个常见命令的集合，而结合起来就成了一个有意义的程序，这就是解释型语言的编程逻辑。

### 变量和运算符

在 `shell` 中，`#` 表示行注释，相当于C++中的 `//` 。但一般第一句会比较特殊，通常脚本的第一句用来指定解释器的路径，通常这样写：

```bash
#!/bin/bash
```

这表示解释器是bash。所以运行脚本时有两个选择：

```bash
#直接用bash运行:
bash 1.sh
#先给权限，然后直接运行:
chmod +x 1.sh
./1.sh
```

#### 变量

在 `shell` 中，变量无需定义，直接使用就行了：

```bash
#!/bin/bash
name='kunkun'
age=28
echo "$name is $age"
```

输出结果：

```bash
kunkun is 28
```

可以看出，变量无需定义，也不用声明类型，可以直接使用。

使用变量的值时，需要在前面加上 `$` ，如果要拼成一句话，最好用双引号引起来，用双引号引起来的一段话会解析其中的 `$` 后的内容，而**如果用单引号引起来，则直接原样打印字符串，不会做任何解析。**

其中，`$` 不仅可以跟变量，还可以这样做：`$(语句)` ，这种方法可以直接得到某个语句的结果：

```bash
#!/bin/bash

path=$(pwd)

echo "$path'
```

`path` 的值就是 `pwd` 语句的执行结果。

#### 运算符

**算数运算符**

使用 `$((算术表达式))` 可以求出一个算数表达式的值。如：

```bash
#!/bin/bash

a=4
b=2

echo "$((a + b))"
echo "$((a ** b))" #表达求a的b次方
```

输出结果：

```
6
16
```

两层小括号中中直接用一般的算数表达式即可，无需用 `$a` 提取变量的值。

**比较运算符**

关于数字的比较运算符

```bash
-eq #等于
-ne #不等于
-gt #大于
-1t #小于
-ge #大于等于
-1e #小于等于
```

关于文件的比较运算符

```bash
-e #文件/目录是否存在
-f #文件是否存在
-d #目录是否存在
-s #文件是否存在且非空
-r #是否可读
-w #是否可写
-x #是否可执行
-L #链接文件是否存在
```

示例

```bash
if [ -f "$file" ]; then
	echo "$file 存在"
fi
```

关于字符串的比较运算符

```bash
-z #字符串长度是否为0
-n #长度是否非0
```

字符串也可以用 `< >` 等运算符来比较大小，比较方式就是字典序。

示例：

```bash
if [ -z "$1" ]; then
	echo "未提供参数"
fi
```

**逻辑运算符**

```bash
-a #逻辑与
-o #逻辑或
!  #逻辑非
```

这三种逻辑运算符是写在 `[]` 内的，现在更常见的写法是直接在中括号外用 `&& || !` 来连接多个中括号：

```bash
if [ -f "$file" ] && [ -r "$file" ]; then
	echo "$file 存在且可读"
fi
```

### 选择结构

#### if结构

基本结构：

```bash
if [ 条件 ]; then

elif [ 条件 ]; then

else

fi
```

示例：

判断参数的个数：

```bash
#!/bin/bash

if [ $# -eq 0 ]; then
	echo "No argument."
else
	echo "There is $# arguments."
fi
```

#### case结构

基本结构：

```bash
case 变量 in
	模式)
		命令 ;;
	模式)
	 	命令 ;;
	 *)
	 	命令 ;;
esac
```

示例：

判断用户的是输入

```bash
#!/bin/bash

read -p "输入Yes/No : " ans
case $ans in
	Yes|yes|YES|y|Y)
		echo "YES" ;;
	No|no|NO|n|N)
		echo "NO" ;;
	*)
		echo "无效输入" ;;
esac
```

### 循环结构

#### for循环

基本结构：

```bash
for var in 值1 值2 值3; do
	echo $var
done	
```

1、输出1加到100的值

```bash
#!/bin/bash

sum=0
for i in $(seq 1 100); do
	sum=$((sum + i))
done

echo $sum
```

**bash下专属的for循环写法**

```shell
for ((i = 1; i <= 100; i++)); do
	echo $i
done
```

这是C语言的风格，所以赋值时可以加空格。

2、用for循环遍历当前路径下的所有文件和目录，并区分它们。

```bash
#!/bin/bash

for file in $(ls); do
	if [ -f $file ]; then
		echo "$file is a file"
	elif [ -d $file ]; then
		echo "$file is a directory"
	fi
done
```

#### while循环

基本结构：

```bash
while [ 条件 ]; do
	命令
done
```

1、用 `while` 完成1加到100的值

```bash
#!/bin/bash

i=1
sum=0
while [ $i -le 100 ]; do
	sum=$((sum + i))
	i=$((i + 1))
done

echo $sum
```

2、逐行读取文件

```bash
#!/bin/bash

if [ -z "$1" ]; then #没有参数直接退出
	echo "未提供参数"
	exit 0
fi

i=1
while read line; do
	echo "$i : $line"
	i=$((i + 1))
done < $1 #让read的读取源是$1
```

### 函数

基本逻辑：

```shell
函数名() {
	local var=10 #函数内的局部变量
	#如果要给函数传参，则直接用字段传入，用$1 $2接收即可
	echo $((var+$1)) #通过输出的方式返回
	return 0 #通过return的方式返回
}
```

例如：

```shell
add() {
	echo $(($1 + $2)) #返回结果
}

echo $(add 3 5) #打印返回值，3 5就是在传参
```

```shell
add() {
	return $(($1 + $2)) #返回结果
}

add 3 5 #调用函数
echo $? #用$?获得最近一次函数调用的返回值
```

### 数组

`shell` 中可以定义数组：

```shell
arr=(red green blue)

echo ${arr[0]} #输出red，注意访问数组元素时，要用{}
echo ${arr[@]} #输出red green blue，输出所有元素
echo ${#arr[@]} #输出3，表示数组的长度

for color in ${arr[@]}; do
	echo $color
done
```

### 综合练习

#### 目录统计

遍历某个指定目录（如果不传就默认是当前目录），分别标记并打印普通文件和目录文件的文件名，并统计两者的数量。

```shell
#!/bin/bash

path='.'
if [ $# -gt 0 ]; then
	if [ -d $1 ]; then
		path=$1
	else
		echo "$1不是一个合法的路径"
	exit 0
	fi
fi

countfile=0
countdir=0
for file in $(ls $path); do
	if [ -f $file ]; then
		echo "[文件] $file"
		countfile=$((countfile + 1))
	elif [ -d $file ]; then
		echo "[目录] $file"
		countdir=$((countdir + 1))
	fi
done

echo "共有$countfile个文件和$countdir个目录"
```

## Linux下的文件和I/O接口

操作系统一般会提供很多底层的开发接口，用于用户更加方便的调用系统功能。这些函数有着平台依赖性，即换一个平台就无法使用了，所以一旦使用这些接口，代码就不再拥有跨平台的可移植性了。

在Linux中，通常使用 `SYSTEM V` 库和 `POSIX` 库作为其底层库，这些库提供了用户和操作系统间沟通的接口。

### open

打开一个文件，头文件：

```c
#include <fcntl.h>
#include <sys/types.h>
```

函数声明：

```c
int open(const char* filename, int flag, ...);
```

**参数列表**

1、要打开的文件名

2、打开方式

`O_RDONLY` 只读

`O_WRONLY` 只写

`O_RDWR` 读写

`O_CREAT` 文件不存在则创建

`O_TRUNC` 文件存在则覆盖

`O_APPEND` 文件存在则追加 

如果要多选，则用位或连接多个选项，如：`O_WRONLY | O_CREAT | O_TRUNC` 。

3、不定参数，平时省略，但如果参数2的选项中存在 `O_CREAT` ，则这个参数用来描述新创建的文件的权限。传入时通常用八进制传入，如 `0644` ，含义参考命令 `chmod` 。

#### 返回值

如果打开成功则返回一个**文件描述符**，失败则返回 `-1` ，并将错误信息记录到 `stderr` 中。`stderr` 中的信息可以使用 `perror` 函数提取并打印。之后只要是 `SYSTEM` 库中的函数，失败情况都是这样，不再赘述。

#### 文件描述符

在 `Linux` 下，可以用一个整型来表示一个已经打开的文件或设备，这个整型数据就是文件描述符。

在一个应用程序中，通常标准输入输出和标准错误就占据了 `0 1 2` 三个文件描述符，在后续分配时，会尽可能小的分配文件描述符。

### creat

创建并打开一个文件，头文件：

```c
#include <fcntl.h>
#include <sys/types.h>
```

函数声明：

```c
int creat(const char* filename, mode_t mode);
```

这个函数相当于第二个参数是 `O_WRONLY | O_CREAT | O_TRUNC` 的 `open`

### close

关闭一个文件描述符，头文件：

```c
#include <unistd.h>
```

函数声明：

```c
int close(int fd);
```

传入一个已打开的文件描述符，关闭它。正常返回0，失败照旧。

### read/write

读写文件描述符，头文件:

```c
#include <unistd.h>
```

函数声明:

```c
int read (int fd, 		void* buf, size_t size);
int write(int fd, const void* buf, size_t size);
```

**参数列表**

1、已经打开的文件描述符

2、要从文件中读取到哪/要把什么写入文件

3、读/写的最大长度

**返回值**

成功则返回实际读/写的字节数，失败照旧。

### lseek

移动文件描述符的读写位置，头文件:

```c
#include <unistd.h>
```

函数声明:

```c
int lseek(int fd, off_t offset, int where);
```

**参数列表**

1、已经打开的文件描述符

2、偏移量，如果是正则表示向后，是负则表示向前。

3、偏移时参照的基本位置，有三个宏:

```
SEEK_SET 文件开头
SEEK_CUR 当前位置
SEEK_END 文件末尾
```

**返回值**

成功则返回偏移后的位置距离文件开头的字节数（相当于 `fte11` ），失败照旧。

### stat/fstat

取得文件状态，头文件：

```c
#include <sys/stat.h>
```

函数声明：

```c
int stat(const char* filename, struct stat* buf);
int fstat(int fd, struct stat* buf);
```

**参数列表**

1、文件名/已打开的文件描述符

2、出参，取出文件的状态

`struct stat` 中常用的成员有两个:

`st_size` 查看文件的总大小

`st_mode` 查看文件的类型和权限

​	判断文件类型时有两个宏：

​	`S_ISDIR` 将 `st_mode` 作为参数传入，判断其是否是目录。

​	`S_ISREG` 将 `st_mode` 作为参数传入，判断其是否是普通文件。

返回值照旧。

### access

判断某个文件是否存在/判断当前用户是否拥有文件的某种权限，头文件：

```c
#include <unistd.h>
```

函数声明：

```c
int access(const char* filename, int flag);
```

**参数列表**

1、文件名

2、要判断的具体项目

```
F_OK  校验文件是否存在
R_OK  \
W_OK  -  校验是否拥有某种权限
X_OK  /
```

如果判断通过则返回0，失败照旧。

### sync/fsync

保存文件。头文件：

```c
#include <unistd.h>
```

函数声明：

```c
int sync();
int fsync(int fd);
```

将数据从内存写入磁盘，其中 `fsync` 是针对某个已打开的文件描述符，而 `sync` 会保存所有已打开的文件描述符。返回值照旧。

### unlink

删除文件。头文件：

```c
#include <unistd.h>
```

函数声明：

```c
int unlink(const char* filename);
```

参数就是要删除的文件，返回值照旧。

### rename

重命名一个文件。头文件：

```c
#include <stdio.h>
```

函数声明：

```c
int rename(const char* oldname, const char* newname);
```

参数1重命名/移动为参数2，返回值照旧。

### remove

删除一个文件或空目录，头文件：

```c
#include <stdio.h>
```

函数声明：

```c
int remove(const char* filename);
```

参数就是要删除的文件或空目录，返回值照旧。

## Linux下的目录接口

### opendir

打开一个目录，头文件及函数声明：

```c
#include <dirent.h>
DIR* opendir(const char* path);
```

参数是要打开的目录文件的路径，成功则返回一个目录指针，失败返回 `NULL` ，其他照旧。

### closedir

关闭一个目录，头文件及函数声明：

```c
#include <dirent.h>
int closedir(DIR* dir);
```

参数是要关闭的目录指针，返回值照旧。

### readdir

读取目录指针中的内容，头文件及函数声明：

```c
#include <dirent.h>
struct dirent* readdir(DIR* dir);
```

传入目录指针，将目录中具体的每个文件/目录的信息以 `struct dirent*` 的格式返回，每次返回一个。即如果目录中有多个文件，则先返回第一个，之后每次调用都会返回下一个，直到返回 `NULL` 时表示访问结束。

这个结构有点类似之前的 `struct stat` ，表示文件的基本信息，但细节上有很多不同，不过关心的内容依然相似，只关心如下两个成员即可：

`d_name` 文件/目录名

`d_type` 文件类型

如下两个：

文件类型中，每种文件类型都单独用一个二进制位表示，直接判断是不是具体的具体的文件类型即可，常用的宏有：

​	`DT_DIR` 类型为目录

​	`DT_REG` 类型为普通文件

🌟使用 `readdir` 读取目录下的内容时，其会读取所有的内容，包括隐藏文件，也包括 `.` 和 `..` 。

### mkdir

创建一个空目录，头文件及函数声明：

```c
#include <unistd.h>
int mkdir(const char* path, mode_t mode);
```

参数1是要创建的目录名，参数2是新目录的权限，返回值照旧。

### rmdir

删除一个目录，头文件及函数声明：

```c
#include <unistd.h>
int rmdir(const char* path);
```

参数就是要删除的目录，返回值照旧。

### chdir

更改当前的活动路径，头文件及函数声明：

```c
#include <unistd.h>
int chdir(const char* path);
```

参数是要切换到的目录，返回值照旧。

## 进程

一般而言，编译好的程序会存放在计算机的磁盘（外存储器）中。在程序运行时，计算机会将磁盘中程序的数据读取到内存（主存储器）中，然后为其分配空间并运行程序。

**进程是操作系统分配资源的最小单位。**

计算机在对多个进程进行**调度**时，每个进程都存在指定的运行**时间片**，当该进程的时间片耗尽时，这个进程就会记录自己当前的状态（保存**上下文**），切换到下一个进程的时间片继续运行，而原来的进程如果还没运行完，就重新回到调度队列的末尾等待调度。

在Linux中，每个进程都有一个 `task_struct` 结构，这个结构中记录着进程的所有信息，在平时使用中并不需要关心太多的内容，只需要关心如下几个关键项即可：

1、**进程标识符（PID）**：进程的身份证号。

2、**父进程标识符（PPID）**：当前进程的父进程的标识符。

在Linux中存在一棵进程树，根节点是原初的0号进程，所有的进程都是源自这个进程。0号进程会创建出1号和2号进程，其中2号主要负责各种系统进程的创建，而1号主要负责用户进程的创建。

除了0号进程外，每个进程都有一个父进程，在创建子进程时，由于父进程会直接拷贝自己的PCB，所以刚创建出的子进程和父进程几乎是完全一致的。

3、**进程状态**：表示进程当前所处的状态，如运行态，睡眠态等。

4、**进程的优先级**：修改进程的优先级主要依靠设置 `NICE` 值实现。每个进程再创建的时候都拥有一个基础优先级，这个优先级通常不能更改（通常为 `80、9、-40` 等几个固定数值），在这个优先级上加上 `NICE` 值，就形成了 `PRICE` 值。`PRICE` 值表示进程的优先级。数值越小优先级越高。`NICE` 值的取值范围是[-20，20)

### PCB（进程控制块）

PCB（Process Control Block）指的是一个进程的所有资源，通常包含一个 `task_struct` 结构，以及其在内存中的空间。

父进程在创建子进程时，会直接拷贝自己的PCB。

### 进程的状态

**R（Running/Runnable）**：运行态或就绪态

进程正在运行中，或者已经具备运行条件。在Linux中对这两种状态不做区分。

**S（Sleeping）**：睡眠态或等待态（可中断的等待态）

进程处于睡眠中，等待某个事件（如I/O事件）将自己唤醒。唤醒后，通常会直接进入R状态。

**D（Disk Sleep）**：磁盘睡眠态（不可中断的等待态）

这种睡眠态也叫深度睡眠态，当进程正在进行一些不可被中断的操作时()

**T（sTopped/Traced）**：停止态或跟踪态

当进程被停止信号（`SIGCONT`）停止运行时，或者正在被调试器调试时，此时就进入了停止态，处于停止进程只能被 `SIGCONT` 信号唤醒并继续运行。

**Z（Zombie）**：僵尸态

僵尸态也叫僵死态，当前进程已经结束，但父进程没有读取自己的退出状态。此时当前进程的内存空间已经回收，但 `task_struct` 依然还在，此时当前进程就处于僵尸态。通常是因为父进程没有正确使用 `wait` 回收子进程资源导致的。

**孤儿进程**

当父进程先于子进程退出时，子进程就成了孤儿进程，孤儿进程会被1号进程收养。

### 进程相关的命令

#### top

Linux下的任务管理器，在这个工具下可以完成很多对进程的基础操作：

```
h/? ：呼出帮助菜单
P   ：按照CPU使用率排序
M   ：按照内存使用率排序
T   ：按照运行时间排序
k   ：输入一个PID，然后输入9，就可以杀死某个进程
r   ：输入一个PID，修改某个进程的nice值
b   ：高亮显示状态为R的进程
q   ：退出
```

#### PS

静态查询进程信息，默认只是显示当前进程及当前进程的子进程的信息。

常用参数：

`-e` 查看所有进程的基本信息（PID、运行时间、创建命令）

`-f` 主要用于查询父进程PID

`-l` 主要查看进程的优先级（PRI和NI）和状态

`aux` 显示的项类似top，主要用于查询内存和CPU使用率

### 进程相关函数

#### getpid/getppid

创建一个子进程，头文件及函数声明：

```c
#include <unistd.h>
pid_t getpid();
pid_t getppid();
```

返回对应的进程标识符，返回值照旧。

#### fork

创建一个子进程，头文件及函数声明：

```c
#include <unistd.h>
pid_t fork();
```

这个函数一经调用，就会用当前进程的PCB生成一个和当前进程相同的子进程，两个进程各自独立，调度次序随机。

##### 返回值

如果成功，则:

1、在父进程中会得到子进程的PID

2、在进程中会得到0

失败照旧。

#### wait

让父进程回收子进程的状态，头文件：

```c
#include <sys/wait.h>
```

函数声明：

```c
pid_t wait(int* retStatus);
```

参数是一个出参，可以用它接受自己成的退出状态和返回值（子进程的返回值需要经过 `WEXITSTATUS` 宏处理后才能获得）。

🌟这个函数是阻塞函数，它会等到子进程结束后再继续向下。

#### exit

结束当前进程，头文件及函数声明：

```c
#include <stdlib.h>
int exit(int retVal);
```

参数就是进程的返回值，相当于在主函数里直接 `return` ，但这个函数可以在任意位置调用。

#### execl/execlp/execle

执行一个新的应用程序，头文件及函数声明：

```c
#include <unistd.h>
int execl (const char* exename, const char* cmd, ...);
int execlp(const char* exename, const char* cmd, ...);
int execle(const char* exename, const char* cmd, ..., char* const envp[]);
```

这系列函数可以执行一个新的应用程序，执行时会用 `exename` 对应的应用程序直接代换掉当前进程的PCB，从而直接切换到了对应进程中执行。其中，`execl` 不适配任何的环境变量，`execlp` 会直接沿用当前进程的环境变量，`execle` 则需要通过最后一个参数单独传入环境变量。

**参数列表**

1、可执行文件的路径

2、执行的命令，按顺序传入 `$0 $1 $2` ，最后以 `NULL` 做结尾，如命令为 `ls -al`，则传入 `"ls", "-al", NULL` 。

3、`execle` 需要的环境变量。

**返回值**

程序一旦执行成功，则目标进程会直接代换掉当前进程，函数直接中止了，所以不存在返回值，失败照旧。

**示例**

```c
#include <stdio.h>
#include <unistd.h>

int main(){
    printf("exec front\n");
    execlp("/bin/ls","ls", "-al", NULL);
    printf("exec back\n"); //  由于被代换，所以这行代码不执行。
    return 0;
}
```

#### execv/execvp/execve

执行一个新的应用程序，头文件及函数声明：

```c
#include <unistd.h>
int execv (const char* exename, char* const argv[]);
int execvp(const char* exename, char* const argv[]);
int execve(const char* exename, char* const argv[], char* const envp[]);
```

这系列函数可以执行一个新的应用程序，执行时会用 `exename` 对应的应用程序直接代换掉当前进程的PCB，从而直接切换到了对应进程中执行。其中， `execv` 不适配任何的环境变量， `execvp` 会直接沿用当前进程的环境变量， `execve` 则需要通过最后一个参数单独传入环境变量。

**参数列表**

1、可执行文件的路径

2、要传给对应进程的 `argv` ，这个数组需要以 `NULL` 作为最后一个元素

3、`execve` 需要的环境变量

**返回值**

一旦执行成功，则目标进程会直接代换掉当前进程，函数直接中止了，所以不存在返回值，失败照旧。

**示例**

```c
#include <stdio.h>
#include <unistd.h>

int main(){
    printf("exec front\n");
    char* strs[] = {"ls", "-al", NULL};
    execvp("/bin/ls", strs);
    printf("exec back\n");
    return 0;
}
```

### 页表

#### 虚拟地址

当进程被创建时，操作系统会给其分配一个执行空间，这个空间和其他进程的执行空间**互相独立**。为了保证这种互相独立，所以操作系统会给每个进程都分配一个虚拟地址，让进程以为自己在这个地址上。其目的是为了保证操作系统的安全性，避免给单个进程过多的权限。

#### 页表

操作系统管理虚拟地址的方式就是通过页表（不一定所有的操作系统都是这样，但一定会用类似的方法来管理进程），页表中可以标记每个进程的真实物理地址和虚拟地址间的映射关系。每当进程间需要交互数据时，都需要经过操作系统。

由于父进程在创建子进程时会直接拷贝自己的PCB，所以父子进程的虚拟地址往往是相同的。

## 进程间通讯（IPC）

由于操作系统一般要通过页表来管理不同进程的虚拟空间，所以进程间通讯就变得复杂起来了。常用的进程间通讯手段有以下几种：

### 文件

文件是一种**读写分离**的通讯手段，这种手段的优点在于适合某个进程公开自己的某个公共资源，如某个进程负责联网，它可以把网络状态写入某个文件，其他进程只要需要状态就可以去读这个文件。这种一对多的通讯是读写分离的舒适区。

但它不适合发送大型数据，毕竟大型数据需要多次发送，此时需要约定时序，或者全部写入文件后再读取，无论哪种，效率都非常差。

### 管道

管道文件是一种特殊的文件，文件类型为 `p` ，它在传输时是**先入先出**的。数据一旦从管道中读走就消失了。

`read` 函数在管道中是阻塞函数，如果管道中没有数据，则会等到管道中有数据再向下执行。

管道在使用过程中分为两种，分别是**匿名管道**和**命名管道**（也叫无名管道和有名管道）

#### 管道相关函数

##### pipe

创建一个匿名管道，头文件及函数声明：

```c
#include <unistd.h>
int pipe(int fd[2]));
```

参数是两个出参，带回了两个文件描述符，其中 `fd[0]` 是管道的读端， `fd[1]` 是管道的写端，返回值照旧。

这样创建的管道就是匿名管道，这种管道只能用于父子进程间通讯。

##### 案例

使用匿名管道完成父子进程间的半双工通讯。

```c
#include <unistd.h>
#include <sys/types.h>
#include <stdio.h>
#include <string.h>

#define _RD 0
#define _WR 1

int main(){
    int fd[2];
    pipe(fd);
    char buf[256];

    pid_t ret = fork();
    if(ret < 0){
        perror("fork error ");
        return -1;
    }
    else if(ret > 0){
        while(1){
            memset(buf, 0, sizeof(buf));
            scanf("%255[^\n]%*c", buf);
            write(fd[_WR], buf, 255);
            if(strcmp(buf, "exit") == 0){
                break;
            }

            usleep(100);
            memset(buf, 0, sizeof(buf));
            read(fd[_RD], buf, 255);
            if(strcmp(buf, "exit") == 0){
                break;
            }
            printf("son : %s\n", buf);
        }
    }
    else{
        while(1){
            memset(buf, 0, sizeof(buf));
            scanf("%255[^\n]%*c", buf);
            read(fd[_WR], buf, 255);
            if(strcmp(buf, "exit") == 0){
                break;
            }
            printf("dad : %s\n", buf);
            
            memset(buf, 0, sizeof(buf));
            write(fd[_RD], buf, 255);
            if(strcmp(buf, "exit") == 0){
                break;
            }
            usleep(100);
        }

    }
    close(fd[0]);
    close(fd[1]);
    return 0;
}
```

##### mkfifo

创建管道文件，头文件及函数声明：

```c
#include <sys/stat.h>
int mkfifo(const char* fifoname, mode_t mode);
```

参数1是要创建的管道文件名，参数2是管道文件的权限，返回值照旧。

#### 管道相关命令

##### mkfifo

创建管道文件，基本逻辑：

```
mkfifo 管道名1 [ 管道名2 [...]]
```

#### 命名管道

命名管道通讯指的就是通过实体管道文件的一种进程间通讯方式。而这种管道文件可以由任意进程打开，所以，它可以用于无关进程间通讯。

#### 案例

通过命名管道完成半双工通讯。

创建管道：

```bash
mkfifo 1.fifo 2.fifo
```

对话双方的代码：

```c
#include <unistd.h>
#include <stdio.h>
#include <fcntl.h>
#include <string.h>
int main(){
    int fdr = open("1.fifo", O_RDONLY);
    if(fdr < 0){
        perror("fdr error ");
        return -1;
    }
    int fdw = open("2.fifo", O_WRONLY);
    if(fdw < 0){
        perror("fdw error ");
        return -1;
    }
    char buf[2048];
    while(1){
        memset(buf, 0, sizeof(buf));
        scanf("%2047[^\n]%*c", buf);
        read(fdr, buf, sizeof(buf));
        if(strcmp(buf, "exit") == 0){
            break;
        }

        printf("fang : %s\n", buf);
        memset(buf, 0, sizeof(buf));
        scanf("%2047[^\n]%*c", buf);
        write(fdw, buf, sizeof(buf));
        if(strcmp(buf, "exit") == 0){
            break;
        }
    }
    close(fdr);
    close(fdw);
    return 0;
}
```

```c
#include <unistd.h>
#include <stdio.h>
#include <fcntl.h>
#include <string.h>
int main(){
    int fdw = open("1.fifo", O_WRONLY);
    if(fdw < 0){
        perror("fdw error ");
        return -1;
    }
    int fdr = open("2.fifo", O_RDONLY);
    if(fdr < 0){
        perror("fdr error ");
        return -1;
    }
    
    char buf[2048];
    while(1){
        memset(buf, 0, sizeof(buf));
        scanf("%2047[^\n]%*c", buf);
        write(fdw, buf, sizeof(buf));
        if(strcmp(buf, "exit") == 0){
            break;
        }

        memset(buf, 0, sizeof(buf));
        scanf("%2047[^\n]%*c", buf);
        read(fdr, buf, sizeof(buf));
        if(strcmp(buf, "exit") == 0){
            break;
        }
        printf("cheng : %s\n", buf);
    }
    close(fdr);
    close(fdw);
    return 0;
}
```

### 共享内存

共享内存（shared memory）是Linux提供的一种基于内存的IPC方式，具体方法是单独开辟一个特殊的页表项，这个页表项，不属于任何一个进程，它指向一个独立的内存空间。这块空间的大小由用户指定，它可以被任何一个进程映射到自己的虚拟空间中去。如果两个或多个进程都映射了这块内存，则它们就可以通过这块内存互相通讯了。

共享内存是读写分离的。

#### shmget

创建/获取共享内存，头文件及函数声明：

```c
#include <sys/shm.h>
#include <sys/ipc.h>
int shmget(key_t key, size_t size, int shmflag);
```

**参数列表**

1、共享内存编码，多个进程获取时都使用同一个编码的话，则可以拿到同一块共享内存。

2、共享内存的大小

3、共享内存的配置，如果给0，表示直接获取，如果是创建，则需要给 `IPC_CREAT` 标签并加上权限，如 `IPC_CREAT | 0666` 。

**返回值**

正常则返回一个共享内存ID（`shmid`），失败照旧。

#### shmat

连接共享内存，头文件及函数声明：

```c
#include <sys/shm.h>
void* shmat(int shmid, const void* addr, int flag);
```

**参数列表**

1、共享内存ID

2、表示共享内存要映射到当前虚拟空间中的什么位置，通常传 `NULL` ，表示让内核自行决定

3、共享内存的配置，通常给0

**返回值**

成功则返回共享内存映射到当前虚拟空间中的首地址，失败返回 `(void*)-1` ，其他照旧。

#### shmdt

脱离共享内存，头文件及函数声明：

```c
#include <sys/shm.h>
int shmdt(const void* shmaddr);
```

传入 `shmat` 的返回值（连接好的共享内存的首地址），脱离共享内存，失败照旧。

#### shmctl

控制共享内存，通常用来删除共享内存，头文件及函数声明：

```c
#include <sys/shm.h>
#include <sys/ipc.h>
int shmctl(int shmid, int cmd, struct shmid_ds* buf);
```

**参数列表**

1、共享内存ID

2、要执行的具体操作

3、根据操作的不同，这个参数的作用也不同：

​	`IPC_STAT` 参数3为出参，表示获取共享内存的状态

​	`IPC_SET` 参数3为入参，表示配置共享内存的属性

​	`IPC_RMID` 参数3给NULL，表示删除共享内存

返回值照旧

#### 案例

基于共享内存的进程间通讯实例：

`SHM.h`

```c
#ifndef __SHMSHM_H_
#define __SHMSHM_H_

#include <sys/shm.h>
#include <sys/ipc.h>

#define KEY 727
#define SHMSIZE 1024

#endif // __SHMSHM_H_
```

`shm.c`

```c
#include <stdio.h>
#include "SHM.h"

int main(){
    int shmid = shmget(KEY, SHMSIZE, IPC_CREAT | 0666);
    if (shmid < 0){
        perror("shmget error ");
        return -1;
    }

    getchar();

    int ret = shmctl(shmid, IPC_RMID, NULL);
    if(ret < 0){
        perror("shmctl error ");
        return -1;
    }
    return 0;
}
```

`write.c`

```c
#include "SHM.h"
#include <stdio.h>
#include <string.h>
#include <unistd.h>

int main(){
    int shmid = shmget(KEY, SHMSIZE, 0);
    if(shmid < 0){
        perror("shmget error ");
        return -1;
    }
    char* buf = (char*)shmat(shmid, NULL, 0);
    if(buf == NULL){
        perror("shmat error ");
        return -1;
    }

    while(1){
        scanf("%1023[^\n]%*c", buf);
        if(strcmp(buf, "exit") == 0){
            break;
        }
    }
    shmdt(buf);
    return 0;
}
```

`read.c`

```c
#include "SHM.h"
#include <stdio.h>
#include <string.h>
#include <unistd.h>

int main(){
    int shmid = shmget(KEY, SHMSIZE, 0);
    if(shmid < 0){
        perror("shmget error ");
        return -1;
    }
    char* buf = (char*)shmat(shmid, NULL, 0);
    if(IS_ERR(buf)){
        perror("shmat error ");
        return -1;
    }

    while(1){
        // puts(buf);
        printf("%s ", buf);
        fflush(stdout);
        sleep(1);
        if(strcmp(buf, "exit") == 0){
            break;
        }
    }
    shmdt(buf);
    return 0;
}
```

### 消息队列

消息队列（`message queue`）也是Linux提供的一种基于内存的IPC方式，这种方法会用到由内核维护的一个队列。当进程向消息队列发送数据时，内核会拷贝这个数据，并放入队列，当进程向消息队列索取数据时，消息队列会将该数据拷贝到对应讲程的虚拟空间中，随即释放该结点。即使用消息队列时，数据要经过一个“进内核”和“出内核”的过程。

#### msgget

创建/获取消息队列，头文件及函数声明：

```c
#include <sys/msg.h>
#include <sys/ipc.h>
int msgget(key_t key, int shmflag);
```

**参数列表**

1、消息队列编码，多个进程获取时都使用同一个编码的话，则可以拿到同一个消息队列。

2、消息队列的配置，如果给0，表示直接获取，如果是创建，则需要给 `IPC_CREAT` 标签并加上权限，如 `IPC_CREAT | 0666` 。

**返回值**

正常则返回一个共享内存ID（`msqid`），失败照旧。

#### msgsnd

将数据放入消息队列，头文件及函数声明：

```c
#include <sys/msg.h>
int msgsnd(int msqid, struct msgbuf* msg, size_t msglen, int msgflag);
```

**参数列表**

1、消息队列ID

2、要向消息队列中放入的消息的内容。放入时必须使用这样一个结构，这个结构的定义为：

```c
struct msgbuf
{
	long mtype;
	char mtext[1];
};
```

用户可以定义一个有具体内容的相似结构：

```c
struct mymsgbuf
{
	long mtype;
	char mtext[128];
};
```

传参时定义一个 `struct mymsgbuf` 结构的变量，然后将其指针强转后传入即可。

3、参数2中 `mtext` 的长度

4、相关配置标记，通常给0

返回值照旧

#### msgrcv

从消息队列中读取数据，这是一个阻塞函数，头文件及函数声明：

```c
#include <sys/msg.h>
int msgrcv(int msqid, struct msgbuf* msg, size_t msglen, long mtype, int msgflag);
```

**参数列表**

参数1、2、3、5参考 `msgsnd`

4、要接受的包的类型，即包中 `mtype` 的值。如果传0，则表示无论 `mtype` 的值是几，这个包都接收。否则只接收指定数值的包。

返回值照旧。

#### msgctl

控制消息队列，通常用来删除消息队列，头文件及函数声明：

```c
#include <sys/msg.h>
#include <sys/ipc.h>
int msgctl(int msqid, int cmd, struct msqid_ds* buf);
```

**参数列表**

1、消息队列ID

2、要执行的具体操作

3、根据操作的不同，这个参数的作用也不同：

​	`IPC_STAT` 参数3为出参，表示获取消息队列的状态

​	`IPC_SET` 参数3为入参，表示配置消息队列的属性

​	`IPC_RMID` 参数3给 `NULL` ，表示删除消息队列

返回值照旧

#### 案例

`MSG.h`

```c
#ifndef __MSQMSQ_H_
#define __MSQMSQ_H_

#include <sys/msg.h>
#include <sys/ipc.h>

#define KEY 1000
#define TEXTLEN 256
#define PKGNO 3

struct myMsgbuf{
    long mtype;
    char mtext[TEXTLEN];
};

#endif // __MSQMSQ_H_
```

`msq.c`

```c
#include "MSQ.h"
#include <stdio.h>

int main(){
    int msqid = msgget(KEY, IPC_CREAT | 0666);
    if(msqid < 0){
        perror("msgget error ");
        return -1;
    }
    getchar();

    int ret = msgctl(msqid, IPC_RMID, NULL);
    if(ret < 0){
        perror("msgctl error ");
        return -1;
    }

    return 0;
}
```

`write.c`

```c
#include "MSQ.h"
#include <stdio.h>
#include <string.h>
#include <time.h>
#include <stdlib.h>
#include <unistd.h>

void divideString(char* cmd, char* words[]){
    int pos = 0;
    words[pos] = strtok(cmd, " ");
    while(words[pos]){
        pos++;
        words[pos] = strtok(NULL, " ");
    }
}

// 从文件读取全部内容，分割单词并逐个发送
int sendWordsFromFile(int msqid, struct myMsgbuf *buf, const char *filePath)
{
    FILE *fp = fopen(filePath, "r");
    if (fp == NULL)
    {
        perror("open file failed");
        return -1;
    }

    char fileBuffer[5120];  // 单次读取文件缓冲区
    char *wordList[1000];    // 存放分割后的单词数组
    while (fgets(fileBuffer, sizeof(fileBuffer), fp) != NULL)
    {
        // 去掉fgets读取自带的换行符
        fileBuffer[strcspn(fileBuffer, "\n")] = '\0';
        memset(wordList, 0, sizeof(wordList));
        divideString(fileBuffer, wordList);

        // 遍历所有分割出来的单词逐个发送
        for (int i = 0; wordList[i] != NULL; i++)
        {
            memset(buf->mtext, 0, TEXTLEN);
            strcpy(buf->mtext, wordList[i]);

            // 发送消息队列
            msgsnd(msqid, (struct msgbuf*)buf, TEXTLEN, 0);
            printf("发送单词：%s\n", buf->mtext);

            usleep(500000); // 间隔0.5s发送一个单词
        }
    }
    fclose(fp);
    return 0;
}

int main()
{
    srand((unsigned)time(0));
    int msqid = msgget(KEY, 0);
    if (msqid < 0)
    {
        perror("msgget error ");
        return -1;
    }
    struct myMsgbuf buf;
    buf.mtype = PKGNO;
    const char *txtPath = "./test.txt";
    // while (1)
    // {
    //     memset(buf.mtext, 0, TEXTLEN);
    //     scanf("%255[^\n]%*c", buf.mtext);
    //     msgsnd(msqid, (struct msgbuf*)&buf, TEXTLEN, 0);
    //     if (strcmp(buf.mtext, "exit") == 0)
    //     {
    //         break;
    //     }
    // }

    while (1)
    {
        int ret = sendWordsFromFile(msqid, &buf, txtPath);
        if(ret < 0) break;
        sleep(1); // 文件全部发完后休眠2秒再重读
    }

    return 0;
}
```

`read.c`

```c
#include "MSQ.h"
#include <stdio.h>
#include <string.h>
#include <unistd.h>

int main(){
    int msqid = msgget(KEY, 0);
    if (msqid < 0)
    {
        perror("msgget error ");
        return -1;
    }
    struct myMsgbuf buf;

    while (1)
    {
        msgrcv(msqid, (struct msgbuf*)&buf, TEXTLEN, PKGNO, 0);
        if (strcmp(buf.mtext, "exit") == 0)
        {
            break;
        }
        printf("%s ", buf.mtext);
        fflush(stdout);
        // puts(buf.mtext);
    }
    return 0;
}
```

### IPC总结

匿名管道仅用于父子进程间通讯，所以不参与讨论，这里来总结一下能在任意进程间通讯的四种方式：

文件：基于文件，读写分离

命名管道：基于文件，先入先出

共享内存：基于内存，读写分离

消息队列：基于内存，先入先出

在点对点发送时，通常会选择使用先入先出的通讯手段，在公布自己的某个状态时（即一对多时），读写分离的手段相对常用。

在彻底无关的进程间（例如你需要去读取某个系统进程的状态）通讯时，通常使用文件。而在有一定关联的进程间（如同一个人/同一个项目组开发的多个进程间）通讯时，通常使用内存。

## 信号

信号是一种**软中断**。程序可以通过信号相关函数**响应**外部向进程发射的一些信号。不同的信号有着不同的默认处理方式。

### 信号相关命令

#### kill

这个命令可以直接向指定进程发射指定信号，基本语句逻辑：

```bash
kill [-信号名/信号编号] 进程的PID
```

如果不给信号名，则默认是15号信号 `SIGTERM`

**常用参数**

`-l` 列出所有的默认信号。

**常用信号**

`2 SIGINT` 表示让程序自行终止，在终端中按下 `Ctrl+c` 即可向当前正在运行中的进程发射该信号。

`15 SIGTERM` 表示让程序自行终止，通常是一种“优雅的”退出方式，一般而言，程序在接收到这个信号后，都要做完收尾处理后再安全退出。

上面两个信号并没有特别明确的区别，做法大差不差。

`9 SIGKILL` 表示杀死程序，这个信号是从外部直接杀死进程，**进程内部无法响应**。

`19 SIGSTOP` 表示停止程序，这个信号是从外部直接停止进程，**进程内部无法响应**。暂停后，程序进入T状态，在终端中按下 `Ctrl+Z` 即可向当前正在运行中的进程发射该信号。

`18 SIGCONT` 表示让停止的程序恢复运行，通常和 `SIGSTOP` 配合使用。

`10 SIGUSR1 / 12 SIGUSR2` 用户预留信号，本身没有特殊意义，专门留出让用户自行定义的信号。

`14 SIGALRM` 定时器信号

### 信号相关函数

#### kill

向指定进程发射指定信号，头文件及函数声明：

```c
#include <signal.h>
int kill(pid_t id, int sig);
```

参数1是要接受信号的进程的 `pid` ，参数2是具体要发射的信号，返回值照旧。

#### raise

向当前进程发射指定信号，头文件及函数声明：

```c
#include <signal.h>
int raise(int sig);
```

参数是具体要发射的信号，失败照旧。

#### signal

让指定信号绑定处理函数，头文件及函数声明：

```c
#include <signal.h>
typedef void (*PFUNC)(int);
PFUNC signal(int signum, PFUNC handler);
```

**参数列表**

1、信号的编码，通常用对应信号名的宏定义来处理，如 `SIGINT` 。

2、信号的处理函数，这个函数有一个int的参数，没有返回值。这个参数是触发函数的信号编号。

除了具体的函数外，这个参数还接受一下两个宏:

`SIG_IGN`忽略这个信号

`SIG_DFL` 恢复这个信号的默认处理方式

返回值是参数2，失败返回 `NULL` ，其他照旧。

#### 案例

让程序无法通过 `Ctrl + c ` 退出

```c
#include <signal.h>
#include <stdio.h>
#include <unistd.h>

void siginthandler(int n){
    printf("就不退，略略略\n");
}

int main(){
    signal(SIGINT, siginthandler); // 这里可以绑定信号处理函数
    signal(SIGKILL, siginthandler); // 但这个信号无法屏蔽
    
    prntf("%d\n", getpid());
    while(1);
    return 0;
}
```

### 定时器相关函数

#### alarm

这个函数是一个由内核管理的软件定时器，在指定的秒数后，内核会向**调用进程**发送  `SIGALRM` 信号。头文件及函数声明：

```c
#include <unistd.h>
unsigned int alarm(unsigned int senconds);
```

转入一个秒数，秒数数到后，向当前进程发射 `SIGALRM` 信号。

#### 案例

让 `SIGALRM` 信号每秒触发一次：

```c
#include <signal.h>
#include <stdio.h>
#include <unistd.h>

void siginthandler(int n){
    alarm(1);
    printf("就不退，略略略\n");
}

int main(){
    signal(SIGALRM, siginthandler);
    
    alarm(1);
    printf("%d\n", getpid());
    while(1);
    return 0;
}
```

**返回值**

如果秒数还没到就再次调用了这个函数，则会重新开始计时，还没数完的秒数就会被返回。如果正常触发返回0，失败照旧。

🌟反直觉的点是，这个函数不会反复触发定时器，而是一次性的。

#### ualarm

微秒版本的 `alarm` ，头文件及函数声明：

```c
#include <unistd.h>
unsigned int ualarm(unsigned int firstus, unsigned int useconds);
```

参数1表示第一次触发信号所要等待的微秒数，参数2表示之后每次触发需要等待的微秒数。

这个函数和 `alarm` 不一样，它会自动触发。但要注意，这两个参数设置的时间都不能超过一秒。

返回值参考 `alarm` 。

#### sleep / usleep

让进程睡眠一段时间，头文件及函数声明：

```c
#include <unistd.h>
unsigned int sleep(unsigned int seconds);
unsigned int usleep(unsigned int useconds);
```

参数是要睡眠的时间，正常结束返回0，如果被其他信号唤醒，则会返回没睡完的秒数/微秒数。失败照旧。

## 守护进程

守护进程（Daemon）是一个**长期在后台运行**且没有任何**控制终端**的进程。许多程序、应用的服务都是以守护进程的形式运行。如最常见的 `systemd` ，其特征为：

1、后台运行，不占用中断，用户无论是注销或者关闭终端，都无法影响到它的运行。

2、声明周期长，系统的守护进程会持续整个开机的时间，即使是应用的，通常开了以后也都不会再被关闭了。

3、无控制终端，即无法接受所有由键盘输入带来的终端信号

### 背景知识

#### 进程组和会话

在进程中存在多个id的概念，其中包括：

`pid` 进程的识别码

`ppid`父进程识别码

`pgid` 进程组识别码

`sid` 会话识别码

打个比方，一个会话相当于一个公司（sid），公司里有很多个部门（pgid），每个部门有很多员工（pid），每个员工也有其介绍人（ppid）。Linux允许员工跨部门工作（允许修改进程的pgid），但不允许员工跳槽到别的公司（修改pgid时，sid不能改变），但允许员工自立门户（创建新的sid），但这个员工不能是公司的领导（不能是某个进程组的组长）。

#### 创建新文件的掩码

这个属性叫 `umask` ，是操作系统默认的属性之一，表示创建文件时给出的文件权限的掩码，使用指令 `umask` 就能查看这个掩码：

```bash
$ umask
0002
```

可以看到默认掩码是 `0002` ，通常 `0002` 和 `0022` 比较常见，这个掩码在用户创建文件时，会将用户决定要使用的权限和这个掩码取反后和原权限做位与运算，以计算结果为最终权限。假设用户打算创建一个权限为 `0666` 的文件时，就会发生:

```
权限：110 110 110
掩码：000 000 010
掩码取反：111111101
取反后的结果和原权限做位与运算得到：
110 110 100
即文件最终的权限是0664
```

使用函数可以修改当前进程使用的默认掩码：

##### umask

设置文件的新掩码，头文件及函数声明：

```c
#include <sys/stat.h>
mode_t umask(mode_t mask);
```

参数是要设置的掩码值，返回值是旧的掩码值，这个函数没有错误调用。

### 编写守护进程的步骤

如果要编写一个标准的守护进程，通常分为5步：

**1、创建子进程，父进程退出**

基本代码：

```c
pid_t pid = forkO;
if (pid<0)
{
	exit(1); //创建失败，则守护进程直接退出
}
else if (pid > 0)
{
	exit(0); //父进程直接关闭，让子进程被1号收养
}
```

用这样的方式可以让守护进程直接脱离当前的终端，以规避终端通过键盘带来的影响。同时因为这个进程并不是当前进程组的第一个进程，所以它不是进程组的组长，为下一步做好了准备。

**2、创建新会话**

```C
setsid();
```

这样做后，当前进程直接成为了新会话的首进程，也就成为了新进程组的组长，从而和原会话彻底划清了关系。

**3、切换工作目录**

```c
chdir("/");
```

这么做是因为

1、进程会占用当前目录的文件系统

2、如果工作目录在可写在目录上(如U盘或者网络路径)上，也不会影响守护进程的稳定性。

所以通常切换到根目录上，以保证安全性。

**4、重设文件权限掩码**

```c
umask(0);
```

让守护进程在创建文件时拥有完全的控制权，因为守护进程其实是一种系统进程，不希望其在创建文件时产生和期望不符的结果。

**5、重定向标准文件描述符**

守护进程已经脱离了终端，但其标准输入/输出/错误还挂在原终端上，所以这一步就是要让最后的这一点联系也断掉

```c
close (STDIN_FILENO);
close (STDOUT_FILENO);
Close (STDERR_FILENO);

int fd = open("/dev/nul1", 0_RDwR);
dup2(fd, STDIN_FILENO);
dup2(fd, STDOUT_FILENO);
dup2(fd, STDERR_FILENO);
```

由于这三个文件描述符的数值恰好是 `0/1/2` ，而 `open` 函数会尽可能小的分配文件描述符，所以这里光关闭不行，毕竟 `open` 还可能复用这些文件描述符，所以标准做法是将这三个文件描述符都通过 `dup2` 函数重定向到系统设备 `/dev/null` 中。这个设备是一个黑洞，给它什么它都会吸收掉，问它要数据也只会给 `EOF` 。这样就可以一劳永逸的解决后续可能带来的 `open` 的问题。

现在，标准输入/输出/错误已经都用不了了，所以在守护进程中如果要保存数据，则需要其他的办法。可以是打开一个文件往里写入，也可以是直接使用系统提供的日志功能。

### 使用syslog记录日志

#### openlog

打开到系统日志的连接，头文件及函数声明：

```c
#include <syslog.h>
void openlog(const char* ident, int option, int facility);
```

参数列表

1、标识字符串，通常是进程名，会出现在当前程序记录的每条日志的开头。

2、选项，通常用宏 `LOG_PID` 表示记录当前进程的PID。

3、日志来源类型，在守护进程中，可以用 `LOG_DAEMON` 宏来表示这是一个守护进程。

#### syslog

记录一条日志，头文件及函数声明：

```c
#include <syslog.h>
void syslog(int priority, const char* format, ...);
```

**参数列表**

1、日志级别，有很多种宏来说明这条日志的级别：

```c
LOG_ERR     : 一般错误信息
LOG_WARNING : 警告信息
LOG_INFO 	: 一般信息
LOG_CRIT 	: 严重错误信息
LOG_ALERT 	: 需要关注的错误信息
```

实际上平时主要用前三个，尤其是第一个和第三个，这些信息的等级都是由程序员自己来决定的。

2、和printf类似，是一个标准字符串，记录日志时，可以通过一些宏定义来辅助记录信息，如 `__FILE__` 、`__TIME__` 、`__LINE__` 等。也可以用 `ctime` 等函数来辅助记录。

3、参考 `printf` 的不定参

#### closelog

关闭到系统日志的连接，头文件及函数声明：

```c
#include <syslog.h>
void closelog();
```

### 实战案例

在实际运维场景中，守护进程最常用的应用场景之一就是监控并保证某个程序始终处于运行状态（即看门狗模式）。

场景：

1、完成一个基本功能程序，让其数数，数到5崩溃。

2、完成一个守护进程看护这个程序，只要它崩溃了，就重新拉起来。

基本程序 `count.c`

```c
#include <stdio.h>
#include <unistd.h>

int main(){
    // 重定向标准输出到文件 /tmp/counter.log
    freopen("/tmp/counter.log", "w", stdout); 
    
    int count = 0;
    while(1){
        count++;
        printf("count = %d\n", count);
        fflush(NULL);
		// 模拟进程在第 5 次循环时“崩溃”
        if(count == 5){
            printf("process crashed\n");
            return -1;
        }
        sleep(5);
    }
    return 0;
}
```

守护程序 `daemon.c`

```c
#include <stdio.h>
#include <sys/signal.h>
#include <stdlib.h>
#include <sys/stat.h>
#include <sys/wait.h>
#include <fcntl.h>
#include <syslog.h>
#include <unistd.h>
#include <time.h>

/* 全局运行标志，控制主循环 */
int g_running = 1;

/* 信号处理函数：收到 SIGINT / SIGALRM 时优雅退出 */
void signal_handler(int sig) {
    g_running = 0;
}

/*
 * 写日志到本地文件
 * 注意：ctime() 返回的字符串自带换行符
 */
void write_log(const char* msg) {
    FILE* fp = fopen("/tmp/watchlog.log", "a");
    if (fp == NULL) {
        return;
    }
    time_t t = time(NULL);
    /* ctime(&t) 自带 \n，所以这里不再额外加换行 */
    fprintf(fp, "%s %s", msg, ctime(&t));
    fclose(fp);
}

/*
 * 初始化守护进程（Daemonize）
 * 返回 0 成功，-1 失败
 */
int init_daemon() {
    pid_t pid = fork();
    if (pid < 0) {
        exit(1);                      // fork 失败，直接退出
    } else if (pid > 0) {
        exit(0);                      // 父进程退出，子进程继续
    }

    /*
     * 创建新会话，脱离终端
     * 使子进程成为会话首进程、进程组首进程
     */
    int ret = setsid();
    if (ret < 0) {
        return -1;
    }

    /* 切换工作目录到根目录，避免占用挂载点 */
    chdir("/");

    /* 清除文件掩码，方便后续文件创建 */
    umask(0);

    /*
     * 关闭标准输入、输出、错误
     * 防止守护进程意外读写终端
     */
    close(STDIN_FILENO);
    close(STDOUT_FILENO);
    close(STDERR_FILENO);

    /*
     * 将所有标准流重定向到 /dev/null
     * 防止后续库函数或子进程输出到终端
     */
    int fd = open("/dev/null", O_RDWR);
    dup2(fd, STDIN_FILENO);
    dup2(fd, STDOUT_FILENO);
    dup2(fd, STDERR_FILENO);

    /* 原始 fd 已复制，可以安全关闭 */
    if (fd > STDERR_FILENO) {
        close(fd);
    }

    return 0;
}

/*
 * 启动被守护的目标进程
 * 返回子进程 PID，失败返回 -1
 */
pid_t start_process(const char* pname) {
    pid_t ret = fork();
    char log[256];

    if (ret < 0) {
        sprintf(log, "fork error: %d", __LINE__);
        write_log(log);
        return -1;
    } else if (ret > 0) {
        /* 父进程：记录子进程 PID */
        sprintf(log, "目标进程已启动，pid = %d", ret);
        write_log(log);
        return ret;
    } else {
        /*
         * 子进程：执行目标程序
         * execlp 成功后不会返回
         */
        execlp(pname, pname, NULL);

        /* execlp 失败才会执行到这里 */
        sprintf(log, "execlp error: %d", __LINE__);
        write_log(log);
        exit(1);  // ⚠️ 建议加上，防止子进程继续运行
    }
}

int main(int argc, char* argv[]) {
    /* 参数检查：必须传入要守护的程序名 */
    if (argc < 2) {
        printf("请传入要守护的应用程序，如：%s <目标程序绝对路径>\n", argv[0]);
        return -1;
    }

    /* 注册信号，用于优雅退出 */
    signal(SIGINT, signal_handler);
    signal(SIGALRM, signal_handler);

    /* 初始化守护进程 */
    int ret = init_daemon();
    if (ret < 0) {
        printf("守护进程创建失败\n");
        return -1;
    }

    /* 打开 syslog，方便系统日志查看 */
    openlog("watchdog", LOG_PID, LOG_DAEMON);
    write_log("=== 守护进程启动 ===");

    int cpid = -1;

    /*
     * 主循环：只要 g_running 为真，就持续守护
     */
    while (g_running) {
        /* 启动目标进程 */
        cpid = start_process(argv[1]);
        if (cpid < 0) {
            write_log("目标进程启动失败，将在3秒后重试");
            sleep(3);
            continue;
        }

        /* 等待子进程退出（阻塞） */
        int status;
        pid_t ret = waitpid(cpid, &status, 0);
        if (ret > 0) {
            char log[256];

            if (WIFEXITED(status)) {
                sprintf(log, "目标进程正常结束，退出码: %d",
                        WEXITSTATUS(status));
            } else if (WIFSIGNALED(status)) {
                sprintf(log, "目标进程被%d号信号终止",
                        WTERMSIG(status));
            } else {
                sprintf(log, "目标进程异常退出");
            }
            write_log(log);
        } else {
            write_log("waitpid异常，重新启动目标进程");
        }

        /* 短暂休眠，避免异常退出时疯狂重启 */
        sleep(2);
    }

    /*
     * 收到退出信号后，清理子进程
     */
    if (cpid > 0) {
        kill(cpid, SIGTERM);
        waitpid(cpid, NULL, 0);
        write_log("被守护进程已经关闭");
    }

    write_log("=== 守护进程退出 ===");
    closelog();
    return 0;
}
```

## 线程

一个进程可以创建多个线程，这些线程是并发的。

**线程是操作系统进行调度的最小单位**

一个进程创建的线程是共用这个进程的PCB的，独属于线程的空间非常小，其运行所消耗的资源大约只有进程的三十分之一。每个线程有自己独立的**运行栈**，其他的空间都是用的当前进程的空间，这些由同一个进程创建出的线程就可以通过堆或者全局区进行通讯了。

### 系统级线程和应用级线程

应用级线程也叫用户级线程，进程在无法直接创建系统级线程的情况下（通常是没有权限，或者使用的不是系统库），会选择这个模式。在这个模式下，多个线程会切分同一个进程的时间片，在这个时间片内做调度。这样做会导致原本的时间片中还要额外产生调度开销，所以在平时的使用中，很少使用这种模式。

系统级线程也叫内核级线程，在这种模式下，每个线程都会像进程一样调度，所以各个线程都有着独立的调度时间片和调度优先级，在这种情况下，一个进程如果创建了多个线程，也就拥有了更多的调度时间片，从而使得进程的运行效率更高。这样做后，进程就同时拥有了多个独立执行的、互不干扰的线程，从而可以完成更多的功能。

每个线程的独立信息不多，包括如下内容：

### TCB (Thread Control Block)

**1、线程ID（TID）**

**2、执行栈**

3、调度优先级

4、执行上下文

5、报警信息、寄存器信息、标记等

通常线程通过一个**函数**生成的，这个函数决定了线程的执行代码，也能一定程度上影响执行栈的大小，生成后，线程执行的代码就是这个函数的代码。而同一个进程创建的多个线程间，除了自己的执行栈是独立的外，其他的内存空间全部都是共享的（即堆和全局都是共享的）。所以线程间通讯完全可以通过堆和全局区。

### 线程的模式

线程有两种模式，`joinable` 和 `detach` 。在应用开发中，线程的模式通常是 `joinable` 。

在 `joinable` 模式下，主线程创建出的子线程可以由主线程回收并获取返回值。这种模式需要更多的系统资源。

在 `detach` 模式下，主线程创建子线程后就直接不去维护子线程了，也无法接收子线程的返回值。

无论是哪种模式，这些线程都依赖于进程的PCB，一旦进程退出，由该进程创建的线程也就全部销毁了。

### 线程的相关函数

Linux的早期函数库是以 `SYSTEM V` 库为基础构建的，这个库有着悠久的历史，之前学习的函数大都是以 `SYSTEM V` 库为基础构建出的Linux系统库函数。而在新时代产生了很多新的需求，为此，Linux又引入了 `POSIX` 库，这个库引入时名义上不是系统库，但实际是以扩展系统库的目的引入的，所以是事实上的系统库。在新版Linux中已经将其视为系统库了，但在目前使用的Ubuntu 20.04上，它还是第三方库。所以如下要使用的线程库（`pthread`库）在编译时需要加上 `-l` 参数才可编译通过：

```bash
gcc thread.c -o thread -lpthread
```

`POSIX` 的出现相对较晚，所以函数名都加上了库名前缀，同时头文件的配置也更加规整了，如下系列函数的头文件都是 `#include <pthread.h>`

#### pthread_create

创建一个线程，函数声明：

```c
int pthread_create(pthread_t* tid,
				   const pthread_attr_t* attr,
				   void* (*thread_func)(void*),
				   void arg);
```

**参数列表**

1、出参，取出线程的 `tid`

2、设置线程属性，通常给 `NULL` ，表示用默认属性创建

3、线程执行函数，在创建线程时，会用该函数生成线程的执行栈。这个函数有一个 `void*` 类型的参数和一个 `void*` 类型的返回值。在C语言中，`void*` 是一种万用类型，所以这相当于什么都能传，什么都能返。

4、传给参数3中现成执行函数的参数

**返回值**

正常为0，失败则返回一个错误码，即只有0才是正常的，其他都是异常的。几乎所有的 `POSIX` 库函数都是如此。

#### pthread_join

等待线程执行完毕（需要线程属性是 `joinable`），函数声明：

```c
int pthread_join(pthread_t tid, void** retval);
```

**参数列表**

1、要等待的线程的 `tid` 。

2、出参，取出线程函数的返回值。传 `NULL` 表示不接收这个返回值。

#### pthread_self

用于获取当前线程的tid。函数声明：

```c
pthread_t pthread_self();
```

#### pthread_exit

类似 `exit` 函数，可以结束当前线程，直接在线程函数中 `return` 也是相同的效果，但这个函数可以用于线程函数调用的子函数中。函数声明：

```c
int pthread_exit(void* retval);
```

#### pthread_detach

将线程设置为 `detach` 模式。函数声明：

```c
int pthread_detach(pthread_t tid);
```

#### pthread_cancel

用于结束某个线程，通常用于主线程管理子线程。函数声明：

```c
int pthread_cancel(pthread_t tid);
```

#### 案例

1、创建五个线程，并给每个线程都传入一个数值，观察现象：

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

void* thread_func(void* arg){
    int i = (long)arg;
    printf("%ld : %d\n", pthread_self(), i);
}

int main(){
    pthread_t tid[5];
    int i;
    for(i = 0;i < 5;i++){
        pthread_create(tid + i, NULL, thread_func, (void*)(long)i);
    }
    for(i = 0;i < 5;i++){
        pthread_join(tid[i], NULL);
    }
    return 0;
}
```

`pthread_create` 的第四个参数如果传成了 `(void*)&i` ，那么子线程中的 `i` 就未必还是当时的那个值了，很可能产生出现的结果有误的情况。因为给子线程传递 `&i` 的动作和主线程 `i++` 的动作是并发的，无法确定谁前谁后，所以子线程拿到 `i` 的时候，主线程的 `i` 可能已经 `++` 了，这样就会导致出现偏差。所以此处可以选择直接传值，这样就不会有问题了。

这里强转成 `long` 的原因是对其变量和指针的长度， `gcc` 环境下， `long` 的长度和指针一致，如果考虑到可移植性，可以考虑用 `size_t` 代替。

注意：这种写法的基础是C语言的 `void*` 比较特殊，可以直接和数字做强转，但C++不能这么做，但可以用 `reinterpret_cast` 关键字来完成相同的功能。

2、设置一个全局变量，让多个线程争抢它的值。

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

int g_src = 10000000;

void* thread_func(void* arg){
    int count = 0;
    while(g_src > 0){
        g_src--;
        count++;
    }
    printf("%ld : %d\n", pthread_self(), count);
}

int main(){
    pthread_t tid[5];
    int i;
    for(i = 0;i < 5;i++){
        pthread_create(tid + i, NULL, thread_func, NULL);
    }
    for(i = 0;i < 5;i++){
        pthread_join(tid[i], NULL);
    }
    return 0;
}
```

某次执行结果：

```
140531395643136 : 3410242
140531370465024 : 2671672
140531387250432 : 5484455
140531378857728 : 4938168
140531404035840 : 6081823
```

可以看出抢到的资源数远大于了一开始的资源数 `10000000` ，这是因为并行时可能由多个线程同时获取了某个资源导致的，显然这种抢夺是不安全的。

### 互斥锁

互斥锁是一种非常常见的、保证资源访问唯一性的手段，使用互斥锁可以防止公共资源在同一时间被多个线程同时修改，从而保证资源获取的唯一性。

被锁锁起来的一段代码具备**原子性**，具备原子性的一段代码要么一口气执行完，要么一行都不执行。

定义互斥锁后，可以在要修改全局资源时上锁，在修改完毕后解锁，这样在所有线程共享一把锁的情况下，就可以完成对资源修改的互斥了。

互斥锁的定义为：

```c
pthread_mutex_t mutex;
```

互斥锁有所属人，通常在哪个线程中上锁，就要在这个线程中解锁，而不能出现在A线程中上锁，却在B线程中解锁的行为。

#### pthread_mutex_init

初始化互斥锁，函数声明：

```c
int pthread_mutex_init(pthread_mutex_t* mutex, const pthread_mutexattr_t* attr);
```

**参数列表**

1、出参，要初始化的互斥锁

2、相关配置项，通常给 NULL

除了使用这种方式初始化外，也可以直接用静态初始化的方式初始化一把锁：

```c
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER
```

#### pthread_mutex_destory

销毁互斥锁：函数声明：

```c
int pthread_mutex_destory(pthread_mutex_t* mutex);
```

#### pthread_mutex_lock / unlock

获取/释放互斥锁，函数声明：

```c
int pthread_mutex_lock(pthread_mutex_t* mutex);
int pthread_mutex_unlock(pthread_mutex_t* mutex);
```

`pthread_mutex_lock` 是一个阻塞函数，如果在尝试获取获取互斥锁时，有其他线程已经获取到互斥锁了，则这个函数就会阻塞。等待其他线程释放了互斥锁后，它才可能获取到互斥锁并向下运行。

#### pthread_mutex_trylock

```c
int pthread_mutex_trylock(pthread_mutex_t* mutex);
```

这个函数和 `lock` 的区别在于，它不会阻塞，如果正常获取到了互斥锁就会返回0，如果没获取到，直接返回非0值后向下运行。

#### 案例

设置一个全局变量，让多个线程争抢它的值，且保证争抢时的总资源数是正确的。

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

int g_src = 10000000;
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

void* thread_func(void* arg){
    int count = 0;
    while(g_src > 0){
        pthread_mutex_lock(&mutex);
        if(g_src > 0){
            g_src--;
            count++;
        }
        pthread_mutex_unlock(&mutex);
    }
    printf("%ld : %d\n", pthread_self(), count);
}

int main(){
    pthread_t tid[5];
    int i;
    for(i = 0;i < 5;i++){
        pthread_create(tid + i, NULL, thread_func, NULL);
    }
    for(i = 0;i < 5;i++){
        pthread_join(tid[i], NULL);
    }
    pthread_mutex_destroy(&mutex);
    return 0;
}
```

### 生产者-消费者模型

这个模型是线程在实际应用场景中极其常见的一种模型，通常由部分线程完成对资源的生产，另一部分线程完成对资源的消费，此时生产和消费时（尤其是消费时），这些线程间就构成了**竞态**关系。

上例中是由一个全局资源 `g_src` 保有资源，而五个消费者线程在消费资源，这种对资源的抢夺具备强随机性，所以需要通过互斥锁来保证资源访问的唯一性。如果加上生产者线程，则生产者也会和消费者之间抢夺公共资源，所以这些线程间都要保证互斥。

如下是一个由一个生产者线程和10个消费者线程组成的生产者-消费者模型：

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

#define CON_NUM 10

int g_src = 0;
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;

void* thread_productor(void* arg){
    int tmp;
    while(g_src >= 0){
        scanf("%d", &tmp);
        pthread_mutex_lock(&mutex);
        g_src += tmp;
        pthread_mutex_unlock(&mutex);
    }
}

void* thread_constumer(void* arg){
    int no = (long)arg;
    int count = 0;
    while(g_src >= 0){
        while(g_src > 0){
            pthread_mutex_lock(&mutex);
            if(g_src > 0){
                g_src--;
                count++;
            }
            pthread_mutex_unlock(&mutex);
        }
        if(count > 0){
            printf("%d : %d\n", no, count);
            count = 0;
        }
    }
}

int main(){
    pthread_t tid[CON_NUM];
    pthread_t tidp;
    pthread_create(&tidp, NULL, thread_productor, NULL);
    int i;
    for(i = 0;i < CON_NUM;i++){
        pthread_create(tid + i, NULL, thread_constumer, (void*)(long)i);
    }
    pthread_join(tidp, NULL);
    for(i = 0;i < CON_NUM;i++){
        pthread_join(tid[i], NULL);
    }
    pthread_mutex_destroy(&mutex);
    return 0;
}
```

这个框架保证了生产和消费的有序。

### 死锁问题

死锁是指当多个线程（或者进程）在运行时，因为争夺资源而造成的一种互相等待的状态。例如线程A持有B锁，等待A锁释放，而线程B持有A锁，等待B锁释放，这样就构成了一种死锁现象，双方互相卡死，无法继续运行。

除了这种现象外，单纯的上了锁但忘记解锁也能产生死锁。

**死锁示例**

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

pthread_mutex_t mutex1 = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_t mutex2 = PTHREAD_MUTEX_INITIALIZER;

void* threadfunc1(void* arg){
    pthread_mutex_lock(&mutex1);
    printf("thread1 get mutex1\n");
    pthread_mutex_lock(&mutex2);
    printf("thread1 get mutex2\n");

    printf("thread1 in running\n");

    pthread_mutex_unlock(&mutex2);
    pthread_mutex_unlock(&mutex1);
}

void* threadfunc2(void* arg){
    pthread_mutex_lock(&mutex2);
    printf("thread2 get mutex2\n");
    pthread_mutex_lock(&mutex1);
    printf("thread2 get mutex1\n");

    printf("thread2 in running\n");

    pthread_mutex_unlock(&mutex1);
    pthread_mutex_unlock(&mutex2);
}

int main(){
    pthread_t tid1, tid2;
    pthread_create(&tid1, NULL, threadfunc1, NULL);
    pthread_create(&tid2, NULL, threadfunc2, NULL);

    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);

    pthread_mutex_destroy(&mutex1);
    pthread_mutex_destroy(&mutex2);
    return 0;
}
```

这个代码就会发生概率死锁（如果解开两行 `usleep` 就是一定会发生死锁）。

死锁问题一旦发生，程序就会彻底卡死，所以一定要想一些办法来规避死锁。此时 `trylock` 就是一个很好的帮手。如：

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>

pthread_mutex_t mutex1 = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_t mutex2 = PTHREAD_MUTEX_INITIALIZER;

void* threadfunc1(void* arg){
    int ret;
    while(1){
        ret = pthread_mutex_trylock(&mutex1);
        if(ret){
            continue;
        }

        printf("thread1 get mutex1\n");
        usleep(10000);
        ret = pthread_mutex_trylock(&mutex2);
        if(ret){
            pthread_mutex_unlock(&mutex1);
            usleep(10000);
            continue;
        }
        printf("thread1 get mutex2\n");

        printf("thread1 in running\n");

        pthread_mutex_unlock(&mutex2);
        pthread_mutex_unlock(&mutex1);
        break;
    }
}

void* threadfunc2(void* arg){
    int ret;
    while(1){
        ret = pthread_mutex_trylock(&mutex2);
        if(ret){
            continue;
        }

        printf("thread2 get mutex2\n");
        usleep(10000);
        ret = pthread_mutex_trylock(&mutex1);
        if(ret){
            pthread_mutex_unlock(&mutex2);
            usleep(10000);
            continue;
        }
        printf("thread2 get mutex1\n");

        printf("thread2 in running\n");

        pthread_mutex_unlock(&mutex1);
        pthread_mutex_unlock(&mutex2);
        break;    
    }
}

int main(){
    pthread_t tid1, tid2;
    pthread_create(&tid1, NULL, threadfunc1, NULL);
    pthread_create(&tid2, NULL, threadfunc2, NULL);

    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);

    pthread_mutex_destroy(&mutex1);
    pthread_mutex_destroy(&mutex2);
    return 0;
}
```

### 可重入函数

可重入函数指的是没有操作全局资源的函数，这样的函数往往是线程安全的，这样的函数即便被打断或参与并发，也不会出现问题。反过来**不可重入函数**就是操作了全局资源的函数，从而导致其一旦用在线程中，就可能发生线程不安全的情况，常见的不可重入函数包括各类输入输出、动态内存分配函数等。

使用不可重入函数时需要注意上锁，保证某些资源的原子性。

🌟信号函数要保证简短，在操作全局资源时，尽量不要操作竞态的全局资源，以保证数据的安全性。

### 信号量

如果说互斥锁是用来保证资源访问唯一性的，那么信号量保证的就是访问资源的总人数至多有多少。互斥锁无法完成这样的事，而信号量就是为此设计的。

🌟本节讨论的是匿名信号量，匿名信号量通常用于用户在线程间约束访问资源的总人数。

#### 相关函数

以下函数的头文件均为 `#include <semaphore.h>`

##### sem_init

初始化信号量，函数声明：

```c
int sem_init(sem_t* sem, int shared, unsigned int value);
```

**参数列表**

1、要初始化的信号量

2、通常给0，表示只在线程间共享。如果要在进程间共享，通常就是一个非零值了。匿名信号量可以配合共享内存在进程间共享。

3、信号量的初始值，一般决定了能访问的最大人数。

##### sem_destroy

销毁信号量，函数声明：

```c
int sem_destroy(sem_t* sem);
```

##### sem_wait

等待信号量，相当于上锁，这是一个阻塞函数，函数声明：

```c
int sem_wait(sem_t* sem);
```

这个函数的具体操作方式是给信号量的值减一，如果信号量的值已经为0，则会阻塞等待信号量的值不为0时，再向下执行。

##### sem_post

发布信号量，相当于解锁，函数声明：

```c
int sem_post(sem_t* sem);
```

这个函数的具体操作方式是给信号量的值加一，它可以让信号量的值超过初始值。

#### 信号量与锁的区别

当匿名信号量的值为1时，其和锁的区别并不大。其主要有如下几个核心区别：

1、信号量即使初值为1，也可以通过 `post` 函数突破初值，而锁不行

2、锁是有所属的，即谁上锁、谁解锁。而信号量没有所属

3、锁如果要通过共享内存进行进程间通讯，则需要额外配置，信号量不需要。

#### 信号量的应用

接下来完成一个基于信号量的可阻塞的循环队列：

要求这个循环队列在入队列时，如果队列已满，则阻塞；在出队列时，如果队列是空的，则阻塞。

这样这个队列便无需判空/判满，即使满了也只是延迟放入。

在这种情况下，可以用两个信号量来完成：

1、让信号量1的初始值为循环队列的总长度，每次入队列时 `wait` ，出队列时 `post` 。这样的话，当队列满了的时候，信号量1阻塞。

2、让信号量2的初始值为0，每次入队列时 `post` ，出队列时 `wait` 。这样的话当队列为空时，信号量2阻塞。

🌟此时信号量2的值就是循环队列中的有效元素个数，这个数值可以 `sem_getValue` 获取，但并不安全，仅做参考。

用这个思路就可以完成一个带有阻塞的消息队列了。

**示例**

```c
#ifndef _LOOPQUEUE_HPP_
#define _LOOPQUEUE_HPP_

#include <iostream>
#include <semaphore.h>
#include <pthread.h>

namespace my{
    template <class T, size_t N>
    class loopQueue{
    protected:
        int m_val[N];
        size_t m_front;
        size_t m_rear;

        sem_t sem_full;
        sem_t sem_empty;
        pthread_mutex_t mutex;
    public:
        loopQueue() : m_front(0), m_rear(0) {
            sem_init(&sem_full, 0, N);
            sem_init(&sem_empty, 0, 0);
            pthread_mutex_init(&mutex, NULL);
        }
        ~loopQueue(){
            sem_destroy(&sem_full);
            sem_destroy(&sem_empty);
            pthread_mutex_destroy(&mutex);
        }
        void push(const T& val){
            sem_wait(&sem_full);
            pthread_mutex_lock(&mutex);
            m_val[m_rear] = val;
            m_rear++;
            m_rear %= N;
            pthread_mutex_unlock(&mutex);
            sem_post(&sem_empty);
        }

        void pop(){
            sem_wait(&sem_empty);
            pthread_mutex_lock(&mutex);
            m_front++;
            m_front %= N;
            pthread_mutex_unlock(&mutex);
            sem_post(&sem_full);
        }

        T front(){
            return m_val[m_front];
        }

        T back(){
            return m_val[(m_rear - 1 + N) % N];
        }
    };
}

#endif // _LOOPQUEUE_HPP_
```

这个队列作为一个定长队列的板子，具备一定的现实意义，且很容易就能移植到进程间通讯（共享内存）中变成类似消息队列的存在。但因为其中有把锁，所以要注意移植的时候给锁配置属性，或者干脆用值为1的信号量代替。但因为消息队列要通过内核发生两次拷贝，所以就效率上而言，这种队列反而更快。

### 条件变量

条件变量用于线程间的同步，可以在A线程触发了某个事件后向B线程发射一个信号，以告知其他线程这个事件发生。例如在生产者消费者模型中，生产者可以在生产了产品后发射一个信号，唤醒消费者来争抢资源，从而规避了在没有资源时，消费者产生的循环空转问题。这种机制可以有效的降低CPU的开销，让消费者只在有资源时才占用CPU资源。

#### 相关函数

##### pthread_cond_init

初始化条件变量，函数声明：

```c
int pthread_cond_init(pthread_cond_t* cond,const pthread_condattr_t* attr);
```

和互斥锁类似，参数是要初始化的条件变量和相关配置项，配置项通常给 `NULL`

和互斥锁类似，它也有静态初始化的方法：

```c
pthread_cond_t cond = PHTREAD_COND_INITIALIZER;
```

##### pthread_cond_destroy

销毁条件变量，函数声明：

```c
int pthread_cond_destroy(pthread_cond_t* cond);
```

##### pthread_cond_wait

等待条件变量，这个函数是阻塞函数。函数声明：

```c
int pthread_cond_destroy(pthread_cond_t* cond, pthread_mutex_t mutex);
```

这个函数如果要用在一段被锁锁住的代码中，则需要把锁传给第二个参数，执行函数时，这个函数会先释放互斥锁，然后让线程阻塞，在醒来后，会重新获取互斥锁，获取到后就向下运行。

##### pthread_cond_signal / broadcast

唤醒条件变量，函数声明：

```c
int pthread_cond_signal(pthread_cond_t* cond);
int pthread_cond_broadcast(pthread_cond_t* cond);
```

`signal` 会随机唤醒一个等待在当前条件变量上的线程，而 `broadcast` 会唤醒所有等待在当前条件变量上的线程。

#### 案例

使用条件变量完成生产者消费者模型：

```c
#include <pthread.h>
#include <stdio.h>

#define CON_NUM 10

int g_src = 0;
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;

void* thread_producer(void* arg){
    int tmp;
    while(g_src >= 0){
        scanf("%d", &tmp);
        pthread_mutex_lock(&mutex);
        g_src += tmp;
        pthread_mutex_unlock(&mutex);
        pthread_cond_broadcast(&cond);
    }
}

void* thread_consumer(void* arg){
    int no = (long)arg;
    int count = 0;
    while(g_src >= 0){
        pthread_mutex_lock(&mutex);
        while(g_src == 0){
            if(count > 0){
                printf("%d : %d\n", no, count);
                count = 0;
            }
            pthread_cond_wait(&cond, &mutex);
        }
        if(g_src > 0){
            g_src--;
            count++;
        }
        pthread_mutex_unlock(&mutex);
    }
}

int main(){
    pthread_t tid[CON_NUM];
    pthread_t tidp;
    pthread_create(&tidp, NULL, thread_producer, NULL);
    int i;
    for(i = 0;i < CON_NUM; i++){
        pthread_create(&tidp, NULL, thread_consumer, (void*)(long)i);
    }
    pthread_join(tidp, NULL);
    for(i = 0;i < CON_NUM;i++){
        pthread_join(tid[i], NULL);
    }
    pthread_mutex_destroy(&mutex);
    pthread_cond_destroy(&cond);
    return 0;
}
```

多生产者多消费者的情况：

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>
#define CON_NUM 15
#define PRODUTA 1000000
#define PRODUTB 6000000
int g_flag = 1;
int g_srcA = 0;
int g_srcB = 0;
pthread_mutex_t mutexA = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_t mutexB = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;

void* thread_productorA(void* arg){
    while(g_flag){
        pthread_mutex_lock(&mutexA);
        g_srcA += PRODUTA;
        pthread_mutex_unlock(&mutexA);
        pthread_cond_broadcast(&cond);
        sleep(2);
    }
}

void* thread_productorB(void* arg){
    while(g_flag){
        pthread_mutex_lock(&mutexB);
        g_srcB += PRODUTB;
        pthread_mutex_unlock(&mutexB);
        pthread_cond_broadcast(&cond);
        sleep(5);
    }
}

void* thread_constumerA(void* arg){
    const char* name = (void*)arg;
    int count = 0;
    while(g_flag){
        pthread_mutex_lock(&mutexA);
        while(g_srcA == 0){
            if(count > 0){
                printf("%s : %d\n", name, count);
                count = 0;
            }
            pthread_cond_wait(&cond, &mutexA);
        }
        if(g_srcA > 0){
                g_srcA--;
                count++;
        }
        pthread_mutex_unlock(&mutexA);
    }
}

void* thread_constumerB(void* arg){
    const char* name = (void*)arg;
    int count = 0;
    while(g_flag){
        pthread_mutex_lock(&mutexB);
        while(g_srcB == 0){
            if(count > 0){
                printf("%s : %d\n", name, count);
                count = 0;
            }
            pthread_cond_wait(&cond, &mutexB);
        }    
            
        if(g_srcB > 0){
            g_srcB--;
            count++; 
        }
        pthread_mutex_unlock(&mutexB); 
    }
}

void* thread_constumerAB(void* arg){
    const char* name = (void*)arg;
    int countA = 0, countB = 0;
    while(g_flag){
        pthread_mutex_lock(&mutexA);
        while(g_srcA == 0 && g_srcB == 0){
            if(countA > 0){
                printf("%s : A : %d\n", name, countA);
                countA = 0;
            }
            if(countB > 0){
                printf("%s : B : %d\n", name, countB);
                countB = 0;
            }
            pthread_cond_wait(&cond, &mutexA);
        }

        if(g_srcA > 0){
            g_srcA--;
            countA++;
        }
        pthread_mutex_unlock(&mutexA);

        pthread_mutex_lock(&mutexB);
        if(g_srcB > 0){
            g_srcB--;
            countB++;
        }
        pthread_mutex_unlock(&mutexB);
    }
}

int main(){
    pthread_t tid[CON_NUM];
    pthread_t tidp1, tidp2;
    char name[15][15] = {"A1  沈景珩", "A2  陆知屿", "A3  顾沐辰", "A4  江砚辞", "A5  傅斯年", 
        "B1  温叙白", "B2  季承宇", "B3  苏晚晴", "B4  许清禾", "B5  宋知微", 
        "AB1 林舒瑶", "AB2 夏星眠", "AB3 阮云汐", "AB4 孟若棠", "AB5 叶芷宁"};

    pthread_create(&tidp1, NULL, thread_productorA, NULL);
    pthread_create(&tidp2, NULL, thread_productorB, NULL);
    int i;
    for(i = 0;i < 15;i++){
        if(i >= 0 && i <= 4){
            pthread_create(tid + i, NULL, thread_constumerA, name[i]);
        }
        else if(i >= 5 && i <= 9){
            pthread_create(tid + i, NULL, thread_constumerB, name[i]);
        }
        else{
            pthread_create(tid + i, NULL, thread_constumerAB, name[i]);
        }
    }

    sleep(30);
    g_flag = 0;
    pthread_join(tidp1, NULL);
    pthread_join(tidp2, NULL);
    for(i = 0;i < CON_NUM;i++){
        pthread_join(tid[i], NULL);
    }
    pthread_mutex_destroy(&mutexA);
    pthread_mutex_destroy(&mutexB);
    pthread_cond_destroy(&cond);
    return 0;
}
```

### 读者-写者模型

在访问公共资源时，通常存在这样一种需求：

1、读取公共资源的读者线程不互斥

2、修改公共资源的写者线程需要和读者线程及其他写者线程进行互斥

这个需求就是读者-写者问题。

#### 问题分析

按照需求而言，读者一旦开始读，之后就只能有其他读者进来读了，而只有当所有读者都退出时，才能有写者进来写，所以读者需要记录数量，当数量归零时，通知某个写者线程来写即可。

同理写者在进入时也需要置位一个写者标记，退出时清理，并通知所有读者和某个其他写者。

读者中，由于读者数量本身也是全局资源，所以在修改读者数量时，需要上锁。

而写者因为全程互斥，所以全程都要上锁。两把锁互不影响，可以是一把锁。

在读者线程进入时，如果看到写者标记被置位，则表示有写者在写，此时读者要陷入阻塞，等待通知，同理，在写者线程进入时，如果发现读者数量不为0，也要陷入阻塞，等待通知。这两个阻塞要进行区分，否则可能会出现同类线程互相通知的情况，所以读者通知写者需要一个条件变量，反之亦然。

综上，除了本来要访问的全局资源外，还一共需要一把锁、两个条件变量、读者数量、写者标记这些全局资源。

##### 读者视角

1、上锁，检查是否有写者，如果有，就 `wait` ，否则就继续向下，给读者数量加1，解锁。

2、读取全局资源

3、上锁，给读者数量减一，如果读者数量归零，就通知某个写者，解锁。

##### 写者视角

1、上锁，检查是否有读者或其他写者，如果有，就 `wait` ，否则置位写者标记

2、修改全局资源

3、清理写者标记，通知所有读者和某个其他写者，解锁。

#### 案例

读者-写者问题的代码：

```c
#include <pthread.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>

char g_src[] = "I once stood on the mountain top, looking down at the land";
int readnum = 0;
int writerflag = 0;
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t condr = PTHREAD_COND_INITIALIZER;
pthread_cond_t condw = PTHREAD_COND_INITIALIZER;


void* thread_reader(void* arg){
    pthread_mutex_lock(&mutex);
    while(writerflag){
        pthread_cond_wait(&condr, &mutex);
    }

    readnum++;
    pthread_mutex_unlock(&mutex);

    puts(g_src);
    sleep(1);

    pthread_mutex_lock(&mutex);
    readnum--;
    if(readnum == 0){
        pthread_cond_signal(&condw);
    }
    pthread_mutex_unlock(&mutex);
}

void* thread_writer(void* arg){
    pthread_mutex_lock(&mutex);
    while(writerflag || readnum){
        pthread_cond_wait(&condw, &mutex);
    }
    writerflag = 1;

    strcpy(g_src, "I only linger, offering vivid endless you.");
    sleep(1);

    writerflag = 0;
    pthread_cond_broadcast(&condr);
    pthread_cond_signal(&condw);
    pthread_mutex_unlock(&mutex);
}

int main(){
    pthread_t tid[7];
    int i;
    for(i = 0;i < 7;i++){
        if(i >= 0 && i <= 2){
            pthread_create(tid + i, NULL, thread_reader, NULL);
        }
        if(i == 3){
            pthread_create(tid + i, NULL, thread_writer, NULL);
            sleep(1);
        }
        if(i >= 4 && i < 7){
            pthread_create(tid + i, NULL, thread_reader, NULL);
        }
    }

    for(i = 0;i < 7;i++){
        pthread_join(tid[i], NULL);
    }

    pthread_mutex_destroy(&mutex);
    pthread_cond_destroy(&condr);
    pthread_cond_destroy(&condw);
    return 0;
}
```

### 线程池

线程池主要用于一些需要线程常驻从而避免频繁的申请释放的场景中。线程池在创建时会维护一些线程，并让这些线程处于 `wait` 的状态。一旦线程池收到任务，就会随机唤醒一条线程来执行任务，处理完所有任务后，又会重新回到 `wait` 状态。这样即避免了无谓的开销（创建销毁线程的开销），也提升了任务处理的速度。

线程池主要应用于**高并发、低延迟**的场景中，一个典型的应用场就是B/S架构，网页服务器可能面临着很大的访问压力，使用线程池既可以提高响应速度（降低延迟），又可以降低CPU的压力。

#### 线程池的运作原理

线程池在创建时，需要：

1、创建若干个线程，并让他们通过条件变量阻塞。

2、创建一个**任务队列**，并开放接口允许用户直接放入任务。

其运作逻辑是：

用户先向任务队列放入任务，此时线程池就随机唤醒一个线程来执行该任务，执行完毕后，该线程会检查任务队列中还有没有任务，如果没有了，就重新陷入阻塞，如果有，就继续处理，直到没有任务了。

在线程池销毁时，往往是通过给退出标记置位的方式让线程退出的，即先给退出标记置位，然后唤醒所有线程，让所有线程先后退出即可。

#### 线程池的实现（了解）

[线程池](./线程池.html)

# 网络

## OSI七层模型

OSI七层模型是1983年由国际标准化组织提出的一种网络通讯的理论模型，为当时的网络架构提供了理论基础。这七层分别是:
**物理层、数据链路层、网络层、传输层、会话层、表示层、应用层**

在平时讨论时，通常用简化版的TCP/IP五层/四层模型：

五层模型：**物理层、数据链路层、网络层、传输层、应用层**

四层模型：**网络接口层、网络层、传输层、应用层**

### 局域网与广域网

LAN（Local Area Network）即局域网，指的是在一个私有网络环境中的若干设备形成的网络，现在通常指由一个NAT网关管理起来的若干设备。

WAN（Wide Area Network）即广域网，通常指互联网。

现代网络概念中，还包含WLAN（无线局域网）、PAN（个域网）等，这些都是不同种类的名词。

### 物理层

物理层是直接和硬件设备沟通的层级，完成物理层转发的设备叫集线器，集线器转发数据时，会将数据直接广播到自己所有的端口上。

物理层在发送数据时要将数据封装为数据帧，方法是给数据打上帧头和帧尾，帧头中存放着源MAC地址和目标MAC地址来表示这个包是发给谁的，二真伪则存放一些校验数据，来检查这个包是否合法（这个数据帧是否有效）。封装好后，数据会通过硬件设备（网卡）发送出去。

#### MAC地址

MAC地址也叫物理地址，是联网设备的身份证号。它是一个6字节的数据，通常用 `:` 或者 `-` 隔开每个字节，每个字节用两位十六进制表示，如：`00:0c:29:de:09:ed` ，这个数据的前三个字节是厂商编码，后三个字节是设备编码。

### 数据链路层

数据链路层也叫链路层，完成数据链路层转发的设备叫**交换机**，交换机相较于集线器，其内置了一个MAC地址表，能完成物理端口对MAC地址的映射。

#### MAC地址表

交换机中，存在一张物理端口对MAC地址的映射表，这就是MAC地址表，当交换机收到包时，此时它就会去MAC地址表中查找目标MAC所对应的物理端口，然后只将该数据经过该端口转发，从而避免了群发。但如果没有查到该MAC地址，则依然会群发。所以一开始，交换机上MAC地址表还是空的时，每次转发也和集线器类似，之后逐渐完善后就是点对点的转发了。

MAC地址表中，允许一个物理端口号对应多个设备，但一个设备只能对应一个物理端口号。因此，如果交换机上的某个端口连接的是另一个交换机，那么由于交换机本身不具备MAC地址，所以相当于这个端口映射了所有连接到了另一个交换机上的所有设备，从而会导致该端口异常繁忙，最终导致信息拥塞。

而由于交换机还可以再连接交换机，这种状况下，MAC表会迅速膨胀，端口的信息拥塞也将进一步加剧，所以这一问题必须处理。

### 网络层

网络层可以完成基于IP地址的转发，完成网络层转发的设备叫**路由器**。

#### IP地址

IP地址的作用是给设备一个临时的入网编号。设备连入不同的网络环境时，其分配到的IP地址也不尽相同，通过IP地址，可以识别当前设备在当前网络中的临时位置。

IP地址分为IPv4地址和IPv6地址，目前使用的主流依然是IPv4地址。

##### ipv4地址

IPv4地址通常表示为一个点分十进制的字符串，如： `101.91.42.232` ，它由四个字节组成，用三个点隔开了四个部分，每个部分表示一个字节，所以其取值范围是 `[0,255]` 。

###### ipv4地址的分类

传统分类方法将IP地址分为A、B、C、D、E五类，这里先介绍前三种。

**网络号**：是用于区分两台机器是否处于同一个子网的手段，如果两个IP地址的网络号相同，就可以认为它们在同一个子网下。

**主机号**：用于区分同一个子网下的不同主机。

🌟网络号和主机号分别不能是全零或者全1。

A类地址：第一个字节是网络号，且第一位固定为0。其余三字节是主机号。

取值范围： `1.0.0.1 ~ 126.255.255.254`

B类地址：前两个字节是网络号，且前两位固定为10。其余两字节是主机号。

取值范围： `128.1.0.1 ~ 191.254.255.254`

C类地址：前三个字节是网络号，且前三位固定为110。其余一字节是主机号。

取值范围： `192.0.1.1 ~ 223.255.254.254`

现代IP地址已经不按这个方式分类了。

D类地址和E类地址依然在使用，表示多点广播和保留地址。

###### 子网掩码

子网掩码代替了原来的ABC类地址，可以自由的决定网络号的位数，这样就更加精细、灵活了。

子网掩码有两种表示形式：

1、直接用点分十进制表示，如：`255.255.255.128` ，这个IP中前25位是1，后7位是0。表示25位子网掩码。

2、直接在IP地址后做标记，如：`192.168.103.143/25`，这表示25位子网掩码。

计算一个子网中最多能拥有多少台主机：

主机数量的公式为：
$$
2^{主机号位数} - 2
$$

其中，主机号的位数就是 `32 - 子网掩码位数` 。

如25位子网掩码下，主机的最大数量是 $2^7-2=126$

在实际操作中，判断两个IP在不在一个网段的方法是让这两个IP地址分别和子网掩码进行位与运算，如果两者结果一致，则表示在同一个子网中，否则表示不在一个子网中。如果在，则直接经过交换机转发，而如果不在就要交给路由器了。路由器的IP地址一般通过默认网关进行配置。

##### DNS（了解）

DNS（Domain Name Server 域名服务器）是我们平时访问网页时必须要经过的服务器，它可以将域名解析成对应的IP地址以供正常访问。IP地址一来不好记，二来并不安全，所以网页通常会注册域名。域名注册后，用户访问域名时，就要通过DNS将域名转换为IP地址再进行访问。

##### NAT网关（了解）

NAT技术可以让若干设备存在于一个内网当中，这些设备如果要访问公网，则需要通过NAT网关。而这些设备上的IP地址实际上是由NAT网关分配的，这些私有IP是不能出现的公网上的，所以在这些设备访问公网时，NAT会将其转换为一个公网IP（即NAT网关的IP地址）。

一共有三个网段被划分给了内网IP：

A类内网IP： `10.0.0.0/8`

B类内网IP： `172.16.0.0/12`

C类内网IP： `192.168.0.0/16`

可看出即使是C类内网IP，其可以支持的主机数量依然高达 `65534` ，所以平时C类见得最多。

###### DHCP

DHCP技术可以动态分配IP地址，即接入网络后，由DHCP分配一个可用的随机地址给到对应设备，当设备脱离网络后，IP地址就回收了。这样的话，只要同时接入的设备不超过 `65534` ，则C类内网IP就足够用。

##### ipv6地址

IPv6地址是为了应对IPv4地址耗尽，由我国主导研发的新一代网络协议。其中，IPv6地址拥有16个字节（128位），即总数量达到了 $2^{128}$ ，约为 $3.4 * 10^{38}$ 个。其通常用冒分十六进制表示，用7个冒号分开8个4位十六进制字段来表示，每个字段表示两字节，如：`240e:454:810b:30c:1171:7f21:5db3:47c4`

### 传输层

到了这一层就是纯软件层了，所以传输层可以转发大型数据，这一层中也抽象出了一个“端口号”的概念，这是一种软端口号，这个端口号就是我们在访问服务器时，经常谈到的端口号。它不是真正的物理端口，而是软件层面模拟出的端口。

#### 端口号

端口号是一个两字节的数据，所以其取值范围是[0,65535]。一般而言，100以下的端口号都是不能用的，10000以下的慎用，因为这个区域内存在大量的**常用端口号**。常用端口号指的是已经被常见协议或常见应用所占用的端口号，重复使用很容易出问题。

除此之外，50000以上的端口号通常用户系统自动分配，所以也不要使用，有可能会被占用，所以在使用时，最好可以先把自己打算使用的端口号喂给AI，看看有没有撞车的情况。

一个端口号只能由一个使用者绑定，所以一旦被占用就无法在被其他应用绑定了。

#### UDP

UDP协议是一个不基于连接的协议，它在传输数据时，只会保证数据包的发送，至于到底有没有到达，这个协议并不关心，所以其发送数据时容易丢包。其丢包率和发送的速度呈正比，和网络状况呈反比（速度越快，丢包率越高，网络越差，丢包率越高）

UDP主要适用于对数据的实时性要求较高，但对数据的安全性要求不高的场景。

#### TCP

TCP协议是一个**基于连接**的协议，它在传输数据时，会保证数据的可靠性，在发送前，双方要先建立连接，在数据发送完成后，双方需要断开连接。

TCP在建立连接时会进行三次握手并创建两条信道，以保证双方可以全双工通讯。而在断开时会通过四次挥手断开两条信道。

##### 三次握手

1、客户端向服务器发起连接请求，方法是向服务器发送一个SYN包。

2、服务器响应客户端的连接请求，方法是回复一个ACK+SYN包。（此时客户端到服务器的信道已经建立）

3、客户端响应服务器的连接请求，方法是回复一个ACK包。（此时服务器到客户端的信道已经建立）

##### 四次挥手

1、客户端向服务器发起断开请求，方法是向服务器发送一个FIN包。

2、服务器响应客户端的断开请求，方法是回复一个ACK包。

🌟客户端在发送FIN包前，会先确认数据已经传输完毕了，所以当服务器收到这个FIN包时，**客户端到服务器的信道**就可以关闭了。但服务器到客户端的信道中很可能还有数据，所以服务器会先回ACK包，再去检查信道中是否还有数据，确认无误后才会继续进行后两步。这就是为什么不能直接回复ACK+FIN包。

1、服务器向客户端发起断开请求，方法是向客户端发送一个FIN包。

2、客户端响应服务器的断开请求，方法是回复一个ACK包。

此时两条信道全部断开，连接断开。

### 会话层和表示层

这两层被TCP/IP协议簇合并到了应用层中，所以一般不单独讨论。

### 应用层

应用层就百花齐放了，不同的应用程序有不同的解决方案，其中包含一些常见的协议，如 `ssh` 、`http/https` 、`ftp` 等。应用层会先通过应用层协议将数据处理好后交给传输层处理。

## 数据包的封装与分用

当一个数据从应用层产生时（`| DATA |`），会经历一个封装的过程： 

1、应用层会对这个数据做一定的处理（如加密等），处理后会打上应用层的包头，这里以 `https` 协议为例，就会产生这样一个包：
`| HTTP | 加密的DATA |` ，随后将包交给传输层。 

2、传输层拿到包后，会先打上传输层的包头：` | TCP | HTTP | 加密的DATA |` ，然后将包丢给网络层。 

3、网络层同样会这样处理：`| IP | TCP | HTTP | 加密的DATA |` ，然后将包丢给物理层。 

4、物理层打上帧头帧尾：`|帧头| IP | TCP | HTTP | 加密的DATA |帧尾|` ，然后将数据帧交给硬件设备。 

帧封装完毕后，硬件设备会将其调制为电信号/光信号/无线电信号发射出去。 

接收方接收到信号后，会先将信号解调成原来的数据帧，随后交给物理层。 

1、物理层先通过帧头帧尾确定帧信息是否可靠，如果可靠，则去掉帧头帧尾（`| IP | TCP | HTTP | 加密的DATA |`），并观察当前包头，将其交给对应的网络层协议处理。 

2、网络层拿到数据后，处理完毕后，便去掉网络层的包头（`| TCP | HTTP | 加密的DATA |`），同时观察传输层的包头，交给对应协议处理。 

3、传输层也是同样的处理方式（`| HTTP | 加密的DATA |`），最终将数据交给对应的应用程序。 

4、应用程序还原数据（`| DATA |`）并展示给用户。

## 字节序

在计算机领域中，通过内存中的多个字节存储一个数据时，方法并不统一，这种多字节存储数据时，字节的顺序叫做字节序。

### 小端序（主机字节序）

在绝大多数编程语言的编译器/解释器中，存储数据时采用的是小端序。小端序的特点是：**数据的低位存在内存的低地址上**，所以在肉眼阅读内存时，看到的是一个按照字节顺序倒过来的结果。小端序主要出现在主机应用程序中，所以也叫主机字节序。

用如下代码可以检查字节序：

```c
#include <stdio.h>

int main() {
	int x = 0x12345678;
	char* ch = (char*)&x;
	printf("%02x %02x %02x %02x\n", ch[0], ch[1], ch[2], ch[3]);
	return 0;
}
```

对于小端序而言，代码的输出结果是：

```
78 56 34 12
```

### 大端序（网络字节序）

在网络传输中，各种数据（如IP地址和端口号等）在传输时都采用的是大端序。大端序的特点是：**数据的低位存在内存的高地址上**，所以如果是大端序，则同样的代码，输出就会变成：

```
12 34 56 78
```

由于这种字节序通常用于网络，所以也叫网络字节序。

### Linux中的字节序相关函数

Linux中提供了字节序转换函数：

#### htonl / htons / ntohl / ntohs

完成字节序转换的，头文件及函数声明：

```c
#include <netinet/in.h>
unsigned long htonl(unsigned long host);
unsigned long htons(unsigned short host);
unsigned long ntohl(unsigned long network);
unsigned long ntohs(unsigned short network);
```

这个四个函数的函数名本身就说明了它的作用：

`h` 表示主机字节序（host）

`n` 表示网络字节序（network）

`1` 表示转换4字节（long），即IPv4地址

`s` 表示转换2字节（short），即端口号

所以 `htons` 就是 `host to network short` 的意思。即将2字节数据（端口号）由主机字节序转换为网络字节序。

#### inet_addr

把一个点分十进制的字符串转换为一个32位的网络字节序数据。头文件及函数声明：

```c
#include <arpa/inet.h>
unsigned long inet_addr(const char* ip);
```

传入点分十进制的字符串，返回转换结果。失败照旧。

🌟这个函数的问题在于 `-1` 可以是一个IP地址，所以这个函数没法报错。这也是平时使用中该函数使用较少的原因。

#### inet_aton / inet_pton

把一个点分十进制的字符串转换为一个32位的网络字节序数据。头文件及函数声明：

```c
#include <arpa/inet.h>
int inet_aton(const char* ip, struct in_addr* addr);
int inet_pton(int domain, const char* ip, void* addr);
```

参数列表

1、`pton` 参数1：协议类型，如果是IPV4则传入 `AF_INET` ，如果是IPV6则传入 `AF_INET6` 。

2、`pton` 参数2/ `aton` 参数1：点分十进制的字符串

3、`pton` 参数3/ `aton` 参数2：得到的转换结果。其中 `pton` 可以是IPv6的结果。

返回值照旧。

## Linux中的socket

### TCP及相关函数

TCP在通讯时主要采用的是C/S架构，所以讨论TCP的函数前，需要先理清服务器和客户端各自做了什么事以建立连接的。

![TCP](D:\作业\1-笔记\Linux.assets\TCP.png)

#### socket

创建socket文件描述符，头文件和函数声明：

```c
#include <sys/socket.h>
int socket(int domain, int type, int protocol);
```

**参数列表**

1、三层相关协议类型：IPv4则传入 `AF_INET` ，IPv4则传入 `AF_INET6` 。

2、四层相关协议类型：UDP则传入 `SOCK_DGRAM` ，TCP则传入 `SOCK_STREAM` 。

3、协议编号，通常给0。

正常返回socket文件描述符，失败照旧。

#### bind

用于服务器，给socket文件描述符绑定一个准入标准，即什么样的IP和端口号是可以响应的，如果不满足条件则直接拒绝连接。头文件及函数声明：

```c
#include <sys/socket.h>
#include <netinet/in.h>
int bind(int sockfd, const struct sockaddr* addr, int socklen);
```

**参数列表**

1、socket文件描述符

2、准入规则需要的IP和端口号，可以适配不同的类型。其中IPv4下的结构通常为 `struct sockaddr_in` ，即定义一个这样的结构，然后强转成参数类型的指针传入即可。如：

```c
struct sockaddr_in addr; //IPv4的结构
addr.sin_family = AF_INET; //使用IPv4协议
addr.sin_port = htons(10086); //设置准入端口号10086
addr.sin_addr.s_addr = INADDR_ANY; //准入任意IP

bind(sockfd, (struct sockaddr*)&addr, sizeof(addr));
```

3、参数2的长度。

返回值照旧。

🌟客户端填写服务器的IP地址和端口号时，也需要填写 `struct sockaddr_in` 结构，两边的填写思路差不多，只是客户端填写时要指定服务器的IP。

#### listen

用于服务器，激活socket连接并监听连接，头文件及函数声明：

```c
#include <sys/socket.h>
int listen(int sockfd, int backlog);
```

参数2是准入的最大连接数（一般不超过128），返回值照旧。

#### counect

用于客户端，向服务器发起连接请求。头文件：

```c
#include <sys/socket.h>
int connect(int sockfd, const struct sockaddr* addr, int socklen);
```

这个函数的参数和 `bind` 完全一致，不同的是填写IP时，要填写对应的服务器的IP地址，如：

```c
addr.sin_addr.s_addr = inet_addr("127.0.0.1");
```

```c
inet_aton("127.0.0.1", &addr.sin_addr);
```

返回值照旧。

#### accept

用于服务器，接收连接请求。它是一个阻塞函数，如果没有客户端发起连接请求就一直阻塞。头文件及函数声明：

```c
#include <sys/socket.h>
int accept(int sockfd, struct sockaddr* addr, int* scoklen);
```

这个函数的参数和 `connect` 的一一对应，后两个参数成了出参，表示接收 `connect` 传来的内容。

**返回值**

成功则得到一个和指定客户端通讯的socket（和之前创建的socket不同，这个socket是用于和客户端通讯的），失败照旧。

#### 示例

用TCP完成一个C/S架构的程序，让客户端和服务器完成一次数据交互。

`TCP.h`

```c
#ifndef _TCP_H_
#define _TCP_H_

#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>

#define IP "127.0.0.1"
#define PORT 10086

#endif //_TCP_H_
```

`server.c`

```c
#include "TCP.h"
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main(){
    int serverfd = socket(AF_INET, SOCK_STREAM, 0);
    if(serverfd < 0){
        perror("socket error ");
        return 1;
    }

    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(PORT);
    addr.sin_addr.s_addr = INADDR_ANY;

    int ret = bind(serverfd, (struct sockaddr*)&addr, sizeof(addr));
    if(ret < 0){
        perror("bind error ");
        return 1;
    }

    ret = listen(serverfd, 16);
    if(ret < 0){
        perror("listen error ");
    }

    int len;
    int clientfd = accept(serverfd, (struct sockaddr*)&addr, &len);
    if(clientfd < 0){
        perror("accept error ");
        return 1;
    }

    char buf[1024];
    read(clientfd, buf, 1024);
    printf("芳: %s\n", buf);
    memset(buf, 0, sizeof(buf));
    strcpy(buf, "天空没有极限");
    write(clientfd, buf, 1024);

    close(clientfd);
    close(serverfd);

    return 0;
}
```

`client.c`

```c
#include "TCP.h"
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main(){
    int clientfd = socket(AF_INET, SOCK_STREAM, 0);
    if(clientfd < 0){
        perror("socket error ");
        return 1;
    }

    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(PORT);
    inet_aton(IP, &addr.sin_addr);

    int ret = connect(clientfd, (struct sockaddr*)&addr, sizeof(addr));
    if(ret < 0){
        perror("connect error ");
        return 1;
    }

    char buf[1024] = "启示录";
    write(clientfd, buf, 1024);

    memset(buf, 0, sizeof(buf));
    read(clientfd, buf, 1024);
    printf("白: %s\n", buf);

    close(clientfd);

    return 0;
}
```

**完成客户端和服务器间的全双工通讯**

`TCP.h`

```c
#ifndef _TCP_H_
#define _TCP_H_

#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>

#define IP "127.0.0.1"
#define PORT 10086
#define BUFSIZE 1024

#endif //_TCP_H_
```

`server.c`

```c
#include "TCP.h"
#include <unistd.h>
#include <stdio.h>
#include <string.h>
#include <pthread.h>

void* thread_read(void* arg){
    int sockfd = (long)arg;
    char buf[BUFSIZE];
    while(1){
        memset(buf, 0, sizeof(buf));
        read(sockfd, buf, BUFSIZE);
        if(strcmp(buf, "exit") == 0){
            return NULL;
        }
        printf("client : %s\n", buf);
    }
}

void* thread_write(void* arg){
    int sockfd = (long)arg;
    char buf[BUFSIZE];
    while(1){
        memset(buf, 0, sizeof(buf));
        scanf("%1023[^\n]%*c", buf);
        write(sockfd, buf, BUFSIZE);
        if(strcmp(buf, "exit") == 0){
            return NULL;
        }
    }
}

int main(){
    int serverfd = socket(AF_INET, SOCK_STREAM, 0);
    if(serverfd < 0){
        perror("socket error ");
        return 1;
    }

    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(PORT);
    addr.sin_addr.s_addr = INADDR_ANY;

    int ret = bind(serverfd, (struct sockaddr*)&addr, sizeof(addr));
    if(ret < 0){
        perror("bind error ");
        return 1;
    }

    ret = listen(serverfd, 16);
    if(ret < 0){
        perror("listen error ");
    }

    int len;
    int clientfd = accept(serverfd, (struct sockaddr*)&addr, &len);
    if(clientfd < 0){
        perror("accept error ");
        return 1;
    }

    pthread_t tid1, tid2;
    pthread_create(&tid1, NULL, thread_read, (void*)(long)clientfd);
    pthread_create(&tid2, NULL, thread_write, (void*)(long)clientfd);

    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);

    close(clientfd);
    close(serverfd);

    return 0;
}
```

`client.c`

```c
#include "TCP.h"
#include <unistd.h>
#include <stdio.h>
#include <string.h>
#include <pthread.h>

void* thread_read(void* arg){
    int sockfd = (long)arg;
    char buf[BUFSIZE];
    while(1){
        memset(buf, 0, sizeof(buf));
        read(sockfd, buf, BUFSIZE);
        if(strcmp(buf, "exit") == 0){
            return NULL;
        }
        printf("server : %s\n", buf);
    }
}

void* thread_write(void* arg){
    int sockfd = (long)arg;
    char buf[BUFSIZE];
    while(1){
        memset(buf, 0, sizeof(buf));
        scanf("%1023[^\n]%*c", buf);
        write(sockfd, buf, BUFSIZE);
        if(strcmp(buf, "exit") == 0){
            return NULL;
        }
    }
}

int main(){
    int clientfd = socket(AF_INET, SOCK_STREAM, 0);
    if(clientfd < 0){
        perror("socket error ");
        return 1;
    }

    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(PORT);
    inet_aton(IP, &addr.sin_addr);

    int ret = connect(clientfd, (struct sockaddr*)&addr, sizeof(addr));
    if(ret < 0){
        perror("bind error ");
        return 1;
    }

    pthread_t tid1, tid2;
    pthread_create(&tid1, NULL, thread_read, (void*)(long)clientfd);
    pthread_create(&tid2, NULL, thread_write, (void*)(long)clientfd);

    pthread_join(tid1, NULL);
    pthread_join(tid2, NULL);

    close(clientfd);

    return 0;
}
```

**服务器的循环响应框架**

`server.c` 中 `accept` 的部分

```c
    int len;
    while(1){
        int clientfd = accept(serverfd, (struct sockaddr*)&addr, &len);
        if(clientfd < 0){
            perror("accept error ");
            return 1;
        }

        char buf[BUFSIZE] = "欢迎光临，你好，几位";
        write(clientfd, buf, BUFSIZE);

        close(clientfd);
    }
    close(serverfd);
```

`client.c` 中 `connect` 的部分

```c
	int ret = connect(clientfd, (struct sockaddr*)&addr, sizeof(addr));
    if(ret < 0){
        perror("bind error ");
        return 1;
    }

    char buf[BUFSIZE];
    read(clientfd, buf, BUFSIZE);
    puts(buf);

    close(clientfd);
```

#### recv / send

在TCP协议中接收、发送数据。头文件及函数声明：

```c
#include <sys/socket.h>
int recv(int sockfd,       void* buf, size_t len, int flag);
int send(int sockfd, const void* buf, size_t len, int flag);
```

这两个函数本质上就是 `read/write` ，只是多了第四个参数，这个参数在TCP首发数据时可以传入一些特殊标记，但通常给0，在不需要这个参数时，直接使用 `read/write` 即可。返回值也和 `read/write` 一致。

### UDP相关函数

UDP中，由于不需要建立连接，所以只需要创建文件描述符即可。之后服务器需要绑定准入规则，大家就可以开始通讯了。由于收发没有任何限制，所以是天然的全双工手段。通过聊天软件聊天时，服务器就是通过UDP完成用户间数据转发的。

#### recvfrom / sendto

在UDP协议中接收、发送数据。头文件及函数声明：

```c
#include <sys/socket.h>
int recvfrom(int sockfd,       void* buf, size_t len, int flag,
			struct sockaddr* addr, int* socklen);
int sendto  (int sockfd, const void* buf, size_t len, int flag,
			const struct sockaddr* addr, int socklen);
```

前四个参数是 `recv/send` 的参数，后两个参数是 `accept/connect` 的参数。返回值同 `recv/send`

#### 案例

通过UDP，完成一个信息收纳打印的服务器

`UDP.h`

```c
#ifndef _UDP_H_
#define _UDP_H_

#include <sys/socket.h>
#include <netinet/in.h>
#include <arpa/inet.h>

#define IP "127.0.0.1"
#define PORT 10086
#define BUFSIZE 1024

#endif //_UDP_H_
```

`server.c`

```c
#include "UDP.h"
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main(){
    int sockfd = socket(AF_INET, SOCK_DGRAM, 0);
    if(sockfd < 0){
        perror("socket error ");
        return 1;
    }

    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(PORT);
    addr.sin_addr.s_addr = INADDR_ANY;

    int ret = bind(sockfd, (struct sockaddr*)&addr, sizeof(addr));
    if(ret < 0){
        perror("bind error ");
        return 1;
    }

    char buf[BUFSIZE];
    int len;
    while(1){
        memset(buf, 0, sizeof(buf));
        recvfrom(sockfd, buf, BUFSIZE, 0, (struct sockaddr*)&addr, &len);
        if(strcmp(buf, "exit") == 0){
            break;
        }
        puts(buf);
    }
    close(sockfd);
    return 0;
}
```

`client.c`

```c
#include "UDP.h"
#include <unistd.h>
#include <stdio.h>
#include <string.h>

int main(){
    int sockfd = socket(AF_INET, SOCK_DGRAM, 0);
    if(sockfd < 0){
        perror("socket error ");
        return 1;
    }

    struct sockaddr_in addr;
    addr.sin_family = AF_INET;
    addr.sin_port = htons(PORT);
    inet_aton(IP, &addr.sin_addr);

    char buf[BUFSIZE];
    int len;
    while(1){
        memset(buf, 0, sizeof(buf));
        scanf("%1023[^\n]%*c", buf);
        sendto(sockfd, buf, BUFSIZE, 0, (struct sockaddr*)&addr, sizeof(addr));
        if(strcmp(buf, "exit") == 0){
            break;
        }
    }
    close(sockfd);
    return 0;
}
```

# 数据库

🌟本节讨论的数据库特指关系型数据库

## 数据库基础

数据库是一种管理数据的底层应用程序，其可以在磁盘（外存储器）上进行大规模的数据管理。在管理时，数据库会通过一些数据结构和算法保证其插入、删除、查找的时间复杂度。数据库的存在主要为了解决以下两个问题：

1、数据量十分庞大，内存根本无法完成对数据的管理，只能通过磁盘/磁盘阵列管理。

2、对数据的增删查改的速度有一定要求，且数据量相较于当前设备的内存过大，不适合使用内存存储。

数据库存储数据时，遵循**库-表二级结构**。即数据库是通过库来管理不同的数据仓库的，而每个库中存在若干数据表来管理具体数据。以 `xls` 文件打比方，一个文件就是一个库，一个 `sheet` 就是一个数据表。

## 主流数据库

`SQL Server` ：微软的数据库，在 `.net` 开发中比较常见，使用方便，可以配合 `Office Access` 使用。

`MySQL` ：开源数据库，目前被 `Oracle` 收购，并发性好，易上手，常用于个人用户和小型公司，也适合新手上路。

`Oracle` ： `Oracle` 公司开发的商用数据库，是目前最主流的数据库之一，适合大型项目及复杂的业务逻辑。

`SQLite` ：轻量级的数据库，主要用在嵌入式设备上，易于安装、使用，大小通常只有MB级甚至更小。

## 数据库的基础知识

采用 `MySQL` 的分支 `MariaDB` 完成。安装语句：

```bash
sudo apt-get install mariadb-server
```

相关开发库的安装：

```bash
sudo apt-get install libmariadb-dev libmariadbclient-dev
```

安装完毕后，需要打开数据库服务器：

```bash
sudo systemctl start mariadb
```

之后就可以打开客户端连接服务器了：

```bash
sudo mysql -uroot -p
```

第一次登录时要设置密码，留空即可。

在 `MariaDB` 中，存在一个数据库服务器，所有的库都是放在这个服务器中的。可以用某个用户的身份登录一个客户端来访问服务器。这里用的是 `root` 用户的身份。 `-p` 参数是输密码的，这里后面留空即可。

## SQL语句

### SQL语句的分类

`DDL` 数据定义语言，主要用于维护存储结构。其主要是对数据库和数据表做宏观的增删查改，包括 `CREATE DROP ALTER` 等

`DML` 数据操作语言，主要用于维护数据。其主要是对数据表中的具体数据做增删查改，包括 `INSERT DELETE UPDATE` 等

> `DQL` 数据查找语言，是 `DML` 语句的子集，主要包括各种各样的 `SELECT`

`DCL` 数据控制语言，主要负责用户管理、权限管理等数据库的配置工作，包括 `REVOKE COMMIT` 等

### 库、表的增删差改（DDL）

🌟SQL语句中默认 `;` 作为语句的结尾（这个结尾可以修改，默认也能使用 `\G` ）。敲入时，如果漏掉了分号，那么会认为你的输入还没结束，在下一行补上即可。

🌟SQL中的关键字不区分大小写，所以接下来的笔记中，关键字会保持大写。这也是SQL脚本编写时的常见写法。

#### 添加数据库

基本语句逻辑：

```mysql
CREATE DATABASE [IF NOT EXISTS] `db_name` [配置项]
```

这条命令就可以创建一个名为 `db_name` 的库。

##### 命令语法

之后在介绍SQL语句逻辑时，会用到命令语法，其中只包含三类字符:

`[]` 该字段为可选字段

`|` 多个字段选其一

`{}` 该字段为必选字段，通常和 `|` 一起使用。

##### 反引号

在SQL语句中，关键字非常的多，而且不区分大小写，所以标识符很容易就和关键字撞车了。故在SQL中需要一种即使和关键字撞车了，也不影响使用的机制。用反引号引起来的字段不会检查其是否和关键字撞车，一定可以成库名/表名/字段名。

##### IF NOT EXISTS

原语句中，如果数据库不存在，则可以直接创建数据库，如果数据已存在，那么如果不加这个字段，创建会失败，如果加了，则不会报错，只会给出警告。

##### 配置项

在数据库的后面通常有三种常见配置项，即字符集、校验规则、引擎，这里不做展开。

在创建时，如果不指定，则默认字符集为 `utf8mb4` ，默认校验规则为 `utf8mb4_general_ci`，默认引擎为 `InnoDB` 。

#### 删除数据库

基本语句逻辑：

```mysql
DROP DATABASE [IF EXISTS] `db_name`;
```

这条命令就可以删除名为 `db_name` 的库。

#### 修改数据库

基本语句逻辑：

```mysql
ALTER DATABASE `db_name` [配置项];
```

这条命令可以修改 `db_name` 的属性，如字符集、引擎等：

```mysql
ALTER DATABASE fang CHARSET=gbk;
```

#### 查询数据库情况

查询已有的所有数据库的库名：

```mysql
SHOW DATABASES;
```

查询已有的某个数据库的创建语句：

```mysql
SHOW CREATE DATABASE `db_name`;
```

#### 切换数据库

🌟这个命令是 `MySQL` 提供的

```mysql
USE `db_name`; 
```

如下数据表的操作语句如果要生效，则必须要先切换到某个数据库上，此时所有的表操作都是在当前使用的数据库上做操作的。

#### 添加数据表

基本语句逻辑：

```mysql
CREATE TABLE [IF NOT EXISTS] `tb_name` (
	`co1_name` 数据类型 [约束]
	[, `co1_name` 数据类型 [约束] [, ...]]
    
	[, 索引信息]
) [配置项];
```

这种写法类似结构体，里面要放很多字段（列），字段间用逗号隔开，每一列都可以单独指定类型和约束。最后的配置项如果不给，则会沿用当前库的配置项，如果给了，则直接使用设定的配置项。

如下是一个学生表的添加语句：

```mysql
CREATE TABLE `student`(
	`id` INT UNSIGNED PRIMARY KEY AUTO_INCREMENT COMMENT '学号',
    `name` VARCHAR(128) NOT NULL COMMENT '姓名',
    `math` FLOAT(4,1) UNSIGNED DEFAULT 0.0 COMMENT '数学',
    `chinese` FLOAT(4,1) UNSIGNED DEFAULT 0.0 COMMENT '数学',
    `english` FLOAT(4,1) UNSIGNED DEFAULT 0.0 COMMENT '英语'
);
```

#### 删除数据表

基本语句逻辑：

```mysql
DROP TABLE [IF EXISTS] `tb_name`;
```

#### 修改数据表的字段信息

基本语句逻辑：

```mysql
ALTER TABLE `tb_name` { ADD | DROP | MODIFY } COLUMN `col_name` [类型 [约束]]
[FIRST | AFTER `col_name`];
```

如：

```mysql
--在name列后插入class列
ALTER TABLE `student` ADD COLUMN `class` INT UNSIGNED AFTER `name`;
--给class列加上非空约束
ALTER TABLE `student` MODIFY COLUMN `class` INT UNSIGNED NOT NULL;
--删除class列
ALTER TABLE `student` DROP COLUMN `class`;
```

#### 查询数据表的信息

查询当前库中的所有表：

```mysql
SHOW TABLES;
```

查询某张表的创建语句：

```mysql
SHOW CREATE TABLE `tb_name`;
```

查询某张表的列信息：

```mysql
DESC `tb_name`;
```

### 数据的增删改查（DML）

#### 添加数据

基本语句逻辑：

```mysql
INSERT [INTO] `tb_name` [(`col_name` [, `col_name` [, ...]])] 
VALUES (数据列表)[, (数据列表)[, ...]];
```

可以通过这个语句向表中插入一组或者多组数据，如果表名后有列名列表，则后面的数据列表只需要和其一一对应即可。而如果没有列名列表，则数据列表必须要按照表的字段顺序给出每一列的值。如：

```mysql
--全列插入
INSERT `student` VALUES
(1, '一架老爷车', 78, 56, 97),
(2, '伊莉雅', 97, 68, 77),
(3, '萝卜大元帅', 85, 61, 45),
(4, '一分流水', 48, 55, 71);

--列名列表插入
INSERT `student`(`name`, `math`, `chinese`, `english`) VALUES
('菲菲十三格', 49, 66, 37),
('何人辛', 87, 66, 96),
('小度熊', 97, 96, 89),
('金陵十三叔', 87, 46, 47);
```

#### 查找数据

基本语句逻辑：

```mysql
SELECT [DISTINCT] {* | `tb_name`[(`col_name` [, `col_name` [,...]])]}
	[FROM `tb_name`]
	[WHERE 逻辑表达式/比较表达式]
	[ORDER BY `col_name` [ASC | DESC]]
	[LIMIT {N[,M] | M OFFSET N}];
```

##### DISTINCT

对列出的数据进行查重。查重时，需要结果集中所有列的数据都相同才认为是重复数据，只要有任意一列不同，就是不同的数据，不会查重。

##### 查询范围

这里使用 `*` 表示全列查询，也可以放一个列名列表，表示查询某些列，甚至可以在这里做运算。如：

```mysql
SELECT `name`, `math`, `chinese`, `english`, `math` + `chinese` + `english` 
FROM `student`;
```

但这样一来总分列名字太长，可以用 `AS` 起别名，`AS` 可以省略：

```mysql
SELECT `name`, `math`, `chinese`, `english`, `math` + `chinese` + `english` 
AS total FROM `student`;
```

##### FROM子句

这个子句表示数据来源，可以是一个或多个表名，如果没有来源，这个子句可以省略。

##### WHERE子句

`WHERE` 后可以对要查询的数据做出一定的筛选，筛选时需要用到一些比较运算符和逻辑运算符。

###### 比较运算符

`> < >= <=` ：正常比较

`=` ：等于

`!= <>` ：不等于

`<=>` ：`NULL` 安全的等于

`IS NULL` ：判断这个值是否为 `NULL`

`IS NOT NULL` ：判断这个值是否不为 `NULL`

###### 逻辑运算符

`AND` ：逻辑与

`OR` ：逻辑或

`NOT` ：逻辑非

###### 特殊的判断方式

`BETWEEN n1 AND n2` ：判断某个字段的数值是否在 `[n1, n2]` 的范围内。

`IN` ：后跟一个集合，判断该值是否在集合中出现过

`LIKE` ：字符串的模糊匹配，包含两个通配符： `%` 表示任意多个字符串，用 `_` 表示任意一个字符，如：

```mysql
SELECT * FROM `student` WHERE `name` LIKE '%十三%';  --找名字里带“十三”的
SELECT * FROM `student` WHERE `name` LIKE '一%'; 	--找以“一”开头的名字
SELECT * FROM `student` WHERE `name` LIKE '一___'; 	--找以“一”开头的四字名
```

##### ORDER BY子句

这个子句可以根据某列进行排序，被排序的列可以是生成的列。排序时，默认采用升序，如果要排降序，需要加 `DESC` 。

```mysql
-- 按照数学分数降序排序
SELECT * FROM `student` ORDER BY `math` DESC;
-- 按照数学分数降序排序，如果数学分数一样，就按照语文降序排序
SELECT * FROM `student` ORDER BY `math` DESC, `chinese` DESC;
-- 按照总分降序排序
SELECT `name`, `math`, `chinese`, `english`, `math` + `chinese` + `english` 
AS `total` FROM `student` ORDER BY `total` DESC;
```

##### LIMIT子句

在实际查询中，查询出的结果集可能会非常庞大，可以用 `LIMIT` 子句筛选要显示的行数（分页显示）。如：

```mysql
-- 显示前三行
SELECT * FROM `student` LIMIT 3;
-- 从第四行后显示三行
SELECT * FROM `student` LIMIT 4,3;
SELECT * FROM `student` LIMIT 3 OFFSET 4;
```

#### 修改数据

基本语句逻辑：

```mysql
UPDATE `tb_name` SET `col_name` = 值 [, `col_name` = 值 [,...]]
	[WHERE 逻辑表达式/比较表达式]
	[ORDER BY `col_name` [ASC | DESC]]
	[LIMIT {N[,M] | M OFFSET N}];
```

这个语句的前半部分表示要修改表中某些列的值，后面子句用来筛选要改的行。通过这样的方式来唯一确定要修改的格子。如：

```mysql
-- 将学号是1 2 3的学生分到10班
UPDATE `student` SET `class` = 10 WHERE `id` < 4;
-- 将其他学生分到20班
UPDATE `student` SET `class` = 20 WHERE `class` IS NULL;
-- 将成绩最好的三个人分到30班
UPDATE `student` SET `class` = 30 ORDER BY `math` + `chinese` + `english` DESC
LIMIT 3;
-- 给所有人的语文成绩加3分
UPDATE `student` SET `chinese` = `chinese` + 3;
```

#### 删除数据

基本语句逻辑：

```mysql
DELETE FROM `tb_name`
	[WHERE 逻辑表达式/比较表达式]
	[ORDER BY `col_name` [ASC | DESC]]
	[LIMIT {N[,M] | M OFFSET N}];
```

删除满足条件的行，如：

```mysql
DELETE FROM `student` WHERE `class` = 10; -- 删除10班所有人
DELETE FROM `student`; -- 删除所有人
```

🌟这种删除不会影响到 `AUTO_INCREMENT` 的自增，新增的学生的学号依然会继续增长。如果不希望保留，则可以使用其他的语句：

##### 表截断

基本语句逻辑：

```mysql
TRUNCATE `tb_name`;
```

这个语句相当于将表删了重建，所有的数据都直接丢弃了。

## SQL中的数据类型

### 整数类型

```mysql
BIT[(M)] -- 位类型，M表示指定的位数，不写则为1，取值范围[1, 64]
BOOL --布尔类型
TINYINT [UNSIGNED]  -- 1字节整型
SMALLINT [UNSIGNED] -- 2字节整型
INT [UNSIGNED]  	-- 4字节整型
BIGINT [UNSIGNED]	-- 8字节整型
```

### 浮点数类型

```mysql
FLOAT[(M, D)] [UNSIGNED]  -- M表示有效数字位数，D表示小数位数
DOUBLE[(M,D)] [UNSIGNED] -- 这两种类型都遵循IEEE754标准
-- 假如M是6，D是2，则取值范围就是[-9999.99,9999,99]，如果加了UNSIGNED，就只有[0,9999.99]了。
DECIMAL(M,D) [UNSIGNED] -- 高精度浮点型
```

### 字符串类型

```mysql
CHAR(S)    -- 定长字符串类型，S表示字符串的长度，取值范围是[1,255]，其大小和字符集有关。
VARCHAR(S) -- 变长字符串类型，S表示字符串的最大长度，S的取值范围上限和字符集有关，最大不能超过65535字节。
		   -- 其中有3字节用来存长度，剩余的用来存储具体数据。于是：
-- 当字符集中单个字符的长度为1时，S的上限是65532，长度为2时，S的上限是32766，为3是21844，为4是16383
TEXT -- 大文本类型，没有上限
BLOB -- 二进制大文本类型，没有上限
```

### 时间类型

```mysql
DATE -- 日期，格式为年月日
TIME -- 时间，格式为时分秒
DATETIME  -- 日期时间，格式为年月日时分秒
TIMESTAMP -- 时间戳，表示为年月日时分秒
```

### 枚举类型

```mysql
ENUM(项1 [项2, [项3]]) -- 枚举类型，存储时是数值，打印时用对应字符串
```

比如：

```mysql
ENUM('男', '女') -- 存储时实际上是0和1，打印时就是这两项
```

## 表的约束

约束指的是对表中某一列的具体数据有一些要求，或者是对其进行了一定的说明。例如要求某列不能为空，某列的数值必须唯一等。在创建表时，约束要放在数据类型之后，它不是必选项。如果要给一个字段加多个约束，那么它们出现的先后次序通常没有严苛的要求，但有些约束有特别的要求。

### 非空约束

在创建表时，可以在某个字段后加上 `NOT NULL` 表示这一列中不能有数据的值为 `NULL` 。如：

```mysql
CREATE TABLE `student` (
    `name` VARCHAR(128) NOT NULL
);
```

#### 空属性

在SQL中， `NULL` 不是一个数值，而是一种状态，表示这个位置上的数据是不存在的。 `NULL` 在参与运算时，运算结果依然是 `NULL`

### 默认值

在创建表时，任何字段都可以给一个默认值，在插入数据时，如果不给这一列数据，则会填入默认值。如：

```mysql
CREATE TABLE `student` (
	`math` FLOAT(4,1) UNSIGNED DEFAULT 0.0
);
```

### 列描述

在字段后可以加一个描述，相当于注释，如：

```mysql
CREATE TABLE `student` (
	`math` FLOAT(4,1) UNSIGNED DEFAULT 0.0 COMMENT '数学成绩'
);
```

这个字段通常只能用 `SHOW CREATE TABLE` 语句查阅。

### 零填充

这个约束只能用在整数类型上，所以，它的位置必须紧贴整数类型，表示打印时要在数字前补0，如：

```mysql
CREATE TABLE `student`(
	`id` INT(8) UNSIGNED ZEROFILL
);
```

这个约束是对整型的补充说明，所以必须紧贴整型。

### 主键

一般而言，一张表中需要有一个能唯一确定某条信息的字段，这个字段就会被设置为主键。主键要求非空且唯一，一张表中只能有一个主键。

```mysql
CREATE TABLE `student`(
	`id` INT UNSIGNED PRIMARY KEY
);
```

#### 自增长

这个约束通常和主键一起出现，表示如果不给主键值，则主键会沿着当前的最大值自动向下增长。由于主键要求唯一，所以主键通常会上一个自增长约束，之后就不用再给了，让其自动向上增加。如：

```mysql
CREATE TABLE `student`(
	`id` INT UNSIGNED PRIMARY KEY AUTO_INCREMENT
);
```

### 唯一键

如果表中的某个字段需要具备唯一性，则可以加上唯一键约束，唯一键不限制该字段必须不能为 `NULL` ，且不会对 `NULL` 查重。即允许不给数值，但只要给了就必须唯一。如果加上了非空约束，也就和主键区别不大了。如：

```mysql
CREATE TABLE `student`(
	`id` INT UNSIGNED UNIQUE
);
```

### 主外键约束

如果主表中的一列数据要求必须从另一个表中获取，不可出现另一个表中没有的数据，则需要添加**主外键约束**，外键字段的值必须来自于主键列，如：

```mysql
ALTER TABLE dept ADD PRIMARY KEY(deptno);
ALTER TABLE emp ADD FOREIGN KEY(deptno) REFERENCES dept(deptno);
```

## 数据库的常用函数

### 日期时间相关函数

`CURRENT_DATE` ：当前的日期（年月日）

`CURRENT_TIME` ：当前的时间（时分秒）

`CURRENT_TIMESTAMP` ：当前的时间戳（年月日时分秒）

**`NOW` ：当前的时间戳（年月日时分秒）**

`DATE_ADD / DATE_SUB` ：可以传入一个日期和一个天使，计算向后/向前推这么多天后的日期。使用方式：

```mysql
SELECT DATE_ADD(CURRENT_DATE(), INTERVAL 1000 DAY);
```

`DATEDIFF` ：传入两个日期，计算差值

```mysql
SELECT DATEDIFF(CURRENT_DATE(), '2024-05-06');
```

### 字符串相关函数

`CHARSET` ：可以显示一个字符串的字符集

**`CONCAT` ：连接字符串，可以传入无穷多个字符串，将他们拼接后返回**

`LENGTH` ：求字符串长度

`UCASE / LCASE` ：将字符串转换为大写/小写

`LEFT` ：传入字符串和n，取前n个字符

`STRCMP` ：比较字符串

`SUBSTRING` ：传入字符串，然后传入一个或两个变量，如果传入一个，表示从某个位置后都要，如果传入两个，表示从某个位置后要几个字符。注意：这个位置不是下标，而是第几个字符。

### 数学函数

`ABS` ：求绝对值

`SQRT` ：开平方

`RAND` ：取出一个值范围为 `[0, 1)` 的随机数

`FLOOR` ：向下取整

`CEILING` ：向上取整

`BIN / HEX` ：将数据转换为为二进制/十六进制

**`FORMAT` ：传入一个浮点数和要保留的小数位数，表示保留几位小数**

🌟`FORMAT` 会在整数部分每三位加一个逗号，经过 `FORMAT` 处理后的数据就变成了字符串。

`MOD` ：取余数，既可以当函数用，也可以当运算符用

### 其他函数

`MD5` ：可以提取一个字符串的 `MD5` 值

`DATABASE` ：可以获取当前所在的函数库名

**`IFNULL` ：传入两个参数，如果参数1不为 `NULL` ，则返回参数1，否则返回参数2。**

**课堂练习**

显示所有员工的年薪：

```mysql
SELECT `ename`, `sal`, `comm`, `sal` * 12 + IFNULL(`comm`, 0) AS ysal FROM emp;
```

## 查询进阶

### 聚合函数和GROUP BY子句

聚合函数可以将数据分组处理，处理时可以进行查重，通常和 `GROUP BY` 子句一起使用。

`GROUP BY` 子句后要跟一个列名，表示对某一列中的数据分组统计。

#### 聚合函数实例

基本使用逻辑：

```mysql
COUNT([DISTINCT] * | `col_name`);
```

聚合函数一共有五种，使用逻辑都是这样，这五个函数分别是：

`COUNT` ：统计数据的个数

`MAX` ：统计数据的最大值

`MIN` ：统计数据的最小值

`SUM` ：统计数据的总合

`AVG` ：统计数据的平均值

例如：

```mysql
SELECT COUNT(*) FROM emp; -- 统计emp表中的总行数
SELECT `deptno`, COUNT(*) FROM emp GROUP BY `deptno`; -- 统计每个部门的人数
SELECT FORMAT(AVG(`sal`), 2) FROM emp; -- 统计emp表中的平均薪资
SELECT `deptno`, FORMAT(AVG(`sal`), 2) AS asal FROM emp GROUP BY `deptno`; -- 统计每个部门的平均薪资
```

### 多表查询

例1：打印每个员工的姓名和部门名

在这个问题中， `ename` 列来自于 `emp` 表，而 `dname` 列来自于 `dept` 表，所以需要将两张表合起来做查询，基本方式是：

```mysql
SELECT * FROM emp, dept;
```

一共56条数据，实际上就是将两张表中的所有数据做了一个排列组合。

接下来要先筛选出需要数据，这就必须找**关联项**。在这个问题中，两侧的 `deptno` 相等的项才是我们需要的，所以 `deptno` 列就是关联项，最后选出需要的列即可：

```mysql
SELECT `ename`, `dname` FROM emp, dept WHERE emp.deptno = dept.deptno;
```

#### 自查询

多表联查中，一般都是一般都有一个提供主信息的主表和一个提供补充信息的副表，副表一般只负责将主表中的某列或某几列翻译出来。而自查询指的是主表和副表是同一张表的情况，这种情况中，因为两张表的名字和内容完全一致，所以无论是访问列还是访问表时都必须做出区分，其他情况和多表一致。

例2：打印员工编号、姓名、领导姓名（没有领导的不打印）

```mysql
SELECT e1.empno, e1.ename, e2.ename FROM emp AS e1, emp AS e2 WHERE e1.mgr = e2.empno;
```

### 子查询

子查询就是在一个 `SELECT` 语句中又嵌套了另外的 `SELECT` 语句，这个嵌套的 `SELECT` 语句可以作为另一张表完成多表查询，也可以作为一个数据参与筛选。

例题：打印每个部门薪资最高的员工，如果并列最高，则全部打印：

#### FROM后的子查询

首先先完成一张表，即每个部门的最高薪资

```mysql
SELECT deptno, MAX(sal) AS msal FROM emp GROUP BY deptno; 
```

得到结果：

```
+--------+---------+
| deptno | msal    |
+--------+---------+
| 	  10 | 5000.00 |
|     20 | 3000.00 |
|     30 | 2850.00 |
+--------+---------+
```

然后需要将这张表和 `emp` 表做多表联查，以 `deptno` 作为关联项，比较薪资即可：

```mysql
SELECT emp.deptno, ename, sal
	FROM emp, (
		SELECT deptno, MAX(sal) AS msal
			FROM emp
			GROUP BY deptno
		) AS maxsal
	WHERE emp.deptno = maxsal.deptno AND sal = msal;
```

#### WHERE的子查询

如果同样的问题用 `WHERE` 后的子查询来处理，问题就不一样了。此时子查询查到的是一个多行多列的表，所以在比对时有特殊的写法，如：

```mysql
(emp.deptno, sal) = (maxsal.deptno, msal);
```

这个写法可以比较多列是否都一致。而如果数据有多个，则需要用到 `IN ANY ALL` 三个关键字， `IN` 判断这个数据是否在这个集合中， `ANY` 提供集合中的任意一个数据，而 `ALL` 提供集合中的所有数据。有了这些工具后就可以写了：

```mysql
SELECT deptno, ename, sal
	FROM emp
	WHERE (deptno, sal) IN(
		SELECT deptno, MAX(sal) AS msal
			FROM emp
			GROUP BY deptno
	);
```

### 合并查询

这种查询方式允许将多个 `SELECT` 的查询结果拼合起来，在拼合时会顺手查重。例如还是那个例题，现在就可以分别查询三个部门的最高薪资的员工，然后将结果拼合起来，即：

```mysql
SELECT deptno, ename, sal FROM emp WHERE deptno = 10 AND sal = (SELECT MAX(sal)
FROM emp WHERE deptno = 10)
UNION
SELECT deptno, ename, sal FROM emp WHERE deptno = 20 AND sal = (SELECT MAX(sal)
FROM emp WHERE deptno = 20)
UNION
SELECT deptno, ename, sal FROM emp WHERE deptno = 30 AND sal = (SELECT MAX(sal)
FROM emp WHERE deptno = 30);
```

这里分别查出了三个部门薪资最高的员工，然后将查询结果合并了。

如果在合并过程中不希望它替你查重，则可以使用 `UNION ALL` ，这样就可以不查重了。

## 视图

视图是一种特殊的表，其依赖于其他的表存在，如果其依赖的表被修改了，则视图会跟着修改。

视图是通过 `SELECT` 语句生成的，`SELECT` 语句中用到了什么表，则视图就依赖什么表。而因为 `SELECT` 语句中可能有计算出的列，所以视图中的数据有的并不直接来自于原表。

视图在查询时，是当做一般的表来查询的，其在管理时也和一般表一样拥有独立权限。

视图允许 `UPDATE` ，如果修改的列是直接来源于原表，则原表中的数据也会随之修改，而如果不是，修改往往无效。所以为了避免修改时产生的语句语义不一致问题，所以一般**不修改视图**。

### 创建视图

基本语句逻辑：

```mysql
CREATE VIEW `v_name` AS SELECT ...;
```

例如将刚刚的每个部门的最高薪资员工表做成一个视图，那么：

```mysql
CREATE VIEW empMaxsal AS
SELECT emp.deptno, ename, sal
	FROM emp, (
		SELECT deptno, MAX(sal) AS msal
			FROM emp
			GROUP BY deptno
	) AS maxsal
	WHERE emp.deptno = maxsal.deptno AND sal = msal;
```

查询时当做普通的表查询即可：

```mysql
SELECT * FROM empMaxsal;
```

同理，视图也可以参与多表联查，也可以对结果排序等。

### 删除视图

基本语句逻辑：

```mysql
DROP VIEW `v_name`;
```

## 索引

在一个海量表中，如果要查询一个数据，那么在磁盘上查询时会非常缓慢，直接用 $O(n)$ 遍历的话查询效率非常差。所以需要在表中设置一些索引列来管理数据，以保证查询的效率，所以海量表中，索引是刚需。通常使用B-树等结构加速查询。

但如果索引添加过多，又会导致在插入、删除数据时，需要维护更多的索引结构，所以索引并不能无脑添加，一般要考虑如下三点：

1、如果某个字段频繁的用于查找，那么应该创建索引

2、更新过于频繁的字段不应该创建索引

3、唯一性太差的字段不应该创建索引

大量数据查找时，需要添加索引才可以加速查询的效率，在SQL中一共存在如下几种索引：

### 索引的类型

#### 主键索引

主键具备非空且唯一的特性，且一张表中只能有一个，所以主键的查询性能是最好的。添加主键索引的方式：

```mysql
ALTER TABLE emp ADD PRIMARY KEY(empno);
```

在创建表时，索引信息可以放在表的最后，如：

```mysql
CREATE TABLE `student`(
	`id` INT UNSIGNED AUTO_INCREMENT,
    
	PRIMARY KEY(`id`)
);
```

如果要删除主键，可以直接这么写：

```mysql
ALTER TABLE emp DROP PRIMARY KEY;
```

#### 唯一键索引

可以为空但内容必须唯一的字段是唯一键字段，添加唯一键索引的方式：

```mysql
ALTER TABLE emp ADD UNIQUE(empno);
```

同理，创建表时可以指定唯一键，如：

```mysql
CREATE TABLE `student`(
	`id` INT UNSIGNED AUTO_INCREMENT,
    
	UNIQUE(`id`)
);
```

#### 普通索引

普通索引对字段没有任何要求，随便一列都可以设置，添加普通索引的方式：

```mysql
ALTER TABLE emp ADD INDEX(`ename`);
```

同理，创建表时可以指定普通索引，如：

```mysql
CREATE TABLE `student`(
	`id` INT UNSIGNED AUTO_INCREMENT,
    
	INDEX(`id`)
);
```

普通索引和唯一键索引都是使用 `INDEX` 来完成删除的：

```mysql
ALTER TABLE emp DROP INDEX `ename`;
```

## 事务

事务是一组 `DCL` 语句完成的功能，事务的目的是保证一组 `DML` 语句可以不被打断、不被拆分的执行完毕（具备原子性）。开启事务后，此时对当前数据库的操作都是临时的、可以随时回滚的，且外界无法观测。提交时，如果对数据库做了修改，则这些修改了数据库的语句都会被一并提交。

### 事务的特性（ACID）

1、原子性（Atomicity）：事物中所有的语句都具备原子性，即这些语句在提交时，要么全部成功，要么一行都不执行。

2、一致性（Consistency）：事务一定会将数据库从一个一致状态转移到另一个一致状态，不存在中间态。

3、隔离性（Isolation）：事务在处理时和外部完全隔离，内部的所有处理都不会影响到外部的数据库。

4、持久性（Durability）：事务提交后，其修改不可再次回滚，具备持久性。

| 隔离级别                    | 脏读     | 不可重复读 | 幻读          |
| --------------------------- | -------- | ---------- | ------------- |
| 读未提交 (Read Uncommitted) | ✅ 可能   | ✅ 可能     | ✅ 可能        |
| 读已提交 (Read Committed)   | ❌ 不可能 | ✅ 可能     | ✅ 可能        |
| 可重复读 (Repeatable Read)  | ❌ 不可能 | ❌ 不可能   | ✅ 可能 (标准) |
| 串行化 (Serializable)       | ❌ 不可能 | ❌ 不可能   | ❌ 不可能      |

### 事务的基本操作

开启一个事务：

```mysql
START TRANSACTION;
```

结束并提交一个事务：

```mysql
COMMIT
```

在事务中回滚（直接回滚掉所有修改）

```mysql
ROLLBACK
```

开启事务后，相当于直接在现有数据库上开了一个沙盒，目前所有在这个沙盒中的修改都可以随时反悔。沙盒内部环境和外部环境完全隔离（视隔离级别的不同可能产生互相影响，但任何影响都是不正常的），其修改只在沙盒内部生效。这些还未生效的语句就是可以以回滚的方式撤销。一旦提交，则所有的修改都将固定，不能再次回滚。

设置保存点：

```mysql
SAVEPOINT `sp_name`;
```

可以回滚到某个指定保存点：

```mysql
ROLLBACK TO `sp_name`;
```

如果设置了多个保存点，且回滚到了较早的保存点，则所有在那之后的保存点都将失效。例如依次创建了： `A B C D` 四个保存点，然后回滚到了 `B` 保存点，则 `C D` 失效。

## Linux下的数据库开发

两个库属于第三方库，所以在编译时需要加上 `-lmariadb`

### 相关函数

如下函数的头文件均为 `include <mariadb/mysql.h>`

#### mysql_init

初始化连接对象，参数直接给 `NULL` 即可。返回一个 `MYSQL*` ，后称之为句柄。返回 `NULL` 表示出错

#### mysql_close

传入句柄，关闭句柄。

#### mysql_error

传入句柄，返回最近一次句柄中产生的错误的字符串。

#### mysql_real_connect

用来连接数据库，一共8各参数，依次为：

1、句柄

2、主机名（一个字符串，通常为 `localhost` 或服务器的点分十进制字符串）

3、用户名（一个字符串，这里直接给 `root` 即可）

4、密码（一个字符串，明文的密码串）

5、数据库名（一个字符串，如 `fang` ）

6、端口号（通常给0或3306，表示使用默认端口）

7、套接字路径（通常给 `NULL` ）

8、客户端标记（通常给0）

返回一个连接好的句柄（旧的句柄依然可用）。返回 `NULL` 表示出错。

#### mysql_query

传入句柄和一个字符串，执行字符串对应的SQL语句，正常返回0，失败返回错误码。

##### 课堂案例（写入数据）

使用数据库接口向数据库中写入数据：

```c
#include <mariadb/mysql.h>
#include <stdio.h>

int main(){
    MYSQL* conn = mysql_init(NULL);
    if(conn == NULL){
        puts(mysql_error(conn));
        return 1;
    }

    MYSQL* tmp = mysql_real_connect(conn, "localhost", "root", "", "school", 0, NULL, 0);
    if(tmp == NULL){
        puts(mysql_error(conn));
        mysql_close(conn);
        return 1;
    }
    
    int ret = mysql_query(conn, "INSERT `student`(name, class, math, chinese, english) VALUES('bai', 10, 35, 22, 12);");
    if(ret){
        puts(mysql_error(conn));
        mysql_close(conn);
        return 1;
    }
    
    mysql_close(conn);
    return 0;
}
```

#### mysql_store_result

传入句柄，返回一个 `MYSQL_RES*` 的结构，后称之为“结果集”。这个结构中存储着所有查询到的结果。

#### mysql_free_result

传入结果集，将其释放

#### mysql_num_rows / mysql_num_fields

传入结果集，返回其行数/列数。

#### mysql_fetch_row

传入结果集，将第一行的所有结果以 `MYSQL_ROW` （其实就是 `char**` ，字符串数组）的结构返回，再次调用就会返回下一行的结果，直到读完时，就返回 `NULL` 。同一行中，可以直接用下标取出每个字符串数据。

##### 课堂案例（读取内容）

使用代码读取 `student` 表的所有内容。

```c
#include <mariadb/mysql.h>
#include <stdio.h>

int main(){
    MYSQL* conn = mysql_init(NULL);
    if(conn == NULL){
        puts(mysql_error(conn));
        return 1;
    }

    MYSQL* tmp = mysql_real_connect(conn, "localhost", "root", "", "school", 0, NULL, 0);
    if(tmp == NULL){
        puts(mysql_error(conn));
        mysql_close(conn);
        return 1;
    }

    int ret = mysql_query(conn, "SELECT * FROM student");
    if(ret){
        puts(mysql_error(conn));
        mysql_close(conn);
        return 1;
    }

    MYSQL_RES* allmsg = mysql_store_result(conn);
    int i, j;
    for(i = 0;i < mysql_num_rows(allmsg);i++){
        MYSQL_ROW row = mysql_fetch_row(allmsg);
        for(j = 0;j < mysql_num_fields(allmsg);j++){
            printf("%-5s", row[j]);
        }
        putchar('\n');
    }

    mysql_free_result(allmsg);
    mysql_close(conn);
    return 0;
}
```

#### 案例（应用开发）

```c
#include <mariadb/mysql.h>
#include <stdio.h>

struct student{
    char name[128];
    int class;
    float score[3];
};

int writeDatabase(struct student stu[], size_t n){
    MYSQL* conn = mysql_init(NULL);
    if(conn == NULL){
        puts(mysql_error(conn));
        return 1;
    }

    MYSQL* tmp = mysql_real_connect(conn, "localhost", "root", "", "school", 0, NULL, 0);
    if(tmp == NULL){
        puts(mysql_error(conn));
        mysql_close(conn);
        return 1;
    }
    int i;
    char cmd[1024];
    for(i = 0;i < n;i++){
        sprintf(cmd, "INSERT `student`(name, class, math, chinese, english) VALUES('%s', %d, %.1f, %.1f, %.1f)", 
            stu[i].name, stu[i].class, stu[i].score[0], stu[i].score[1], stu[i].score[2]);
        int ret = mysql_query(conn, cmd);
        if(ret){
            puts(mysql_error(conn));
            mysql_close(conn);
            return 1;
        }
    }
    mysql_close(conn);
}

int main(){
    struct student stu[5] = {
        {"Modi", 10, 40, 50, 70},
        {"Jae-myung", 10, 40, 40, 30},
        {"Mask", 10, 80, 40, 80},
        {"Vace", 10, 60, 60, 90},
        {"Sanae", 10, 30, 20, 40}
    };
    writeDatabase(stu, 5);
    return 0;
}
```

# 动态库

## 静态库和动态库

### 静态库

静态库也叫编译时库，在Linux下，是 `.o` 后缀，而在windows下，是 `.lib` 后缀。命令行中，用 `gcc -c` 就可以编译出静态库。在编译工程时，工程中的每一个 `.c` / `.cpp`文件都会生成一个静态库。

C/C++的编译过程分为四步，即预编译、编译、汇编、链接，静态库就是前三步结束后的结果。这三步针对的是单个的文件，所以每个文
件都可以得到一个静态库，最后将这些静态库链接起来即可生成可执行文件。

在编译时，会将所有的数据分为 `DATA` 段和 `TEXT` 段，这就是一个静态库的组成部分了。而链接期就是合并了每一个静态库的 `DATA` 段和 `TEXT` 段，并兑现所有的 `extern` 声明，生成可执行文件。

### 动态库

动态库（Dynamic Link Library 动态链接库）也叫运行时库或共享库，在Linux下是 `.so` 后缀，而在Window下，是 `.dll` 后缀。

动态库在内存中只加载一次，第一个应用程序用到它时，它就被加载进了内存。加载后，动态库并不是依附于某个应用程序，而是像共享内存那样，游离于所有进程之外，谁要用到它，就把它加载到谁的虚拟空间中，这样，无论有多少个进程使用了它，它在内存中都只有一个备份，从而避免了重复的内存占用。

动态库在编译时，会提供一个特殊的 `.lib` 文件，这个文件不是静态库，但起到了静态库的作用。编译器在使用这个 `.lib` 时，它会告诉编译器某些函数先"欠着”，等到运行时再去寻找。而如果在运行时无法找到这个动态库，则会直接报错退出。

#### 动态库的使用方式

动态库有两种常见的使用方式

1、操作系统中经常有一些大部分应用程序都需要加载系统模块，如果直接以静态库的形式编译到可执行文件中，则会无谓的堆高可执行文件的大小，并且会在内存中反复加载同一段代码，这会造成严重的浪费。而如果做成动态库，则无论多少程序使用都只会加载一次，从而节省了内存空间。

2、当应用程序十分庞大时，是不可能将所有的内容都放在可执行文件里的。那么如何控制可执行文件的大小就是问题了。此时就可以将部分代码拆分成 `dll`，从而拆分可执行文件。这样一来，当需要修改某部分功能时，就可以只更新 `dll` 了，这样也方便了程序的更新维护。

## 动态库的开发

### Windows下的动态库

#### 命令行开发

`VS` 提供了一个命令行的编译套件，叫做 `Developer Command Prompt` ，这个命令行工具中有相关编译工具的环境，可以直接执行各种编译指令。 `MSVC` 环境下，其编译器的指令是 `cl` 。

现在随便找一个空路径，在下面编辑如下代码：

`mydll.c`

```c
__declspec(dllexport) //这一行就是动态库函数的前缀
int add(int a, int b)
{
	return a + b;
}
```

然后用 `Developer Command Prompt` 工具切换到准备好的路径下（方法是用 `d:` 这样的方式切盘符，然后用 `cd` 指令跳转即可），然后执行编译语句编译动态库代码：

```bash
cl /LD mydll.c
```

编译完成后，路径下就有了一对 `.lib` 和 `.dll` 文件（其他文件不关心），这个 `.lib` 就是编译时用的文件，而 `.dll` 就是编译好的动态库。

接下来用一个代码来使用动态库：

`test.c`

```c
#include <stdio.h>
int main()
{
	printf("%d\n", add(3, 6));
	return 0;
}
```

这个代码中并没有做任何声明或定义 `add` 的行为，但只要依赖动态库，就不会有问题：

```bash
cl test.c /link mydll.lib
```

此时编译出的 `test.exe` 就对 `mydll.dll` 产生了依赖，一旦找不到，则程序就无法运行。

之后如果维护了动态库，那么即使不去重新编译 `test.exe` ，其执行的函数也将会变成维护后的。

##### 动态库的动态加载

动态库可以在不产生依赖的情况下直接加载到代码中，这样可以让应用程序在检测到 dll 不存在时还可以采取一些补救措施，这需要调用Windows提供的一些系统函数，调用方式和Linux应用开发是一样的：

```c
#include <windows.h>
#include <stdio.h>

typedef int (*PFUNC)(int, int);
int main()
{
	//生成动态库句柄
	HMODULE hdll = LoadLibrary("mydll.dll");
	if (hdll == NULL)
	{
		printf("dll加载失败\n");
		return 1;
	}
	PFUNC add = (PFUNC)GetProcAddress(hdll, "add");
	if (add == NULL)
	{
		printf("获取add函数失败\n");
		return 1;
	}
	printf("%d\n", add(3, 6));
	FreeLibrary(hdll);
	return 0;
}
```

这样的代码在编译时，不再需要使用 `/link` 参数让其对 `dll` 产生依赖，只需要直接编译即可：

```bash
cl test2.c mydll.lib
```

这样，如果 `dll` 文件不存在，则程序也不会直接崩溃，而是可以判断加载失败后退出

#### VS的动态库开发

在VS下可以直接创建动态链接库的项目，创建完毕后会生成四个文件，目前只关心 pch.h 和 pch.cpp 。接下来可以在这两个文件中添加一些动态库的代码：

`pch.h`

```c
#ifndef PCH_H
#define PCH_H

#include "framework.h"

#define MATH_API __declspec(dllexport)

extern "C" { //这样写是为了保证函数名一定是add
	MATH_API int add(int a, int b);
}

#endif //PCH_H
```

`pch.cpp`

```c
#include "pch.h"

MATH_API int add(int a, int b)
{
	return a + b;
}
```

完成代码后，直接编译生成 `dll` 文件即可。

🌟注意，如果直接点击的是编译并运行，则一定会报运行错误，因为 `dll` 是没法运行的，主要看下面的提示，留意生成是否成功。

随后右键**解决方案**，选择：添加→新建项目。添加一个正常的C项目以供测试 `dll` 。测试代码：

```c
#include <stdio.h>
int main()
{
	printf("%d\n", add(5, 7));
	return 0;
}
```

完成后当然无法通过编译，因为 `add` 无法找到实体。于是要做如下配置：

1、右键项目→属性→链接器→常规→附加库目录，在这里填入要依赖的 `.lib` 文件的路径。这个路径最好是一个相对路径，可以使用变量 `SolutionDir` 来表示当前解决方案的路径（ `.sln` 所在的路径），默认情况下在  `$(SolutionDir)/x64/Debug` 下，随后点击右下角的确定或应用。

2、右键项目→属性→链接器→输入→附加依赖项，下拉菜单点编辑后，将 .lib 文件的名字填入，如 `myDll.lib` ，表示让当前项目依赖这个 `.lib` 对应的动态库，随后点击右下角的确定或应用。

配置好后，直接点击C项目的编译并运行，即可执行出结果。

🌟注意，如果要编译Release版本，则要重新做配置。Release和Debug的配置是分开的

### Linux下的动态库

在Linux下，动态库是一种第三方库，编译时要加上 `-ldl` ，其代码和Windows下的代码结构完全一致，只是接口有所区别而已。

动态库的代码：

`myso.c`

```c
__attribute__((visibility("default")))
int add(int a, int b)
{
	return a + b;
}
```

然后可以用如下编译语句完成编译：

```bash
gcc myso.c -fPIC -shared -o myso.so
```

之后可以写一个测试代码：

`test.c`

```c
#include <dlfcn.h>
#include <stdio.h>

typedef int (*PFUNC)(int, int);

int main()
{
	void* hdll = dlopen("./myso.so", RTLD_LAZY);
	if (hdll == NULL)
	{
		printf("so加载失败\n");
		return 1;
	}
    
	PFUNC add = (PFUNC)dlsym(hdll, "add");
	if (add == NULL)
	{
		printf("获取add函数失败\n");
		return 1;
	}
    
	printf("%d\n", add(3, 6));
	dlclose(hdll);
	return 0;
}
```

这个代码直接编译即可：

```bash
gcc test.c -o test -ldl
```

# GIT

Git是 GNU 组织研发的一个代码管理工具，它是一种分布式的管理工具。在每一个Git终端上，都有一个本地服务器，这个服务器上记录着Git仓库中所有的相关信息。所以哪怕主服务器出了问题，也可以直接在本地服务器上使用，而之后只要用其中某个仓的数据就可以直接重建整体仓库了。这种方式相对安全。

## 创建Git仓库

1、先打开 `gitee` 网站，申请账号（ https://gitee.com/ ），密码一定要牢记

2、鼠标放到右上角的加号处，点击新建仓库，填好信息后点击下方的新建按钮即可。此时会跳到仓库界面。

3、第一次创建时，会给出命令提示，将其中前两段的命令粘下来，如：

```bash
git config --global user.name "xuanyan"
git config --global user.email "2058763315@qq.com"

mkdir test
cd test
git init 
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin https://gitee.com/yuanyanya/test.git
git push -u origin "master"
```

此时，打开 `git bash` ，跳转到某个目录下（也可以右键某个目录选择 `git bash here` ），然后将上面的这两段代码粘到 `git bash` 中，输入 `gitee` 网站的用户名和密码，配置好这个新仓库。

这个配置是第一次创建项目时需要做的，之后如果要拉取仓库，可以直接用如下代码即可：

```bash
git config --global user.name "xuanyan"
git config --global user.email "2058763315@qq.com"
git clone https://gitee.com/yuanyanya/test.git
```

## Git的常用命令

`init` ：初始化本地仓库，创建 `.git` 文件夹，为后续操作做准备

`clone` ：直接克隆线上仓库，这是拉取仓库的常见操作

`add` ：后跟路径，表示将新增/删除的文件先纳入到要提交的列表中（重命名也属于添加删除操作），通常跟一个 `.` 表示当前路径下的所有。

`commit` ：将本地的修改提交到本地服务器上，这个操作通常不会出问题。

`push` ：推送数据，将当前本地服务器的数据推送到云服务器上。推送前会检查当前本地服务器代码是否是最新版本，如果不是一般需要先执行 `pull` 操作。

🌟`add commit push` 是一套连招

`pull` ：拉取数据，直接从云服务器上拉取最新的数据。但如果云上的数据和本地的数据产生了冲突，则需要处理冲突后才能提交。

`log` ：查看历史上的所有修改信息

`diff` ：查阅当前还没提交的修改

## 冲突（conflict）

当本地更改了某个文件中的某段代码后，如果服务器上的同一段代码也由另一个人已经更改并上传了，此时本地就必须先 `pull` 才能上传。而因为同一段代码被两个人都改了，此时就会产生**冲突**。 `pull` 操作就会先将该冲突留待你处理（其他部分照常拉取）。

冲突产生时，本地文件会提示冲突是什么样的，格式如下：

```
<<<<<<< HEAD
We are win a lot.
=======
Make American Great Again
>>>>>>> c6d55bc1538be798c461a6f457dfe607ce38c21f
```

这表示本地文件的内容是 `We are win a lot.` ，云上文件的内容是 `Make American Great Again` ，

产生了冲突，解决冲突的方式是删掉其中一边，如：

```
Make American Great Again
```

其实就是将多余的东西删掉。删完后现在就可以 `commit` 了，正常后就可以 `push` 了

🌟冲突无法避免，但Git比较智能，只要两次修改不是同一段代码就不会产生冲突。

## Git的分支管理

在Git中，用分支来管理代码是一种非常常见的现象。如公司的主要代码都在 `master` 分支上，然后从这个分支拉出几个大项目组的模块，如前台总分支、后台总分支等。然后每个大分支再划分成多个小分支（如给每个项目组再分出一个分支，项目组再给每个人都分一个分支等等），这样就形成了一个分支树。个人分支要向项目组分支同步时，需要有人审核，这就要求个人要提出 `Merge Request(MR)` ，由负责人审核后确认是否要合并到更高一级分支上。通过这样的方式就可以有效的管理代码了。

分支中常见的命令：

```bash
git branch #查看自己在哪个分支
git branch 分支名 #创建分支
git checkout 分支名 #切换分支
git checkout -b 分支名 #创建并切换分支
```

创建分支后，分支只在本地生效，需要用 `git push` 将其推送输出：

```bash
git push -u origin 分支名
```

如果要将另一个分支的数据同步到当前分支上，则需要使用：

```bash
git merge 分支名
```

加入要将 `bai` 分支合并到 `master` 分支，则需要在 `master` 分支上执行这个语句。

如果要删除这个分支，则需要使用：

```bash
git branch -D 分支名
```

这样删除只能删除本地的分支，如果要删除远程分支，则需要使用：

```bash
git push origin --delete 分支名
```







# C语言补充

## 指针

`const` 修饰谁，谁就不能改

`const char* pch` 指向的类型是 `const char`，内容只读，指针本身可改

`char* const pch` 指向不能变，但指向的值可以变。

`const char* pch` 等价 `char const* pch`

## main函数传参

main函数最多可以有3个参数，平时只用前两个。三个参数分别为：

```c
int main(int argc, char* argv[], char* envp[])
{
    return 0;
}
```

1、相当于 `shell` 中的 `$#` ，形容参数个数。

2、相当于将 `$1 $2` 等以数组的形式传入了，即 `argv[1]` 就是 `$1` ，以此类推。这个数组最后会以一个 `NULL` 作为结束。

3、环境变量，平时不用。

要想查看它们的内容，可以这样写：

```c
#include <stdio.h>
int main(int argc, char* argv[], char* envp[])
{
    int i;
	for (i = 0; i < argc; i++)
	{
		puts(argv[i]);
    }
    for (i = 0; envp[i];i++)
		puts(envp[i]);
    }
	return 0;
}
```

## Linux下的换行符

在 `Windows` 下，默认的换行符是两个字符，即 `CR+LF` `(\r\n)` ，而在 `Linux` 下默认的换行符只是 `LF` `(\n)` 。所以在 `Linux` 下 `\r` 有其他的作用，其作用并不是换行，而是让光标回到当前行的行首。

使用这个功能，就可以设计一个进度条程序：

```c
#include <stdio.h>
#include <unistd.h>

void printBar(size_t curPos, size_t allPos){
    int per = curPos * 100 / allPos;
    putchar('[');
    int i;
    for(i = 0;i < per / 2;i++){
        putchar('#');
    }
    for(; i < 50; i++){  //保证两个for一共打印50个字符
        putchar('-');
    }
    printf("][%d%%][%ld/%ld]\r", per, curPos, allPos);
    fflush(stdout);  //在Linux下，不见到\n是不会清空输出缓存的，所以这里手动清空一下
}

int main(){
    int i, n = 10000;
    for(i = 0; i <= n; i++){
        printBar(i, n);
        usleep(100);
    }
    putchar('\n');  //推一个回车保证终端显示不会铺盖进度条
	return 0;
}
```

## 柔性数组

柔性数组指在**结构体（类）**中的**最后一个长度为0或者1的数组**成员。

### 柔性数组的作用

柔性数组有两个常见作用：

#### 作用1

可以规避内存碎片和减少对堆空间进行申请释放的次数。

观察如下结构：

```c
struct test
{
	int* arr;//记录每个元素
	size_t num;//表示下面的arr中有多少个
}
```

如果现在要给该结构体的变量申请空间，则需要申请两次：

```c
struct test* pt = (struct test*)malloc(sizeof(struct test));
pt->num = 10;
pt->arr = (int*)malloc(pt->num * sizeof(int));
```

同理释放也需要两次。

而如果改用柔性数组，则结构就变成了：

```c
struct test
{
    size_t num;
	int arr[0];//由于ANSI C标准不支持这种写法，所以有时也写成1
}
```


🌟C99和C++都支持这种写法，只在C语言编译老旧时（仅支持ANSI C时），考虑将数组长度写成1。

现在再给该结构体申请空间，则可以这么写：

```c
struct test* pt = (struct test*)malloc(sizeof(struct test) + 10 * sizeof(int));
pt->num = 10;
```

同理，释放只需要一次。

这么做有两个好处：

1、向堆中申请、释放空间是一件非常耗时的工作，这么做减少了一次申请、释放。

2、两块游离的空间变成了一个整体，避免了内存碎片的产生。

#### 作用2

柔性数组在作为接口传参时，可以作为一个接口的模板，允许用户传入另一个结构。如柔性数组结构：

```c
struct softarr
{
	int num;
	char str[0];
}
```

当它作为参数时：

```c
void func(struct softarr* sa, size_t strsize);
```

可以由用户定义一个待传入的结构：

```c
struct softarrtmp
{
	int num;
	char str[128];
};
```

然后就可以这样传参：

```c
struct softarrtmp stt;
func((struct softarr*)&stt, 128);
```

## 服务器

### Gunicorn

**Gunicorn**（Green Unicorn）是一个 Python **WSGI HTTP 服务器**，基于 pre-fork worker 模型，常用于部署 Python Web 应用（Flask、Django、FastAPI 等）。

```bash
nohup gunicorn -w 1 -b 0.0.0.0:5000 app:app > app.log 2>&1 &
```

`-w 1` 等价 `--workers=1`，worker 工作进程数量，这里启动 1 个工作进程

`-b 0.0.0.0:5000` 等价 `--bind`，绑定监听地址与端口，`0.0.0.0`：监听本机**所有网卡 IP**（内网 / 外网都能访问）

`> app.log` 重定向写入 `app.log`，覆盖原有内容；如果想要追加日志使用 `>> app.log`。

`2>&1`

`1` = 文件描述符：标准输出 stdout

`2` = 文件描述符：标准错误 stderr（报错、异常信息）

含义：把错误输出，和正常输出合并，一起定向到 `app.log` 

不加这条：报错信息仍然打印到终端，不会存入日志文件。

`app:app` Gunicorn 入口规范

1、第一个 `app`：文件名 `app.py`

1、第二个 `app`：`app.py` 内部定义的 Flask/Django 应用实例对象

### nohup

**nohup** = **No Hang Up**（不挂断），是 Unix/Linux 系统中的一个命令，用于让程序在**后台持续运行**，即使用户退出终端、断开 SSH 连接也不会被终止。

```bash
nohup 命令 [参数...] &
```

`nohup` 忽略挂断信号

`python app.py` 要运行的命令

`&` 放到**后台运行**（不阻塞当前终端）

### ps

```bash
ps aux
```

`ps` 查看当前系统进程快照

`a` 显示所有用户的进程

`u` 以用户友好格式展示（用户名、CPU、内存、启动命令等）

`x` 展示无控制终端的后台进程（守护进程必备）

```bash
ps aux | grep gunicorn
```

查看 `gunicorn` 正在运行的进程信息。

### kill

```bash
kill [-信号编号/信号名] PID
```

`-2` 和终端 `Ctrl + C` 效果一致

`-9` **强制杀死**，进程无法捕获、无法拦截，内核直接回收进程

### systemd

systemd 系统服务管理，基本逻辑如下：

```
systemctl [动作] 服务名.service
```

启动和停止ssh服务如下：

```bash
sudo systemctl start ssh  #启动服务
sudo systemctl enable ssh #自启动
sudo systemctl stop ssh   #停止服务
```





# Linux练习

## 用户命令练习

 1.1 创建一个新用户组，命名为 `sales` 。

 1.2 创建一个新用户，用户名为 `alice` ，并设置其初始密码为 `Alice2024` 。

 1.3 将 `alice` 用户的主组更改为 `sales` 组。（提示：注意删除自动生成的同名组 `alice` ，如果该组

已无其他用户）

 2.1以 `root` 身份（或使用 `sudo` ）创建一个新用户 `bob` ，要求：

主目录为 `/home/bob` ，自动创建。

指定 shell 为 `/bin/bash` 。

将 `bob` 用户**附加**到 `sales` 组（即保留其原有主组，同时加入 `sales` 组）。

 2.2 切换到 `alice` 用户，在其主目录下创建一个名为 `shared_docs` 的目录，并设置权限为：

所属用户（`alice`）可读、可写、可执行。

**组用户（sales组）** 可读、可写、可执行。

其他用户没有任何权限。

 2.3 在 `shared_docs` 目录中创建一个文件 `price_list.txt` ，内容为：

 2.4 修改 `price_list.txt` 的权限，使其对 **组用户（sales组）** 只有读权限（不可写），其他用户无

任何权限。

 3.1 切换到 `bob` 用户，尝试：

进入 `/home/alice/shared_docs` 目录。

查看 `price_list.txt` 的内容。

尝试向 `price_list.txt` 追加一行（例如 `Product D: $40` ），观察是否成功。

 3.2 创建一个临时用户 `testuser` （不加入 `sales` 组），尝试访问 `price_list.txt` ，验证是否被拒

绝。

 4 删除所有创建的用户 `(alice , bob , testuser)` 及其主目录，删除 `sales` 组。

```bash
sudo -i
#1
groupadd sales
useradd -md /home/alice -s /bin/bash alice
passwd alice
usermod -g sales alice  #更改主组
groupdel alice
#2
useradd -md /home/bob -s /bin/bash bob
usermod -aG sales bob
su alice
cd
mkdir shared_docs
chmod 770 shared_docs
cd shared_docs
touch price_list.txt
echo 'Product A: $10' > price_list.txt
echo 'Product B: $20' >> price_list.txt
echo 'Product C: $30' >> price_list.txt
chmod 640 price_list.txt
exit #退出alice用户回到root
#3
su bob
cd /home/alice/shared_docs
cat price_list.txt #成功
echo 'Product D: $40' >> price_list.txt #无权限，失败
exit
useradd -md /home/testuser -s /bin/bash testuser
su testuser
cat /home/alice/shared_docs/price_list.txt #被拒绝
exit
#4
userdel -r alice
userdel -r bob
userdel -r testuser
groupdel sales
groupdel bob
```

## sed练习

```bash
sed -i.bak 's/example\.com/newdomain\.com/' eamil.txt 
#将所有的 example.com 替换成 newdomain.com
sed '/anotherdomain/d' email.txt 
#删除 email.txt 中包含字符串 anotherdomain 的行，输出结果
sed -n '/newdomain.com/!q' eamil.txt
grep -v newdomain.com email.txt
#筛选出所有不是 newdomain.com 的行

sort -nrk10 access.log | head 
#分析日志文件，找出访问量最高的前10行（最后一列为访问量）
sed -n '/^192.168/p' access.log | sort -nk10 | head -n 1
#筛选出以192.168开头的IP，并找到访问量最低的IP地址；
sed -e '/404/d' -e '1s/200/SUCCESS/' access.log
#删除包含 404 的行，并将剩余行中第一个 200 替换为 SUCCESS
```

## awk练习

1、用awk打印 `score.txt` 中每个人的姓名和英语成绩（第4列）。

```bash
awk '{print $1" "$4}' score.txt
```

2、打印 `score.txt` 中第3列大于等于90分的行。

```bash
awk '$3 >= 90 {print}' score.txt
```

3、计算 `score.txt` 中所有学生第2列的平均值，输出结果保留两位小数。

```bash
awk '{sum += $2} END{printf "第二列平均成绩为 %.2f\n", sum / NR}' score.txt
```

4、在 `access.log` 中，统计状态码为200的请求中，哪个IP产生的流量（最后一列）最大？输出该IP和总流量。

```bash
awk '$9 == 200 {print $1, $NF}' access.log | sort -rnk2 | head -n 1
```

5、对 `email.txt` ，输出：`行号: 字段个数: 整行内容`。

```bash
awk -F: '{print NR": " NF": " $0}' email.txt
```

6、对 `email.txt` ，输出用户名和域名前段相同的项，如 `alice:alice@xx.com` 就是相同的项，`bob:bobJackson@xx.com` 就是不同的项。

```bash
awk -F[:@] '$1 == $2 {print}' email.txt
```

7、对 `email.txt` 提取出用户名和域名（例如 `john` 和 `example.com` ），然后统计每个域名的出现次数。

```bash
awk -F[:@] '{count[$3]++; print $1" "$3} END{for(i in count) print i" "count[i]}' email.txt
```

## shell练习

1、编写一个脚本，输出100以内和7无关的数

```bash
#!/bin/bash

for i in $(seq 1 100); do
	if [ $((i % 7)) -ne 0 ] && [ $((i / 10)) -ne 7 ] && [ $((i % 10)) -ne 7 ]; then
		echo -n "$i "
	fi
done
echo ""
```

2、编写一个脚本，可以传入一个路径，可以将该路径下的所有非空文件全部打包成一个 `tar.gz`

tips：打包文件虽然可以用很多文件作为列表，但毕竟不方便，可以考虑先将待打包的文件集中到一起。

```bash
#!/bin/bash
#批量打包
curpath='.'
tarname='user_tmp'
if [ $# -eq 2 ]; then
	tarname=$2
	curpath=$1
elif [ $# -eq 1 ]; then
	curpath=$1
fi

mkdir .__tmpTarDir
for fname in $(ls $curpath); do
	if [ -s "$curpath/$fname" ]; then
		cp "$curpath/$fname" .__tmpTarDir
	fi
done

tar -zcvf "$tarname.tar.gz" .__tmpTarDir/*
rm -r .__tmpTarDir

```

3、检索当前目录下所有的 `.txt`文件，在这些文件的前面加上 `backup_` 前缀。如 `1.txt` 变为 `backup_1.txt`

```bash
#!/bin/bash

for file in *.txt; do
	if [ -f $file ]; then
		mv $file "backup_$file"
	fi
done
```

4、输入n，输出斐波那契数列的第n项

```bash
#!/bin/bash

an_2=0
an_1=1
an=$((an_1+an_2))
read -p "请输入数字 : " n
if [ $n -le 0 ]; then
	echo "n不能小于等于0"
	exit
for ((i = 2;i < n;i++)); do
    an_2=$an_1
    an_1=$an
    an=$((an_1+an_2))
done

echo "$an"
```

5、（思考题）根据 `students.csv` 中的数据，完成一个和用户交互的脚本：

用户按下1表示按学号查找，按下2表示按照姓名模糊匹配，按下3显示所有学生，按下4退出脚本。

```bash
#!/bin/bash

filename='students.csv'
searchById()
{
	read -p "请输入学号：" stuid
	res=$(awk -F, -v id="$stuid" '$1 == id {print}' $filename)
	if [ -z "$res" ]; then
		echo "未找到该学号的行"
	else
		echo "$res" | awk -F, '{printf "学号：%d 名：%s 姓：%s 分数：%d\n", $1, $2, $3, $4}'
	fi
}

searchByName()
{
	read -p "请输入姓名：" stuname
	res=$(awk -F, -v name="$stuname" '$2 ~ name || $3 ~ name {print}' $filename)
	if [ -z "$res" ]; then
		echo "未找到姓名包含$stuname的行"
	else
	echo "$res" | awk -F, '{printf "学号：%d 名：%s 姓：%s 分数：%d\n", $1, $2, $3, $4}'
	fi
}

showAll()
{
    awk -F, 'BEGIN{print "学号   名              姓           分数"} NR > 1 {printf "%-6s %-15s %-12s %3s\n", $1, $2, $3, $4}' $filename
}

main_menu()
{
	option=('按学号查找' '按姓名查找' '显示所有信息' '退出');
	while true; do
		i=1;
		for opt in ${option[@]}; do
			echo "$i、$opt"
			i=$((i + 1))
		done
		read -p "请选择操作：" choice
		case $choice in
			1)
				searchById
				;;
			2)
				searchByName
				;;
			3)
				showAll
				;;
			4)
				echo '再见'
				exit 0
				;;
			*)
				echo '选项无效'
				;;
		esac
	done
}
if [ -f $filename ]; then
	main_menu
else
	echo "错误，$filename不存在"
	exit 0
fi
```

## 完成cp指令

```c
#include <fcntl.h>
#include <sys/stat.h>
#include <unistd.h>
#include <stdio.h>
#include <string.h>
#define BUFSIZE 1024

void printBar(size_t curPos, size_t allPos){
    int per = curPos * 100 / allPos;
    putchar('[');
    int i;
    for(i = 0;i < per / 2;i++){
        putchar('#');
    }
    for(; i < 50; i++){
        putchar('-');
    }
    printf("][%d%%][%ld/%ld]\r", per, curPos, allPos);
    fflush(stdout);
}

int copyfile(const char* srcfile, const char* dstfile){
	int fdr = open(srcfile, O_RDONLY);
	if(fdr < 0){
		perror("[源文件]open error ");
		return -1;
	}
	int fdw = open(dstfile, O_WRONLY | O_TRUNC | O_CREAT, 0644);
	if(fdw < 0){
		perror("[目标文件]open error ");
		return -1;
	}

	struct stat msg;
	fstat(fdr, &msg);

	char buf[BUFSIZE];
	int sum = 0;
	
	int ret = read(fdr, buf, BUFSIZE);
	while(ret > 0){
		sum += ret;
		printBar(sum, msg.st_size);
		write(fdw, buf, ret);
		ret = read(fdr, buf, BUFSIZE);
	}
	close(fdr);
	close(fdw);
	putchar('\n');
	return 0;
}

int main(int argc, char* argv[]){
	if(argc < 3){
		printf("参数不足\n");
		return -1;
	}
	struct stat msg;
	int ret = stat(argv[2], &msg);
	char filename[256];
	if(ret >= 0 && S_ISDIR(msg.st_mode)){
		strcpy(filename, argv[2]);
		strcat(filename, "/");
        
		char* tmp = strrchr(argv[1], '/');
		if(tmp){
			strcat(filename, tmp + 1);
		}else{
			strcat(filename, argv[1]);
		}
		ret = copyfile(argv[1], filename);
		if(ret < 0){
			return -1;
		}
	}
	else{
		ret = copyfile(argv[1], argv[2]);
		if(ret < 0){
			return -1;
		}
	}
	return 0;
}
```

## 完成cp-r指令

```c
#define _DEFAULT_SOURCE
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>
#include <sys/stat.h>
#include <dirent.h>
#include <string.h>
#include <ctype.h>
#define BUFSIZE 1024

int copyfile(const char* srcpath, const char* dstpath){
    int fdr = open(srcpath, O_RDONLY);
    if(fdr < 0){
		perror("源文件打开失败");
		return -1;
	}
	int fdw = open(dstpath, O_WRONLY | O_TRUNC | O_CREAT, 0644);
	if(fdw < 0){
		perror("目标文件打开失败");
		return -1;
	}
    char buf[BUFSIZE];
    int ret = read(fdr, buf, BUFSIZE);
    while(ret > 0){
        write(fdw, buf, ret);
        ret = read(fdr, buf, BUFSIZE);
    }
    close(fdr);
    close(fdw);
    return 0;
}

int copydir(const char* srcpath, const char* dstpath){
    mkdir(dstpath, 0777);
    char oldpath[256];
    char newpath[256];
    strcpy(oldpath, srcpath);
    strcpy(newpath, dstpath);
    strcat(oldpath, "/");    
    strcat(newpath, "/");

    DIR* dir = opendir(srcpath);
    if(dir == NULL){
        perror("dir error");
        return -1;
    }
    struct dirent* msg = readdir(dir);
    while(msg){
        if(strcmp(msg->d_name, ".") == 0 || strcmp(msg->d_name, "..") == 0){
            msg = readdir(dir);
            continue;
        }

        strcat(oldpath, msg->d_name);
        strcat(newpath, msg->d_name);
        int ret;
        if(msg->d_type == DT_DIR){
            ret = copydir(oldpath, newpath);
        }
        else{
            ret = copyfile(oldpath, newpath);
        }
        if(ret < 0){
            return ret;
        }

        strrchr(oldpath, '/')[1] = '\0';
        strrchr(newpath, '/')[1] = '\0';
        msg = readdir(dir);
    }
    closedir(dir);
    
    return 0;
}

int main(int argc, char* argv[]){
    if(argc < 3){
        printf("参数不足\n");
        return -1;
    }
    struct stat msg;
    
    int ret = stat(argv[1], &msg);
    if(ret < 0){
        perror("stat error ");
        return -1;
    }

    char filename[256];
    if(S_ISDIR(msg.st_mode)){
        ret = stat(argv[2], &msg);
        if(ret >= 0 && S_ISDIR(msg.st_mode)){
            strcpy(filename, argv[2]);
            strcat(filename, "/");
            char* tmp = strrchr(argv[1], '/');
            if(tmp > 0){
                strcat(filename, tmp + 1);
            }
            else{
                strcat(filename, argv[1]);
            }
            copydir(argv[1], filename);
        }
        else if(ret >= 0){
            printf("%s是个文件\n", argv[2]);
            return -1;
        }
        else{
            copydir(argv[1], argv[2]);
        }
    }
    else{
        ret = stat(argv[2], &msg);

        if(ret >= 0 && S_ISDIR(msg.st_mode)){
            strcpy(filename, argv[2]);
            strcat(filename, "/");
            char* tmp = strrchr(argv[1], '/');
            if(tmp > 0 && tmp[1]){
                strcat(filename, tmp + 1);
            }
            else{
                strcat(filename, argv[1]);
            }
            copyfile(argv[1], filename);
        }
        else{
            copyfile(argv[1], argv[2]);
        }

    }
    return 0;
}
```

## 完成rm-r指令

```c
#define _DEFAULT_SOURCE
#include <unistd.h>
#include <stdio.h>
#include <sys/stat.h>
#include <dirent.h>
#include <string.h>

int removedir(const char* path){
    char repath[256];
    strcpy(repath, path);
    strcat(repath, "/");
    DIR* dir = opendir(path);
    if(dir == NULL){
        perror("dir error ");
        return -1;
    }
    struct dirent* msg = readdir(dir);
    while(msg){
        if(strcmp(msg->d_name, ".") == 0 || strcmp(msg->d_name, "..") == 0){
            msg = readdir(dir);
            continue;
        }

        strcat(repath, msg->d_name);
        int ret;
        if(msg->d_type == DT_DIR){
            ret = removedir(repath);
        }
        else{
            ret = unlink(repath);
        }
        if(ret < 0){
            return ret;
        }

        strrchr(repath, '/')[1] = '\0';
        msg = readdir(dir);
    }
    closedir(dir);
    if (rmdir(path) == -1)
    {
        perror("rmdir error ");
        return -1;
    }
    return 0;
}

int main(int argc, char* argv[]){
    if(argc < 2){
        printf("参数不足\n");
        return -1;
    }
    struct stat msg;
    for(int i = 1;i < argc;i++){
        int ret = stat(argv[i], &msg);
        if(ret < 0){
            printf("%s ", argv[i]);
            fflush(stdout);
            perror("stat error ");
            continue;
        }
        if(S_ISDIR(msg.st_mode)){
            removedir(argv[i]);
        }
        else{
            unlink(argv[i]);
        }
        if(ret < 0){
            printf("%s ", argv[i]);
            fflush(stdout);
            perror("unlink error ");
        }
    }
    return 0;
}
```

## 命名管道复制

```c
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>
#define BUFSIZE 2048
int main()
{
	int fdr = open("1.mp4", O_RDONLY);
	if (fdr < 0)
	{
		perror("open error ");
		return -1;
	}
	int fdw = open("cp.fifo", O_WRONLY);
	if (fdw < 0)
	{
		perror("open error ");
		return -1;
	}
	char buf[BUFSIZE];
	int ret = read(fdr, buf, BUFSIZE);
	while (ret > 0)
	{
		write(fdw, buf, ret);
		ret = read(fdr, buf, BUFSIZE);
	}
	close(fdr);
	close(fdw);
	return 0;
}
```

```c
#include <fcntl.h>
#include <unistd.h>
#include <stdio.h>
#define BUFSIZE 2048
int main()
{
	int fdr = open("cp.fifo", O_RDONLY);
	if (fdr < 0)
	{
		perror("open error ");
		return -1;
	}
	int fdw = open("2.mp4", O_WRONLY | O_CREAT | O_TRUNC, 0666);
	if (fdw < 0)
	{
		perror("open error ");
		return -1;
	}
	char buf[BUFSIZE];
	int ret = read(fdr, buf, BUFSIZE);
	while (ret > 0)
	{
		write(fdw, buf, ret);
		ret = read(fdr, buf, BUFSIZE);
	}
	close(fdr);
	close(fdw);
	return 0;
}
```

## popen简易终端

```c
#include <stdio.h>
#include <string.h>
int main()
{
	char cmd[2048] = "";
	char buf[2048] = "";
	FILE* pf;
	while (1)
	{
		printf("$ ");
		fflush(stdout);
		memset(cmd, 0, sizeof(cmd));
		scanf("%2047[^\n]%*c", cmd);
		if (strcmp(cmd, "exit") == 0)
		{
			break;
		}
		pf = popen(cmd, "r");
		if (pf == NULL)
		{
			perror("popen error ");
			continue;
		}
		fgets(buf, 2047, pf);
		while (!feof(pf))
		{
			printf("%s", buf);
			fgets(buf, 2047, pf);
		}
		pclose(pf);
	}
	return 0;
}
```

## 自定义信号

设计一个程序，利用信号列表中的 `SIGUSR1` 和 `SIGUSR2` 信号，完成以下动作：

（1）程序本身有一个计数器，会不断自增1，当收到 `SIGUSR1` 信号后，自增/自减速度会增加，当收到 `SIGUSR2` 信号后，则会改为自减操作，自减的速度跟原来自增的速度保持一致。

（2）如果5秒内没有收到 `SIGUSR1` 和 `SIGUSR2` 信号，则将自增/自减速度调整回1。

```c
#include <stdio.h>
#include <unistd.h>
#include <signal.h>

int step = 1;
int count = 0;

void sigusr1_handler(int sig) {
    if(step < 0){
        step--;
    }
    else{
        step++;
    }
    alarm(5);
}

void sigusr2_handler(int sig) {
    step *= -1;
    alarm(5);
}

void alarm_handler(int sig){
    step = 1;
}

int main(){
    printf("pid : %d\n", getpid());
    signal(SIGUSR1, sigusr1_handler);
    signal(SIGUSR2, sigusr2_handler);
    signal(SIGALRM, alarm_handler);

    while(1){
        count += step;
        printf("%d ", count);
        fflush(stdout);
        sleep(1);
    }
    return 0;
}
```

## 多生产者多消费者

设计一个程序，有两个资源可供抢夺：

要求：生产者1每2秒生产1000000个资源A，生产者B每5秒生产6000000个资源B。

消费者1~5 抢夺资源A，消费者 6~10 抢夺资源B。

（思考题）消费者11~15既可以消费资源A，也可以消费资源B。

30秒后，程序结束，所有线程全部结束。

※最好通过线程函数的参数给每个线程起个名字。

```c
#include <pthread.h>
#include <stdio.h>
#include <unistd.h>
#define CON_NUM 15
#define PRODUTA 1000000
#define PRODUTB 6000000
int g_flag = 1;
int g_srcA = 0;
int g_srcB = 0;
pthread_mutex_t mutexA = PTHREAD_MUTEX_INITIALIZER;
pthread_mutex_t mutexB = PTHREAD_MUTEX_INITIALIZER;

void* thread_productorA(void* arg){
    while(g_flag){
        pthread_mutex_lock(&mutexA);
        g_srcA += PRODUTA;
        pthread_mutex_unlock(&mutexA);
        sleep(2);
    }
}

void* thread_productorB(void* arg){
    while(g_flag){
        pthread_mutex_lock(&mutexB);
        g_srcB += PRODUTB;
        pthread_mutex_unlock(&mutexB);
        sleep(5);
    }
}

void* thread_constumerA(void* arg){
    const char* name = (void*)arg;
    int count = 0;
    while(g_flag){
        while(g_srcA > 0){
            pthread_mutex_lock(&mutexA);
            if(g_srcA > 0){
                g_srcA--;
                count++;
            }
            pthread_mutex_unlock(&mutexA);
        }
        if(count > 0){
            printf("%s : %d\n", name, count);
            count = 0;
        }
    }
}

void* thread_constumerB(void* arg){
    const char* name = (void*)arg;
    int count = 0;
    while(g_flag){
        while(g_srcB > 0){
            pthread_mutex_lock(&mutexB);
            if(g_srcB > 0){
                g_srcB--;
                count++;
            }
            pthread_mutex_unlock(&mutexB);
        }
        if(count > 0){
            printf("%s : %d\n", name, count);
            count = 0;
        }
    }
}

void* thread_constumerAB(void* arg){
    const char* name = (void*)arg;
    int countA = 0, countB = 0;
    while(g_flag){
        while(g_srcA > 0 || g_srcB > 0){
            if(g_srcA > 0){
                pthread_mutex_lock(&mutexA);
                g_srcA--;
                countA++;
                pthread_mutex_unlock(&mutexA);
            }
 
            if(g_srcB > 0){
                pthread_mutex_lock(&mutexB);
                g_srcB--;
                countB++;
                pthread_mutex_unlock(&mutexB);
            }
        }
        if(countA > 0){
            printf("%s : A : %d\n", name, countA);
            countA = 0;
        }
        if(countB > 0){
            printf("%s : B : %d\n", name, countB);
            countB = 0;
        }
    }
}

int main(){
    pthread_t tid[CON_NUM];
    pthread_t tidp1, tidp2;
    char name[15][20] = {"A1 沈景珩", "A2 陆知屿", "A3 顾沐辰", "A4 江砚辞", "A5 傅斯年", 
                         "B1 温叙白", "B2 季承宇", "B3 苏晚晴", "B4 许清禾", "B5 宋知微",
                         "AB1 林舒瑶", "AB2 夏星眠", "AB3 阮云汐", "AB4 孟若棠", "AB5 叶芷宁"};

    pthread_create(&tidp1, NULL, thread_productorA, NULL);
    pthread_create(&tidp2, NULL, thread_productorB, NULL);
    int i;
    for(i = 0;i < 15;i++){
        if(i >= 0 && i <= 4){
            pthread_create(tid + i, NULL, thread_constumerA, name[i]);
        }
        else if(i >= 5 && i <= 9){
            pthread_create(tid + i, NULL, thread_constumerB, name[i]);
        }
        else{
            pthread_create(tid + i, NULL, thread_constumerAB, name[i]);
        }
    }

    sleep(30);
    g_flag = 0;
    pthread_join(tidp1, NULL);
    pthread_join(tidp2, NULL);
    for(i = 0;i < CON_NUM;i++){
        pthread_join(tid[i], NULL);
    }
    pthread_mutex_destroy(&mutexA);
    pthread_mutex_destroy(&mutexB);
    return 0;
}
```

## 多线程全双工

如何通过多线程完成进程间的全双工通讯

基础思路是通过两条命名管道通讯，通讯两侧分别创建读线程和写线程，以完成全双工通讯

这里只提供其中一侧的代码，另一侧只需要交换读写的管道即可。

```c
#include <fcntl.h>
#include <unistd.h>
#include <string.h>
#include <stdio.h>
#include <pthread.h>

#define BUFSIZE 1024

void* thread_read(void* arg){
    int fd = open("2.fifo", O_RDONLY);
    if(fd < 0){
        return NULL;
    }

    char buf[BUFSIZE];
    while(1){
        memset(buf, 0, sizeof(buf));
        read(fd, buf, BUFSIZE);
        if(strcmp(buf, "exit") == 0){
            break;
        }
        printf("bai : %s\n", buf);
    }
    close(fd);
}

void* thread_write(void* arg){
    int fd = open("1.fifo", O_WRONLY);
    if(fd < 0){
        return NULL;
    }

    char buf[BUFSIZE];
    while(1){
        memset(buf, 0, sizeof(buf));
        scanf("%1023[^\n]%*c", buf);
        write(fd, buf, BUFSIZE);
        if(strcmp(buf, "exit") == 0){
            break;
        }
    }
    close(fd);
}

int main(){
    pthread_t tidr, tidw;
    pthread_create(&tidr, NULL, thread_read, NULL);
    pthread_create(&tidw, NULL, thread_write, NULL);
    
    pthread_join(tidr, NULL);
    pthread_join(tidw, NULL);
    return 0;
}
```

## 多线程文件拷贝

通过多线程加速文件的拷贝

基础思路是直接创建多个线程，每个线程复制文件的一部分，多个线程合作完成复制。

```c
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/stat.h>
#include <pthread.h>

#define THREAD_NUM 10
#define BUFSIZE 2048

struct fileMsg{
    const char* inputfile;
    const char* outputfile;
    size_t startPos;
    size_t endPos;
};

void* thread_copy(void* arg){
    struct fileMsg msg = *(struct fileMsg*)arg;
    int fdr = open(msg.inputfile, O_RDONLY);
    if(fdr < 0){
        perror("open read file error ");
        return NULL;
    }

    int fdw = open(msg.outputfile, O_WRONLY);
    if(fdw < 0){
        perror("open write file error ");
        return NULL;
    }
    size_t bytes = msg.endPos - msg.startPos;
    lseek(fdr, msg.startPos, SEEK_SET);
    lseek(fdw, msg.startPos, SEEK_SET);

    char buf[BUFSIZE];
    int ret;
    while(bytes > BUFSIZE){
        ret = read(fdr, buf, BUFSIZE);
        write(fdw, buf, ret);
        
        bytes -= ret;
    }
    ret = read(fdr, buf, BUFSIZE);
    write(fdw, buf, BUFSIZE);

    close(fdr);
    close(fdw);
}

int main(int argc, char* argv[]){
    if(argc < 3){
        printf("参数不足\n");
        return -1;
    }

    struct fileMsg data[THREAD_NUM];

    struct stat msg;
    int ret = stat(argv[1], &msg);
    if(ret < 0){
        perror("stat error ");
        return 1;
    }
    int fd = creat(argv[2], 0664);
    if(fd < 0){
        perror("creat error ");
        return 1;
    }
    close(fd);

    int i;
    for(i = 1;i < THREAD_NUM;i++){
        data[i - 1].endPos = data[i].startPos
        = msg.st_size / THREAD_NUM * i;
        data[i].inputfile = argv[1];
        data[i].outputfile = argv[2];
    }
    data[0].startPos = 0;
    data[THREAD_NUM - 1].endPos = msg.st_size;

    data[0].inputfile = argv[1];
    data[0].outputfile = argv[2];

    pthread_t tid[THREAD_NUM];
    for(i = 0;i < THREAD_NUM;i++){
        pthread_create(tid + i, NULL, thread_copy, (void*)&data[i]);
    }
    for(i = 0;i < THREAD_NUM;i++){
        pthread_join(tid[i], NULL);
    }
    return 0;
}
```

## 分发资源

如果需要对线程进行有序调度，即不再让多个线程竞态获取资源，而是按照次序分发资源，应该使用什么样的方式完成？尝试完成一个让若干线程轮流获取资源的程序。

```c
#include <semaphore.h>
#include <pthread.h>
#include <stdio.h>

#define THREAD_NUM 5
int g_src = 5235;
sem_t sem[THREAD_NUM];

void* consumer(void* arg){
    int i = (long)arg;
    int count = 0;
    while(g_src > 0){
        sem_wait(sem + i);
        if(g_src > 0){
            g_src--;
            count++;
        }
        sem_post(sem + (i + 1) % THREAD_NUM);
    }
    printf("%d : %d\n", i, count);
    return NULL;
}

int main(){
    int i;
    pthread_t tid[THREAD_NUM];
    for(i = 0;i < THREAD_NUM;i++){
        sem_init(sem + i, 0, 0);
        pthread_create(tid + i, NULL, consumer, (void*)(long)i);
    }

    sem_post(sem);

    for(i = 0;i < THREAD_NUM;i++){
        pthread_join(tid[i], NULL);
    }

    for(i = 0;i < THREAD_NUM;i++){
        sem_destroy(sem + i);
    }
    return 0;
}
```

## SQL练习

```mysql
CREATE TABLE `notetext`(
    `id` INT UNSIGNED PRIMARY KEY AUTO_INCREMENT COMMENT '编号',
	`title` VARCHAR(128) NOT NULL DEFAULT '无标题' COMMENT '标题',
    `isprivate` BIT NOT NULL DEFAULT 'FALSE' COMMENT '是否私密',
    `bgc` INT UNSIGNED NOT NULL DEFAULT 0 COMMENT '背景颜色',
    `isAwake` BOOL DEFAULT FALSE COMMENT '是否提醒',
    `AwakeTime` TIMESTAMP COMMENT '提醒时间',
    `createTime` TIMESTAMP NOT NULL COMMENT '创建时间',
    `reviseTime` TIMESTAMP NOT NULL COMMENT '修改时间',
    `group` INT UNSIGNED COMMENT '所属分组' 
);
```

```mysql
CREATE TABLE `notegroup`(
	`id` INT UNSIGNED PRIMARY KEY AUTO_INCREMENT COMMENT '编号',
    `groupname` VARCHAR(128) NOT NULL DEFAULT '默认分组' COMMENT '组名称',
    `createtime` DATETIME COMMENT '创建时间',
    `revisetime` DATETIME COMMENT '修改时间'
);
```

```mysql
SELECT `ename` FROM `emp` WHERE (`sal` > 500 or `job` = 'MANAGER') AND `ename` LIKE 'J%';
SELECT * FROM `emp` ORDER BY `sal` DESC;
SELECT `ename`, `job` FROM `emp` WHERE `job` = 'SALESMAN' ORDER BY `sal` DESC LIMIT 1;
SELECT `ename`, `deptno` FROM `emp` WHERE `comm` IS NULL;
SELECT * FROM `emp` WHERE `hiredate` BETWEEN '1981-01-01 00:00:00' AND '1981-12-31 23:59:59' LIMIT 3;
```

```mysql
(1) 显示职位为SALESMAN中的工资最高的员工的名字和工作岗位
SELECT `ename`, `job` FROM `emp` WHERE `job` = 'SALESMAN' AND `sal` = 
	(SELECT max(sal) FROM `emp` WHERE `job` = 'SALESMAN');
	
(2) 显示所有没有奖金的雇员及其部门
SELECT `ename`, `deptno` FROM `emp` WHERE `comm` IS NULL;

(3) 显示所有不是1981年入职的员工中薪资最高的三人
SELECT `ename`, `sal` FROM `emp` 
	WHERE `hiredate` NOT BETWEEN '1981-01-01 00:00:00' 
	AND '1981-12-31 23:59:59' ORDER BY `sal` DESC LIMIT 3;

(4) 按照年薪给所有人降序排序（需要考虑 comm 列）
SELECT `ename`, `sal` * 12 + IFNULL(`comm`, 0) AS `salary` FROM `emp` 
	ORDER BY `salary` DESC;
	
(5) 给数据表 emp 添加主键，让 empno 成为主键
ALTER TABLE `emp` ADD PRIMARY KEY(`empno`);

(6) 给数据表 emp 添加一般索引，让 ename 成为索引
ALTER TABLE `emp` ADD INDEX(`ename`);

(7) 创建视图：每个员工的编号、姓名、薪资、奖金、年薪
CREATE VIEW `new` AS 
	SELECT `empno`, `ename`, `sal`, `comm`, `sal` * 12 + IFNULL(`comm`, 0) 
	AS `salary` FROM `emp`;

(8) 显示每个部门的平均工资和最高工资，保留两位小数
SELECT `deptno`, FORMAT(AVG(`sal`), 2) AS '平均薪资', MAX(`sal`) AS '最高薪资' 
	FROM `emp` GROUP BY `deptno`;
```

```mysql
(1) （思考题）显示每种岗位的雇员总数，平均工资
SELECT `job`, COUNT(*) AS '人数', FORMAT(AVG(`sal`), 2) AS '平均薪资' 
	FROM `emp` GROUP BY `job`;

(2) （思考题）显示工资高于本部门平均工资的员工信息
SELECT `ename`, `sal`, FORMAT(`avgsal`, 2) AS `avgsal`
	FROM `emp`, (
        SELECT `deptno`, AVG(`sal`) AS `avgsal` FROM `emp` GROUP BY `deptno`
    	) AS `avgtable`
    WHERE emp.deptno = avgtable.deptno AND sal > avgsal;

(3) （思考题）打印每个员工的编号、姓名、薪资、薪资等级
SELECT `empno`, `ename`, `sal`, `grade` FROM `emp`, `salgrade` 
	WHERE `sal` BETWEEN `losal` AND `hisal`;

(4) （思考题）打印每个员工的编号、薪资、领导编号、领导姓名
SELECT e1.empno, e1.sal, e1.mgr, e2.ename 
	FROM `emp` AS e1, `emp` AS e2 
	WHERE e1.mgr = e2.empno; 

(5) （思考题）打印每个部门薪资最高的员工及其部门名和薪资信息。如果有多个并列最高，则全部打出
SELECT `ename`, `sal`, emp.deptno, dept.dname
	FROM `emp`, `dept`, (
        SELECT `deptno`, max(`sal`) AS `maxsal`
         	FROM `emp` 
        	GROUP BY `deptno`
        ) AS `gms`
    WHERE emp.deptno = gms.deptno AND emp.sal = gms.maxsal 
    AND emp.deptno = dept.deptno ORDER BY deptno;
    
SELECT `ename`, `sal`, emp.deptno, dept.dname
	FROM `emp`, `dept`
	WHERE (emp.deptno, sal) IN(
    	SELECT `deptno`, max(`sal`) AS `maxsal`
         	FROM `emp` 
        	GROUP BY emp.deptno
        ) AND emp.deptno = dept.deptno ORDER BY emp.deptno;
    
```





# C/C++练习题

## Fibonacci数列

Fibonacci数列是这样定义的：

```c++
F[0] = 0
F[1] = 1
for each i ≥ 2: F[i] = F[i-1] + F[i-2]
```

因此，Fibonacci数列就形如：0, 1, 1, 2, 3, 5, 8, 13, ...，在Fibonacci数列中的数我们称为Fibonacci数。给你一个N，你想让其变为一个Fibonacci数，每一步你可以把当前数字X变为X-1或者X+1，现在给你一个数N求最少需要多少步可以变为Fibonacci数。

**输入描述：**

输入为一个正整数$N(1 \le N \le 1,000,000)$

**输出描述：**

输出一个最小的步数变为Fibonacci数

**示例：**

**输入：**

```
15
```

**输出：**

```
2
```

```c
#include <stdio.h>
#define MIN(a, b) ((a) < (b) ? (a) : (b)) 

int main(){
	int an, an_2 = 0, an_1 = 1;
	int n;
	scanf("%d", &n);
	an = an_1 + an_2;
   	while(an < n){
 		an_2 = an_1;
        an_1 = an;
        an = an_1 + an_2;
    }
    printf("%d\n", MIN(an - n, n - an_1));
    return 0;
}
```

## 完全数计算

完全数（Perfect number），又称完美数或完备数，是一些特殊的自然数。

它所有的真因子（即除了自身以外的约数）的和（即因子函数），恰好等于它本身。

例如：28，它有约数1、2、4、7、14、28，除去它本身28外，其余5个数相加，1+2+4+7+14=28。

输入n，请输出n以内(含n)完全数的个数。

**输入描述：**

输入一个数字n$(1 \le n \le 5 *10^5)$

**输出描述：**

输出不超过n的完全数

**示例**

**输入：**

```
1000
```

**输出**

```
6
28
496
```

```c
#include <stdio.h>
#include <math.h>
int main(){
    int n;
    scanf("%d", &n);
    for(int i = 2;i < n;i++){
    	int sum = 1;
        int tmp = sqrt(i);
        for(int j = 2;j <= tmp;j++){
         	if(i % j == 0){
              	sum += j;
                sum += i / j;
            }
        }
        if(tmp * tmp == i){
            sum -= tmp;
        }
        if(sum == i){
            printf("%d\n", i);
     	}
   	}
   	return 0;
}
```

## 问卷调查

**题目描述**

你在学校请一些同学一起做一项问卷调查，为了实验的客观性，你先用计算机生成了 `N` 个 1 到 1000 之间的随机整数 ，对于其中重复的数字，只保留一个，把其余相同的数去掉，不同的数对应着不同的学生的学号。然后再把这些数从小到大排序，按照排好的顺序去找同学做调查。请你完成“去重”与“排序”的工作。

**输入格式**

输入有两行，第 1 行为 1 个正整数，表示所生成的随机数的个数 。

第 2 行有 `N` 个用空格隔开的正整数，为所产生的随机数。

**输出格式**

输出也是两行，第 1 行为 1 个正整数 `N` ，表示不相同的随机数的个数。

第 2 行为 `M` 个用空格隔开的正整数，为从小到大排好序的不相同的随机数。

**样例**

**样例输入** 

```
10
20 40 32 67 40 20 89 300 400 15
```

**样例输出** 

```
8
15 20 32 40 67 89 300 400
```

```c++
#include <iostream>
#include <set>
using namespace std;

void show(set<int> st){
 	for(auto it = st.begin();it != st.end();it++){
		cout << *it << " ";
	}
	cout << endl;
}

int main(){
	int n;
	cin >> n;
    set<int> st;
    for(int i = 0;i < n;i++){
       	int tmp;
        cin >> tmp;
        st.insert(tmp);
    }
    cout << st.size() << endl;
    show(st);
    return 0;
}
```

```c++
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
	int n;
    cin >> n;
    vector<int> vi(n);
    int i;
    for (auto& e:vi) {
        cin >> e;
    }
    sort(vi.begin(), vi.end());
    auto it = unique(vi.begin(), vi.end());
    vi.resize(it - vi.begin());

    cout << vi.size() << endl;
    for (auto& e : vi) {
        cout<< e << " ";
    }
    cout << endl;
    return 0;
}
```

## 目标数字

**题目描述**

输入一个目标数字 $n$，只允许你使用乘2操作和加1操作（这两种操作每次操作记一次）到达这个数字 $n$ 。

**输入格式**

共一行，为目标数字 $n$

**输出格式**

共一行，所需操作的次数

**样例**

| **样例输入** | **样例输出** |
| :----------: | :----------: |
|      10      |      5       |
|    114514    |      27      |

```c
#include <stdio.h>

int main(){
	long long n;
    int count = 0;
	scanf("%lld", &n);
	while(n > 0){
		if(n % 2){
			n--;	
		}
		else{
			n /= 2;
		}
		count++;
	}
	printf("%d\n", count);
	return 0;
}
```

```c
#include <stdio.h>

int main(){
    long long n;
    scanf("%lld", &n);
    int count = 0;
    for(int i = 0;i;i /= 2){
        count += i % 2;
        count++;
    }
    printf("%d\n", count - 1);
    return 0;
}
```

## 凯撒密码

**题目描述**

凯撒生活在充满危险和阴谋的年代。为了生存，凯撒首次发明了密码，用于军队的消息传递。假设你是他军团中的一名军官，需要把发送来的消息破译出来、并提供给你的将军。

消息加密的办法是：对消息原文中的每个字母，分别用该字母之后的第5个字母替换（例如：消息原文中的每个字母A都分别替换成字母F），其他字符不变，并且消息原文的所有字母都是大写的。密码中的字母与原文中的字母对应关系如下。

密码字母：A B C D E F G H I J K L M N O P Q R S T U V W X Y Z

原文字母：V W X Y Z A B C D E F G H I J K L M N O P Q R S T U

**输入格式**

共一行，为收到的密文。密文仅有空格和大写字母组成。

**输出格式**

共一行，解密后的明文。

**样例**

**样例输入**

```
HELLO CAESAR
```

**样例输出**

```
CZGGJ XVZNVM
```

```c++
#include <iostream>
#include <string>
using namespace std;
 
#define POS 5

int main(){
	string s;
	getline(cin, s);
	for(auto& e : s){
		if(e == ' '){
			cout << e;
			continue;
		}
		cout << (char)(((e - 'A') - POS + 26) % 26 + 'A');
	}
	cout << endl;
	return 0;
}
```

## 完全平方数

**题目描述**

一个整数 $a$ 是一个完全平方数，是指它是某一个整数的平方，即存在一个整数 $b$ ，使得 $a = b^2$ 。给定一个正整数 $n$ ，请找到最小的正整数 $x$ ，使得它们的乘积是一个完全平方数。

**输入格式**

输入一行包含一个正整数 $n$ 。

**输出格式**

输出找到的最小的正整数 $x$ 。

**样例**

| **样例输入** | **样例输出** |
| :----------: | :----------: |
|      12      |      3       |
|      15      |      15      |

```c++
#include <iostream>
#include <map>
#include <cmath>
using namespace std;

int main(){
    long long n;
    cin >> n;
    map<long long, int> m;
    for(long long i = 2; i <= sqrt(n); i++){
        if(n % i == 0){
            m[i]++;
            n /= i;
            i--;
        }
    }
    m[n]++;
    long long sum = 1;
    for(auto& e : m){
        if(e.second % 2 == 1){
            sum *= e.first;
        }
    }
 	cout << sum << endl;   
	return 0;
}
```

## 两个数的积和差

**题目描述**

两个数 $a$、$b$ 的积为 $m$ ，差为 $n$ ，编写程序得到 $a$ 和 $b$ 。

**输入格式**

共一行，$m$ 和 $n$ ，保证输入有解。

**输出格式**

共一行，$a$ 和 $b$ ，较小数在前。

**样例**

| 样例输入 | 样例输出 |
| :------: | :------: |
|   15 2   |   3 5    |
| 1155 62  |  15 77   |

```c
#include <stdio.h>
#include <math.h>

int main(){
    long long m, n, a, b;
    scanf("%lld %lld", &m, &n);
    for(int i = 2;i <= sqrt(m);i++){
        b = i + n;
        if(i * b == m){
            a = i;
            break;
        }
    }
    printf("%lld %lld\n", a, b);
    return 0;
}
```

```c++
#include <iostream>
#include <cmath>
using namespace std;

long long binaryAnswer(long long m, long long n){
	long long left = 1;
	long long right = sqrt(m);
	long long mid;
	while(left <= right){
		mid = (left + right) / 2;
		if(m / mid - mid < n){
			right = mid - 1;
		}
		else if(m / mid - mid > n){
			left = mid + 1; 
		}
		else {
			return mid;
		}
	}	
	return -1;
}

int main(){
	long long m, n;
	cin >> m >> n;
	int ret = binaryAnswer(m, n);
	cout << ret << " " << m / ret << endl;
	return 0;
}
```

## 奖项统计

**题目描述**

虽然 Miss Medusa 到了北京，领了科技创新奖，但是她还是觉得不满意。原因是：她发现很多人都和她一样获了科技创新奖，特别是其中的某些人，还获得了另一个奖项——特殊贡献奖。而越多的人获得了两个奖项，Miss Medusa 就会越眼红。于是她决定统计有哪些人获得了两个奖项，来知道自己有多眼红。

**输入格式**

第一行两个整数 $n,m$，表示有 $n$ 个人获得科技创新奖，$m$ 个人获得特殊贡献奖。

第二行 $n$ 个正整数，表示获得科技创新奖的人的编号。

第三行 $m$ 个正整数，表示获得特殊贡献奖的人的编号。

**输出格式**

输出一行，为获得两个奖项的人的编号，按在科技创新奖获奖名单中的先后次序输出。

**输入输出样例**

**输入**

```
4 3
8 15 6 2
2 9 8
```

**输出**

```
8 2
```

```c++
//  map实现
#include <iostream>
#include <vector>
#include <map>
using namespace std;

int main(){
	int n, m;
	map<long long, int> ma;
	vector<long long> v;
	cin >> n >> m;
	long long tmp;
	for(int i = 0;i < n;i++){
		cin >> tmp;
		ma[tmp]++;
		v.push_back(tmp);
	}
	for(int j = 0;j < m;j++){
		cin >> tmp;
		ma[tmp]++;
	}
	for(auto& e : v){
		if(ma[e] > 1){
			cout << e << " ";
		}
	}
	return 0;
}
```

```c
//  set实现
#include <iostream>
#include <vector>
#include <set>
using namespace std;

int main() {
    int n, m;
    set<long long> s2;
    vector<long long> v;
    long long tmp;

    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        cin >> tmp;
        v.push_back(tmp);
    for (int j = 0; j < m; j++) {
        cin >> tmp;
        s2.insert(tmp);
    }
    // 遍历第一组原始序列，用set.count判断是否在第二组中
    for (auto num : v) {
        if (s2.count(num)) {
            cout << num << " ";
        }
    }
    return 0;
}
```

## 金币

**题目描述**

国王将金币作为工资，发放给忠诚的骑士。第一天，骑士收到一枚金币；之后两天（第二天和第三天），每天收到两枚金币；之后三天（第四、五、六天），每天收到三枚金币；之后四天（第七、八、九、十天），每天收到四枚金币……；这种工资发放模式会一直这样延续下去：当连续 $n$ 天每天收到 $n$ 枚金币后，骑士会在之后的连续 $n+1$ 天里，每天收到 $n+1$ 枚金币。

请计算在前 $k$ 天里，骑士一共获得了多少金币。

**输入格式**

一个正整数 $k$，表示发放金币的天数。

**输出格式**

一个正整数，即骑士收到的金币数。

**输入输出样例**

| 样例输入 | 样例输出 |
| :------: | :------: |
|    6     |    14    |
|   1000   |  29820   |

```c
#include <stdio.h>

int main(){
    int k;
    scanf("%d", &k);
    int sum = 0, tmp = 1, days = 1;
    for(int i = 1;i <= k;i++){
        sum += tmp;
        if(days == i){
            tmp++;
            days += tmp;
        }
    }
    printf("%d\n", sum);
    return 0;
}
```

```c
#include <stdio.h>
int main()
{
	int n;
	scanf("%d", &n);
	int tmp = 1, sum = 0;
	int i;
	//先把整个阶段的加上
	for (i = 1; i <= n; i += tmp)
	{
		sum += tmp * tmp;
		tmp++;
	}
	//再计算最后一个不完整阶段的工资
	sum += (n - i + tmp) * tmp;
	printf("%d\n", sum);
	return 0;
}
```

## 数据库连接池

Web系统通常会频繁地访问数据库，如果每次访问都创建新连接，性能会很差。为了提高性能，架构师决定复用已经创建的连接。当收到请求，并且连接池中没有剩余可用的连接时，系统会创建一个新连接，当请求处理完成时该连接会被放入连接池中，供后续请求使用。

现在提供你处理请求的日志，请你分析一下连接池最多需要创建多少个连接。

**输入描述**

数据第一行包含一个正整数 $n(1\le n \le 1000)$，表示请求的数量。

紧接着n行，每行包含一个请求编号 $id(A、B、C......)$ 和操作（connect或disconnect）。

**输出描述**

输出连接池最多需要创建多少个连接。

**输入输出样例**

**输入**

```
6
A connect
A disconnect
B connect
C connect
B disconnect
C disconnect
```

**输出**

```
2
```

```c++
#include <iostream>
#include <string>
using namespace std;

int main(){
	int n, count = 0, max = 0;
	string id, con;
	cin >> n;
	for(int i = 0;i < n;i++){
		cin >> id >> con;
		if(con.compare("connect") == 0){
			count++;
			max = (count > max ? count : max);
		}
		else{
			count--;
		}
	}
	cout << max << endl;
	return 0;
} 
```

## 阅读理解

**题目描述**

英语老师留了 $N$ 篇阅读理解作业，但是每篇英文短文都有很多生词需要查字典，为了节约时间，现在要做个统计，算一算某些生词都在哪几篇短文中出现过。

**输入格式**

第一行为整数 $N$，表示短文篇数，其中每篇短文只含空格和小写字母。

按下来的 $N$ 行，每行描述一篇短文。每行的开头是一个整数 $L$ ，表示这篇短文由 $L$ 个单词组成。接下来是 $L$ 个单词，单词之间用一个空格分隔。

然后为一个整数 $M$ ，表示要做几次询问。后面有 $M$ 行，每行表示一个要统计的生词。

**输出格式**

对于每个生词输出一行，统计其在哪几篇短文中出现过，并按从小到大输出短文的序号，序号不应有重复，序号之间用一个空格隔开（注意第一个序号的前面不应有空格）。如果该单词一直没出现过，则输出一个空行。

**输入输出样例**

**输入**

```
3
9 you are a good boy ha ha o yeah
13 o my god you like bleach naruto one piece and so do i
11 but i do not think you will get all the points
5
you
i
o
all
naruto
```

**输出**

```
1 2 3
2 3
1 2
3
2
```

```c++
#include <iostream>
#include <string>
#include <vector>
#include <unordered_set> 
using namespace std;

int main(){
    string tmp;
    int n, l;
    cin >> n;
    vector<unordered_set<string>> vss(n);
    for(auto& e : vss){
		cin >> l;
		for(int i = 0;i < l;i++){
			cin >> tmp;
			e.insert(tmp);
		}
	}
    int m;
    cin >> m;
    for(int i = 0; i < m; i++){
        cin >> tmp;
    	for(int j = 0;j < vss.size();j++){
			if(vss[j].count(tmp)){
				cout << j + 1 << " ";
			}
		}
		cout << endl;
	}
    
    return 0;
}
```

## 五子棋

给你一个棋局，请你帮忙判断其中有没有五子连珠（超过五颗也算）。

**输入格式**

数据为一张20x20的棋盘。

其中黑子用“*”表示，白子用“+”表示，空白位置用“.”表示。

**输出格式**

如果棋盘上存在五子连珠（无论哪种颜色的棋子），输出“Yes”，否则输出“No”。

**样例**

**样例输入**

```
....................
....................
....................
....................
......*.............
.......*............
........*...........
....++++.*..........
..........*.........
....................
....................
....................
....................
....................
....................
....................
....................
....................
....................
....................
```

**样例输出**

```
Yes
```

```c++
#include <iostream>
#include <vector>
#include <string>
using namespace std;
// cb 为 cb 棋盘 
bool Right(const vector<string>& cb, int x, int y){
	if(y > cb[x].size() - 15){
		return false;
	}
	
	char ch = cb[x][y];
	for(int i = 1;i < 5;i++){
		if(cb[x][y + i] != ch){
			return false;
		}
	}
	return true;
}

bool RightDown(const vector<string>& cb, int x, int y){
	if(x > cb.size() - 15 && y > cb[x].size() - 15){
		return false;
	}
	
	char ch = cb[x][y];
	for(int i = 1;i < 5;i++){
		if(cb[x + i][y + i] != ch){
			return false;
		}
	}
	return true;
}

bool Down(const vector<string>& cb, int x, int y){
	if(x > cb.size() - 15){
		return false;
	}
	
	char ch = cb[x][y];
	for(int i = 1;i < 5;i++){
		if(cb[x + i][y + i] != ch){
			return false;
		}
	}
	return true;
}

bool LeftDown(const vector<string>& cb, int x, int y){
	if(x > cb.size() - 15 || y < cb[x].size() - 15 - 1){
		return false;
	}
	
	char ch = cb[x][y];
	for(int i = 1;i < 5;i++){
		if(cb[x + i][y - i] != ch){
			return false;
		}
	}
	return true;
}

bool isFive(const vector<string>& cb, int x, int y){
	if(cb[x][y] == '.'){
		return false;
	}
	
	return Right(cb, x, y) || RightDown(cb, x, y) ||
			Down(cb, x, y) || LeftDown(cb, x, y);
}

int main(){
	vector<string> vstr(20);
	for(auto& e : vstr){
		cin >> e;
	}
	int i, j;
	for(i = 0;i < vstr.size();i++){
		for(j = 0;j < vstr.size();j++){
			if(isFive(vstr, i, j)){
				cout << "Yes" << endl;
				return 0;
			}
		}
	}
	cout << "No" << endl;
	return 0;
}
```

## 谁拿了最多奖学金

**题目描述**

某校的惯例是在每学期的期末考试之后发放奖学金。发放的奖学金共有五种，获取的条件各自不同：

1. 院士奖学金，每人 $8000$ 元，期末平均成绩高于 $80$ 分（$\ge 80$），并且在本学期内发表 篇或 篇，以上论文的学生均可获得；

2. 五四奖学金，每人 $4000$ 元，期末平均成绩高于 $85$ 分（$\ge 85$），并且班级评议成绩高于 $80$ 分（$\ge 80$）的学生均可获得；

3. 成绩优秀奖，每人 $2000$ 元，期末平均成绩高于 $90$ 分（$\ge 90$）的学生均可获得；

4. 西部奖学金，每人 $1000$ 元，期末平均成绩高于 $85$ 分（$\ge 85$）的西部省份学生均可获得；

5. 班级贡献奖，每人 $850$ 元，班级评议成绩高于 $80$ 分（$\ge 80$）的学生干部均可获得；

只要符合条件就可以得奖，每项奖学金的获奖人数没有限制，每名学生也可以同时获得多项奖金。例如姚林的期末平均成绩是 $87$ 分，班级评议成绩 $82$ 分，同时他还是一位学生干部，那么他可以同时获得五四奖学金和班级贡献奖，奖金总数是 $4850$ 元。

现在给出若干学生的相关数据，请计算哪些同学获得的奖金总数最高（假设总有同学能满足获得奖学金的条件）。

**输入格式**

第一行是 $1$ 个整数 $N$ ，表示学生的总数。

接下来的 $N$ 行每行是一位学生的数据，从左向右依次是姓名，期末平均成绩，班级评议成绩，是否是学生干部，是否是西部省份学生，以及发表的论文数。姓名是由大小写英文字母组成的长度不超过 $20$ 的字符串（不含空格）；期末平均成绩和班级评议成绩都是 $0$ 到 $100$ 之间的整数（包括 $0$ 和 $100$）；是否是学生干部和是否是西部省份学生分别用 $1$ 个字符表示，$Y$ 表示是，$N$ 表示不是；发表的论文数是 到的整数（包括 $0$ 和 $10$）。每两个相邻数据项之间用一个空格分隔。

**输出格式**

第 1 行是获得最多奖金的学生的姓名。如果有两位或两位以上的学生获得的奖金最多，输出他们之中在输入文件中出现最早的学生的姓名。

第 2 行是这名学生获得的奖金总数。

第 3 行是这 个学生获得的奖学金的总数。

**输入输出样例**

**输入**

```
4
YaoLin 87 82 Y N 0
ChenRuiyi 88 78 N Y 1
LiXin 92 88 N N 0
ZhangQin 83 87 Y N 1
```

**输出**

```
ChenRuiyi
9000
28700
```

```c++
#include <iostream>
using namespace std;

int main(){
	int n;
	cin >> n;
	string name, maxname;
	int avgscore, classcore;
	string isleader, iswest;
	int num;
	int sum = 0, allsum = 0, max = 0;
	for(int i = 0;i < n;i++){
		cin >> name >> avgscore >> classcore >> isleader >> iswest >> num; 
		if(avgscore > 80 && num >= 1){
			sum += 8000;
		}
		if(avgscore > 85 && classcore > 80){
			sum += 4000;
		}
		if(avgscore > 90){
			sum += 2000;
		}
		if(avgscore > 85 && iswest == "Y"){
			sum += 1000;
		}
		if(classcore > 80 && isleader == "Y"){
			sum += 850;
		}
		allsum += sum;
		if(sum > max){
			max = sum;
			maxname = name;
		}
		sum = 0; 
	}
	cout << maxname << endl << max << endl << allsum << endl;
	return 0;
} 
```

## 第一个只出现一次的字符

找出字符串中第一个只出现一次的字符

数据范围：输入的字符串长度满足 $1\le n \le 1000$

**输入描述：**

输入一个非空字符串

**输出描述：**

输出第一个只出现一次的字符，如果不存在输出-1

**示例：**

**输入：**

```
asdoasdf
```

**输出：**

```
o
```

```c++
#include <iostream>
#include <string>
#include <map>
#include <vector>
using namespace std;

int main(){
	string s, tmp = "-1";
	cin >> s;
	vector<char> vc;
	map<char, int> msi; 
	for(int i = 0; i < s.size();i++){
		vc.push_back(s[i]);
		msi[s[i]]++;
	}
	for(auto& e : vc){
		if(msi[e] == 1){
			tmp = e;
			break;
		}
	}
	cout << tmp << endl;
	return 0;
} 
```

## 合并序列

**题目描述**

有 $N$ 个单词和字符串 ，按字典序输出以字符串 $T$ 为前缀的所有单词。

**输入格式**

输入文件第一行包含一个正整数 $N$ ；

接下来 $N$ 行，每行一个单词，长度不超过 $100$ ；
最后一行包含字符串 $T$ 。

**输出格式**

按字典序升序输出答案。

**输入输出样例**

**输入**

```
6
na
no
ki
ki
ka
ku
k
```

**输出**

```
ka
ki
ki
ku
```

```c++
#include <iostream>
#include <string>
#include <set>
using namespace std;

int main(){
	int n;
	string tmp;
	cin >> n;
	multiset<string> mstr;
	for(int i = 0;i < n;i++){
		cin >> tmp;
		mstr.insert(tmp);
	}
	string t;
	cin >> t;
	for(auto& e : mstr){
		if(e.compare(0, t.size(), t) == 0){
			cout << e << endl;
		}
	}
	return 0;
}
```

## 观星

**题目描述**

Jimmy 和 Symbol 约好一起看星星，浩瀚的星空可视为一个长为 $N$、宽为 $M$ 的矩阵，矩阵中共有 $N\times M$ 个位置，一个位置可以用坐标 $(i,j)$（$1\le i\le N$，$1\le j\le M$）来表示。每个位置上可能是空的，也可能有一个星星。

对于一个位置 $(i,j)$，与其相邻的位置有左边、左上、上面、右上、右边、右下、下面、左下 8 个位置。相邻位置上的星星被视为同一个星座，这种关系有传递性，例如若 $(1,1),(1,2),(1,3)$ 三个位置上都有星星，那么这三个星星视为同一个星座。包含的星星数量相同的星座被视为一个星系（一个星系中的星座不一定相邻），星系的大小为星系中包含的所有星星数量。

由于 Symbol 太喜欢星系了，他就想考一考 Jimmy，让 Jimmy 求出星空中有多少个星系，他还想知道，最大的星系有多大。

**输入格式**

第一行两个整数 $N,M$ 表示矩阵的长宽。

接下来 $N$ 行每行 $M$ 个字符，每个字符只可能是 `.` 或 `*` 。这 $N$ 行中第 $i$ 行的第 $j$ 个字符是 `*` 表示位置 $(i,j)$ 上有一个星星，否则表示它是空的。

**输出格式**

仅一行两个整数，用空格分隔开，分别表示星系的数量与最大星系的大小。

**输入输出样例 #1** 

**输入 #1**

```
5 7
*......
..**..*
.*...*.
...*...
....*..
```

**输出 #1**

```
3 4
```

**输入输出样例 #2**

**输入 #2**

```
10 10
**..**.**.
***....*..
*...**.**.
...*..*...
..........
**...**.*.
..*.*....*
..........
***..*.*..
.***..*...
```

**输出 #2**

```
4 12
```

```c
#include <iostream>
#include <vector>
#include <string>
#include <unordered_map>
#include <algorithm>
using namespace std;

int dx[8] = { 0,0,1,1,1,-1,-1,-1 }, dy[8] = { 1,-1,1,0,-1,1,0,-1 };
void countStarNum(vector<string>& v, int x, int y, int& res) {
	if (x < 0 || x >= v.size() ||
		y < 0 || y >= v[x].size()|| 
		v[x][y] == '.') {
		return;
	}
	res++;
	v[x][y] = '.';
	for (int i = 0; i < 8; i++) {
		int xx = x + dx[i], yy = y + dy[i];
		countStarNum(v, xx, yy, res);
	}
}
int main() {
	int n, m;
	cin >> n >> m;
	vector<string> vstr(n);
	unordered_map<int, int> mii;
	for (auto& e : vstr) {
		cin >> e;
	}
	int res, maxStar = 0;
	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++) {
			if (vstr[i][j] == '*') {
				res = 0;
				countStarNum(vstr, i, j, res);
				if (res > 0) {
					mii[res]++;
				}
			}
		}
	}
	for (auto& e : mii) {
		maxStar = max(maxStar, e.first * e.second);
	}
	cout << mii.size() << " " << maxStar << endl;;
	return 0;
}
```

