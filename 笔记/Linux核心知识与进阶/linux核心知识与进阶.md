## 



# 二、linux核心知识与进阶

## 1、正则表达式

### （1）bash特性

1.bash是一个命令处理器，能在运行的文本窗口中，执行用户输入的命令

2.bash还能从文件中读取linux脚本

3.bash支持通配符、管道、命令替换、条件判断等逻辑控制语句

- 命令行展开

```shell
[root@localhost /]# echo {syh,qq,meimei}
syh qq meimei

[root@localhost /]# echo suyonghong{88,66}
suyonghong88 suyonghong66

[root@localhost /]# echo suyonghong{1..5}
suyonghong1 suyonghong2 suyonghong3 suyonghong4 suyonghong5

[root@localhost /]# echo suyonghong{1..10..2}
suyonghong1 suyonghong3 suyonghong5 suyonghong7 suyonghong9

[root@localhost /]# echo suyonghong{01..10..2}
suyonghong01 suyonghong03 suyonghong05 suyonghong07 suyonghong09
```

- 命令别名

```shell
alias 	#查看别名
unalias	#取消别名
```

- 命令历史

```shell
history	#查看历史命令
！行号	#重复执行命令
！！	#执行上一次命令
```

- 命令补全

```shell
tab键	#补全
```

- 文件路径补全

```shell
[root@localhost /]# cd /etc/sysconfig/network-scripts/
```

**利用{}进行备份**

```shell
[root@localhost etc]# cp /etc/hosts{,.bak}    将这个/etc/hosts文件内容  备份到/etc/hosts.bak文件中
[root@localhost etc]# cat hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
[root@localhost etc]# 
[root@localhost etc]# 
[root@localhost etc]# cat hosts.bak 
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain6
```



### （2）基本正则与扩展正则表达式

基本正则表达式：BRE集合

扩展正则表达式：ERE集合

gerp -E



基本正则表达式BRE集合

- 匹配字符
- 匹配次数
- 位置锚定

|   符号    | 作用                                                         |
| :-------: | :----------------------------------------------------------- |
|     ^     | 尖角符，用于模式的最左侧，如“^oldboy”，匹配以oldboy单词开头的行 |
|     $     | 美元符，用于模式的最右侧，如“oldboy$”，匹配以oldboy结尾的行  |
|    ^$     | 组合符，表示空行                                             |
|     .     | 匹配任意一个且只有一个字符                                   |
|     \     | 转意字符，让特殊含义的字符，出现原形，还原本意，例如         |
|     *     | 匹配前一个字符（连续出现）0次或1次以上，重复0次代表空，即匹配所有内容 |
|    .*     | 组合符，匹配任意长度的任意字符                               |
|    ^.*    | 组合符，匹配以任意多个字符开头的内容                         |
|    .*$    | 组合符，匹配以任意多个字符结尾的内容                         |
|   [abc]   | 匹配[集合内的任意一个字符，a或b或c；可以写[]a-c]             |
|  [^abc]   | 匹配除了^后面的任意字符，a或b或c，^表示对[abc]取反           |
| <pattern> | 匹配完整内容                                                 |
|   <或>    | 定位单词的左、右侧，如<chao>，可找出“chao ge”却找不到“yuchao” |

扩展正则集合

扩张正则必须使用grep -E才能生效

| 字符   | 作用                                         |
| ------ | -------------------------------------------- |
| +      | 匹配前一个字符1次或多次，前面字符至少出现1次 |
| [:/]+  | 匹配括号内的“:”或"/"字符1次或多次            |
| ？     | 匹配前一个字符0次或多次，前面字符可有可无    |
| \|竖线 | 表示或者，同时过滤多个字符串                 |
| （）   | 分组过滤，被括起来的内容表示一个整体         |
| a{n,m} | 匹配前一个字符最少n次，最多m次               |
| a{n,}  | 匹配前一个字符最少n次                        |
| a{n}   | 匹配前一个字符正好n次                        |
| a{,m}  | 匹配前一个字符正最多n次                      |

​	

### （3）grep与正则表达式

grep:文本过滤工具（模式：pattern）作用：根据用户指定的”模式（过滤条件）“对目标文件逐行进行匹配检查，打印匹配的行

语法：grep [options] [pattern]   file

​					参数      模式（条件）  文件数据

参数：

- -f：忽略字符的大小写；

- -o：仅显示匹配到的字符串本身

- -v：显示不能被匹配到的行；也就是取反显示

- -E：支持使用扩展正则表达式元字符；

- -q：静默模式，即不输出任何信息；

  

| 参数选项     | 解释说明               |
| ------------ | ---------------------- |
| -v           | 排除匹配的结果         |
| -n           | 显示匹配的行号         |
| -i           | 不区分大小写           |
| -c           | 只统计匹配的行数       |
| -E           | 使用egrep命令          |
| --color=auto | 为grep过滤结果添加颜色 |
| -w           | 只匹配过滤的单词       |
| -o           | 只输出匹配的的内容     |



案例

#找出有关login的行，并显示行号

```shell
[root@localhost /]# grep "login" /etc/passwd -n
2:bin:x:1:1:bin:/bin:/sbin/nologin
3:daemon:x:2:2:daemon:/sbin:/sbin/nologin
```

#找出没有login的行，且显示行号

```shell
[root@localhost /]# grep "login" /etc/passwd -n -v
1:root:x:0:0:root:/root:/bin/bash
6:sync:x:5:0:sync:/sbin:/bin/sync
```

#找出ROOT有关的行

```shell
[root@localhost /]# grep 'ROOT' /etc/passwd -n -i
1:root:x:0:0:root:/root:/bin/bash
10:operator:x:11:0:operator:/root:/sbin/nologin
```

#同时找出root和sync的行

```shell
[root@localhost /]# grep -E 'root|sync' /etc/passwd -n 
1:root:x:0:0:root:/root:/bin/bash
6:sync:x:5:0:sync:/sbin:/bin/sync
10:operator:x:11:0:operator:/root:/sbin/nologin
```

#统计匹配结果的行数

```shell
[root@localhost /]# grep 'root' /etc/passwd -c
2
```

#只输出匹配出的呢容

```shell
[root@localhost /]# grep 'login' /etc/passwd -0 -n
2:bin:x:1:1:bin:/bin:/sbin/nologin
3:daemon:x:2:2:daemon:/sbin:/sbin/nologin
```

#完整匹配整个单词

```shell
[root@localhost /]# grep 'oldboy' test.txt  -w
oldboy
```

#过滤掉空白行和注释行

```shell
[root@localhost /]# grep -Ev '^$|^#' test.txt -n
2:wo  aini
3:oldboy
4:chaoshen
5:yucha
6:  
```

正则表达式实践

**^符号**

#.输出所有以w开头的行

```shell
[root@localhost /]# grep '^w' test.txt -i -n
2:wo  aini
8:wo shi yizhi xiaoxiaoniao
```

**$符号**

#.输出所有以i结尾的行

```shell
[root@localhost /]# grep 'i$' test.txt -i -n
2:wo  aini
```

tip

- 在Linux平台上，所有文件里的内容的结尾都有一个￥符
- 可用cat -A查看

**\转义符**

#.找出所有以点结尾的行

```shell
[root@localhost /]# grep '\.$' test.txt -n
5:yucha.
10:chuang qian mking yue guang.
```



^$组合符

#找出文件内的空行以及行号

```shell
[root@localhost /]# grep '^$' test.txt -n		#行内带有空格的不属于空行
1:
2:
3:
```

.点符号

#匹配文件中的任意一个字符，有且只有一个字符

```shell
[root@localhost /]# grep '.' test.txt  -n -i
4:wo  aini
5:oldboy
6:chaoshen
7:yucha.
8:               
9:#sdafafafo
```

#匹配出‘.h’的任意两个字符

```shell
[root@localhost /]# grep '.h' test.txt  -n -i
6:chaoshen
7:yucha.
11:wo shi yizhi xiaoxiaoniao
```

*符号

#找出前一个字符h出现0次或多次

```shell
[root@localhost /]# grep 'h*' test.txt -n -i
```

.*组合符

点表示匹配任意一个字符，*则表示匹配前面的字符匹配一次或多次，即匹配所有字符

```shell
[root@localhost /]# grep '.*' test.txt -n 
1:
2:
3:
4:wo  aini
5:oldboy
6:chaoshen
7:yucha.
```

^.*o符号

^:匹配什么开头；.表示任意一个字符；*表示所有内容；o：	普通字符，表示匹配到o字母结束，也叫贪婪匹配

```shell
[root@localhost /]# grep '^.*y' test.txt -n -o
5:oldboy
7:y
11:wo shi y
14:chuang qian mking y
15:y
```

[abc]中括号

[abc]：表示匹配括号内字符a或b或c

表现显示：

[a-z]：匹配小写字母a到z

[A-Z]：匹配大写字母a到z

[a-zA-Z]：匹配小写、大写字母a到z

[0-9]：匹配数字0到9

[a-zA-Z0-9]：匹配大小写字母、数字0-9

```shell
[root@localhost /]# grep '[a-z]' test.txt 
```

grep 参数-o

表示显示匹配出的字符，而不将整行匹配显示

#匹配出a在文件中出现的次数

```shell
[root@localhost /]# grep -o 'a' test.txt|wc -l
20
```

[^abc]

表示除了括号内字符a或b或c的内容

#找出除了小写字母a-z之外的内容

```shell
[root@localhost /]# grep '[^a-z]' test.txt 
```

扩展正则表达式实践

tip：需要使用grep -E

+号

表示匹配前一个字符一次或多次

```SHELL
[root@localhost /]# grep -E 'a+' test.txt 
```

?号

表示匹配前一个字符出现0次或1次

```shell
[root@localhost /]# grep -E 'w?' test.txt 
```

|号

在正则中表示或的意思

#找出文件中包含a或b的字符

```shell
[root@localhost /]# grep  -i -E 'a|b' test.txt
```

（）小括号

表示将一个或多个字符捆绑在一起，当作整体进行处理

作用：

1.分组过滤被括起来，括号内的内容就是一个整体

2.被（）括起来的内容，可以被后面的“\n”正则引用，n为数字，表示应用第几个括号的内容

​	\1:表示从左侧起引用第1个括号中的模式所匹配到的字符

​	\2:表示从左侧起引用第2个括号中的模式所匹配到的字符

案例

#找出包含good和glad的行

```shell
[root@localhost /]# grep -E 'g(oo|la)d' test.txt 
good
glad
```

#引用括号内的内容·

```shell
[root@localhost /]# grep -E '(l..e).*\1' lover.txt 		表示找出后面内容与括号内条件一样的内容
I love my lover
He love his lovers
```

{n，m}匹配次数

{n,m}：表示匹配最少匹配n次，最多匹配m次

```shell
[root@localhost ~]# grep -E -o -i 'a{2,3}' /test.txt 	匹配字母A最少匹配2次、最多3次
AAA
AAA
AAA
AAA
AAA
AAA
AAA
AAA
AA
```



### （4）sed与正则表达式

sed：流编辑器，文本编辑工具

作用：

1.结合正则表达式子可实现增删改查；其中的查询功能又包括过滤和指定字符串、取行（取出指定行）

语法：sed 参数选项   sed内置字符   输入文件

参数选项

| 参数选项 | 解释                                                  |
| -------- | ----------------------------------------------------- |
| -n       | 取消默认sed的输出、常与sed内置命令p一起使用           |
| -i       | 直接将修改结果写入文件、不用-i，sed修改的是内存的数据 |
| -e       | 多次编辑、不需要管道符了                              |
| -r       | 支持正则扩展                                          |

sed内置命令字符、用于对文件进行不同的操作功能、如对文件进行增删改查

sed内置常用字符

| sed的内置字符       | 解释                                                      |
| ------------------- | --------------------------------------------------------- |
| a                   | append、对文件追加、在指定行后面添加一行/多行             |
| d                   | delete，删除匹配行                                        |
| i                   | insert、表示插入文本、在指定行前添加一行/多行文本         |
| p                   | print、打印匹配行的内容、通常p与n一起用                   |
| s/正则/替换的内容/g | 匹配正则内容、然后替换内容（支持正则）、结尾g代表全局匹配 |

sed匹配范围



| 范围     | 解释                                                         |
| -------- | ------------------------------------------------------------ |
| 空地址   | 全文处理                                                     |
| 单地址   | 指定文件某一行                                               |
| /pattern | 被模式匹配到的每一行                                         |
| 范围区间 | 10，20、十到二十行；10，+5第10行向下5行，/pattern1/，/pattern2/ |
| 步长     | 1~2，表示1、3、5、7、9行；2~2.，两个步长，表示2、4、6、8、10 |

sed案例

1.输出文件的第2到第3行的内容

```shell
root@localhost /]# sed -n '2,3p' lucky.txt 
i teach linux
i like play baskedball
```

2.过滤出含有linux的字符串行

```shell
[root@localhost /]# sed -n '/linux/p' lucky.txt 
i teach linux
```

3.删除含有like的行

```shell
[root@localhost /]# sed '/like/d' lucky.txt 		#此写法只是输出结果显示删除了、但是实际没写入到文件
my name is lufei
i teach linux
my qq is 937921303
my website is http://pythonav.cn

[root@localhost /]# cat lucky.txt 
my name is lufei
i teach linux
my qq is 937921303
my website is http://pythonav.cn
i like apple

[root@localhost /]# sed -i '/like/d' lucky.txt 	#-i 不输出结果直接将删除的数据写入到文件
[root@localhost /]# cat lucky.txt 
my name is lufei
i teach linux
my qq is 937921303
my website is http://pythonav.cn
```



4.删除2.3行的数据

```shell
[root@localhost /]# sed -i '2,3d' lucky.txt 
[root@localhost /]# cat lucky.txt 
my name is lufei
my website is http://pythonav.cn
i like apple
```

5.删除第二行到结尾的行

```shell
[root@localhost /]# sed -i '2,$d' lucky.txt 
[root@localhost /]# cat -n lucky.txt 
     1	my name is lufei
```

6.将文件中的my全部替换为his

```shell
[root@localhost /]# sed 's/my/his/g' lucky.txt 	
his name is lufei
his hobiss is da qiu
his telephone number is 13076294002
his qq is 9467128487 
```

7.替换所有his为my 、且qq号改为8888888

```shell
[root@localhost /]# sed -e 's/his/my/g' -e 's/13076294002/8888888/g' lucky.txt 

my name is lufei
my hobiss is da qiu
my telephone number is 8888888
my qq is 9467128487 
```

8.在第二行追加内容写入到文本

```shell
[root@localhost /]# sed -i '2aI am a student' lucky.txt 
[root@localhost /]# cat lucky.txt 
his name is lufei
his hobiss is da qiu
I am a student
his telephone number is 13076294002
his qq is 9467128487 
```

9.在第3行追加多条内容写入到文本

```shell
[root@localhost /]# sed -i '2a I like kobi\n and i like yaoming' lucky.txt 
[root@localhost /]# cat lucky.txt 
his name is lufei
his hobiss is da qiu
I like kobi
 and i like yaoming
I am a student
his telephone num
```

10.在每一行下面插入新内容

```shell
[root@localhost /]# sed 'a ---------------' lucky.txt 
his name is lufei
---------------
his hobiss is da qiu
---------------
I like kobi
---------------
 and i like yaoming
---------------
I am a student
---------------
his telephone number is 13076294002
---------------
his qq is 9467128487 
---------------
```

11.在第二行前面添加内容

```shell
[root@localhost /]# sed '2i 张成是醒目仔' lucky.txt -i
[root@localhost /]# cat lucky.txt 
his name is lufei
张成是醒目仔
his hobiss is da qiu
I like kobi
 and i like yaoming
I am a student
his telephone number is 13076294002
his qq is 9467128487 
```

sed配合正则表达式企业案例

**#取出linux的ip地址**

去头去尾法

```shell
[root@localhost ~]# ifconfig ens33|sed -ne '2s/^.*inet//g' -e '2s/net.*$//gp'
 192.168.0.105 
 
 -n：表示取消匹配模式
 -e：多次编辑
 2s：表示第二行
 ^.*inet:表示匹配以inet之前任意字符开头的所有内容，这里将内容替换为空，将头部信息去掉
 g：全局替换
 p：打印模式
 net.*$：表示匹配以net一直到结尾的所有内容这里将内容替换为空，将尾部部信息去掉
```



### （5）awk基础

awk：更适合编辑、处理匹配到的文本内容格式；主要用于擅长格式化文本，常见的动作为pring、printf

语法：awk 参数  模式 {条件动作} 文件

awk默认分隔符是空格、即使文本含有多个空格、也识别为1个空格分割处理

**awk的内置变量**

| 内置变量                    | 解释                              |
| --------------------------- | --------------------------------- |
| $n                          | 指定分割符后、当前记录的第n个字段 |
| $0                          | 完整的输入记录                    |
| FS                          | 字段分隔符、默认是空格            |
| NF                          | 分割后、当前行一共有多少个字段    |
| NR                          | 当前记录数、行数                  |
| 更多内置变量通过man手册查看 | man awk                           |

awk场景案例

**#取出文本中第二列的内容**

```shell
[root@localhost ~]# cat awk1.txt 
su1 su2 su3 su4 su5 
su7 su8 su9 su10 su11
su12 su13 su14 su15 su16 
[root@localhost ~]# awk '{print $2}' awk1.txt							取出文本中第二列的内容
su2
su8
su13
#tip：这里没有使用参数和模式、awk默认使用空格分隔符、awk是按行处理文件、一行处理完毕再处理下一行以此类推
#$0:表示完整输入、也就是全部行输入
#$NF：表示分割后的最后一列、倒数第一列可写为$(NF-1)
#NF:表示行数
```

**#一次性输出多列**

```shell
[root@localhost ~]# awk '{print $1,$3}' awk1.txt 
su1 su3
su7 su9
su12 su14
```

#自定义输出内容

```shell
[root@localhost ~]# awk '{print "第一列",$1,"第二列",$2}' awk1.txt 
第一列 su1 第二列 su2
第一列 su7 第二列 su8
第一列 su12 第二列 su13

#tip：$1 $2类似的内置变量不要加引号、以免识别为文本
```

#输出整行信息

```shell
[root@localhost ~]# awk '{print}' awk1.txt 
su1 su2 su3 su4 su5 
su7 su8 su9 su10 su11
su12 su13 su14 su15 su16 
[root@localhost ~]# awk '{print $0}' awk1.txt 
su1 su2 su3 su4 su5 
su7 su8 su9 su10 su11
su12 su13 su14 su15 su16

#以上两种写法效果一样
```

**awk参数**



| 参数 | 解释                        |
| ---- | --------------------------- |
| -F   | 指定分隔符                  |
| -v   | 定义或修改一个awk的内置变量 |
| -f   | 从脚本中读取awk命令         |



awk案例

#显示文件中第五行的信息

```shell
[root@localhost /]# awk 'NR==5' awk2.txt 
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
```

#显示文件中第二至第五行的信息

```SHELL
[root@localhost /]# awk 'NR==2,NR==5' awk2.txt 
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologi
```

#给每一行的内容添加行号

```shell
[root@localhost /]# awk '{print NR,$0}' awk2.txt 
1 root:x:0:0:root:/root:/bin/bash
2 bin:x:1:1:bin:/bin:/sbin/nologin
3 daemon:x:2:2:daemon:/sbin:/sbin/nologin
```

#显示文件3-5行信息且输出行号

```shell
[root@localhost /]# awk ' NR==3,NR==5  {print NR$0}' awk2.txt 
3daemon:x:2:2:daemon:/sbin:/sbin/nologin
4adm:x:3:4:adm:/var/adm:/sbin/nologin
5lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
```

#显示awk2.txt中的第一列、倒数第二和最后一列

```shell
[root@localhost /]# awk -F ':' '{print $1,$NF,$(NF-1)}' awk2.txt 
root /bin/bash /root
bin /sbin/nologin /bin
daemon /sbin/nologin /sbin
adm /sbin/nologin /var/adm

#-F：指定冒号分隔符
#$NF:最后一列
#$(NF-1):倒数第二列
```



### （6）awk分隔符

awk分隔符有输入分隔符、输出分隔符

输入分隔符：awk默认是空格、变量名为FS

输出分隔符：变量名为OFS

FS输入分隔符

awk逐行处理文本时，以输入分隔符为准，将文本切成多个段、awk默认时空格分割、当文件内没有空格时，可以通过修改变量名指定分割符进行分割

案例

#将文件种的内容以#号分隔符进行分割

```shell
[root@localhost /]# cat awk3.txt 
su1#su2#su3#su4#su5#
su6#su7#su8#su9#su10#

[root@localhost /]# awk -F '#' '{print $1,$3,$5}' awk3.txt 			#第一种写法，通过参数-F,指定分隔符
su1 su3 su5
su6 su8 su10

[root@localhost /]# awk -v FS='#' '{print $1,$3,$5}' awk3.txt 			第二种写法，通过参数-v 修改内置变量
su1 su3 su5
su6 su8 su10

```

OFS输出分隔符

awk：文本报告生成器，linux为gawk

awk处理内部文本，将文本以输入分隔符为准，切成多个片段；要想输出显示屏幕的分隔符不一致，这就用到输出分隔符

案例

#通过分割线将以下文本进行输出显示

```shell
[root@localhost /]# cat awk3.txt 
su1#su2#su3#su4#su5#
su6#su7#su8#su9#su10#
[root@localhost /]# awk -v FS='#' -v OFS='--' '{print $1,$2,$3,$4,$5}' awk3.txt 
su1--su2--su3--su4--su5
su6--su7--su8--su9--su10
```

输出分隔符与逗号

```shell
[root@localhost /]# awk -v FS='#' '{print $1,$3}' awk3.txt 			加逗号默认空格分割
su1 su3
su6 su8
[root@localhost /]# awk -v FS='#' '{print $1$3}' awk3.txt 		不加逗号
su1su3
su6su8
[root@localhost /]# awk -v FS='#' -v OFS='\t' '{print $1,$3}' awk3.txt 		以制表符进行分割，一个\t相当于四个空格
su1	su3
su6	su8


```



### （7）awk变量

awk参数

| 参数 | 解释                        |
| ---- | --------------------------- |
| -F   | 指定分隔符                  |
| -v   | 定义或修改一个awk内部的变量 |
| -f   | 从脚本文件中读取awk命令     |

对awk可分为内置变量、自定义变量

| 内置变量 | 解释                                                     |
| -------- | -------------------------------------------------------- |
| FS       | 输入字段分隔符、awk默认为空白字符                        |
| OFS      | 输出字段分割符、awk默认为空白字符                        |
| RS       | 输入记录分隔符（输入换行符），指定输入时的换行符         |
| ORS      | 输出记录分隔符（输出换行符），输出时用指定符号代替换行符 |
| NF       | Number of fieid、当前行别分为几列、字段数                |
| NR       | 行号、当前处理的文本行行号                               |
| FNR      | 各文件分别计数的行号                                     |
| FILENAME | 当前文件名                                               |
| ARGC     | 命令行参数的个数                                         |
| ARGV     | 数组、保存的时命令行所给定的各参数                       |

内置变量NF NR FNR

tip:NF NR 内置变量是不用添加$f符号的、而$0、$1、$2才需要添加

案例

#输出每行行号、以及字段总个数

```shell
[root@localhost test]# cat awk_test2.txt 
su1 su2 su3 su4 su5
su6 su7 su8 su9 su10

[root@localhost test]# awk '{print NR,NF}' awk_test2.txt 		统计每行的行数和每行的字段数
1 5
2 5
```

#输出每行的行号和指定的列

```shell
[root@localhost test]# awk '{print NR,$1,$3,$5}' awk_test2.txt 
1 su1 su3 su5
2 su6 su8 su10
```

#处理多个文件显示行号

```SHELL
[root@localhost test]# awk '{print NR,$0}' awk_test1.txt awk_test2.txt 	普通NR变量按顺序给多个文件排序
1 su1#su2#su3#su4#su5#su6#
2 su7#su8#su9#su10#su11#su12#
3 su1 su2 su3 su4 su5
4 su6 su7 su8 su9 su10

[root@localhost test]# awk '{print FNR,$0}' awk_test1.txt awk_test2.txt 	FBR变量则分别对文件进行计数
1 su1#su2#su3#su4#su5#su6#
2 su7#su8#su9#su10#su11#su12#
1 su1 su2 su3 su4 su5
2 su6 su7 su8 su9 su10
```

内置变量RS

作用：输入分割符默认是回车符、可拖过此变量定义一个空格作为分隔符

案例

#以空格为分隔符形式将文件中的内容进行分割

```SHELL
[root@localhost test]# awk -v RS=' ' '{print NR,$0}' awk_test2.txt 		每遇到一个空格即会进行换行
1 su1
2 su2
3 su3
4 su4
5 su5
su6
6 su7
7 su8
8 su9
9 su10
```

内置变量ORS

作用：ORS是输出分隔符的意思、awk默认每行输出完毕后添加回车进行换行、而ORS变量可以改变输出换行符

案例

以@@符进行换行

```SHELL
[root@localhost test]# awk -v ORS='@@' '{print NR,$0}' awk_test2.txt 
1 su1 su2 su3 su4 su5@@2 su6 su7 su8 su9 su10@@[root@localhost test]# 
```

内置变量FILENAME

作用：显示awk正在处理文件的名字

案例

#使用awk命令查看整行信息并输出文件名

```shell
[root@localhost test]# awk  '{print FILENAME,NR,$0}' awk_test2.txt 
awk_test2.txt 1 su1 su2 su3 su4 su5
awk_test2.txt 2 su6 su7 su8 su9 su10
```

变量ARGC、ARGV

ARGV是一个数组、数组内保存的是命令行所给的参数；数组是一种数据类型、类似一个盒子

盒子内有1-12的编号格子、但是从0开始、0-12

案例

取出变量ARGV的索引0到索引2

```shell
[root@localhost test]# awk 'BEGIN{print "how are you?",ARGV[0],ARGV[1],ARGV[2]}' awk_test2.txt awk_test1.txt 
how are you? awk awk_test2.txt awk_test1.txt
```

自定义变量

两种方式定义变量

方法1：通过参数-v  varNname=value

方法2：在程序中直接定义

案例

#使用-三种自定义变量方法输出

```shell
[root@localhost test]# awk -v name="张三" 'BEGIN{print name}' awk_test2.txt 
张三

[root@localhost test]# awk -v name="张三" 'BEGIN{print name}{print $0}' awk_test2.txt 	1.通过-v参数自定义变量输出--其中一种写法
张三
su1 su2 su3 su4 su5
su6 su7 su8 su9 su10
#BEGIN模式：表示对文件输出打印之前优先执行BEGIN里的动作

[root@localhost test]# awk 'BEGIN{name="zhangsan";age="lisi";sex="nan";print name,age,sex}' 	2.第二种写法
zhangsan lisi nan

[root@localhost test]# awk -v myqq=$QQ 'BEGIN{print "我的QQ是:",myqq}'			3.间接引用变量
我的QQ是: 937921303
```



### （8）awk格式化输出

print：只能对文本进行简单的输出、并不能美化、或者修改格式

printf：可以指定格式化输出

**print和printf的区别**

format的使用

要点：
1、其与print命令的最大不同是，printf需要指定format；
2、format用于指定后面的每个item的输出格式；
3、printf语句不会自动打印换行符；\\n

format格式的指示符都以%开头，后跟一个字符；如下：
%c: 显示字符的ASCII码；
%d, %i：十进制整数；
%e, %E：科学计数法显示数值；
%f: 显示浮点数；
%g, %G: 以科学计数法的格式或浮点数的格式显示数值；
%s: 显示字符串；
%u: 无符号整数；
%%: 显示%自身；

printf修饰符：
-: 左对齐；默认右对齐,
+：显示数值符号；  printf "%+d"

tip：

printf动作默认不会添加换行符，需要自己定义

print动作默认空格换行

```shell
[root@localhost test]# awk '{print $1}' awk_test2.txt 
su1
su6
[root@localhost test]# awk '{printf $1}' awk_test2.txt 
su1su6[root@localhost test]# 
```



#格式化输出字符串

```shell
[root@localhost test]# awk '{printf "%s\n",$1}' awk_test2.txt 
su1
su6
%s:表示对字符串输出
\n：换行符
```



#对多个变量进行格式化

1.在linux普通界面输入多个变量

```shell
[root@localhost ~]# printf '%s\n' a b c d 
a
b
c
d
```

2.配合awk使用则需要一一对应字符串

```shell
[root@localhost test]# awk 'BEGIN{printf "%d\n%d\n%d\n%d\n%d\n",1,2,3,4,5}' awk_test2.txt 
1
2
3
4
5
[root@localhost test]# awk 'BEGIN{printf "%s\n%s\n%s\n%s\n%s\n","wo","shi","zhong","guo","ren"}' awk_test2.txt 
wo
shi
zhong
guo
ren
```

tip：

1. printf输出的文本不会换行、必须添加对应的格式替换符和\n
2. 使用printf动作、‘{printf “%s\n”,$1}’、替换的格式和变量之间有有逗号隔开
3. 使用printf动作、%s  %d等格式替换符需要与被格式化的数据一一对应



案例

#自定义格式然后通过格式替换符格式化输出

```shell
[root@localhost test]# awk '{printf "第一列：%s 第二列：%s 第三列:%s\n",$1,$2,$3}' awk_test2.txt 
第一列：su1 第二列：su2 第三列:su3
第一列：su6 第二列：su7 第三列:su8
```

#对passwd文件进行格式化输出



```shell
[root@localhost test]# awk -F ":" 'BEGIN{printf "%-25s\t %-25s\t %-25s\t %-25s\t %-25s\t %-25s\t %-25s\n","用户名","密码","UID","GID","用户注释","用户家目录","用户使用的解释器"} {printf "%-25s\t %-25s\t %-25s\t %-25s\t %-25s\t %-25s\t %s\n",$1,$2,$3,$4,$5,$6,$7}' passwd.txt 
用户名                      	 密码                       	 UID                      	 GID                      	 用户注释                     	 用户家目录                    	 用户使用的解释器                 
root                     	 x                        	 0                        	 0                        	 root                     	 /root                    	 /bin/bash
bin                      	 x                        	 1                        	 1                        	 bin                      	 /bin                     	 /sbin/nologin
daemon                   	 x                        	 2                        	 2                        	 daemon                   	 /sbin                    	 /sbin/nologin
adm                      	 x                        	 3                        	 4                        	 adm                      	 /var/adm                 	 /sbin/nologin
lp                       	 x                        	 4                        	 7                        	 lp                       	 /var/spool/lpd           	 /sbin/nologin
sync                     	 x                        	 5                        	 0                        	 sync                     	 /sbin                    	 /bin/sync
shutdown                 	 x                        	 6                        	 0                        	 shutdown                 	 /sbin                    	 /sbin/shutdown
halt                     	 x                        	 7                        	 0                        	 halt                     	 /sbin                    	 /sbin/halt
mail                     	 x                        	 8                        	 12                       	 mail                     	 /var/spool/mail          	 /sbin/nologin
operator                 	 x                        	 11                       	 0                        	 operator                 	 /root                    	 /sbin/nologin
games                    	 x                        	 12                       	 100                      	 games                    	 /usr/games               	 /sbin/nologin
ftp                      	 x                        	 14                       	 50                       	 FTP User                 	 /var/ftp                 	 /sbin/nologin
nobody                   	 x                        	 99                       	 99                       	 Nobody                   	 /                        	 /sbin/nologin
systemd-network          	 x                        	 192                      	 192                      	 systemd Network Management	 /                        	 /sbin/nologin
dbus                     	 x                        	 81                       	 81                       	 System message bus       	 /                        	 /sbin/nologin
polkitd                  	 x                        	 999                      	 998                      	 User for polkitd         	 /                        	 /sbin/nologin
sshd                     	 x                        	 74                       	 74                       	 Privilege-separated SSH  	 /var/empty/sshd          	 /sbin/nologin
postfix                  	 x                        	 89                       	 89                       	                          	 /var/spool/postfix       	 /sbin/nologin
chrony                   	 x                        	 998                      	 996                      	                          	 /var/lib/chrony          	 /sbin/nologin
syh                      	 x                        	 1000                     	 1000                     	                          	 /home/syh                	 /bin/bash
nginx                    	 x                        	 997                      	 995                      	 Nginx web server         	 /var/lib/nginx           	 /sbin/nologin
```



### （9）awk模式

awk语法：awk option（参数）  ‘pattern模式（action动作）’   file文件

BEGIN模式:

1.不需要指定文本就可以直接输出语句；

2.处理文本之前会优先执行的动作

案例

```shell
[root@localhost test]# awk 'BEGIN{print "我是黄种人"}' 	#这是没有处理文本直接输出
我是黄种人
[root@localhost test]# awk 'BEGIN{print "我是黄种人"}{print $1}' awk_test2.txt		#这是处理文本之前优先执行了前面那句话 
我是黄种人
su1
su6
```



END模式：

1.处理完文本后再执行动作

案例

```shell
[root@localhost test]# awk '{print $1} END{print "今天吃饭了吗"}' awk_test2.txt 	#先执行文本再执行后面的动作
su1
su6
今天吃饭了吗
```



awk与BEGIN、END的结合使用

```shell
[root@localhost test]# awk 'BEGIN{print "我是谁"}{print $1} END{print "今天吃饭了吗"}' awk_test2.txt 
我是谁
su1
su6
今天吃饭了吗
```

**awk模式pattern**(条件)

除了以上BEGIN、END模式外还有别的条件模式

awk关系运算符

| 关系运算符 | 解释       | 示例      |
| ---------- | ---------- | --------- |
| <          | 小于       | x<y       |
| <=         | 小于等于   | x<=y      |
| ==         | 等于       | x==y      |
| !=         | 不等于     | x!=y      |
| >=         | 大于等于   | x>=y      |
| >          | 大于       | x>u       |
| ~          | 匹配正则   | x~/正则/  |
| !~         | 不匹配正则 | x!~/正则/ |

模式（条件）案例

1.取出字段数等于5的第一列内容

```shell
[root@localhost test]# cat -n awk_test2.txt 
     1	su1 su2 su3 su4 su5
     2	su6 su7 su8 su9 su10
     3	su11 su12 su13 su14 su15
     4	su16 su17 su18 su19 su20 su21
     5	su22 su23 su24 su25 su26 su27 su28

[root@localhost test]# awk 'NF==5{print $1}' awk_test2.txt 
su1
su6
su11
```

2.取出行号大于3的所有内容

```shell
[root@localhost test]# cat -n awk_test2.txt 
     1	su1 su2 su3 su4 su5
     2	su6 su7 su8 su9 su10
     3	su11 su12 su13 su14 su15
     4	su16 su17 su18 su19 su20 su21
     5	su22 su23 su24 su25 su26 su27 su28
[root@localhost test]# awk 'NR>3{print $0}' awk_test2.txt 
su16 su17 su18 su19 su20 su21
su22 su23 su24 su25 su26 su27 su28
```

3.取出第一个字段是su22的整行信息

```shell
[root@localhost test]# cat -n awk_test2.txt 
     1	su1 su2 su3 su4 su5
     2	su6 su7 su8 su9 su10
     3	su11 su12 su13 su14 su15
     4	su16 su17 su18 su19 su20 su21
     5	su22 su23 su24 su25 su26 su27 su28
[root@localhost test]# awk '$1=="su22"{print $0}' awk_test2.txt 
su22 su23 su24 su25 su26 su27 su28
```

3.取出第二行到第五行的内容

```shell
[root@localhost test]# awk 'NR==2,NR==5{print $0}' awk_test2.txt 
su6 su7 su8 su9 su10
su11 su12 su13 su14 su15
su16 su17 su18 su19 su20 su21
su22 su23 su24 su25 su26 su27 su28
```

awk与正则表达式

不指定模式：awk每一行都会执行对应的动作

指定模式：只有被模式匹配到的、符合条件的才会执行动作

awk正则语法：

awk	‘/正则表达式/动作’  文件名

gerp正则语法

grep 	‘正则语法’  文件名

案例

1.用awk进行文本过滤

```shell
[root@localhost test]# awk '/^syh/{print $0}' passwd.txt 		#利用awk过滤查找
syh:x:1000:1000::/home/syh:/bin/bash

[root@localhost test]# awk '/^syh/' passwd.txt 					#省略写法
syh:x:1000:1000::/home/syh:/bin/bash
```

2.强大的格式化文本，找出以s开头的行的第一列、第二列、最后一列的信息



```shell
[root@localhost test]# awk -F ":" 'BEGIN{printf "%-10s\t%-10s\t%-10s\n","用户名","密码","解释器"}/^s/{printf "%-10s\t%-10s\t%-10s\n",$1,$2,$(NF-1)}' passwd.txt 
用户名       	密码        	解释器       
sync      	x         	/sbin     
shutdown  	x         	/sbin     
systemd-network	x         	/         
sshd      	x         	/var/empty/sshd
syh       	x         	/home/syh 

执行顺序：1.先执行BEGIN模式的动作、2.再执行匹配的正则、3找出文本的指定列
```

4.找出passwd.txt中禁止用户登录的行

```shell
[root@localhost test]# awk -F ':' '/\/sbin\/nologin/{print $0}' passwd.txt 		#反斜杠代表转义符
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:999:998:User for polkitd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
chrony:x:998:996::/var/lib/chrony:/sbin/nologin
nginx:x:997:995:Nginx web server:/var/lib/nginx:/sbin/nologin
#\/sbin\/nologin：查找的条件
#{print $0}：查找整行

[root@localhost test]# grep  '/sbin/nologin$' passwd.txt 				#使用grep查找
bin:x:1:1:bin:/bin:/sbin/nologin
daemon:x:2:2:daemon:/sbin:/sbin/nologin
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
mail:x:8:12:mail:/var/spool/mail:/sbin/nologin
operator:x:11:0:operator:/root:/sbin/nologin
games:x:12:100:games:/usr/games:/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/sbin/nologin
nobody:x:99:99:Nobody:/:/sbin/nologin
systemd-network:x:192:192:systemd Network Management:/:/sbin/nologin
dbus:x:81:81:System message bus:/:/sbin/nologin
polkitd:x:999:998:User for polkitd:/:/sbin/nologin
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
chrony:x:998:996::/var/lib/chrony:/sbin/nologin
nginx:x:997:995:Nginx web server:/var/lib/nginx:/sbin/nologin

```

找出文件的区间内容

正则模式：awk ‘/正则内容/动作’  文件名

行范围模式： awk ‘/正则1/,/正则2/动作’  文件名

案例

1.找z出以sshd开头到syh开头之间的内容

```shell
[root@localhost test]# awk -F ':' '/^sshd/,/^syh/{print $0}' passwd.txt 
sshd:x:74:74:Privilege-separated SSH:/var/empty/sshd:/sbin/nologin
postfix:x:89:89::/var/spool/postfix:/sbin/nologin
chrony:x:998:996::/var/lib/chrony:/sbin/nologin
syh:x:1000:1000::/home/syh:/bin/bash
```

2.关系表达式模式

```shell
[root@localhost test]# awk -F ':' 'NR>=4 && NR<=8 {print $0}' passwd.txt 	#找出都四行到第八行的行
adm:x:3:4:adm:/var/adm:/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/sbin/nologin
sync:x:5:0:sync:/sbin:/bin/sync
shutdown:x:6:0:shutdown:/sbin:/sbin/shutdown
halt:x:7:0:halt:/sbin:/sbin/halt
```



## 2、定时任务

### （1）at与malix命令

定时任务：是指周期性的自动取去执行一些动作、好比定期备份数据库、定期清空日志

at：定时任务工具、依赖于atd服务，适用于执行一次就结束调度的任务

语法：

at 时间格式

```
语法
HH:MM
YYYY-mm-dd
noon    正午中午12点
midnight    午夜晚12点
teatime    下午茶时间，下午四点
tomorrow    明天
now+1min  #一分钟之后
now+1minutes/hours/days/weeks
```

案例

```shell
[root@localhost ~]# at now+1min
at> ls /opt
at> <EOT>													#输入执行动作后，再输出CTRL+D提交任务
job 6 at Thu Sep 17 20:01:00 2020

一分钟之后运行ls /opt
at now+1min

[root@chaogelinux ~]# at  now+1min        #ctrl+d提交任务
at> ls /data
at> <EOT>
job 2 at Thu Nov 21 10:38:00 2019


运行之后，通过邮件检查
[root@chaogelinux ~]#
您在 /var/spool/mail/root 中有新邮件
[root@chaogelinux ~]# mail  #通过mail，检查at的任务结果


#检查定时任务
at -l  #列出等待中的作业
#通过文件交互式读取任务，不用交互式输入
[root@chaogelinux data]# cat mytasks.at
echo "chaoge 666"
[root@chaogelinux data]# at -f ./mytasks.at now+3min
job 5 at Thu Nov 21 10:51:00 2019

#删除任务
at -d 6
atrm 6  #效果一样

```



### （2）cron定时任务

cron最快只能到分为单位执行、如要精确到秒则不使用、必须通过脚本编写

以下是秒级的脚本

```shell
#秒级shell脚本
[root@pylinux tmp]# cat test_cron.sh
#!/bin/bash
while true
do
echo "超哥还是强呀"
sleep 1
done
```

检查cron服务包是否安装

```shell
[root@MiWiFi-R3-srv ~]# rpm -qa |grep cron
cronie-anacron-1.4.11-14.el7.x86_64        
crontabs-1.11-6.20121102git.el7.noarch     
cronie-1.4.11-14.el7.x86_64    #定时任务主程序包，提供crond守护进程等工具

rpm -ivh  安装rpm软件
rpm -qa 查看软件是否安装
rpm -ql 查看软件详细信息s
rpm -qf 查看命令属于的安装包
rpm -e  卸载软件
```

检查cron服务包是否运行

```shell
systemctl status crond.service  #centos7
service crond status    #centos6
```

cron 	crond   crintab

1. cron：定时任务的名字
2. crond：定时任务的进程名
3. crontab：管理定时任务的命令

cron定时任务分为系统定时任务、用户定时任务

系统定时任务

crond服务除了在工作时查看/var/spool/cron文件外、还会查看/etc/cron.d目录以及/etc/anacrontab下面的文件内容、里面存放每天、每周、每月需要执行的i定时任务

```
[root@pylinux ~]# ls -l /etc/|grep cron*
-rw-------.  1 root  root      541 4月  11 2018 anacrontab
drwxr-xr-x.  2 root  root     4096 8月  30 11:08 cron.d        #系统定时任务
drwxr-xr-x.  2 root  root     4096 8月   8 2018 cron.daily    #每天的任务
-rw-------.  1 root  root        0 4月  11 2018 cron.deny
drwxr-xr-x.  2 root  root     4096 8月   8 2018 cron.hourly    #每小时执行的任务
drwxr-xr-x.  2 root  root     4096 6月  10 2014 cron.monthly    #每月的定时任务
-rw-r--r--   1 root  root      507 5月  10 2019 crontab
drwxr-xr-x.  2 root  root     4096 6月  10 2014 cron.weekly    #每周的定时任务
```

系统定时任务配置文件

```shell
[root@chaogelinux data]# cat /etc/crontab
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin        #路径信息很少，因此定时任务用绝对路径
MAILTO=root            #执行结果发送邮件给用户

# For details see man 4 crontabs

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name  command to be executed

#每一行，就是一条周期性任务
user-name 是以某一个用户身份运行任务
command to be executed  任务是什么
```



用户定时任务

当root用户或普通用户创建里定时任务、可使用crontab命令配置、当crond服务启动时、会每分钟查看/var/spool/cron路径下系统用户名命令的定时任务文件、哟以确定是否有需要执行的任务。



```shell
#root用户有一个定时任务文件
[root@pylinux ~]# ls -l /var/spool/cron/
总用量 4
-rw------- 1 root root 141 10月  9 14:42 root

#查看此root定时任务文件的内容
[root@pylinux ~]# cat /var/spool/cron/root
*/1 * * * * /usr/local/qcloud/stargate/admin/start.sh > /dev/null 2>&1 &
0 0 * * * /usr/local/qcloud/YunJing/YDCrontab.sh > /dev/null 2>&1 &
2>&1:将标准正确和错误信息输出
&:后台运行

#等同于如下命令
[root@pylinux ~]# crontab -l
*/1 * * * * /usr/local/qcloud/stargate/admin/start.sh > /dev/null 2>&1 &
0 0 * * * /usr/local/qcloud/YunJing/YDCrontab.sh > /dev/null 2>&1 &
```

crontab命令

作用：用于提交和管理用户的需要周期性执行的任务

| 参数    | 解释                                                   | 使用示例         |
| ------- | ------------------------------------------------------ | ---------------- |
| -l      | list查看定时任务                                       | crontab -l       |
| -e      | edit编辑定时任务、建议手动编辑                         | crontab -e       |
| -i      | 删除定时任务、提示用户确认删除、避免出错               | crontab -i       |
| -r      | 删除定时任务、移除/var/spool/cron/username文件，全没了 | crontab -r       |
| -u user | 指定用户执行任务、root可管理普通用户                   | crontab -u su -l |

crontab命令就是在修改/var/spool/cron中的文件

案例

#用户查看定时任务

```shell
crontab -l #列出用户设置的定时任务，等于cat var/spool/cron/root
crontab -e  #编辑用户的定时任务，等于如上命令编辑的是 vi /var/spool/cron/root文件
```

检车crond服务是否运行

```shell
[root@pylinux ~]# systemctl is-active crond
active

[root@pylinux ~]# ps -ef|grep crond
root       711     1  0 10月20 ?      00:00:01 /usr/sbin/cron
d -n
```

定时任务相关文件

/var/spool/cron  定时任务的配置文件所在目录
/var/log/cron  定时任务日志文件
/etc/cron.deny  定时任务黑名单



**定时任务语法格式**

口诀：什么时候做什么事情

#查看定时任务配置文件

```shell
[root@luffycity ~]# cat /etc/crontab
```

![](C:\Users\admin\Desktop\定时任务.jpg)

案例

```shell
每天上午8点30，去上学
30 08 * * *  go to school

每天晚上12点回家回家睡觉
00 00  *  * *  go home
```

定时任务符号

```
crontab任务配置基本格式：
*  *　 *　 *　 *　　command
分钟(0-59)　小时(0-23)　日期(1-31)　月份(1-12)　星期(0-6,0代表星期天)　 命令

第1列表示分钟1～59 每分钟用*或者 */1表示
第2列表示小时1～23（0表示0点）
第3列表示日期1～31
第4列表示月份1～12
第5列标识号星期0～6（0表示星期天）
第6列要运行的命令

（注意：day of month和day of week一般不同时使用）
（注意：day of month和day of week一般不同时使用）
（注意：day of month和day of week一般不同时使用）
```

时间表示法：

- 特定值：时间点有效取值范围内的值
- 通配符：某时间点有效范围内的所有值、表示“每”的意思

| 特殊符号 | 含义                                                         |
| -------- | ------------------------------------------------------------ |
| *        | 表示“每”的意思、如00 23cmd表示每月每周每日的23点00整点执行命令 |
| -        | 表示时间范围分隔符、如17-19，代表每天的17点、18点、19点      |
| ，       | 表示分隔时段、如30 17，18，19 *cmd表示每天的17点、18点、19点半执行命令 |
| /n       | n表示可以整除的数字、每隔n的单位时间、如每隔10分钟表示/10* cmd |

式例

```shell
0 * * * *   每小时执行，每小时的整点执行
1 2 * * 4   每周执行，每周四的凌晨2点1分执行
1 2 3 * *   每月执行，每月的3号的凌晨2点1分执行
1 2 3 4 *    每年执行，每年的4月份3号的凌晨2点1分执行
1 2 * * 3,5   每周3和周五的2点1分执行
* 13,14 * * 6,0  每周六、周日的下午1点和2点的每一分钟都执行
0 9-18 * * 1-5   周一到周五的每天早上9点一直到下午6点的每一个整点(工作日的每个小时整点)
*/10 * * * *  每隔10分钟执行一次任务
*7 * * * *    如果没法整除，定时任务则没意义,可以自定制脚本控制频率
定时任务最小单位是分钟，想完成秒级任务，只能通过其他方式（编程语言）
```

案例1

```shell
*/1 * * * * /bin/sh  /scripts/data.sh      #每分钟执行命令

30 3,12 * * *  /bin/sh  /scripts/data.sh  #每天的凌晨3点半，和12点半执行脚本

30 */6 * * *    /bin/sh  /scripts/data.sh     #每隔6小时，相当于6、12、18、24点的半点时刻，执行脚本

30 8-18/2  * * * /bin/sh  /scripts/data.sh  #  30代表半点，8-18/2表示早上8点到下午18点之间每隔两小时也就是8、10、12、14、16、18的半点时刻执行脚本

30 21 * * *  /opt/nginx/sbin/nginx -s reload  #每天晚上9点30重启nginx

45 4 1,10 * *  /bin/sh  /scripts/data.sh   #每月的1、10号凌晨4点45执行脚本

10 1  * 6,0  /bin/sh  /scripts/data.sh   #每周六、周日的凌晨1点10分执行命令

0,30 18-23 * * *  #每天的18点到23点之间，每隔30分钟执行一次

00 */1 * * *  /bin/sh  /scripts/data.sh   #每隔一小时执行一次

00 11 * 4 1-3 /bin/sh  /scripts/data.sh        #4月份的周一到周三的上午11点执行脚本
```

案例2

```shell
# 每天早上7点到上午11点，每2小时运行cmd命令
00 07-11/2 * * * CMD


0 6 * * * /var/www/test.sh        #每天6点执行脚本
0 4 * * 6 /var/www/test.sh    #每周六凌晨4:00执行
5 4 * * 6 /var/www/test.sh    #每周六凌晨4:05执行
40 8 * * * /var/www/test.sh    #每天8:40执行
31 10-23/2 * * *    /var/www/test.sh    #在每天的10:31开始，每隔2小时重复一次
0 2 * * 1-5 /var/www/test.sh    #每周一到周五2:00
0 8,9 * * 1-5  /var/www/test.sh   #每周一到周五8:00，每周一到周五9:00
0 10,16 * * *  /var/www/test.sh   #每天10:00、16:00执行
```

生产环境用户配置定时任务流程

需求：每分钟向/testcron/hellosu.txt文件中写入一句话“nihao”

步骤1：确保定时任务的正确执行

```shell
[root@localhost ~]# mkdir /testcron								创建好需要的文件夹、以及测试文件、并检测文件内容
[root@localhost testcron]# vim nihao.sh 						编写脚本内容
#!/bin/bash
echo '吃饭了吗？'>> /testcron/hellosu.txt

```

步骤2：编辑定时任务、

```shell
[root@localhost ~]# crontab -e
* * * * *  /bin/bash /testcron/nihao.sh
编写好:wq!保存
crontab: installing new crontab
```

步骤3：检查编写的定时任务

```
[root@localhost testcron]# crontab -l
* * * * *  /bin/bash /testcron/nihao.sh
```

步骤4：检测文件输出内容

```shell
[root@localhost testcron]# tail -f hellosu.txt 
吃饭了吗？
吃饭了吗？
吃饭了吗？
吃饭了吗？
吃饭了吗？
吃饭了吗？
```

每5分钟进行服务器时间同步

```shell
crontab -e 
*/5 * * * * /usr/sbin/ntpdae  ntp1.aliyun.com &> /dev/null
```

每晚0点整、把站点目录/var/www/html下的内容打包备份到/data目录下

```shell
1.检查文件夹是否存在，不存在则创建
[root@pylinux ~]# ls -d /var/www/html /data
ls: 无法访问/var/www/html: 没有那个文件或目录
ls: 无法访问/data: 没有那个文件或目录

2.创建文件夹
[root@pylinux ~]# mkdir -p /var/www/html  /data
[root@pylinux ~]# ls -d /var/www/html /data
/data  /var/www/html

3.创建测试文件
[root@pylinux ~]# touch /var/www/html/chaoge{1..10}.txt
[root@pylinux ~]# ls /var/www/html/
chaoge10.txt  chaoge1.txt  chaoge2.txt  chaoge3.txt  chaoge4.txt  chaoge5.txt  chaoge6.txt  chaoge7.txt  chaoge8.txt  chaoge9.txt

4.打包压缩命令
[root@pylinux www]# tar -zcvf /data/bak_$(date +%F).tar.gz  ./html/

5.创建测试脚本

```



## 3、软件包管理

### （1）程序软件与rpm

linux程序包

```
软件包顾名思义就是将应用程序、配置文件和数据打包的产物，所有的linux发行版都采用了某种形式的软件包系统，这使得linux软件管理和在windows下一样方便，suse、red hat、fedora等发行版都是用rpm包，Debian和Ubuntu则使用.deb格式的软件包。

mysql-5-3-4.rpm
redis-3-4-3.rpm
nginx2-3-2.rpm
```

rpm包格式

```
格式：name-version-release.arch.rpm
wget-1.14-18.el7.x86_64.rpm
名字，版本号，架构型号
```

rpm源代码格式

```
格式：name-version.tar.gz

nginx-1.12.0.tar.gz
node-v10.15.3-linux-x64.tar
```

开源镜像网站

```
http://mirrors.aliyun.com
http://mirrors.sohu.com
http://mirrors.sohu.com/centos/7.5.1804/os/x86_64/Packages/
http://mirrors.163.com
```

第三方软件epel

```
http://mirrors.sohu.com/fedora-epel/7/x86_64/Packages/
https://mirrors.aliyun.com/epel/7/x86_64/Packages/m/
```

rpm命令

```shell
rpm命令：
语法：rpm  [OPTIONS]  [PACKAGE_FILE]
# i表示安装 v显示详细过程 h以进度条显示，每个#表示2%进度
安装软件的命令格式                rpm -ivh filename.rpm    
升级软件的命令格式                rpm -Uvh filename.rpm
卸载软件的命令格式                rpm -e filename.rpm
查询软件描述信息的命令格式         rpm -qpi filename.rpm
列出软件文件信息的命令格式         rpm -qpl filename.rpm
查询文件属于哪个 RPM 的命令格式 　 rpm -qf filename
```

案例

```shell
wget http://www.rpmfind.net/linux/mageia/distrib/7/x86_64/media/core/release/lrzsz-0.12.21-22.mga7.x86_64.rpm

#测试rpm包
[root@chaogelinux pyrpm]# rpm -ivh --test lrzsz-0.12.21-22.mga7.x86_64.rpm
警告：lrzsz-0.12.21-22.mga7.x86_64.rpm: 头V4 RSA/SHA256 Signature, 密钥 ID 80420f66: NOKEY
准备中...                          ################################# [100%]


[root@chaogelinux pyrpm]# rpm -ivh lrzsz-0.12.21-22.mga7.x86_64.rpm
警告：lrzsz-0.12.21-22.mga7.x86_64.rpm: 头V4 RSA/SHA256 Signature, 密钥 ID 80420f66: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:lrzsz-0.12.21-22.mga7            ################################# [100%]

#升级rpm包
[root@chaogelinux pyrpm]# rpm -Uvh lrzsz-0.12.21-22.mga7.x86_64.rpm
警告：lrzsz-0.12.21-22.mga7.x86_64.rpm: 头V4 RSA/SHA256 Signature, 密钥 ID 80420f66: NOKEY
准备中...                          ################################# [100%]
正在升级/安装...
   1:lrzsz-0.12.21-22.mga7            ################################# [100%]


#卸载lrzsz工具
rpm -e lrzsz
```

rpm工具缺点：不能解决依赖关系问题

### （2）yum工具

自动解决依赖关系的软件安装工具yum

yum客户端、仓库的配置文件

```shell
/etc/yum.conf  #为所有仓库提供公共配置

[root@chaogelinux yum.repos.d]# cat /etc/yum.conf
[main]
cachedir=/var/cache/yum/$basearch/$releasever
keepcache=0                                    #本地缓存是否保留，0否，1是
debuglevel=2                                #调试日志级别
logfile=/var/log/yum.log        #日志路径
exactarch=1                                    #精确系统平台版本匹配
obsoletes=1            
gpgcheck=1                                    #检查软件包的合法性
plugins=1                
installonly_limit=5                    #同时安装几个工具包
bugtracker_url=http://bugs.centos.org/set_project.php?project_id=23&ref=http://bugs.centos.org/bug_report_page.php?category=yum
distroverpkg=centos-release        


#  This is the default, if you make this bigger yum won't see if the metadata
# is newer on the remote and so you'll "gain" the bandwidth of not having to
# download the new metadata and "pay" for it by yum not having correct
# information.
#  It is esp. important, to have correct metadata, for distributions like
# Fedora which don't keep old packages around. If you don't like this checking
# interupting your command line usage, it's much better to have something
# manually check the metadata once an hour (yum-updatesd will do this).
# metadata_expire=90m

#请放置你的仓库在这里，并且命名为*.repo类型
# PUT YOUR REPOS HERE OR IN separate files named file.repo
# in /etc/yum.repos.d
```



repo仓库文件

![](C:\Users\admin\Desktop\image-20191121163310784.png)

```shell
/etc/yum.repos.d/*.repo #提供仓库的地址文件

CentOS-Base.repo
[base]
name=CentOS-$releasever - Base - mirrors.aliyun.com        #仓库文件的说明
failovermethod=priority    #存在多个url的时候，按顺序来连接，如果是roundrobin，意为随机挑选
baseurl=http://mirrors.aliyun.com/centos/$releasever/os/$basearch/    #指定仓库的网站地址
        http://mirrors.aliyuncs.com/centos/$releasever/os/$basearch/
        http://mirrors.cloud.aliyuncs.com/centos/$releasever/os/$basearch/
gpgcheck=1    #是否检测秘钥
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7      #公钥文件存放路径

#released updates  指定rpm包需要升级的地址，此处可以去网页上寻找对应的包
[updates]
name=CentOS-$releasever - Updates - mirrors.aliyun.com
failovermethod=priority
baseurl=http://mirrors.aliyun.com/centos/$releasever/updates/$basearch/
        http://mirrors.aliyuncs.com/centos/$releasever/updates/$basearch/
        http://mirrors.cloud.aliyuncs.com/centos/$releasever/updates/$basearch/
gpgcheck=1
gpgkey=http://mirrors.aliyun.com/centos/RPM-GPG-KEY-CentOS-7


epel.conf
[epel]
name=Extra Packages for Enterprise Linux 7 - $basearch
baseurl=http://mirrors.aliyun.com/epel/7/$basearch
failovermethod=priority
enabled=1        #是否启用此仓库
gpgcheck=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
```

自定义一个repo文件

```shell
touch chaoge.repo #写入

[base]
name=Chaoge repo 
baseurl=http://chaoge.com/centos/7/os/x86_64/
gpgcheck=0

[epel]
name=Chaoge epel repo
baseurl=http://chaoge.com/epel/7/os/x86_64/
gpgcheck=0
```

yum源的配置方法

源地址

repo文件下载地址：https://developer.aliyun.com/mirror/

```shell
1.备份现有repo仓库			#可跳过此步骤、
2.下载新的repo文件到指定/etc/yum.repos.d/路径
CentOS 6
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo

CentOS 7
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

3.清空旧yum缓存，生成新的缓存
yum clean all
yum makecache

4.针对阿里云镜像，可能出现无法解析地址的异常
sed -i -e '/mirrors.cloud.aliyuncs.com/d' -e '/mirrors.aliyuncs.com/d' /etc/yum.repos.d/CentOS-Base.repo

```



yum工具的优缺点

1.不能指定路径安装；

2.安装包的版本可能不是最新的；

3.可能下载的版本包功能受限制、存在漏洞；

4.适合新手安装、解决依赖安装；



### （3）源代码编译安装

编译安装优点

1.可手动下载最新源代码、按照指定需求、设置参数、指定安装路径、扩展第三方功能、更为灵活

2.无法自动解决依赖关系、对于新手不友好



编译三部曲

前提条件准备

```
开发工具：gcc make等
开发组件：
yum groupinstall "Development Tools"
yum groupinstall "Server Platform Development"
```

![](C:\Users\admin\Desktop\image-20191121173708164.png)

第一曲：执行脚本configure文件

```shell
./configure --prefix=软件安装路径

针对C、C++代码，进行编译安装，需要指定配置文件`Makefile`，需要通过`configure`脚本生成
通过选项传递参数，指定启用特性、安装路径等<执行时会生成makefile
检查依赖到的外部环境
```

第二曲：执行make命令

```shell
make是Linux开发套件里面自动化编译的一个控制程序，他通过借助 Makefile 里面编写的编译规范进行自动化的调用 gcc 、ld 以及运行某些需要的程序进行编译的程序。一般情况下，他所使用的 Makefile 控制代码，由 configure 这个设置脚本根据给定的参数和系统环境生成。

make这一步就是编译，大多数的源代码包都经过这一步进行编译（当然有些perl或python编写的软件需要调用perl或python来进行编译）

make 的作用是开始进行源代码编译，以及一些功能的提供，这些功能由他的 Makefile 设置文件提供相关的功能，比如 make install 一般表示进行安装，make uninstall 是卸载，不加参数就是默认的进行源代码编译。
```

第三曲：开始安装make install

```shell
开始安装软件到./configure指定的安装路径
```

案例

nginx源码编译安装

```shell
1.准备编译环境
yum install gcc patch libffi-devel python-devel  zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gdbm-devel db4-devel libpcap-devel xz-devel openssl openssl-devel -y

2.获取nginx源代码
wget -c https://nginx.org/download/nginx-1.12.0.tar.gz

3.解压缩nginx源代码
tar -zxvf nginx-1.12.0.tar.gz

4.进入源码目录
cd nginx-1.12.0

5.开始编译三部曲
./configure --prefix=/opt/nginx112/  --with-http_ssl_module --with-http_stub_status_module 

6.执行make指令，调用gcc等编译工具
make

7.开始安装
make install

8.安装后启动nginx软件，找到二进制程序，以绝对路径执行
/opt/ngx112/sbin/nginx

9.检查环境变量，需要手动配置nginx的PATH路径，否则必须绝对路径才能找到			#这里还可以修改/etc/profile配置文件
编辑文件/etc/profile.d/nginx.sh											#在结尾写入export PATH=/opt/ngx112/sbin:$PATH，再																					 soure/etc/profile生效
写入export PATH=/opt/ngx112/sbin:$PATH

10.退出回话，重新登录机器
logout

11.检查环境变量
[root@chaogelinux ngx112]# cat /etc/profile.d/nginx.sh
export PATH=/opt/ngx112/sbin:$PATH

12.启动nginx，可以访问页面										#若不能访问、可能是防火墙未关闭 用systemctl stop firewalld 关闭
```

环境变量配置文件

```shell
/etc/profile
用于设置系统级的环境变量和启动程序，在这个文件下配置会对所有用户生效。当用户登录(login)时，文件会被执行，并从/etc/profile.d目录的配置文件中查找shell设置。如果对/etc/profile修改的话必须重启才会生效

~/.profile
每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次!默认情况下,他设置一些环境变量,执行用户的.bashrc文件.

~/.bashrc
该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该该文件被读取.

~/.bash_logout
当每次退出系统(退出bash shell)时,执行该文件，通常存放清理工作的命令。

执行顺序
登陆shell
登陆shell时，首先执行/etc/profile，之后执行用户目录下的~/.profile,~/.profile中会执行~/.bashrc。
```



## 4、xshell特性

shell的作用：

1. 解释用户所输入的命令或程序等；
2. 用户输入一条命令则解释一条；
3. 键盘输入命令、Linux响应返回一个结果，称之为交互式

![](C:\Users\admin\Desktop\image-20191123175043170.png)

交互的整个过程：


![](C:\Users\admin\Desktop\image-20191123181307432.png)

shell脚本

linux：以*.sh结尾的文件称之为shell脚本

windows:以*.bat结尾的文件为批处理脚本

shell脚本的组成

```shell
#! /bin/bash		#指定解释器
				#后面的内容带#号的为注释
```

执行shell的方法

```shell
[root@chaogelinux data]# cat test.sh
#!/bin/bash
echo "超哥强呀，奥力给"
#!/bin/bash 这里就是注释的作用了
[root@chaogelinux data]#
[root@chaogelinux data]#
[root@chaogelinux data]# sh < test.sh		#执行的第一种方法、比较少见
超哥强呀，奥力给
[root@chaogelinux data]# sh test.sh				#执行的第二种方法
超哥强呀，奥力给
[root@chaogelinux data]# bash test.sh			#执行的第三种方法
超哥强呀，奥力给
[root@chaogelinux data]# source test.sh			#执行的第四种方法
超哥强呀，奥力给
[root@chaogelinux data]# . /data/test.sh		#执行的第五种方法
超哥强呀，奥力给

权限不足
[root@chaogelinux data]# ./test.sh
-bash: ./test.sh: 权限不够
[root@chaogelinux data]# chmod +x test.sh
[root@chaogelinux data]# ./test.sh
超哥强呀，奥力给
```



## 5、磁盘管理

### （1）磁盘介绍及原理

- **磁盘使用的基本流程：1、磁盘分区；2、磁盘格式化、创建文件系统；3、挂载目录；4、进行数据读写**

磁盘的内部结构

<img src="C:\Users\admin\Desktop\271.jpg" style="zoom: 33%;" />

扇区（sector）：磁盘最小的存储单位大小为512Bytes、即0.5kb

块（block）：操作系统文件的最小单位是块，由8个连续的扇区组成，大小为0.5*4=4kb，1个block存放一个文件，所以在linux中文件的大叫都为4的整数倍

簇：windos上的相邻扇区称之为簇

### （2）磁盘存储单位

>  1byte（字节）=8bit(位)
>  1kb(千字节)=1024b
>  1MB（兆字节）=1024KB
>  1GB(千兆字节)=1024MB
>  1TB=1024GB

寻址：磁头在多个连续的扇区进行找取数据



### （3）磁盘转速与寻道时间

- 磁盘的RPM：磁盘内的主轴旋转速度、1分钟内磁盘盘篇完成的最大旋转数

- $$
  --3600RPM:60s*60hz=3600rpm/min
  --5400RPM:3600rpm/min*1.5=5400
  --7200RPM:rpm/min*2=7200
  $$

  比较：数据存储的密度性、连续性高、转速高才能有着高的效率提取数据

### （4）fdisk分区、重读分区表

1.分区类型

- MBR分区：硬盘容量受限制，最大2T，用于fdisk分区
- GPT分区：硬盘容量不受限制，分区个数不受限制、且自带保护t机制，用于parted分区

2.linux磁盘分区

主分区、扩展分区、扩展分区下又分为多个逻辑分区、



<img src="C:\Users\admin\Desktop\image-20191202104411361.png" style="zoom:33%;" />



- 一般常用的硬盘读取以sda、sdb形式表示硬盘文件形式
- /dev/sda
- 主分区、逻辑分区默认区号为1-4、
- 逻辑分区，默认区号从5开始

- **fdisk分区命令**
- **语法：fdisk 参数 指定的硬盘文件**
- **参数 -l     #列出分区表**

```shell
[root@localhost ~]# fdisk -l|grep sd
磁盘 /dev/sda：21.5 GB, 21474836480 字节，41943040 个扇区
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200    41943039    19921920   8e  Linux LVM
磁盘 /dev/sdb：21.5 GB, 21474836480 字节，41943040 个扇区
/dev/sdb1            2048     1026047      512000   83  Linux
/dev/sdb2         1026048    41943039    20458496    5  Extended
/dev/sdb5         1028096    21999615    10485760   83  Linux
/dev/sdb6        22001664    41943039     9970688   83  Linux
```

分区步骤如下：

**1.指定一块硬盘进行分区**

```shell
[root@localhost ~]# fdisk /dev/sdc
欢迎使用 fdisk (util-linux 2.23.2)。

更改将停留在内存中，直到您决定将更改写入磁盘。
使用写入命令前请三思。

Device does not contain a recognized partition table
使用磁盘标识符 0xeabe5b3f 创建新的 DOS 磁盘标签。

命令(输入 m 获取帮助)：
```

**2.输入n新建分区，选择p主分区、再选择默认起点、终点输入+500M**

```shell
命令(输入 m 获取帮助)：n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
分区号 (1-4，默认 1)：1
起始 扇区 (2048-20971519，默认为 2048)：
将使用默认值 2048
Last 扇区, +扇区 or +size{K,M,G} (2048-20971519，默认为 20971519)：+500M
分区 1 已设置为 Linux 类型，大小设为 500 MiB
```

**3.同上输入n新建分区，选择e扩展分区，并输入分区大小**

```shell
命令(输入 m 获取帮助)：n
Partition type:
   p   primary (1 primary, 0 extended, 3 free)
   e   extended
Select (default p): e
分区号 (2-4，默认 2)：
起始 扇区 (1026048-20971519，默认为 1026048)：
将使用默认值 1026048
Last 扇区, +扇区 or +size{K,M,G} (1026048-20971519，默认为 20971519)：
将使用默认值 20971519
分区 2 已设置为 Extended 类型，大小设为 9.5 GiB
```

**4.同上输入n新建分区，选择l逻辑分区，并输入分区大小**

```shell
命令(输入 m 获取帮助)：n
Partition type:
   p   primary (1 primary, 1 extended, 2 free)
   l   logical (numbered from 5)
Select (default p): l
添加逻辑分区 5
起始 扇区 (1028096-20971519，默认为 1028096)：
将使用默认值 1028096
Last 扇区, +扇区 or +size{K,M,G} (1028096-20971519，默认为 20971519)：+5G
分区 5 已设置为 Linux 类型，大小设为 5 GiB
```

**5.最后输入w，写入磁盘**

```shell
命令(输入 m 获取帮助)：w
The partition table has been altered!

Calling ioctl() to re-read partition table.
正在同步磁盘。
```

**6.分区后可查看详细分区情况**

```shell
命令(输入 m 获取帮助)：p

磁盘 /dev/sdc：10.7 GB, 10737418240 字节，20971520 个扇区
Units = 扇区 of 1 * 512 = 512 bytes
扇区大小(逻辑/物理)：512 字节 / 512 字节
I/O 大小(最小/最佳)：512 字节 / 512 字节
磁盘标签类型：dos
磁盘标识符：0xeabe5b3f

   设备 Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048     1026047      512000   83  Linux
/dev/sdc2         1026048    20971519     9972736    5  Extended
/dev/sdc5         1028096    11513855     5242880   83  Linux
```



**重读分区表：当文件删除后，磁盘仍占用空间，则需要重读使用**

- partprobe命令：用于centos5之前版本，可不重启电脑重读分区表

- partx -a：使内核重读分区表，一般分区格式化后使用

  ```shell
  [root@localhost ~]# partx -a /dev/sdc
  partx: /dev/sdc: error adding partitions 1-2
  partx: /dev/sdc: error adding partition 5
  ```

  

### （5）parted分区

**parted分区：用于大于2TB的磁盘分区，且转换磁盘格式为GPT格式**

parted命令

结合fdisk删除之前的分区表，再进行parted分区

**步骤1：指定硬盘进入**

```shell
[root@localhost ~]# fdisk /dev/sdc
欢迎使用 fdisk (util-linux 2.23.2)。

更改将停留在内存中，直到您决定将更改写入磁盘。
使用写入命令前请三思。


命令(输入 m 获取帮助)：m
```

**步骤2：输入d，删除分区，默认从后面依次删除**

```shell
命令(输入 m 获取帮助)：d
分区号 (1,2,5，默认 5)：5
分区 5 已删除

命令(输入 m 获取帮助)：d
分区号 (1,2，默认 2)：2
分区 2 已删除

命令(输入 m 获取帮助)：d
已选择分区 1
分区 1 已删除

命令(输入 m 获取帮助)：d
No partition is defined yet!
```

**步骤3：输入w写入磁盘**

```shell
命令(输入 m 获取帮助)：w^H
The partition table has been altered!

Calling ioctl() to re-read partition table.
正在同步磁盘。
[root@localhost ~]# fdisk /dev/sdc
欢迎使用 fdisk (util-linux 2.23.2)。
```

**步骤4：使用parted命令进入指定磁盘，并输入help**

```shell
[root@localhost ~]# parted /dev/sdc
GNU Parted 3.1
使用 /dev/sdc
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) help
```

**步骤5：更改磁盘标签为gpt类型****

```shell
(parted) mklabel gpt                                                      
警告: The existing disk label on /dev/sdc will be destroyed and all data on this disk will be lost. Do you
want to continue?
是/Yes/否/No? y
```



**步骤6：输入p打印分区表类型**

```shell
(parted) p                                                                
Model: VMware, VMware Virtual S (scsi)
Disk /dev/sdc: 10.7GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start  End  Size  File system  Name  标志
```



**步骤7：创建主分区，并指定分区大小为500M**

```shell
(parted) mkpart primary 0 500
警告: The resulting partition is not properly aligned for best performance.
忽略/Ignore/放弃/Cancel? Ignore  
```

**步骤8：创建逻辑分区，并指定分区大小****

```shell
(parted) mkpart logical 501 10G
(parted) P                                                                
Model: VMware, VMware Virtual S (scsi)
Disk /dev/sdc: 10.7GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags: 

Number  Start   End     Size    File system  Name     标志
 1      17.4kB  500MB   500MB                primary
 2      501MB   10.0GB  9499MB               logical
```



**步骤9：输入q退出，提示一下需要更新挂载**

```shell
(parted) q                                                         
信息: You may need to update /etc/fstab.
```



### （6）软、硬连接ln命令

- **ln命令**
- 语法：ln  参数  源文件绝对路径   快捷方式绝对路径
- 不加参数则创建硬链接
- 参数：-s   #添加软链接（快捷方式）

软链接特点：

- 1个源文件可生成多个软链接
- 2.创建软链接对源文件无影响
- 3.删除源文件，则软连接会标红，则快捷方式失效
- 4.可创建文件夹的软链接，通过软链接可以进入文件夹

- 创建软连接

```shell
[root@localhost syh]# ln -s /syh/aa.txt /syh/aa1.txt

[root@localhost syh]# ll
总用量 0
lrwxrwxrwx. 1 root root 11 8月  31 13:24 aa1.txt -> /syh/aa.txt
```



- 创建硬链接

```shell
[root@localhost syh]# ln aa.txt  aa2.txt

[root@localhost syh]# ls -il

16789291 -rw-r--r--. 2 root root 16 8月  31 13:34 aa2.txt
16789291 -rw-r--r--. 2 root root 16 8月  31 13:34 aa.txt

```

**readlink命令**

作用：可直接查看快捷方式所指向的源文件路径

```shell
[root@localhost syh]# readlink aa1.txt 
/syh/aa.txt
```

inode

操作系统中专门用于管理和存储文件的信息软件成为文件系统

文件系统将元信息（文件大小、修改信息、创建时间等，文件名除外	）

文件由文件inode号+文件数据内容形成一个单个文件

- **ls -li命令**
- 作用：查看文件inode号

> ```shell
> [root@localhost syh]# ls -li aa.txt 
> 16789291 -rw-r--r--. 1 root root 16 8月  31 13:34 aa.txt
> ```

- **stat 命令**
- 作用：查看文件元信息

> ```shell
> [root@localhost syh]# stat aa.txt 
> 文件："aa.txt"
> 大小：16        	块：8          IO 块：4096   普通文件
> 设备：fd00h/64768d	Inode：16789291    硬链接：1
> 权限：(0644/-rw-r--r--)  Uid：(    0/    root)   Gid：(    0/    root)
> 环境：unconfined_u:object_r:default_t:s0
> 最近访问：2020-08-31 13:34:46.087575129 +0800
> 最近更改：2020-08-31 13:34:46.087575129 +0800
> 最近改动：2020-08-31 13:34:46.088575119 +0800
> ```

**软、硬链接的区别：**

- 1.软链接可对文件、文件夹创建、硬连接只能对文件创建；
- 2.软连接的inode号不一样，硬链接inode号一样；
- 3.同时存在一个文件的软连接和硬连接，则删除其中一个软连接对源文件和硬连接无影响；
- 4.同时存在一个文件的软连接和硬连接，则删除其中一个硬连接对源文件和硬连接也无影响；
- 5.同时存在一个文件的软连接和硬连接，删除源文件则对软连接有影响，对硬链接无影响；





### **（7）文件系统格式化**

**1.常见的文件系统**

windows：

fat16、fat32：最早的windows文件系统、缺点：单个文件不超过2GB

NTFS：支持文件加密、采用日志形式记录读写磁盘的操作、且能恢复数据、提高数据的安全性.、单个文件大小4G限制

exFAT:新式文件系统、单个文件大小16G、可在windos和linux、mas中同时识别

linux：

- ext2：
- ext3：centos5
- ext4：centos6
- xfs：centos7

日志型文件系统：将数据写入日志区域，可通过日志恢复数据

**2.vfs文件系统**

vfs：虚拟文件系统

作用：通过vfs可实现不同文件系统的使用，

- **mkfs命令**
- 作用：针对磁盘分区后进行创建文件系统

- **fsck命令**
- 作用：修复文件系统，默认读取etc/fstab 开机挂机文件
- 末尾为0，则表示开机不检测修复，为1则开机检测修复

```shell
cat /etc/fstab
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=68ab460e-835f-4fb5-a273-04853c1eb0cd /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0

```

- **dumpesfs命令**
- 作用：查看文件系统的属性，适用centos7之前的文件系统，适用于ext2、ext3、ext4文件系统

- **xfs_info命令**
- 作用：查看文件系统的属性，使用centos7之后的文件系统，适用于xfs文件系统

- **tune2fs命令**
- 作用：设置linux是否开机自动检查文件系统正常与否

- **lsblk命令**
- 作用：列出所有的设备即文件系统
- lsblk -f #列出分区的文件系统类型



- **格式化文件系统步骤**

步骤1：查看磁盘分区的文件系统

```shell
[root@localhost ~]# lsblk -f
NAME            FSTYPE      LABEL           UUID                                   MOUNTPOINT
sda                                                                                
├─sda1          xfs                         68ab460e-835f-4fb5-a273-04853c1eb0cd   /boot
└─sda2          LVM2_member                 Iiv4Tf-aKWy-Riis-eWay-2Fhp-aeYa-iMRsrv 
  ├─centos-root xfs                         1cce9747-f741-4829-9b03-88f61c6ba753   /
  └─centos-swap swap                        d7a0b7ad-da95-46eb-915a-dcdc80bf7ab1   [SWAP]
sdb                                                                                
├─sdb1          ext4                        0d52677a-5026-46ca-b728-391d18d95325   
├─sdb2                                                                             
├─sdb5                                                                             
└─sdb6                                                                             
sdc                                                                                
├─sdc1                                                                             
└─sdc2  
```

步骤2：选择sdb5进行文件系统创建

```shell
[root@localhost ~]# mkfs.xfs /dev/sdb5
meta-data=/dev/sdb5              isize=512    agcount=4, agsize=655360 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=2621440, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```

步骤3：查看sdb5的文件类型

```shell
[root@localhost ~]# lsblk -f
sdb                                                                                
├─sdb1          ext4                        0d52677a-5026-46ca-b728-391d18d95325   
├─sdb2                                                                             
├─sdb5          xfs                         e6a38a87-7049-454a-a498-b55f70ea0b63   
└─sdb6  
```

步骤4：关闭文件系统自检（正确工作中打开，可忽略此步骤）

tune2fs -c -1 ：c表示设置自检的挂载次数，设为-1则表示关闭

```shell
[root@localhost ~]# tune2fs -c -1 /dev/sdb1
tune2fs 1.42.9 (28-Dec-2013)
Setting maximal mount count to -1
```

若要进行修复检查，需用f'sck -t 文件系统类型   设备名

```shell
[root@localhost ~]# fsck -t etx4 /dev/sdb1
fsck，来自 util-linux 2.23.2
e2fsck 1.42.9 (28-Dec-2013)
/dev/sdb1: clean, 11/128016 files, 26684/512000 blocks
```

### **（8）文件系统挂载**

- **挂载：指的是将一个存储设备挂载到另一个文件夹，就可以访问这个文件夹的设备内容**
- **挂载点：被挂载的这个文件夹就是挂载点**
- **mount命令：**
- 作用：将指定的文件系统挂载到指定的目录上
- 语法：mount  挂载的设备   挂载点

**挂载一个磁盘分区**

```SHELL
[root@localhost mnt]# mount /dev/sdb1 /mnt/mnt11
[root@localhost mnt]# df -hT
文件系统                类型      容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root xfs        17G  1.2G   16G    7% /
devtmpfs                devtmpfs  1.9G     0  1.9G    0% /dev
tmpfs                   tmpfs     1.9G     0  1.9G    0% /dev/shm
tmpfs                   tmpfs     1.9G   12M  1.9G    1% /run
tmpfs                   tmpfs     1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/sda1               xfs      1014M  142M  873M   14% /boot
tmpfs                   tmpfs     378M     0  378M    0% /run/user/0
/dev/sdb1               xfs       497M   26M  472M    6% /mnt/mnt11
```

参数 

- -l    	#显示系统的所有挂设备信息
- -t		#指定设备的文件系统类型，若不指定，则默认自动选择挂载的文件类型
- -o		#添加挂载的功能选项，
- -r		#read，挂载后的设备，是只读的
- -w		#读写参数，-o  rw权限，表示挂载后允许读写操作





**df -hT命令·**

作用：显示所有已挂载的磁盘使用情况

```shell
[root@localhost ~]# df -hT
文件系统                类型      容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root xfs        17G  1.2G   16G    7% /
devtmpfs                devtmpfs  1.9G     0  1.9G    0% /dev
tmpfs                   tmpfs     1.9G     0  1.9G    0% /dev/shm
tmpfs                   tmpfs     1.9G   12M  1.9G    1% /run
tmpfs                   tmpfs     1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/sda1               xfs      1014M  142M  873M   14% /boot
tmpfs                   tmpfs     378M     0  378M    0% /run/user/0
```



- **挂载案例**

步骤1：将/dev/sdb6 挂载到/mnt/mnt16

```shell
[root@localhost mnt]# mount /dev/sdb6 /mnt/mnt16
[root@localhost mnt]# mount -l|grep sdb6
/dev/sdb6 on /mnt/mnt16 type xfs (rw,relatime,seclabel,attr2,inode64,noquota)
```

步骤2：进入/mnt/mnt16写入数据，并查看其大小

```shell
[root@localhost mnt16]# echo {1..10000000} >>test.txt 
[root@localhost mnt16]# du -h *
128M	test.txt
文件系统       类型  容量  已用  可用 已用% 挂载点
/dev/sdb6      xfs   9.5G  161M  9.4G    2% /mnt/mnt16
```

步骤3：取消挂载

**注意：取消挂载前提是此磁盘没人使用的情况**

```shell
[root@localhost mnt16]# umount /mnt/mnt16
umount: /mnt/mnt16：目标忙。
        (有些情况下通过 lsof(8) 或 fuser(1) 可以
         找到有关使用该设备的进程的有用信息)
#可用lsof命令检查进程是否占用
[root@localhost mnt16]# lsof /mnt/mnt16
COMMAND  PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
bash    1551 root  cwd    DIR   8,22       35   64 /mnt/mnt16
lsof    1862 root  cwd    DIR   8,22       35   64 /mnt/mnt16
lsof    1863 root  cwd    DIR   8,22       35   64 /mnt/mnt16
#可用kills -9 进程号  或用fuser -km /mnt/mnt16杀死进程
[root@localhost /]# kills -9 1700
[root@localhost /]#fuser -km /mnt/mnt16

```

步骤4：以只读形式挂载设备

```shell
[root@localhost /]# mount -o ro /dev/sdb6 /mnt/mnt16
[root@localhost /]# mount -l|grep sdb6
/dev/sdb6 on /mnt/mnt16 type xfs (ro,relatime,seclabel,attr2,inode64,noquota)
[root@localhost mnt16]# touch 123
touch: 无法创建"123": 只读文件系统
#只有只读权限，所以无法创建文件
```

步骤5：禁止挂载后的设备，执行二进制文件

```shell
#发现进程存在无法取消卸载
[root@localhost /]# lsof |grep mnt/mnt16
vim       1724         root  cwd       DIR               8,22        22         64 /mnt/mnt16

#通过kills -9 1724杀死进程
[root@localhost /]# lsof |grep mnt/mnt16
[1]+  已杀死               vim scripts.sh(工作目录：/mnt/mnt16)
(当前工作目录：/)

#此时可取消挂载
[root@localhost /]# umount /mnt/mnt16
[root@localhost /]# mount -l|grep sdb6     #查看挂载列表有无sdb6相关信息

#重新挂载
[root@localhost /]# mount /dev/sdb6 /mnt/mnt16
[root@localhost /]# mount -l|grep sdb6
/dev/sdb6 on /mnt/mnt16 type xfs (rw,relatime,seclabel,attr2,inode64,noquota)

[root@localhost mnt16]# ./aa.sh
-bash: ./aa.sh: 权限不够

```

### （9）swap交换分区

- 交换分区：当实际内存不够用的时候，操作系统会从内存取出一部暂时不用的数据，放在交换内存 ，从而给与当前程序使用更多的内存。

- **mkswap命令**
- **作用：格式化交换分区**
- **语法：mkswap  设备名**
- **swapon：启用交换分区**
- **swapoff：关闭交换分区**

- 分配规则：
- 内存小于2G，分配同等swap大小的分区
- 内存大于2G，分配2G交换空间

创建交换分区步骤

```shell
#1.首先利用fdisk命令创建一个主分区
fdisk /dev/sdc
m   #查看帮助信息
n	#创建一个分区
p	#选择主分区
默认起点分区
终点：输入+500M
t	#更改分区类型为82linux交换分区


#2.再通过mkswap命令，格式化分区
[root@localhost ~]# mkswap /dev/sdc1
mkswap: /dev/sdc1: warning: wiping old swap signature.
正在设置交换空间版本 1，大小 = 511996 KiB
无标签，UUID=562dc64f-bc7c-4a4f-a69b-09be2cf29b41

#3.启用交换分区
[root@localhost ~]# swapon /dev/sdc1

#4。查看交换分区情况，多个500M
[root@localhost ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           3.7G        156M        3.3G         11M        198M        3.3G
Swap:          2.5G          0B        2.5G

#5.永久生效swap
vim /etc/fstab
输入i,进入插入模式
/dev/sdc1 swap swap defaults 0 0 
按下esc建退出插入模式，输入：wq!保存

#6.关闭交换分区
[root@localhost ~]# swapoff /dev/sdc1
[root@localhost ~]# free -h
              total        used        free      shared  buff/cache   available
Mem:           3.7G        156M        3.3G         11M        203M        3.3G
Swap:          2.0G          0B        2.0G

```

- 挂载永久生效方法
- 在/etc/fstab配置文件结尾追加内容；内容如下：
- /dev/sdb1 (分区位置)    /mnt（挂载点）   xfs（文件类型）   defaults 0 0



### **（10）buffers和cache**

- buffers：缓冲，加上速写入数据，把分散的数据写入保存到缓冲区，达到一定的程度集中写入硬盘，减少磁盘碎片，以及反复的寻道时间，加速数据写入。
- cache：缓存，加速读取数据，从磁盘读取出来，放到缓存区，再次读取则直接从缓存区读取，加速读取。



### （11）raid多级技术

- raid： 磁盘冗余阵列。由多个磁盘组成一个磁盘组。
- 作用：1.能够保障数据的安全性；2.提升硬盘的的读写效率
- raid级别
- raid0：特点：依次写入搭配物理硬盘，写入速度翻倍，一旦一块硬盘损坏，则所有数据都被破坏。适用于追求极致性能，数据安全性不高的场景。
- raid1：特点：将两块以上的硬盘绑定关系，同时写入两个硬盘数据，磁盘损坏，也有备份数据，缺点是极大的减少了利用率。
- raid3：特点：必须要搭建三块以上的硬盘，存储的异或值的磁盘不能损坏，否则无法恢复
- 计算机的异或运算，数字相同为0，数字不同则为1
- 针对磁盘的异或运算：1的个数是奇数，结果为1;1的个数是偶数，结果为0;
- 写法：AxorBxorC表示A异或B异或C

- raid5：特点：比raid3更强大，校验码均匀的放在每一块硬盘上，即使任意一块磁盘损坏，都能够反推出原本的数据

- raid10：企业目前在使用。实际则是raid0+raid1，
- 特点：
- 1.即吸收了raid0的特点提升写入速度，又吸收了raid1的安全性，至少需要4块硬盘完成；
- 2.通过raid1技术，实现磁盘两两备份，数据安全性提高；
- 3.针对2个raid1.又部署了raid0，则又提高了磁盘的读写效率；
- 4.只要不是同一个磁盘组损坏，那么即使挂掉一块硬盘也不受影响。

### （12）软raid和硬raid的区别

- 软raid：linuxt通过软件命令创建，由cpu去控制硬盘驱动器进行数据转换;
- 硬raid：即raid卡（硬件）

区别：

- 1.软raid会额外的消耗cpu的资源，造成服务器压力；
- 2.硬raid更加稳定，且软raid可能会造成磁盘发热过量，导致问题出现
- 3.硬raid的兼容性更好，而软raid则以来操作系统，可能会出现问题
- 4.硬raid完胜

### （13）raid10的创建使用

1.首先准备4个磁盘（在虚拟机创建），用于创建raid10；

2.检查磁盘信息

```shell
[root@xiaosu ~]# ls /dev/sd*
/dev/sda  /dev/sda1  /dev/sda2  /dev/sdb  /dev/sdc  /dev/sdd  /dev/sde
[root@xiaosu ~]# fdisk -l|grep sd
磁盘 /dev/sda：21.5 GB, 21474836480 字节，41943040 个扇区
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200    41943039    19921920   8e  Linux LVM
磁盘 /dev/sdd：5368 MB, 5368709120 字节，10485760 个扇区
磁盘 /dev/sde：5368 MB, 5368709120 字节，10485760 个扇区
磁盘 /dev/sdb：5368 MB, 5368709120 字节，10485760 个扇区
磁盘 /dev/sdc：5368 MB, 5368709120 字节，10485760 个扇区
```

3.mdadm命令，用于管和监控raid技术的命令

```shell
#以下命令用于创建raid10
[root@xiaosu dev]# mdadm -Cv  /dev/md0 -a yes -n 4 -l 10 /dev/sdb /dev/sdc /dev/sdd /dev/sde
mdadm: layout defaults to n2
mdadm: layout defaults to n2
mdadm: chunk size defaults to 512K
mdadm: size set to 5237760K
mdadm: Fail create md0 when using /sys/module/md_mod/parameters/new_array
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
-c:创建raid阵列
-v：显示过程
/dev/md0 raid阵列的名字
-a yes：	自动创建阵列设备文件
-n 4：用4块硬盘创建
最后为4块硬盘的名字
```

4.格式化磁盘阵列

```shell
[root@xiaosu dev]# mkfs.xfs /dev/md0
meta-data=/dev/md0               isize=512    agcount=16, agsize=163712 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=2618880, imaxpct=25
         =                       sunit=128    swidth=256 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=8 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0

```

5.挂载

```shell
#新建一个挂载目录
[root@xiaosu /]# mkdir xiaoming
#将磁盘挂载到xioaming
[root@xiaosu /]# mount /dev/md0  xiaoming/

```

7.检查挂载情况

```shell
[root@xiaosu /]# mount -l|grep md0
/dev/md0 on /xiaoming type xfs (rw,relatime,seclabel,attr2,inode64,sunit=1024,swidth=2048,noquota)
```

8.通过df命令检查磁盘使用情况

```shell
[root@xiaosu /]# df -hT|grep md0
/dev/md0                xfs        10G   33M   10G    1% /xiaoming
#注意：四个硬盘大小共20G，每个硬盘5G，实际可利用率为50%则为10G
```

9.检查raid10的信息

```shell
[root@xiaosu /]# mdadm -D /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Fri Sep  4 19:42:50 2020
        Raid Level : raid10
        Array Size : 10475520 (9.99 GiB 10.73 GB)
     Used Dev Size : 5237760 (5.00 GiB 5.36 GB)
      Raid Devices : 4
     Total Devices : 4
       Persistence : Superblock is persistent

       Update Time : Fri Sep  4 19:47:09 2020
             State : clean 
    Active Devices : 4
   Working Devices : 4
    Failed Devices : 0
     Spare Devices : 0

            Layout : near=2
        Chunk Size : 512K

Consistency Policy : resync

              Name : xiaosu:0  (local to host xiaosu)
              UUID : 6019fdb7:dc8b67d9:65d0a16a:89dc5fe5
            Events : 17

    Number   Major   Minor   RaidDevice State
       0       8       16        0      active sync set-A   /dev/sdb
       1       8       32        1      active sync set-B   /dev/sdc
       2       8       48        2      active sync set-A   /dev/sdd
       3       8       64        3      active sync set-B   /dev/sde
```

10.可在磁盘阵列中写入数据。可用top命令动态检测



```shell
[root@xiaosu /]# echo {1..10000000} > /xiaoming/test.txt 
[root@xiaosu /]# du -h /xiaoming/
76M	/xiaoming/
[root@xiaosu /]# df -h
文件系统                 容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root   17G  1.2G   16G    7% /
devtmpfs                 1.9G     0  1.9G    0% /dev
tmpfs                    1.9G     0  1.9G    0% /dev/shm
tmpfs                    1.9G   12M  1.9G    1% /run
tmpfs                    1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/sda1               1014M  142M  873M   14% /boot
tmpfs                    378M     0  378M    0% /run/user/0
/dev/md0                  10G  387M  9.7G    4% /xiaoming
```

11.取消挂载，无法访问挂载点的文件，必须重新挂载

```shell
[root@xiaosu /]# umount /dev/md0 
[root@xiaosu /]# ls /xiaoming/test.txt 
ls: 无法访问/xiaoming/test.txt: 没有那个文件或目录
[root@xiaosu /]# ls /xiaoming/
```

11.开机自动挂载,并重启机器查看

```shell
[root@xiaosu /]# mount /dev/md0  /xiaoming/
[root@xiaosu /]# ls /xiaoming/test.txt 
/xiaoming/test.txt
[root@xiaosu /]# vim /etc/fstab 
#在结尾输入/dev/md0 /xiaoming xfs defaults 0 0，保存退出
#
# /etc/fstab
# Created by anaconda on Thu Aug 13 04:54:02 2020
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=68ab460e-835f-4fb5-a273-04853c1eb0cd /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0
/dev/sdc1 swap swap defaults 0 0
/dev/md0 /xiaoming xfs defaults 0 0 

```

### （14）raid10故障处理

故障点：其中一块硬盘损坏

处理步骤

1.模拟一块硬盘损坏，即可移除一块硬盘即可

```shell
[root@xiaosu ~]# mdadm /dev/md0 -f /dev/sdb
mdadm: set /dev/sdb faulty in /dev/md0
```

2.检查raid10的状态

```shell
[root@xiaosu ~]# mdadm -D /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Fri Sep  4 19:42:50 2020
        Raid Level : raid10
        Array Size : 10475520 (9.99 GiB 10.73 GB)
     Used Dev Size : 5237760 (5.00 GiB 5.36 GB)
      Raid Devices : 4
     Total Devices : 4
       Persistence : Superblock is persistent

       Update Time : Fri Sep  4 20:10:53 2020
             State : clean, degraded 
    Active Devices : 3
   Working Devices : 3
    Failed Devices : 1
     Spare Devices : 0

            Layout : near=2
        Chunk Size : 512K

Consistency Policy : resync

              Name : xiaosu:0  (local to host xiaosu)
              UUID : 6019fdb7:dc8b67d9:65d0a16a:89dc5fe5
            Events : 19

    Number   Major   Minor   RaidDevice State
       -       0        0        0      removed
       1       8       32        1      active sync set-B   /dev/sdc
       2       8       48        2      active sync set-A   /dev/sdd
       3       8       64        3      active sync set-B   /dev/sde

       0       8       16        -      faulty   /dev/sdb
```

3.挂了一块硬盘不影响其他硬盘读写，可继续读写使用

4.购买或置换一块新硬盘，重新添加到raid10即可

5.添加之前，必须取消挂载

```shell
#解除挂载
[root@xiaosu ~]# umount /dev/md0 
#重启系统
[root@xiaosu ~]# reboot
#重连系统后，由于自动开机挂载生效，需再次进行挂载
[root@xiaosu ~]# umount /dev/md0 
#此时添加一块硬盘
[root@xiaosu ~]# mdadm /dev/md0 -a /dev/sdb
mdadm: added /dev/sdb
#查看磁盘情况，等待修复过程，等待激活硬盘数、工作硬盘数为4个即可
#[root@xiaosu ~]# mdadm -D /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Fri Sep  4 19:42:50 2020
        Raid Level : raid10
        Array Size : 10475520 (9.99 GiB 10.73 GB)
     Used Dev Size : 5237760 (5.00 GiB 5.36 GB)
      Raid Devices : 4
     Total Devices : 4
       Persistence : Superblock is persistent

       Update Time : Fri Sep  4 20:26:05 2020
             State : clean 
    Active Devices : 4
   Working Devices : 4
    Failed Devices : 0
     Spare Devices : 0

            Layout : near=2
        Chunk Size : 512K

Consistency Policy : resync

              Name : xiaosu:0  (local to host xiaosu)
              UUID : 6019fdb7:dc8b67d9:65d0a16a:89dc5fe5
            Events : 44

    Number   Major   Minor   RaidDevice State
       4       8       16        0      active sync set-A   /dev/sdb
       1       8       32        1      active sync set-B   /dev/sdc
       2       8       48        2      active sync set-A   /dev/sdd
       3       8       64        3      active sync set-B   /dev/sde


```

### （15）raid重启

1.创建raid的配置文件

```shell
[root@xiaosu ~]# echo DEVICE /dev/sd[b-e] >> /etc/mdadm.conf
[root@xiaosu ~]# cat /etc/mdadm.conf 
DEVICE /dev/sdb /dev/sdc /dev/sdd /dev/sde
```

2.扫描磁盘阵列信息，追加到这个文件中

```shell
[root@xiaosu ~]# mdadm -Ds >> /etc/mdadm.conf 
[root@xiaosu ~]# cat /etc/mdadm.conf 
DEVICE /dev/sdb /dev/sdc /dev/sdd /dev/sde
ARRAY /dev/md/0 metadata=1.2 name=xiaosu:0 UUID=6019fdb7:dc8b67d9:65d0a16a:89dc5fe5
```

3.取消raid10的挂载

```shell
[root@xiaosu ~]# umount /dev/md0
```

4.停止raid10

```shell
[root@xiaosu ~]# mdadm -S /dev/md0
mdadm: stopped /dev/md0
```

5.检查磁盘阵列的信息

```shell
[root@xiaosu ~]# mdadm -D /dev/md0
mdadm: cannot open /dev/md0: No such file or directory  #证明已经停止
```

6.存在配置文件的情况下，可正常启动raid10

```shell
[root@xiaosu ~]# mdadm -A /dev/md0
mdadm: Fail create md0 when using /sys/module/md_mod/parameters/new_array
mdadm: /dev/md0 has been started with 4 drives.
```

7.可正常查看raid10的信息了

```shell
[root@xiaosu ~]# mdadm -D /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Fri Sep  4 19:42:50 2020
        Raid Level : raid10
        Array Size : 10475520 (9.99 GiB 10.73 GB)
     Used Dev Size : 5237760 (5.00 GiB 5.36 GB)
      Raid Devices : 4
     Total Devices : 4
       Persistence : Superblock is persistent

       Update Time : Fri Sep  4 20:40:42 2020
             State : clean 
    Active Devices : 4
   Working Devices : 4
    Failed Devices : 0
     Spare Devices : 0

            Layout : near=2
        Chunk Size : 512K

Consistency Policy : resync

              Name : xiaosu:0  (local to host xiaosu)
              UUID : 6019fdb7:dc8b67d9:65d0a16a:89dc5fe5
            Events : 44

    Number   Major   Minor   RaidDevice State
       4       8       16        0      active sync set-A   /dev/sdb
       1       8       32        1      active sync set-B   /dev/sdc
       2       8       48        2      active sync set-A   /dev/sdd
       3       8       64        3      active sync set-B   /dev/sde
```

### （17）raid10的卸载

1.卸载挂载中的设备

```shell
[root@xiaosu ~]# umount /dev/md0
```

2.停止raid10服务

```shell
[root@xiaosu ~]# mdadm -S /dev/md0
mdadm: stopped /dev/md0
```

3.卸载raid10中所有磁盘信息

```shell
[root@xiaosu ~]# mdadm --misc --zero-superblock /dev/sdb
[root@xiaosu ~]# mdadm --misc --zero-superblock /dev/sdc
[root@xiaosu ~]# mdadm --misc --zero-superblock /dev/sdd
[root@xiaosu ~]# mdadm --misc --zero-superblock /dev/sde
```

4.删除raid的配置文件

```shell
[root@xiaosu ~]# rm /etc/mdadm.conf 
rm：是否删除普通文件 "/etc/mdadm.conf"？y
```

5.清除开机挂载的配置信息

```shell
[root@xiaosu /]# vim /etc/fstab 
将与/dev/md0的挂载信息删除，并保存
```

### （18）raid备份盘

1.四块磁盘，一个做备份盘，其余3个工作盘。三块则用raid5

```shell
#首先创建阵列
[root@xiaosu ~]# mdadm -Cv /dev/md0 -n 3 -l 5 -x 1 /dev/sdb /dev/sdc /dev/sdd /dev/sde
mdadm: layout defaults to left-symmetric
mdadm: layout defaults to left-symmetric
mdadm: chunk size defaults to 512K
mdadm: size set to 5237760K
mdadm: Fail create md0 when using /sys/module/md_mod/parameters/new_array
mdadm: Defaulting to version 1.2 metadata
mdadm: array /dev/md0 started.
-C:创建raid
-v：显示创建过程
-n：指定3个磁盘
-l:指定为raid5
-x：指定1个备份盘
后面为四个盘的名字
```

2.检查磁盘状态

```shell
[root@xiaosu ~]# mdadm -D /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Fri Sep  4 20:56:48 2020
        Raid Level : raid5
        Array Size : 10475520 (9.99 GiB 10.73 GB)
     Used Dev Size : 5237760 (5.00 GiB 5.36 GB)
      Raid Devices : 3
     Total Devices : 4
       Persistence : Superblock is persistent

       Update Time : Fri Sep  4 20:57:14 2020
             State : clean 
    Active Devices : 3
   Working Devices : 4
    Failed Devices : 0
     Spare Devices : 1

            Layout : left-symmetric
        Chunk Size : 512K

Consistency Policy : resync

              Name : xiaosu:0  (local to host xiaosu)
              UUID : 71b4ef93:1c7bb810:9c2f36f9:6563badf
            Events : 18

    Number   Major   Minor   RaidDevice State
       0       8       16        0      active sync   /dev/sdb
       1       8       32        1      active sync   /dev/sdc
       4       8       48        2      active sync   /dev/sdd

       3       8       64        -      spare   /dev/sde
       #备份盘为/dev/sde，活动硬盘为3个，等待工作的为4个
```

3.磁盘格式化

```shell
[root@xiaosu ~]# mkfs.xfs -f /dev/md0
meta-data=/dev/md0               isize=512    agcount=16, agsize=163712 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=2618880, imaxpct=25
         =                       sunit=128    swidth=256 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=8 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```

4.开始挂载使用阵列组，并查看挂载情况

```shell
[root@xiaosu ~]# mount /dev/md0 /xiaoming/
[root@xiaosu ~]# mount -l|grep md0
/dev/md0 on /xiaoming type xfs (rw,relatime,seclabel,attr2,inode64,sunit=1024,swidth=2048,noquota)
```

5.查看磁盘空间情况

```SHELL
[root@xiaosu ~]# df -hT 
文件系统                类型      容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root xfs        17G  1.2G   16G    7% /
devtmpfs                devtmpfs  1.9G     0  1.9G    0% /dev
tmpfs                   tmpfs     1.9G     0  1.9G    0% /dev/shm
tmpfs                   tmpfs     1.9G   12M  1.9G    1% /run
tmpfs                   tmpfs     1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/sda1               xfs      1014M  142M  873M   14% /boot
tmpfs                   tmpfs     378M     0  378M    0% /run/user/0
/dev/md0                xfs        10G   33M   10G    1% /xiaoming
```

6.输入数据，查看磁盘空间有无变化

```SHELL
[root@xiaosu xiaoming]# echo {1..10000000} >/xiaoming/test.txt 
[root@xiaosu xiaoming]# df -hT
文件系统                类型      容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root xfs        17G  1.2G   16G    7% /
devtmpfs                devtmpfs  1.9G     0  1.9G    0% /dev
tmpfs                   tmpfs     1.9G     0  1.9G    0% /dev/shm
tmpfs                   tmpfs     1.9G   12M  1.9G    1% /run
tmpfs                   tmpfs     1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/sda1               xfs      1014M  142M  873M   14% /boot
tmpfs                   tmpfs     378M     0  378M    0% /run/user/0
/dev/md0                xfs        10G  108M  9.9G    2% /xiaoming
```

7.见证备份盘情况，从三个工作盘中删除一个磁盘。

```shell
[root@xiaosu xiaoming]# mdadm /dev/md0 -f /dev/sdb
mdadm: set /dev/sdb faulty in /dev/md0
```

8.检查磁盘状态

```shell
[root@xiaosu xiaoming]# mdadm -D /dev/md0
/dev/md0:
           Version : 1.2
     Creation Time : Fri Sep  4 20:56:48 2020
        Raid Level : raid5
        Array Size : 10475520 (9.99 GiB 10.73 GB)
     Used Dev Size : 5237760 (5.00 GiB 5.36 GB)
      Raid Devices : 3
     Total Devices : 4
       Persistence : Superblock is persistent

       Update Time : Fri Sep  4 21:13:23 2020
             State : clean 
    Active Devices : 3
   Working Devices : 3
    Failed Devices : 1
     Spare Devices : 0

            Layout : left-symmetric
        Chunk Size : 512K

Consistency Policy : resync

              Name : xiaosu:0  (local to host xiaosu)
              UUID : 71b4ef93:1c7bb810:9c2f36f9:6563badf
            Events : 37

    Number   Major   Minor   RaidDevice State
       3       8       64        0      active sync   /dev/sde   #可见备份盘已经加入阵列组工作
       1       8       32        1      active sync   /dev/sdc
       4       8       48        2      active sync   /dev/sdd

       0       8       16        -      faulty   /dev/sdb   #这个磁盘已经挂了
```



### （19）lvm原理及动态调整空间使用

l**vm是什么**：逻辑卷技术。将一个或多个硬盘进行和并，当磁盘空间不够，可将其他硬盘的容量拿过来使用的技术

**lvm的使用方式：**

​	**基于分区形式创建lvm**

- 1.硬盘的多个分区，由lvm同一进行管理为卷组，可弹性的调整大小，加入新硬盘，可充分的利用磁盘容量
- 2.文件系统创建在逻辑卷上，逻辑卷可以根据需求改变大小 （总容量控制在卷组中）

​	**基于多个硬盘创建lvm**

- 1.多块硬盘做成逻辑卷，将整个逻辑卷同一管理，对分区进行动态扩容



lvm的常见名词

- PP：物理分区，lvm直接创建在屋里分区上
- PV：物理卷，物理卷，处于lvm的最底层，一个pv对应一个pp
- PE：物理区域，PV中可用于分配的最小存储单位，同一个vg所有的pv中的pe大小相同，例如1M,2M
- VG:卷组，创建在PV之上，可分为多个pv
- LE:扩展逻辑单元，是组成LV的基本单元，一个LE对应一个LV
- LV:逻辑卷，创建在VG之上



**创建lvm的过程**

- 1.物理分区阶段，针对物理磁盘或分区，进行fdisk格式化，修改系统的id，默认83，改为8e，lvm类型
- 2.PV阶段，通过pvcreate，pvdisplay将linux分区改为物理卷PV
- 3.创建VG的阶段，通过vgcreate、vgdisplay，将创建好的物理卷改为物理卷组
- 4.创建的LV，通过lvcreate，将卷组分为u若干个逻辑卷

**命令转换过程**

- 1.fdisk修改磁盘的系统id
- 2.pvcreate，创建PV，以及显示PV的信息， pvdisplay 也可直接输入pvs查看简单的信息
- 3.创建VG卷组，vgcreate    以及显示卷组信息 vgs
- 4.创建lv逻辑卷， lvcreate   以及显示逻辑信息 lvs
- 5.开始格式化文件系统，使用lv分区了

**lvm常见管理命令**

- pvcreate 创建物理卷
- pvscan   扫描物理卷信息
- pvdisplay   显示各个物理卷详细参数
- pvremove  删除物理卷

- lv、vg命令等同上类似

- vgcreate    创建卷组
- vgscan       扫描卷组信息
- vgdisplay   显示各个卷组的详细参数
- vgreduce   缩小卷组，将物理卷从卷组移除
- vgextend   扩大卷组，将新的物理卷加入到卷组中
- vgremove  删除整个卷组



创建过程

1.选择两块硬盘进行创建物理卷，并扫描物理卷信息

```shell
[root@xiaosu ~]# pvcreate /dev/sdb /dev/sdc
  Physical volume "/dev/sdb" successfully created.
  Physical volume "/dev/sdc" successfully created.
  
[root@xiaosu ~]# pvs
  PV         VG     Fmt  Attr PSize   PFree 
  /dev/sda2  centos lvm2 a--  <19.00g     0 
  /dev/sdb          lvm2 ---   10.00g 10.00g
  /dev/sdc          lvm2 ---   10.00g 10.00g
```

2.创建卷组,并扫描卷组信息

```shell
[root@xiaosu ~]# vgcreate su_vg1 /dev/sdb /dev/sdc
  Volume group "su_vg1" successfully created
[root@xiaosu ~]# vgs
  VG     #PV #LV #SN Attr   VSize   VFree 
  centos   1   2   0 wz--n- <19.00g     0 
  su_vg1   2   0   0 wz--n-  19.99g 19.99g
```

3.分别查看pv和vg的详细信息

```shell
[root@xiaosu ~]# pvdisplay 
[root@xiaosu ~]# vgdisplay 
```

4.针对卷组扩容

​	步骤1：将sdd创建物理卷

```shell
[root@xiaosu ~]# pvcreate /dev/sdd
  Physical volume "/dev/sdd" successfully created.
```

​	步骤2：将sdd的加入卷组	

```
[root@xiaosu ~]# vgextend su_vg1 /dev/sdd
  Volume group "su_vg1" successfully extended
```

5.显示卷组信息

```shell
[root@xiaosu ~]# vgs
  VG     #PV #LV #SN Attr   VSize   VFree  
  centos   1   2   0 wz--n- <19.00g      0 
  su_vg1   3   0   0 wz--n- <24.99g <24.99g
[root@xiaosu ~]# vgdisplay
 --- Volume group ---
  VG Name               su_vg1
  System ID             
  Format                lvm2
  Metadata Areas        3
  Metadata Sequence No  2
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                3
  Act PV                3
  VG Size               <24.99 GiB
  PE Size               4.00 MiB
  Total PE              6397
  Alloc PE / Size       0 / 0   
  Free  PE / Size       6397 / <24.99 GiB
  VG UUID               Ovvhgi-ymru-ReWg-cyQD-Sn7h-gfcg-zsDFAS
  
```

6.缩小容量，将其中一块磁盘从卷组中移除

```shell
[root@xiaosu ~]# vgreduce su_vg1 /dev/sdd
  Removed "/dev/sdd" from volume group "su_vg1"
[root@xiaosu ~]# vgs
  VG     #PV #LV #SN Attr   VSize   VFree 
  centos   1   2   0 wz--n- <19.00g     0 
  su_vg1   2   0   0 wz--n-  19.99g 19.99g
```

7.可将移除的磁盘剔除

```shell
[root@xiaosu ~]# pvremove  /dev/sdd
  Labels on physical volume "/dev/sdd" successfully wiped.
```

8.使用此时的卷组创建lv逻辑卷,并查看lv信息

```shell
[root@xiaosu ~]# lvcreate -n lv1 -L +500M su
  Logical volume "lv1" created.
[root@xiaosu ~]# lvs
  LV   VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root centos -wi-ao---- <17.00g                                                    
  swap centos -wi-ao----   2.00g                                                    
  lv1  su     -wi-a----- 500.00m   
```

9.将lv卷进行格式化

```shell
[root@xiaosu ~]# mkfs.xfs /dev/su/lv1 
meta-data=/dev/su/lv1            isize=512    agcount=4, agsize=32000 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=128000, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=855, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0

[root@xiaosu ~]# lsblk -f
NAME            FSTYPE      LABEL           UUID                                   MOUNTPOINT
sda                                                                                
├─sda1          xfs                         68ab460e-835f-4fb5-a273-04853c1eb0cd   /boot
└─sda2          LVM2_member                 Iiv4Tf-aKWy-Riis-eWay-2Fhp-aeYa-iMRsrv 
  ├─centos-root xfs                         1cce9747-f741-4829-9b03-88f61c6ba753   /
  └─centos-swap swap                        d7a0b7ad-da95-46eb-915a-dcdc80bf7ab1   [SWAP]
sdb             LVM2_member                 I3QG1A-SIKO-e5iH-kFdr-IoL1-2tRc-0yVc4F 
└─su-lv1        xfs                         74a7f6fd-930c-4baf-a7f6-6cbca05d09f7   
sdc             LVM2_member                 UpHn6B-devb-5Yal-6lsi-pYqu-eOsx-BCTrCT 
sdd                                                                                
sr0             iso9660     CentOS 7 x86_64 2018-05-03-20-55-23-00  
```

10.挂载lv1分区，就可进行读写使用

```shell
[root@xiaosu syh]# mount /dev/su/lv1 /syh/syh_lv1/
[root@xiaosu syh]# df -hT 
文件系统                类型      容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root xfs        17G  1.2G   16G    7% /
devtmpfs                devtmpfs  1.9G     0  1.9G    0% /dev
tmpfs                   tmpfs     1.9G     0  1.9G    0% /dev/shm
tmpfs                   tmpfs     1.9G   12M  1.9G    1% /run
tmpfs                   tmpfs     1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/sda1               xfs      1014M  143M  872M   15% /boot
tmpfs                   tmpfs     378M     0  378M    0% /run/user/0
/dev/mapper/su-lv1      xfs       497M   26M  472M    6% /syh/syh_lv1
```

11.挂载点进行数据写入，查看磁盘容量变化

```shell
[root@xiaosu syh_lv1]# echo {1..1000000}>test1.txt 
[root@xiaosu syh_lv1]# df -hT
文件系统                类型      容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root xfs        17G  1.2G   16G    7% /
devtmpfs                devtmpfs  1.9G     0  1.9G    0% /dev
tmpfs                   tmpfs     1.9G     0  1.9G    0% /dev/shm
tmpfs                   tmpfs     1.9G   12M  1.9G    1% /run
tmpfs                   tmpfs     1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/sda1               xfs      1014M  143M  872M   15% /boot
tmpfs                   tmpfs     378M     0  378M    0% /run/user/0
/dev/mapper/su-lv1      xfs       497M   32M  465M    7% /syh/syh_lv1
```

12.实现开机自动挂载

```shell
echo '/dev/mapper/su-lv1  /syh/syh_lv1 xfs defaults 0 0'
```

13.针对逻辑卷扩容，容量可从卷组余量卷组获取

 	1.将lv1卸载

```shell
[root@xiaosu /]# umount /syh/syh_lv1/
```

​	2.命令逻辑卷扩容，并查看扩容后大小

```shell
[root@xiaosu /]# lvextend  -L +10G /dev/su/lv1 
  Size of logical volume su/lv1 changed from 500.00 MiB (125 extents) to <10.49 GiB (2685 extents).
  Logical volume su/lv1 successfully resized.
[root@xiaosu /]# lvs
  LV   VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root centos -wi-ao---- <17.00g                                                    
  swap centos -wi-ao----   2.00g                                                    
  lv1  su     -wi-a----- <10.49g  
```

​	3.重新挂载到指定目录

```shell
[root@xiaosu /]# mount /dev/su/lv1  /syh/syh_lv1/
```

14.调整文件系统大小，并查看扩展后的容量

```shell
[root@xiaosu /]# xfs_growfs /dev/su/lv1 
meta-data=/dev/mapper/su-lv1     isize=512    agcount=4, agsize=32000 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=128000, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=855, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 128000 to 2749440

[root@xiaosu /]# df -h
文件系统                 容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root   17G  1.2G   16G    7% /
devtmpfs                 1.9G     0  1.9G    0% /dev
tmpfs                    1.9G     0  1.9G    0% /dev/shm
tmpfs                    1.9G   12M  1.9G    1% /run
tmpfs                    1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/sda1               1014M  143M  872M   15% /boot
tmpfs                    378M     0  378M    0% /run/user/0
/dev/mapper/su-lv1        11G   35M   11G    1% /syh/syh_lv1
```

15.不想用lvm逻辑卷时的删除方法

​	1.卸载设备

```shell
[root@xiaosu /]# umount /syh/syh_lv1
```

​	2.删除逻辑卷	

```shell
[root@xiaosu /]# lvremove /dev/su/lv1 
Do you really want to remove active logical volume su/lv1? [y/n]: y
  Logical volume "lv1" successfully removed
```

​	3.删除卷组

```shell
[root@xiaosu /]# vgremove  su
  Volume group "su" successfully removed
```

​	4.删除物理卷设备

```shell
[root@xiaosu /]# pvs									#查看物理卷有哪些
  PV         VG     Fmt  Attr PSize   PFree 
  /dev/sda2  centos lvm2 a--  <19.00g     0 
  /dev/sdb          lvm2 ---   10.00g 10.00g
  /dev/sdc          lvm2 ---   10.00g 10.00g
[root@xiaosu /]# pvremove  /dev/sdb /dev/sdc			   #删除物理卷设备
  Labels on physical volume "/dev/sdb" successfully wiped.
  Labels on physical volume "/dev/sdc" successfully wiped.
```

​	5.检查所有的lvm信息

```shell
[root@xiaosu /]# pvs
  PV         VG     Fmt  Attr PSize   PFree
  /dev/sda2  centos lvm2 a--  <19.00g    0 
[root@xiaosu /]# vgs
  VG     #PV #LV #SN Attr   VSize   VFree
  centos   1   2   0 wz--n- <19.00g    0 
[root@xiaosu /]# lvs
  LV   VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root centos -wi-ao---- <17.00g                                                    
  swap centos -wi-ao----   2.00g  
```



## 6、进程管理

进程：相当于工厂的车间

线程：相当于车间里的工人，

### （1）.linux的进程管理命令

**ps命令**

作用：用于查询进程信息，配合kill命令搭配，进行进程杀死，找出进程pid

语法：ps 参数 要操作的对象

1.ps不加参数

```shell
[root@localhost ~]# ps
   PID TTY          TIME CMD
  2250 pts/1    00:00:00 bash
  2268 pts/1    00:00:00 ps
#PID:进程的身份证号
#TTY:进程所属的控制台号码
#TIME:进程所使用cpu的总时间
#CMD：正在执行的系统命令行的是啥
```

2.配合grep使用

```shell
[root@localhost ~]# ps |grep bash
  2250 pts/1    00:00:00 bash
  #找出与bash相关的进程信息
```

3.ps -ef     #显示出所有的进程信息

```shell
[root@localhost ~]# ps -ef
UID         PID   PPID  C STIME TTY          TIME CMD
#-e：表示显示所有正在运行的进程
#-f：表示显示uid time 等信息

UID：进程是被哪个用户执行
PID:进程的表示码，用于停止进程使用
PPID：进程的父亲
C：表示cpu使用的百分比
STIME:进程开始的使用时间
TTY：该进程在哪个终端上使用
TIME：进程使用的cpu总时长
CMD:用户执行命令，产生的进程信息
```

4.ps -ef|grep vim    #过滤找出vim进程的信息

```shell
[root@localhost ~]# ps -ef|grep vim
root       2316   2297  0 20:03 pts/2    00:00:00 vim nihao.txt    #这个为vim进程
root       2318   2250  0 20:03 pts/1    00:00:00 grep --color=auto vim   #这个是使用命令vim产生的临时进程

```

**kill命令**

作用：杀死进程

语法：kill 参数 pid

kill -9 pid

参数

-l		#列出所有杀死信号

常用信号：

-  1) SIGHUP	：挂起进程，终端掉线，用户突然退出
-  2) SIGINT	：终端信号，一般用ctrl+c发送信号2
-  3) SIGQUIT	：退出信号，一般用ctrl+\发送信号2
-  9) SIGKILL	：强制中断信号，立即杀死某进程
-  15) SIGTERM：默认kill使用的信号，终止进程
-  20) SIGTSTP：暂停进程，组合键为ctrl+z发出暂停信号

终止进程

- kill pid  #默认15信号杀死进程
- kill  -9  pid	#强制杀死进程

kill之特殊信号0

kill -0 pid	#进程id存在的话，不做任何事情，可以检测进程的存活

echo $?	#shell的特殊变量，取出上一次命令的执行结果，返回0则表示命令正确，不为0则为错误状态码

```shell
[syh@localhost /]$ kill -0 2334	   #存在这个进程，所有不做处理
[syh@localhost /]$ echo $?			#为0则表示上一次的命令执行正确
0
[syh@localhost /]$ ls dasdfaf
ls: 无法访问dasdfaf: 没有那个文件或目录
[syh@localhost /]$ echo $?				#为0则表示上一次的命令执行错误
2
```

- **killall命令**
- 作用：可通过名字直接杀死一个pid进程

```shell
[root@localhost ~]# ps -ef|grep ping
syh       12164   2334  0 21:40 pts/1    00:00:00 ping jd.com
root      12186  12169  0 21:41 pts/2    00:00:00 ping www.baidu.com
root      12210  12191  0 21:41 pts/3    00:00:00 grep --color=auto ping
[root@localhost ~]# killall ping
```

- **pkill命令**
- 作用：可通过名字杀死多个进程，killall可能一次性杀不死进程（含子进程），killall要杀多次，而pkill能够直接杀死父、子进程

```shell
[root@localhost ~]# pkill ping 
```

pkill  -t 终端号			#通过终端名字杀死终端所有进程

t:表示指定终端号

```shell
[root@localhost ~]# pkill -t pts/1  #杀死终端号为1的所有进程
[root@localhost ~]# pkill -9  -t pts/1  #强制踢出终端号为1的所有进程，包括ssh
```



### （2）.ps的两种书写风格

- ps ef		#不带减号，  e表示列出进程信息时，添加每个进程所在的环境变量、f表示以ASCII码字符显示进程间的关系

- ps -ef       #带减号，    -e表示显示所有进程的信息、-f表示显示出UID 、PID、C、STIME、TIME等

  



ps  aux	#参数	a：当前终端下所有的进程，包括其他用户的进程信息；u以用户为主的信息显示；x：显示所有进程

```shell
[root@localhost ~]# ps aux
USER        PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
```

参数解释：

- USER:进程属于哪个用户使用
- PID:进程ID号
- %CPU:显示cpu的百分比使用情况
- %MEN:显示内存的百分比使用情况
- VSZ:该进程使用的swap内存单位
- RSS:进程所占用的内存量
- TTY:进程所在的终端信息
- STAT:进程的状态
  - S:终端睡眠状态，可被唤醒
  - s:该进程含有子进程，用s表示有子进程
  - R:进程运行中
  - D:进程不可终端睡眠
  - T:进程已停止
  - Z:进程时僵尸进程---父进程崩溃
  - +:前台进程
  - N:低优先级进程
  - <:高优先级进程
  - L:该进程已经被锁定
- START:进程使用的开始时间
- TIME:进程运行的时长
- CMD:进程执行的命令是啥

### （3）.常用两大进程**

- ps aux|grep mysql
- ps -ef|grep mysql

### （4）ps的基本用法

**1.ps -u		#显示指定用户的进程**

```shell
[root@localhost ~]# ps -u syh
   PID TTY          TIME CMD
  2334 pts/1    00:00:00 bash
  2356 pts/1    00:00:00 vim
```

**2.ps -eH 	#显示进程树的信息，**

```SHELL
[root@localhost ~]# ps -eH
   PID TTY          TIME CMD
  2246 ?        00:00:00     sshd
  2250 pts/1    00:00:00       bash
  2333 pts/1    00:00:00         su
  2334 pts/1    00:00:00           bash
#显示父进程、子进程的目录结构信息
```

**3.ps -eo   #自定义格式显示所有进程**

```shell
[root@localhost ~]# ps -eo pid,args
   PID COMMAND
```

**4.pstree 	#查看进程树**

安装进程树包  yum install psmisc -y

```shell
[syh@localhost /]$ pstree
systemd─┬─NetworkManager─┬─dhclient
        │                └─2*[{NetworkManager}]
        ├─VGAuthService
        ├─anacron
        ├─auditd───{auditd}
        ├─chronyd
        ├─crond
        ├─dbus-daemon───{dbus-daemon}
        ├─firewalld───{firewalld}
        ├─irqbalance
        ├─login───bash
        ├─lvmetad
        ├─master─┬─pickup
        │        └─qmgr
        ├─polkitd───5*[{polkitd}]
        ├─rsyslogd───2*[{rsyslogd}]
        ├─sshd─┬─sshd───bash
        │      └─sshd───bash───su───bash───pstree
        ├─systemd-journal
        ├─systemd-logind
        ├─systemd-udevd
        ├─tuned───4*[{tuned}]
        └─vmtoolsd───{vmtoolsd}
```

**5.pgrep命令	#通过程序的名字区查询相关进程，一般用于判断进程是否存活**

```shell
[syh@localhost /]$ pgrep bash   # 查询bash相关进程
1533
1554
2250
2334
```

​	**pgrep -l   #输出进程id号，以及进程名**

```shell
[syh@localhost /]$ pgrep -l ssh
1077 sshd
1550 sshd
2246 sshd
```

### （5）top资源管理器

- 1.top命令
- 作用：用于实时的监控系统的处理器状态，以及其他硬件负载信息、动态的进程信息等
- 进入top命令下，按z会变化字体颜色

用法

```shell
[root@localhost ~]# top
```

**2.第一大模块解释**

``` shell
[root@localhost ~]# top
top - 19:20:27 up 5 min,  1 user,  load average: 0.00, 0.03, 0.02
Tasks: 123 total,   1 running, 121 sleeping,   1 stopped,   0 zombie
%Cpu(s):  0.0 us,  0.1 sy,  0.0 ni, 99.9 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  3863568 total,  3525792 free,   154404 used,   183372 buff/cache
KiB Swap:  2097148 total,  2097148 free,        0 used.  3469696 avail Mem 
```

- 19:20:27 up：表示当前时间
- 5 min：表示 系统运行了5分钟
- 1user：当前机器几个用户在使用
- load average：0.00, 0.03, 0.02  #显示系统的平均负载情况，分别为3分钟、2分钟（值越小，负载压力越低）
- Tasks: 123 total,   1 running, 121 sleeping,   1 stopped,   0 zombie   #总共的进程任务情况（总共123个任务、）
- %Cpu(s):  0.0 us,  0.1 sy,  0.0 ni, 99.9 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st：
- us：用户占的cpu百分比情况
- sy：系统内核空间占用的cpu百分比
- ni：用户进程空间占用的cpu百分比
- id：空闲的cpu百分比
- wa：等待输输出的百分比
- KiB Mem :  3863568 total,  3525792 free,   154404 used,   183372 buff/cache				#内存的状态
- ​				物理内存总大小    空闲的内存量   使用的内存里      缓存的使用情况
- KiB Swap:  2097148 total,  2097148 free,        0 used.  3469696 avail Mem 					#交换空间的状态
- ​				交换空间缓存使用情况

**3.第二大模块解释**

```shell
  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND     
```

- PID:可对他启停进程
- USER:使用的用户
- PR:进程的优先级高低
- NI:nice值，越高表示优先级越高
- VIRT:进程使用的虚拟内存总量
- RES:进程使用的物理内存大小
- SHR:共享内存大小kb
- S:进程的状态
- %CPU：cpu使用百分比
- %MEM：内存使用百分比
- TIME+
- COMMAND

**4.实际使用**

- 进入top命令后输入1：#可查看linux的逻辑cpu个数
- 进入top命令后输入M：#可对内存大小进行倒序排序
- top -c ： # 可显示进程的绝对路径
- top -d 2 :#每2秒进行刷新
- top -n 3：#设置刷新的次数，刷新3s后停止刷新
- top -p PID:  #指定某个进程的资源使用情况

- #指定某一列高亮
- 输入z，打开颜色
- 输入x，某一列高亮
- 输入b，某一列颜色加粗
- 输入<>：可左右移动列



### （6）nohup后台运行程序

nohup命令

作用：将程序放在后台运行，不在终端打印显示信息，不方便输入命令操作

语法:nohup 执行的命令

案例

1.后台运行ping命令，将结果写入nohup.out内

```shell
[root@localhost ~]# nohup ping jd.com
nohup: 忽略输入并把输出追加到"nohup.out" 		#将输出结果写入到nohup.out中，程序会卡在前台，终端关闭也不会结束进程
```

2.后台运行命令，终端还能继续输入命令

```shell
[root@localhost ~]# nohup ping jd.com &
[root@localhost ~]# nohup: 忽略输入并把输出追加到"nohup.out"
```

3.不显示命令的输出结果，并将结果写入空洞文件中

```shell
[root@localhost ~]# ping jd.com >su.out 2>&1 &   #在后台将标准错误、正确输出都写入到文件输出
[6] 1636
```

**bg命令**

作用：将进程放入后台运行，使前台可运行别的命令

等同于命令&

案例

- 1.输入ping命令，运行一个进程

  ```shell
  [root@localhost ~]# ping jd.com
  PING jd.com (118.193.98.63) 56(84) bytes of data.
  64 bytes from 118.193.98.63 (118.193.98.63): icmp_seq=1 ttl=46 time=39.8 ms
  ```

  

- 2.输入ctrl+z，暂停进程并将进程放入后台运行

  ```shell
  ^Z
  [10]+  已停止               ping jd.com
  ```

  

- 3.通过jobs命令检查后台任务

  ```shell
  [root@localhost ~]# jobs
  [10]+  已停止               ping jd.com
  ```

  

- 4.要想前台继续运行进程，则输入fg 序号即可

  ```shell
  [root@localhost ~]# fg 10
  ping jd.com
  64 bytes from 118.193.98.63 (118.193.98.63): icmp_seq=18 ttl=46 time=39.3 ms
  ```

  

- 5.使用ctrl+z将进程暂停放入后台，可使用bg+序号

  ```shell
  [10]+  已停止               ping jd.com
  [root@localhost ~]# bg 10
  [10]+ ping jd.com &
  [root@localhost ~]# 64 bytes from 118.193.98.63 (118.193.98.63): icmp_seq=30 ttl=46 time=39.9 ms
  64 bytes from 118.193.98.63 (118.193.98.63): icmp_seq=31 ttl=46 time=40.1 ms
  ```

  

- 6.若不想日志在前台输出，可重定向输出到黑洞文件（三种写法）

  ```shell
  [root@localhost ~]# nohup ping jd.com 1> su.txt 2> su.txt
  [root@localhost ~]# nohup ping jd.com > su.txt 2>&1
  [root@localhost ~]# nohup ping jd.com &> su.txt 2
  #三种写法效果一样，都是将后台程序运行的标准正确输出、标准错误输出都写入到文件中
  ```

### （7）linux系统的运行级别

**runlevel命令			#检查当前系统的运行级别**

常见的运行级别如下：

- 0：关机
- 1：单用户模式
- 2：多用户模式、无网络模式
- 3：完全的多用户模式、有网络模式
- 4：用户自定义的级别
- 5：图形化界面的多用户模式
- 6：重启机器

**init命令**

- init：linux进程的初始化工具、是所有进程的父进程、进程id号默认是1
- 用法：init +级别号 ，可直接操作系统运行级别

```shell
[root@localhost ~]# init 6     #重启机器
```

### （8）htop资源管理工具

可通过yum intsall htop -y下载安装包

使用方法

- 1.输入htop进入界面；
- 2.F12或点击set up进入设置界面
- 3.可通过上下左右进行状态栏选择，以及用空格切换风格
- 4.F3进入搜索进程，输入进程名即可
- 5.定位一个进程后，可通过F9,再按下回车杀死即可
- 6.F5，显示进程的父、子进程关系

7.支持以下快捷键：

- ​		M:以内存使用大小进行排序
- ​		P:以cpu使用量大小进行排序
- ​		T:以进程运行时间进行排序
- ​		/:进入搜索，查找指定进程

### （9）glances资源管理工具

**1、glancesd的作用.**

1..可以检测cpu使用率、内存使用情况、内核统计信息、硬盘的IO速度、文件系统的剩余空间、网络的IO速度、缓存空间（swap）的使用情况、进程动态信息、系统负载信息；

2.将采集到的数据输出到文件中，便于做服务器性能报表分析以及绘制图表等

**2、安装方式**

- 1.命令：yum install glances -y
- 2.通过python软件包安装，pip3 install glance

**3、glance的使用**

- h：显示glances帮助信息
- q：退出glances
- c：以cpu排序
- m:以内存排序
- i:以io速率排序
- p：以进程名排序
- d：打开，关闭键盘读写情况
- f：打开，关闭文件系统剩余空间使用情况

**4.glances的web服务功能**

1. ​	#安装依赖工具包     	yum install python python-pip python-devel gcc -y
2. ​	#通过python软件包工具安装一个模块，启动web服务        pip  install  bottle
3. ​	#使用glances运行web服务			glances  -w（若访问不了可能是防火墙打开，输入iptables -F后#再次启动）
4. ​    #支持cs模式，glances可运行一个服务端，在用客户端去远程连接访问服务端，查看系统状况





## 7、linux网络命令

### （1）网络通信介绍

1、什么是计算机通信？

- 指的是计算机和数据终端的一种通信方式，可实现计算机之间，人和计算机之间的数据生成，存储、数据处理，最后传递到不同的机器之间。
- 要求处理不同地理位置的计算机，能够相互知道对方的存在，才能相互传递信息、共享资源

2、什么计算机网络协议？

- 是计算机网络中进行数据交而建立的一套标准。TCP\IP就是lnternet的标注语言（类似通用的普通话）

### （2）osi七层网络模型



| osi七层模型 | tcp/ip层概念   | 层功能                                       | 协议                        |
| ----------- | -------------- | -------------------------------------------- | --------------------------- |
| 物理层      |                |                                              |                             |
| 数据链路层  | 物理链路层     | 以二进制的数据在物理媒介上进行传输           | ISO@2100                    |
| 网络层      | 网络           | 为数据包选择路由（就是数据的路径选择）       | IP、ICMP、BGP、OSFP等       |
| 传输层      | 传输           | 提供端对端的接口（ip port）                  | TCP、UDP                    |
| 会话层      |                | 解除或建立与别的节点连接                     | 无                          |
| 表示层      |                | 数据格式化、代码转换、数据加密               | 无                          |
| 应用层      | 最接近用户的层 | 提供文件传输、邮件、文件按共享、数据加密等等 | HTTP、SNMP、FTP、NFS、DNS、 |

**1、应用层介绍**

任务：通过进程进程间的数据交互来完成特定的网络应用

- 域名解析系统：dns协议（即将ip地址绑定一个方便记忆的名字，可通过这个名字访问这个网站）
- 邮箱协议： SMTP协议
- web服务：HTTP协议

**2、传输层介绍**

作用：向两台主机之间的进程进行数据提供传输

**涉及到以下两种协议**

- 1、传输控制协议（TCP）:提供面向连接的，可靠的数据传输服务（传输前反复确定对方身份）
- 2、用户数据协议（UDP）:t提供无连接传输，但不保证数据安全性（不管对方是否回应，直接都将数据发送给你）

**TCP和UDP的区别**

- 1.UDP是无连接的，TCP是面向连接的（类似打电话）；
- 2.UDP只尽力传输，不保障数据可靠性。TCP安全性高，有两个传输的端点，是点对点那，一对一的形式
- 3.UDP是没有报文，TCP是由可靠的报文交互，传输的数据无差错、不重复、不丢失
- 4.UDP支持一对一、一对多、多对一、多对多的交互通信（应用在聊天室方面）

（3）ifconfig命令

作用：1、用于配置网卡ip信息、等网络参数信息或查看显示网络接口信息、类似于windos的ipconfig命令；2、还能够临时性的配置ip地址、子网掩码、广播地址、网关信息等

安装命令

yum install net-tools

使用案例

#1.查看网络地址信息

```shell
[root@localhost ~]# ifconfig			#查看所有网络接口信息
[root@localhost ~]# ifconfig ensee		#查看指定的网卡信息
```

#2.指定开启、关闭网卡（真实服务器切记不可使用）

```shell
[root@localhost ~]# ifconfig ens33 down			#关闭网卡
[root@localhost ~]# ifconfig ens33 up			#重启网卡
```

3.修改、设置ip地址

```shell
#方式1
[root@localhost ~]# ifconfig ens33:0 192.168.0.110 netmask 255.255.255.0 up			#添加一个新的ip
#方式2
[root@localhost ~]# ifconfig ens33:1 192.168.0.111/24 up						   #添加一个新的ip
#查看添加的ip
[root@localhost ~]# ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.105  netmask 255.255.255.0  broadcast 192.168.0.255
        inet6 fe80::20c:29ff:fe60:2901  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:60:29:01  txqueuelen 1000  (Ethernet)
        RX packets 2331  bytes 334671 (326.8 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1297  bytes 142644 (139.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens33:0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.110  netmask 255.255.255.0  broadcast 192.168.0.255
        ether 00:0c:29:60:29:01  txqueuelen 1000  (Ethernet)

ens33:1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.111  netmask 255.255.255.0  broadcast

```

4.修改机器的mac地址

```shell
[root@localhost ~]# ifconfig ens33 hw ether 00:6e:87:6b:00
[root@localhost ~]# ifconfig|grep ether
        ether 00:6e:87:6b:00:00  txqueuelen 1000  (Ethernet)
        ether 00:6e:87:6b:00:00  txqueuelen 1000  (Ethernet)
        ether 00:6e:87:6b:00:00  txqueuelen 1000  (Ethernet)
```

5.永久修改网络设备信息

修改/etc/sysconfig/network-scripts/ifcfg-ens33即可

tip：ONBOOT要设为yes，开机加载读取配置文件

ONBOOT=yes
IPADDR=192.168.0.105
NETMASK=255.255.255.0
GATEWAY=192.168.0.1
DNS1=192.168.0.1 



### （3）ifcofng命令

作用：

1.用于配置网卡ip信息、等网络参数信息

2.能够查看显示网络接口信息、子网掩码、广播地址、网关信息等

3.临时性的配置ip地址、子网掩码、广播地址、网关信息等

安装：

yum	install net-tools

使用案例

1.查看所有网络接口信息信息

```shell
[root@localhost ~]# ifconfig
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.105  netmask 255.255.255.0  broadcast 192.168.0.255
        inet6 fe80::20c:29ff:fe60:2901  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:60:29:01  txqueuelen 1000  (Ethernet)
        RX packets 554  bytes 51624 (50.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 324  bytes 29181 (28.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 64  bytes 5568 (5.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 64  bytes 5568 (5.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

2.查看指定设备的信息

```shell
[root@localhost ~]# ifconfig ens33
ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.105  netmask 255.255.255.0  broadcast 192.168.0.255
        inet6 fe80::20c:29ff:fe60:2901  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:60:29:01  txqueuelen 1000  (Ethernet)
        RX packets 630  bytes 57864 (56.5 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 364  bytes 33333 (32.5 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

3.指定开启或关闭网卡（关闭网卡不得在服务器上操作）

```shell
[root@localhost ~]# ifconfig ens33 down			#关闭网卡信息
[root@localhost ~]# ifconfig ens33 up			#开启网卡信息
```

4.添加ip地址

```shell
[root@localhost ~]# ifconfig ens33:0 192.168.0.106 netmask 255.255.255.0 up			#第一种添加方法
[root@localhost ~]# ifconfig ens33:1 192.168.0.107/24 up						   #第二种添加方法
```

5.修改mac地址

```shell
原本的mac地址如下
[root@localhost ~]# ifconfig|grep ether
        ether 00:0c:29:60:29:01  txqueuelen 1000  (Ethernet)
        ether 00:0c:29:60:29:01  txqueuelen 1000  (Ethernet)
        ether 00:0c:29:60:29:01  txqueuelen 1000  (Ethernet)
[root@localhost ~]# ifconfig ens33 hw ether 00:0c:28:13:11:CF			#修改mac并查看mac信息
[root@localhost ~]# ifconfig|grep ether
        ether 00:0c:28:13:11:cf  txqueuelen 1000  (Ethernet)
        ether 00:0c:28:13:11:cf  txqueuelen 1000  (Ethernet)
        ether 00:0c:28:13:11:cf  txqueuelen 1000  (Ethernet)
```

7.以上修改只是临时修改，永久修改需要修改配置文件信息

路径：[root@localhost ~]# cat /etc/sysconfig//network-scripts/ifcfg-ens33 



### （4）route命令

路由命令route

作用：对linux内的IP路由表进行一个操作

路由的概念：

主机A-主机可直接发发送网络数据，也可以经过多个路线（多个节点，即为路由器）发送到目标电脑

静态路由、动态路由

静态路由：linux机器上配置的都是静态路由，由运维人员通过route命令去管理

动态路由：无需人为干预、由交换机、路由器自动分配规则规划

route名令使用案例

1.查看路由表信息

```shell
[root@localhost ~]# route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         gateway         0.0.0.0         UG    0      0        0 ens33
link-local      0.0.0.0         255.255.0.0     U     1002   0        0 ens33
```

2.不以dns解析的主机名查看路由信息表

```shell
[root@localhost ~]# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.0.1     0.0.0.0         UG    0      0        0 ens33
169.254.0.0     0.0.0.0         255.255.0.0     U     1002   0        0 ens33
192.168.0.0     0.0.0.0         255.255.255.0   U     0      0        0 ens33
```

参数解析：

Destination ：网络号，network的意思

Gateway：网关，即是网络从这个ip的关口出去。若显示0.0.0.0的ip则表示该路由是由本机转发出去的

Genmask：子网掩码，ip地址配合子网掩码组成一个完整的网络信息

Flags：路由标记，标记当前的网络状态

​	U：up运行的状态

​	G：表示这是一个网关路由器

​	H:表示这个网关是一个主机

​	！：表示当前这个路由已禁止



网卡的概念

数据包不经过任何的设定路由表最后经过的地址关口



3.添加或删除默认路由表

```shell
[root@localhost ~]# route del default				#删除默认的网关信息
[root@localhost ~]# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
169.254.0.0     0.0.0.0         255.255.0.0     U     1002   0        0 ens33
192.168.0.0     0.0.0.0         255.255.255.0   U     0      0        0 ens33

[root@localhost ~]# route add default gw 192.168.0.1		#添加默认的网关信息
[root@localhost ~]# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.0.1     0.0.0.0         UG    0      0        0 ens33
169.254.0.0     0.0.0.0         255.255.0.0     U     1002   0        0 ens33
192.168.0.0     0.0.0.0         255.255.255.0   U     0      0        0 ens33

```

### （5）ip命令

作用：

1. 包含ifconfig、route命令的作用；
2. 查看系统路由、网络设备、设置策略等功能。

ip命令操作的对象

- OBJECT 对象
- ilink	网络设备·
- adress	定义ipv4 ipv6的地址
- neghour：查看ARP缓存地址（APR用于解析MAC地址）
- route：路由表二对象
- maddress：多播地址

tunel：ip上的通道

ip针对对象要操作的动作，一般为增删改查

ip命令使用案例

1.查看、显示 网络设备信息

```shell
[root@localhost ~]# ip addr show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:60:29:01 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.105/24 brd 192.168.0.255 scope global ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fe60:2901/64 scope link 
       valid_lft forever preferred_lft forever
```

2指定网络设备信息显示

```shell
[root@localhost ~]# ip link show dev ens33
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 00:0c:29:60:29:01 brd ff:ff:ff:ff:ff:ffs
[root@localhost ~]# ip -s link show  dev ens33					#更显示的显示设备信息
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 00:0c:29:60:29:01 brd ff:ff:ff:ff:ff:ff
    RX: bytes  packets  errors  dropped overrun mcast   
    169073     2134     0       0       0       0       
    TX: bytes  packets  errors  dropped carrier collsns 
    91657      982      0       0       0       0     
```

3.关闭或激活网络设备

```shell
[root@localhost ~]# ip link set ens33 down			#关闭网络设备
[root@localhost ~]# ip link set ens33 up			#激活网络设备
```

4.修改网卡的mac地址

```shell
[root@localhost ~]# ip link set ens33 address 00:11:5f:21:61:40
```

5.显示网卡信息

```shell
[root@localhost ~]# ip a			#第一种写法，简写
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:11:5f:21:61:40 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.105/24 brd 192.168.0.255 scope global ens33
       valid_lft forever preferred_lft forever
    inet6 fe80::20c:29ff:fe60:2901/64 scope link 
       valid_lft forever preferred_lft forever
[root@localhost ~]# ip addr show	#第二中写法
```

7.添加、删除ip信息

```shell
[root@localhost ~]# ip address add 192.168.0.200/24  dev ens33		#添加一个ip地址
[root@localhost ~]# ip address del 192.168.0.200/24  dev ens33		#删除一个ip地址
```

8.ip命令给网卡添加别名

```shell
[root@localhost ~]# ip address add 192.168.0.110/24 dev ens33 label ens33:1
[root@localhost ~]# ifconfig
ens33:1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.0.110  netmask 255.255.255.0  broadcast 0.0.0.0
        ether 00:11:5f:21:61:40  txqueuelen 1000  (Ethernet)

```

9.通过ip命令检查路由信息

```shell
[root@localhost ~]# ip route
192.168.0.0/24 dev ens33 proto kernel scope link src 192.168.0.105 
```

10.检查arp缓存信息（显示网络邻居信息），检查mac信息

```shell
[root@localhost ~]# ip neighbour
192.168.0.102 dev ens33 lladdr b4:2e:99:82:f2:ac DELAY
192.168.0.1 dev ens33 lladdr 74:05:a5:88:7c:c6 STALE
```



### （6）netstat网络端口查看

作用：

显示网络连接状况、路由表信息、端口状态、网络连接情况等信息

使用案例

1.查看所有的网络连接情况

```shell
[root@localhost ~]# netstat -an
-a	#显示所有套接字信息
-n	#显示数字地址信息而非主机名
```

字段解释：

- Proto ：套接字使用的协议
- Recv-Q ：连接这个套接字用户，还未拷贝的字节数
- Send-Q ：远程还未确认的字节数
- Local Address：套接字（一个连接情况）本地的地址和端口号
- Foreign Address ：套接字的远程主机地址和端口号
- State 套接字的运行情况，listen 监听中

重要的套接字的连接情况

- ESTABLISHED:套接字有一个有效连接
- SYN_RECV：套接字尝试建立一个连接
- SYN_RECV：从网络上收到一个连接请求
- FIN_WAIT1：套接字已关闭，连接正在断开
- FIN_WAIT2：套接字已关闭，等待远程方中止
- TIME_WAIT:在关闭之后，套接字等待处理仍然在网络中的分组
- CLASED:套接字未使用
- CLOSED_WAIT:远程放已关闭、等待套接字关闭
- LAST_ACK:远程方中止，套接字已关闭。等待确认
- LISTEN:t套接字监听进来的连接。若不设置 --listening(-l)或者 --all（-a）选项，将不显示出来这些连接
- CLOSING:套接字都已关闭、而还未把所有数据发出
- UNKNOWN:套接字状态未知

netstat使用案例

1.常用的参数组合

```shell
[root@localhost ~]# netstat -tunpl				#查看机器上正在运行的端口情况
#t	表示显示tcp的连接情况
#u	表示显示udp的连接情况
#n	不进行dns解析
#p	只显示正在监听的套接字情况
```

127.0.0.1：本地回环地址，用于机器内部应用通信，每个硬件都有自己的回环地址

0.0.0.0：绑定机器所有的网卡地址

2.检查3306端口是否被占用

```shell
[root@localhost ~]# netstat -an|grep 3306
```

3.显示系统的路由表情况

```shell
[root@localhost ~]# netstat -rn				#等同于route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
192.168.0.0     0.0.0.0         255.255.255.0   U         0 0          0 ens33
```

4.显示网络的接口情况

```shell
[root@localhost ~]# netstat -i		#-i：显示所有网络接口的情况
Kernel Interface table
Iface             MTU    RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
ens33            1500     4665      0      0 0          2317      0      0      0 BMRU
ens33:1          1500      - no statistics available -                        BMRU
lo              65536       64      0      0 0            64      0      0      0 LRU
```

字段解释

- Iface ：网络设备名字
- MTU：最大的传输单元，单位是字节
- RX-OK/TX-OK：正确接收、发送了多少数据包
- RX-ERR/TX-ERR：接收、发送数据包时，丢弃了多少数据包
- RX-DRP/TX-DRP：
- RX-OVR/TX-OVR：错误遗失了多少数据包

Flg：标记

- ​	L：表示回环地址
- ​	R：这个网络接口运行中
- ​	U：接口处于活动状态
- ​	B：设置了广播地址
- ​	M：接收所有的数据包
- ​	O:在该接口上禁止arp
- ​    P:端对端的连接

查看RX-ERR/TX-ERR最好是0，否则则有哦丢包情况

5.查看服务器监听情况

```shell
[root@localhost ~]# netstat -tunpl|grep 3306			#监听数据库端口使用情况
[root@localhost ~]# netstat -tunpl|grep 80			#监听web服务端口使用情况
[root@localhost ~]# netstat -tunpl|grep 443			#监听https服务端口使用情况
```

6.centos7之后出了一个ss（网络查看工具）

安装命令：yum install iproute-y

```shell
[root@localhost ~]# ss -an		#显示所有的套接字连接情况
```

9.显示出所有正在监听中的套接字情况

```shell
[root@localhost ~]# ss -tunpl
Netid State      Recv-Q Send-Q      Local Address:Port                     Peer Address:Port              
udp   UNCONN     0      0               127.0.0.1:323                                 *:*                   users:(("chronyd",pid=684,fd=1))
udp   UNCONN     0      0                     ::1:323                                :::*   
```

### （7）ping命令

作用：

1..测试当前主机和远方的主机能否通信；

2.ping的对方可以是域名还是ip地址

使用案例

1.检测主机和服务器的ip通信情况

```shell
[root@localhost ~]# ping 127.0.0.1
PING 127.0.0.1 (127.0.0.1) 56(84) bytes of data.					#表示主机像127.0.0.1发送56个字节数据
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.032 ms			#表示对方返回64字节的数据，icmp_seq为序号11，数据包存活时间ttl=64s 延迟时间为time=0.032 ms
64 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.037 ms

```

2.用于检测主机网络状态

```shell
[root@localhost ~]# ping jd,com
ping: jd,com: 未知的名称或服务
#tip：若出现以上情况从两方面判断，一是该主机是否能上网；二是检查该机器能够进行域名解析
```

检查能否域名解析的办法

验证：

1.检查dns客户端配置文件/etc/resolv.conf,确保有以下信息

nameservcer		114.114.114.114

nameservcer		223.5.5.5

2.再次验证能否dns域名解析，再次ping（一般网络配置没问题都能ping通）

3.若还是不通，则需要检查自己主机能否上网，检查配置文件或检差网络是否选择桥接模式

### （8）telnet命令

作用：用于检测远程主机是否打开某端口

安装命令：yum install telnet -y

1.检测远程机器的22端口是否打开

```shell
[root@localhost ~]# telnet 192.168.0.105 22				#以下表示检测连接成功
Trying 192.168.0.105...
Connected to 192.168.0.105.
Escape character is '^]'.
SSH-2.0-OpenSSH_7.4

[root@localhost ~]# telnet 192.168.0.105 25				#以下表示连接不成功，25端口未打开或拒绝连接
Trying 192.168.0.105...
telnet: connect to address 192.168.0.105: Connection refused

```



### （9）ssh命令

ssh：安全的远程连接命令

作用：远程连接服务器

语法：ssh 用户名@服务器ip

参数

-p	port	默认ssh端口为22 	若改成22408

1.指定端口远程连接

```shell
root@localhost ~]# ssh root@192.168.1.100 -p 24408
```



2.远程登录并执行命令

```shell
[root@localhost ~]# ssh root@192.168.0.105 "free -m"
total        used        free      shared  buff/cache   available
Mem:           3773         119        3492          11         161        3434
Swap:          2047           0        2047
```



### （10）wget命令

作用：

1.用于指定的下载url文件；如图片的url

2.无论网速好坏，都能很强的适应网络环境进行下载

3.支持断点续传（网络中断后网络恢复还能够继续下载）



使用案例

1.下载一张照片

```shell
[root@localhost ~]# wget http://pic1.win4000.com/pic/0/76/08dee6d99a_250_300.jpg
```

2.下载文件且指定保存文件名

参数 -O	#下载url后指定目录且改名

```shell
[root@localhost /]# wget -O /tmp/王一博.jpg http://pic1.win4000.com/pic/9/77/73b3c36567_250_300.jpg
[root@localhost /]#[root@localhost /]# ll -h /tmp
-rw-r--r--  1 root root  613 9月  14 19:07 王一博.jpg
```



3.限制wget的下载速度，限制为2kb/s

参数 --limit-rate

```shell
[root@localhost /]# wget  --limit-rate=2k http://pic1.win4000.com/pic/7/e1/18b81385169.jpg	#以2kb/s的速度下载图片
```

4.断点下载

参数 -c

```shell
[root@localhost /]# wget  --limit-rate=2k http://pic1.win4000.com/pic/7/e1/18b81385169.jpg	#以2kb/s断点下载
--2020-09-14 19:19:03--  http://pic1.win4000.com/pic/8/be/dfc81411580.jpg
正在解析主机 pic1.win4000.com (pic1.win4000.com)... 112.90.242.20
正在连接 pic1.win4000.com (pic1.win4000.com)|112.90.242.20|:80... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：269533 (263K) [image/jpeg]
正在保存至: “dfc81411580.jpg”

 4% [====>                                                                                                                            ] 11,680      2.00KB/s 剩余 2m 7s 
 
C					#ctrl+c中断

[root@localhost /]# wget -c --limit-rate=2k http://pic1.win4000.com/pic/8/be/dfc81411580.jpg	#再次执行命令继续下载
--2020-09-14 19:20:56--  http://pic1.win4000.com/pic/8/be/dfc81411580.jpg
正在解析主机 pic1.win4000.com (pic1.win4000.com)... 112.90.242.20
正在连接 pic1.win4000.com (pic1.win4000.com)|112.90.242.20|:80... 已连接。
已发出 HTTP 请求，正在等待回应... 206 Partial Content
长度：269533 (263K)，剩余 255805 (250K) [image/jpeg]
正在保存至: “dfc81411580.jpg”

10% [++++++======>                                                                                                                    ] 27,739      2.00KB/s 剩余 1m 59s 
```

5.后台运行下载

参数 -b

```shell
[root@localhost tmp]# wget -b --limit-rate=3k http://pic1.win4000.com/pic/7/e1/18b81385170.jpg	#在后台以3kb/s的速度下载图片
继续在后台运行，pid 为 1698。
将把输出写入至 “wget-log”。
[root@localhost tmp]# 
[root@localhost tmp]# ls	
18b81385170.jpg    systemd-private-4bed81c593ed40e0ab3c6a9ffb8af234-chronyd.service-aKV8XQ  vmware-root
glances.log        systemd-private-b7608543bc9946adbf70695f82de14c0-chronyd.service-EY4tfs  wget-log
pip-fWw507-unpack  systemd-private-f6866cbb40794c6cb03d9a2fab227c09-chronyd.service-vqikM1  王一博.jpg
[root@localhost tmp]# tail -f wget-log 	查看后台下载生成日志信息
--2020-09-14 19:27:22--  http://pic1.win4000.com/pic/7/e1/18b81385170.jpg
正在解析主机 pic1.win4000.com (pic1.win4000.com)... 112.90.242.20
正在连接 pic1.win4000.com (pic1.win4000.com)|112.90.242.20|:80... 已连接。
已发出 HTTP 请求，正在等待回应... 200 OK
长度：60518 (59K) [image/jpeg]
正在保存至: “18b81385170.jpg”

     0K .......... .......... .......... .......... .......... 84% 2.97K 3s
    50K .........                                             100% 3.21K=20s

2020-09-14 19:27:42 (3.00 KB/s) - 已保存 “18b81385170.jpg” [60518/60518])
```

6.以不同的身份去下载网页信息，身份有移动端、PC端

参数 --user-agent

```shell
#以移动端身份下载网页
[root@localhost tmp]# wget --user-agent="Mozilla/5.0 (Linux; Android 6.0.1; Moto G (4)) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Mobile Safari/537.36" http://m.win4000.com/meinvtag2584.html

#以pc端身份下载网页
[root@localhost tmp]# wget --user-agent="Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36" http://m.win4000.com/meinvtag2584.html
```

--**user**-agent的值通过谷歌浏览器F12进入开发者界面、找到network，任意选择一个请求，找到请求头信息的user_agent的值即可



7.检测网站是否存活

参数	-q：安静输出	-T：指定访问网站的超时时间	-t：重试访问网站的次数	--spide：不下载任何资源

echo	$?	#检测上一次命令执行是否成功，返回0则成功

```shell
[root@localhost tmp]# wget -q -T 3 -t 1 --spide www.baidu.com		检测网站是否存活
[root@localhost tmp]# echo $?
0										#返回0则说明上一次命令执行成功，网站存活
```

