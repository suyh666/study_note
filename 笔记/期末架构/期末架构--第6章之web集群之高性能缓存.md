# 期末架构

## 第六章 Web集群之高性能缓存



### 6.1、Web集群问题与缓存引入

#### Memcached

#### 谁在用Memcached

国外：Yahoo, facebook, twitter, wiki等

国内：新浪网，豆瓣网，开心网，搜狐，赶集网等

#### 是什么

<img src="notes/image-20200807100537253.png" alt="image-20200807100537253" style="zoom:50%;" />

### 6.2、Memcached使用场景

#### 有什么用

![image-20200807100712713](notes/image-20200807100712713.png)



### 6.3、缓存软件介绍

#### 介绍Memcached

Memcached是一套利用系统内存进行数据缓存的软件，常用于在动态Web集群，系统后端，数据库前端处使用。

用处：

- 可以临时缓存Web系统查询过的数据库数据，当用户请求查询数据时，由Memcached优先提供服务，从而减少Web系统直接请求数据库的次数，大大的降低后端数据库的压力，从而提升网站系统的性能。
- Memcached是通过分配指定的内存空间存取数据，比起MySQL要快很多（MySQL读写磁盘）
- Memcached也用来共享存储集群架构所有Web阶段服务器的session会话数据。

Memcached是一个开源的，支持高性能，高并发的分布式内存缓存系统，由C语言开发。从软件名称上看，mem就是内存的缩写，cache就是缓存的意思，最后一个字符d就是daemon的意思，表示该软件是`守护进程模式内存缓存`。

Memcached分为`服务端`和`客户端`两部分。

Memcached是以LiveJournal旗下Danga Interactive公司的Brad Fitzpatric为首开发的一款软件。现在已成为mixi、hatena、Facebook、Vox、LiveJournal等众多服务中提高Web应用扩展性的重要因素。

Memcached是一种基于内存的key-value存储，用来存储小块的任意数据（字符串、对象）。这些数据可以是数据库调用、API调用或者是页面渲染的结果。

Memcached简洁而强大。它的简洁设计便于快速开发，减轻开发难度，解决了大数据量缓存的很多问题。它的API兼容大部分流行的开发语言。

本质上，它是一个简洁的key-value存储系统。

一般的使用目的是，通过缓存数据库查询结果，减少数据库访问次数，以提高动态Web应用的速度、提高可扩展性。

![image-20200807102215679](notes/image-20200807102215679.png)

Memcached 官网：https://memcached.org/。

#### Memcached作用

传统场景下，多数Web应用都将数据保存到关系型数据库中（MySQL），Web服务器从该数据库中读取数据给与用户响应，显示在浏览器中。

但是随着数据库的增大，集中式的访问，关系型数据库的负担会加重，导致响应缓慢，造成网站打开延迟等问题，影响用户体验。

此时在MySQL前面试用Memcached的作用就出现了，通过Memcached在内存中缓存MySQL的查询结果，减少MySQL的访问，提升整体网站架构性能。

生产环境下Memcached也被用来保存经常需要读取的数据，好比我们的浏览器也会对网页数据缓存一样，通过内存缓存的读取速度要远快于磁盘。

#### 内存缓存软件

| 软件              | 类型        | 作用                                       | 数据场景                                                     |
| ----------------- | ----------- | ------------------------------------------ | ------------------------------------------------------------ |
| Memcached         | 纯内存      | 缓存网站后端各类数据                       | 缓存用户重复请求的动态数据，如博客，帖子，用户session信息    |
| Redis、MemcacheDB | 持久化+内存 | 缓存后端数据库数据，作为关系型数据库的补充 | 除了用户重复请求的动态数据，以及当做数据库使用，存储如点赞数，排行榜，计数器，粉丝统计等需要持久化的数据。 |
| Squid，Nginx      | 内存+磁盘   | 缓存Web前端数据                            | 用于缓存静态数据，如图片，文件，JS，CSS，HTML，代码等。提供CDN功能 |



### 6.4、Memcached工作场景介绍

#### 网站更新缓存工作流

- 当程序需要更新或删除数据时，首先会处理后端数据库中的数据。
- 处理后端数据库的同事，也会通知Memcached，它缓存的数据过期了，因此要保证Mecmached的数据和数据库中保持一致，这个一致性非常重要。

#### 场景1：电商数据

![image-20200807105035039](notes/image-20200807105035039.png)

```
上述流程图，例如Memcached用来存储商品分类功能的数据，一般商品分类是很少变动的，因此可以提前把该数据放在Memcached，Web后端直接读取Memcached即可，无须访问mysql，减轻数据库压力，还能提升访问速度。

若是商品分类有了变化，公司内部管理人员需要更新数据库后，再同时更新到缓存Memcached里。
```

#### 场景2：热点数据缓存

热点数据缓存指的是如淘宝的卖家，在卖家新增商品后，网站程序会吧这个商品写入后端数据库，同时吧这部分的数据，放入Memcached内存，下一次访问该商品的请求，就直接从Memcached里读取，这个方式用来缓存网站的热点数据，也就是会被用户经常访问的数据。

![image-20200807140041498](notes/image-20200807140041498.png)

这种形式一般通过程序实现，也可以在MySQL上做设置，直接由MySQL吧数据更新到Memcached中，实现例如从库的效果。

![image-20200807142747245](notes/image-20200807142747245.png)

- 如果碰到电商双11，秒杀高并发的业务场景，必须要事先预热各种缓存，包括前端的Web缓存和后端的数据库缓存。
- 也就是先把数据放入内存预热，然后逐步动态更新。此时，会先读取缓存，如果缓存里没有对应的数据，再去读取数据库，然后把读到的数据放入缓存。如果数据库里的数据更新，需要同时触发缓存更新，防止给用户过期的数据，当然对于百万级别并发还有很多其他的工作要做。
- 绝大多数的网站动态数据都是保存在数据库当中的，每次频繁地存取数据库，会导致数据库性能急剧下降，无法同时服务更多的用过户（比如MySQL特别频繁的锁表就存在此问题），那么，就可以让Memcached来分担数据库的压力。增加Memcached服务的好处除了可以分担数据库的压力以外，还包括无须改动整个网站架构，只须简单地修改下程序逻辑，让程序先读取Memcached缓存查询数据即可，当然别忘了，更新数据时也要更新Memcached缓存。



### 6.5、cookies和session

#### 作为集群节点的session会话共享存储

也就是吧客户端用户请求多个前端应用服务器集群产生的session信息，统一存储到一个Memcached缓存中。

#### Memcached在Web集群架构图

![image-20200807141849701](notes/image-20200807141849701.png)

图2

![image-20200807142908418](notes/image-20200807142908418.png)

#### session/cookie是什么

![image-20200807154511725](notes/image-20200807154511725.png)

简单来说Cookies就是服务器暂存放在你的电脑里的资料让服务器辨认电脑，很多购物网站的购物车功能、论坛自动登录功能都是靠Cookie实现。

**cookie是什么意思?**

　　Cookie的英文直译是饼干，在计算机术语中是指一种能够让网站服务器把少量数据储存到客户端的硬盘或内存，或是从客户端的硬盘读取数据的一种技术。Cookie可以为用户带来很多便捷，但同时，它也会给用户制造一些麻烦。

　　**浏览器中的Cookie**

　　当你浏览某网站时，由Web服务器置于你硬盘上的一个非常小的文本文件，它可以记录你的用户ID、密码、浏览过的网页、停留的时间等信息。当你再次来到该网站时，网站通过读取Cookie，得知你的相关信息，就可以做出相应的动作，如在页面显示欢迎你的标语，或者让你不用输入ID、密码就直接登录等等。如果你清理了Cookie，那么你曾登录过的网站就没有你的修改过的相关信息。Cookie是非常常见的， 基本上你的浏览器中都会存储了成百上千条Cookie信息。

#### Cookies和session

![image-20200807154100497](notes/image-20200807154100497.png)



### 6.6、Memcached特性



#### Memcached特点

作为高并发，高性能的缓存服务，具有如下特点：

- 协议简单。Memcached的协议实现很简单，采用的是基于文本行的协议，能通过telnet/nc等命令直接操作memcached服务存储数据。
- 支持epoll/kqueue异步I/O模型，使用libevent作为事件处理通知机制。
- 简单的说，libevent是一套利用c开发的程序库，它将BSD系统的kqueue，Linux系统的epoll等事件处理功能封装成一个接口，确保即使服务器端的连接数增加也能发挥很好的性能。Memcached就是利用这个libevent库进行异步事件处理的。
- 采用key/value键值数据类型。被缓存的数据以key/value键值形式存在，例如：

```shell
benet–>36,key=benet,value=36
yunjisuan–>28,key=yunjisuan,value=28
#通过benet key可以获取到36值，同理通过yunjisuan key可以获取28值
```

- 全内存缓存，效率高。Memcached管理内存的方式非常高效，即全部的数据都存放于Memcached服务事先分配好的内存中，无持久化存储的设计，和系统的物理内存一样，当重启系统或Memcached服务时，Memcached内存中的数据就会丢失。
- 如果希望重启后，数据依然能保留，那么就可以采用redis这样的持久性内存缓存系统。
- 当内存中缓存的数据容量达到服务启动时设定的内存值时，就会自动使用LRU算法（最近最少被使用的）删除过期的缓存数据。也可以在存放数据时对存储的数据设置过期时间，这样过期后数据就自动被清除，Memcached服务本身不会监控数据过期，而是在访问的时候查看key的时间戳判断是否过期。
- 可支持分布式集群 Memcached没有像MySQL那样的主从复制方式，分布式Memcached集群的不同服务器之间是互不通信的，每一个节点都独立存取数据，并且数据内容也不一样。通过对Web应用端的程序设计或者通过支持hash算法的负载均衡软件，可以让Memcached支持大规模海量分布式缓存集群应用。

下面是利用Web端程序实现Memcached分布式的简单代码： #建立了一个数组

```shell
"memcached_servers" ==>array(
'10.4.4.4:11211',
'10.4.4.5:11211',
'10.4.4.6:11211',
)
```

下面使用Tengine反向代理负载均衡的一致性哈希算法实现分布式Memcached的配置。

```shell
http {
  upstream test {
      consistent_hash $request_uri;
      server 127.0.0.1:11211 id=1001 weight=3;
      server 127.0.0.1:11212 id=1002 weight=10;
      server 127.0.0.1:11213 id=1003 weight=20;
  }
}

提示：
Tengine是淘宝网开源的Nginx的分支，上述代码来自：
http://tengine.taobao.org/document_cn/http_upstream_consistent_hash_cn.html
```

#### Memcached预热重启理念

当网站架构需要大面积重启Memcached时，首先要在前端控制网站入口的访问流量，然后重启Memcached集群，并且进行数据预热，当所有数据预热完毕后，再逐步开放前端网站入口的流量。



### 6.7、Memcacahed多实例启动



#### Memcached安装

准备好一台Linux虚拟机，超哥这里用的是如下环境

基本的linux初始化就不用多说了。。反复的说了一万遍了

```shell
[root@web01 /]# cat /etc/redhat-release
CentOS Linux release 7.5.1804 (Core) 

[root@web01 /]# uname -r
3.10.0-862.el7.x86_64
```

系统初始化

```shell
 wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
 wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
yum makecache


yum install -y bash-completion vim lrzsz wget expect net-tools nc nmap tree dos2unix htop iftop iotop unzip telnet sl psmisc nethogs glances bc ntpdate  openldap-devel git python-pip  gcc automake autoconf python-devel vim sshpass lrzsz readline-devel lsof
```

安装Memcached前需要先安装libevent，有关libevent的内容在前文已经介绍，此处用yum命令安装libevent。操作命令如下：

```shell
yum install -y libevent libevent-devel nc 

# 在centos 7 系统上 nc命令改为了nmap-ncat

[root@web01 /]#  rpm -qa libevent libevent-devel nmap-ncat
nmap-ncat-6.40-19.el7.x86_64
libevent-2.0.21-4.el7.x86_64
libevent-devel-2.0.21-4.el7.x86_64

```

安装memcached

```shell
[root@memcached01 ~]# yum install memcached -y

[root@memcached01 ~]# rpm -qa memcached
memcached-1.4.15-10.el7_3.1.x86_64
```

#### memcached启动

启动Memcached

```shell
[root@web01 /]# which memcached
/usr/bin/memcached

# 启动第一个memcached实例
[root@web01 /]# memcached -m 16m -p 11211 -d -u root -c 8192
```

检查服务启动

```shell
[root@web01 /]# lsof -i :11211
COMMAND    PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
memcached 4065 root   26u  IPv4  78852      0t0  TCP *:memcache (LISTEN)
memcached 4065 root   27u  IPv6  78853      0t0  TCP *:memcache (LISTEN)
memcached 4065 root   28u  IPv4  78856      0t0  UDP *:memcache 
memcached 4065 root   29u  IPv6  78857      0t0  UDP *:memcache 


[root@web01 /]# netstat -antup | grep 11211
tcp        0      0 0.0.0.0:11211           0.0.0.0:*               LISTEN      4065/memcached      
tcp6       0      0 :::11211                :::*                    LISTEN      4065/memcached      
udp        0      0 0.0.0.0:11211           0.0.0.0:*                           4065/memcached      
udp6       0      0 :::11211                :::*                                4065/memcached 

[root@web01 /]# ps -ef|grep memcached|grep -v 'grep'
root       4065      1  0 16:37 ?        00:00:00 memcached -m 16m -p 11211 -d -u root -c 8192
```

可以再启动第二个数据库实例

```shell
[root@web01 /]# memcached -m 16m -p 11212 -d -u root -c 8192
```

```shell
多个实例的概念：

多个实例就是相当于运行多个数据库服务、分别独立的、给与不同的项目去使用
```

同样的检查服务状态

```shell
[root@web01 /]# ps -ef | grep memcached | grep -v grep
root       4065      1  0 16:37 ?        00:00:00 memcached -m 16m -p 11211 -d -u root -c 8192
root       4086      1  0 16:40 ?        00:00:00 memcached -m 16m -p 11212 -d -u root -c 8192

```

若是想要开机就自动运行memcached，可以加入开机启动文件

```shell
[root@memcached01 ~]# tail -2 /etc/rc.local
memcached -m 16m -p 11211 -d -u root -c 8192
memcached -m 16m -p 11212 -d -u root -c 8192
[root@web01 run]# systemctl enable memcached.service 
```

### 6.8、Memcached数据库操作与停止

#### Memcached启动命令相关参数说明

**进程与连接设置**

| 命令参数   | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| -d         | 以守护进程（daemon）方式运行服务                             |
| -u         | 指定运行memcached的用户，如果当前用户为root，需要使用此参数指定用户 |
| -l         | 指定memcached进程监听的服务器ip地址，可以不设置此参数        |
| -p（小写） | 指定memcached服务监听TCP端口号，默认为11211                  |
| -P（大写） | 设置保存memcached的pid文件（$$）,保存PID到指定文件           |

**内存相关设置：**

| 命令参数 | 说明                                                |
| -------- | --------------------------------------------------- |
| -m       | 指定memcached服务可以缓存数据的最大内存，默认是64MB |
| -M       | memcached服务内存不够时禁止LRU，如果内存满了会报错  |
| -n       | 为key+value——flags分配的最小内存空间，默认为48字节  |
| -f       | chunk size增长因子，默认为1。25                     |
| -L       | 启用大内存页，可以降低内存浪费，改进性能            |

**并发连接设置：**

| 并发连接设置 | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| -c           | 最大的并发连接数，默认是1024                                 |
| -t           | 线程数，默认4.由于memcached采用的是NIO，所以太多线程作用不大 |
| -R           | 每个event最大请求数，默认是20                                |
| -C           | 禁用CAS（可以禁止版本计数，减少开销）                        |

**测试参数：**

| -v   | 打印较少的errors/warnings                        |
| ---- | ------------------------------------------------ |
| -vv  | 打印非常多调试信息和错误输出到控制台             |
| -vvv | 打印极多的调试信息和错误输出，也打印内部状态转发 |

**其他选项可通过“memcached -h”命令来显示**

#### 写入Memcached数据

```shell
Memcached中添加数据时，注意添加的数据一般为键值对的形式，例如：key1–>values1,key2–>values2
```



### 6.9、Memcached应用介绍

#### 测试Memcached语句

**这里把Memcached添加，查询，删除等的命令和MySQL数据库做一个基本类比，见下表：**

| MySQL数据库管理   | memcached管理         |
| ----------------- | --------------------- |
| MySQL的insert语句 | memcached的set命令    |
| MySQL的select语句 | memcached的get命令    |
| MySQL的delete语句 | memcached的delete命令 |

注意：

**memcached的服务器客户端通信并不使用复杂的XML等格式，而使用简单的基于文本行的协议。**

**我们可以使用telnet、nc等命令，可以连接memcached服务端，进行数据操作。**

![image-20200807151410482](notes/image-20200807151410482.png)

**通过printf配合nc向Memcached中写入数据，命令如下：**

```shell
备注：
printf 是格式化打印的命令，printf 命令格式化输出。
nc  功能强大的网络工具
```

#### 读写案例

写入数据

```shell
# set命令设置的字符数量，必须准确，否则会报错，例如
# 对key1 设置值 xiaomij，字符是7个
[root@web01 ~]# printf "set name 0 0 6\r\nxiaomij\r\n"|nc 127.0.0.1 11211
CLIENT_ERROR bad data chunk
ERROR


# 字符数量正确后
[root@web01 ~]# printf "set name 0 0 6\r\nxiaomi\r\n"|nc 127.0.0.1 11211
STORED
```

读取数据

```shell
[root@web01 ~]# printf 'get name \r\n'|nc 127.0.0.1 11211
VALUE name 0 6
xiaomi
END
```

通过printf配合nc从Memcached中删除数据，命令如下

```shell
[root@web01 ~]# printf "delete name\r\n" | nc 127.0.0.1 11211
DELETED  #出现它表示删除了

# 再次获取就已经找不到了
[root@web01 ~]# printf 'get name \r\n'|nc 127.0.0.1 11211
END
```

#### telnet连接memcached

```shell
1.安装telnet
[root@memcached01 ~]# yum install telnet -y

2.写入数据
[root@web01 ~]# telnet 127.0.0.1 11211
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
set name 0 0 10				#设置字节数
zhangsan					#小于10个字节不报错

STORED
get name
VALUE name 0 10
zhangsan

END
delete name
DELETED
get name
END
quit
Connection closed by foreign host.

```

#### 操作Memcached命令语法

![image-20200807152518221](notes/image-20200807152518221.png)

下表展示命令的详细说明

![image-20200807152619786](notes/image-20200807152619786.png)

#### 关闭memcached服务

```shell
[root@memcached01 ~]#  ps -ef | grep memcached | grep -v grep
root      11937      1  0 02:50 ?        00:00:00 memcached -m 16m -p 11211 -d -u root -c 8192
root      11959      1  0 02:53 ?        00:00:00 memcached -m 16m -p 11212 -d -u root -c 8192

# 在启动memcached的时候，最好指定pid文件，关于启停管理。
[root@web01 run]# memcached -m 16m -p 11211 -d -u root -c 8192 -P /var/run/11211.pid
[root@web01 run]# memcached -m 16m -p 11212 -d -u root -c 8192 -P /var/run/11212.pid



# 杀死pid即可
[root@memcached01 ~]# ps -ef|grep memcached |grep -v grep |awk '{print $2}' | xargs kill

[root@web01 run]# kill 11937 
[root@web01 run]# kill 11959  

[root@memcached01 ~]# kill `cat /var/run/11211.pid`

# 重启了服务端，数据也就丢了
[root@web01 run]# memcached -m 16m -p 11212 -d -u root -c 8192 -P /var/run/11212.pid


[root@web01 run]# telnet 192.168.37.105 11212
Trying 192.168.37.105...
Connected to 192.168.37.105.
Escape character is '^]'.
set sex 0 0 3
ban
STORED

get sex
VALUE sex 0 3
ban
END
quit
Connection closed by foreign host.


# 重新启动，查看数据

[root@memcached01 ~]# kill `cat /var/run/11211.pid`

memcached -m 16m -p 11211 -d -u root -c 8192 -P /var/run/11212.pid

[root@web01 ~]# netstat -tunpl|grep 11212
tcp        0      0 0.0.0.0:11212           0.0.0.0:*               LISTEN      1276/memcached      
tcp6       0      0 :::11212                :::*                    LISTEN      1276/memcached      
udp        0      0 0.0.0.0:11212           0.0.0.0:*                           1276/memcached      
udp6       0      0 :::11212                :::*                                1276/memcached    

[root@web01 ~]# telnet 192.168.37.105 11212
Trying 192.168.37.105...
Connected to 192.168.37.105.
Escape character is '^]'.
get sex
END
quit
Connection closed by foreign host.
#发现之前的数据丢失了
```

#### 企业工作场景中如何配置Memcached

在企业实际工作中，一般是开发人员提出需求，说要部署一个Memcached数据缓存。运维人员在接到这个不确定的需求后，需要和开发人员深入沟通，进而确定要将内存指定为多大，或者和开发人员商量如何根据具体业务来指定内存缓存的大小。此外，还要确定业务的重要性，进而决定是否采取负载均衡，分布式缓存集群等架构，最后确定要使用多大的并发连接数等。

对于运维人员，部署Memcached一般就是安装Memcached服务器端，把服务启动起来，做好监控，配好开机自启动，基本就OK了。

客户端的PHP程序环境一般在安装LNMP环境时都会提前安装Memcached客户端插件，Java程序环境下，开发人员会用第三方的JAR包直接连接Memcached服务。





### 6.10、回归LNMP环境搭建

#### Memcached客户端

这里超哥采用LNMP架构，结合Memcached做缓存，那么就需要大家按照前面的课程，搭建出LNMP环境，以正确访问出`phpinfo`页面为准。



#### LNMP快速部署

#### nginx部署

```shell
#下载依赖包
[root@web01 ~]# yum install pcre pcre-devel  openssl openssl-devel -y 

#下载nginx源码包
[root@web01 opt]# wget http://nginx.org/download/nginx-1.16.0.tar.gz

#解压缩
[root@web01 opt]# tar -zxvf nginx-1.16.0.tar.gz 


#创建nginx用户
[root@web01 opt]# useradd -s /sbin/nologin -M nginx

#切换到nginx解压路径
[root@web01 opt]# cd nginx-1.16.0/

#安装指定
[root@web01 nginx-1.16.0]# ./configure --user=nginx --group=nginx --prefix=/opt/Nginx1.16.0/ --with-http_stub_status_module --with-http_ssl_module

#编译且安装
[root@web01 opt]# make && make install

#删除编译包、已经安装好、可删除
[root@web01 opt]# rm -rf nginx-1.16.0

#启动nginx
[root@web01 opt]# /opt/Nginx1.16.0/sbin/nginx 

[root@web01 opt]# netstat -tunpl|grep nginx
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      4338/nginx: master  

```

测试访问

![image-20201105210741740](notes/image-20201105210741740.png)

#### mysql部署

```shell
[root@web01 opt]# yum install mariadb-server mariadb -y

[root@web01 opt]# rpm -qa mariadb
mariadb-5.5.65-1.el7.x86_64

#启动服务
[root@web01 opt]# systemctl start mariadb

[root@web01 opt]# netstat -tunpl|grep mysql
tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN      4640/mysqld  
```

#### php部署

```shell
#安装依赖工具、常用工具
yum install  gcc gcc-c++ make zlib-devel libxml2-devel libjpeg-devel libjpeg-turbo-devel libiconv-devel \
freetype-devel libpng-devel gd-devel libcurl-devel libxslt-devel libxslt-devel -y

#下载php包
[root@web01 opt]# wget http://mirrors.sohu.com/php/php-7.3.5.tar.gz

#解压缩
[root@web01 opt]# tar -zxvf php-7.3.5.tar.gz 

#进入解压后的php目录
[root@web01 php-7.3.5]# cd php-7.3.5/

#指定安装
./configure --prefix=/opt/php7.3.5 \
--enable-mysqlnd \
--with-mysqli=mysqlnd \
--with-pdo-mysql=mysqlnd \
--with-freetype-dir \
--with-jpeg-dir \
--with-png-dir \
--with-zlib \
--with-libxml-dir=/usr \
--enable-xml \
--disable-rpath \
--enable-bcmath \
--enable-shmop \
--enable-sysvsem \
--enable-inline-optimization \
--with-curl \
--enable-mbregex \
--enable-fpm \
--enable-mbstring \
--with-gd \
--with-openssl \
--with-mhash \
--enable-pcntl \
--enable-sockets \
--with-xmlrpc \
--enable-soap \
--enable-short-tags \
--enable-static \
--with-xsl \
--with-fpm-user=nginx \
--with-fpm-group=nginx \
--enable-ftp \
--enable-opcache=no


#编译且安装
[root@web01 php-7.3.5]# make && make install


#移动开发配置文件并改名
[root@web01 php-7.3.5]# mv php.ini-development /opt/php7.3.5/lib/php.ini


#创建1个新的php-fpm.conf和www.conf
cd /opt/php7.3.5/etc
[root@web01 etc]# cp php-fpm.conf.default php-fpm.conf
[root@web01 etc]# ls
pear.conf  php-fpm.conf  php-fpm.conf.default  php-fpm.d

cd /opt/php7.3.5/etc/php-fpm.d
[root@web01 php-fpm.d]# cp www.conf.default www.conf

#启动php-fpm
[root@web01 /]# /opt/php7.3.5/sbin/php-fpm 

[root@web01 /]# netstat -tunpl|grep php-fpm
tcp        0      0 127.0.0.1:9000          0.0.0.0:*               LISTEN      2664/php-fpm: maste 

```

nginx结合php

```shell
#修改nginx配置文件、内如如下
[root@web01 conf]# cat nginx.conf

[root@web01 conf]# vim nginx.conf

#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
#pid        logs/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    access_log  logs/access.log  main;
    sendfile        on;
    keepalive_timeout  65;

    gzip  on;

include extra/myphp.conf;

}


#创建extra目录且创建myphp.conf
[root@web01 conf]# mkdir extra
[root@web01 conf]# cd extra/
[root@web01 extra]# vim myphp.conf

#编写myphp.conf、内容如下：
oot@web01 conf]# cat extra/myphp.conf 

server {
listen 80;
server_name _;
location / {
    root html/;
    index index.html;
}

#用于转发解析php
location ~ .*\.(php|php5)?$ {
    root html/;
    fastcgi_pass 127.0.0.1:9000;
    fastcgi_index index.php;
    include fastcgi.conf;
}

}


#检查nginx语法、再重启nginx
[root@web01 conf]# /opt/Nginx1.16.0/sbin/nginx -t
nginx: the configuration file /opt/Nginx1.16.0//conf/nginx.conf syntax is ok
nginx: configuration file /opt/Nginx1.16.0//conf/nginx.conf test is successful

[root@web01 conf]# /opt/Nginx1.16.0/sbin/nginx -s reload
```

创建php资源

```shell
[root@web01 conf]# echo "<?php phpinfo(); ?>"  > /opt/Nginx1.16.0/html/index.php
```

测试访问

![image-20201105210714512](notes/image-20201105210714512.png)



### 6.11、php7结合memcacahed结合



#### 部署memcached支持php插件

memcached是分为服务端和客户端的。服务端我们已经启动，这里为lnmp连接memcached属于客户端的插件配置。

这里要注意的是，新版php7对于memcached的支持还需要手动装插件，这里是一个坑，遇见坑不怕，咱可以看报错，上google。。

跟着超哥的办法来，准没问题。



#### 安装依赖环境（重要）

```shell
# 安装依赖环境
#获取插件包
cd /opt && \
wget https://launchpad.net/libmemcached/1.0/1.0.18/+download/libmemcached-1.0.18.tar.gz

#解压缩
[root@web01 opt]# tar -zxf libmemcached-1.0.18.tar.gz 

#进入压缩目录编译安装
cd libmemcached-1.0.18/

[root@web01 libmemcached-1.0.18]# ./configure --prefix=/opt/libmemcached

[root@web01 libmemcached-1.0.18]# make && make install
```



#### 安装memcached扩展--php-memcached.git

```shell
# 然后
cd /opt && \
git clone https://github.com/php-memcached-dev/php-memcached.git
[root@web01 opt]# cd php-memcached/

#执行这个文件生成编译文件
[root@web01 php-memcached]# /opt/php7.3.5/bin/phpize 
Configuring for:
PHP Api Version:         20180731
Zend Module Api No:      20180731
Zend Extension Api No:   320180731


#进行编译指定安装
[root@web01 php-memcached]# ./configure  --with-libmemcached-dir=/opt/libmemcached/   --with-php-config=/opt/php7.3.5/bin/php-config

[root@memcached01 php-memcached]# make && make install

# 最后检查php的插件目录，是否出现了mencached
[root@web01 php-memcached]# ls /opt/php7.3.5/lib/php/extensions/no-debug-non-zts-20180731/
memcached.so  # 看到该模块才是正确
```



#### 安装memcache扩展--pecl-memcache

```shell
1.下载
cd /opt && \
git clone https://github.com/websupport-sk/pecl-memcache

2.安装
[root@web01 opt]# cd pecl-memcache/
[root@web01 pecl-memcache]# /opt/php7.3.5/bin/phpize
Configuring for:
PHP Api Version:         20180731
Zend Module Api No:      20180731
Zend Extension Api No:   320180731


[root@web01 pecl-memcache]# ./configure  --with-php-config=/opt/php7.3.5/bin/php-config

[root@web01 pecl-memcache]# make && make install


# 检查扩展文件，看到这俩才是正确
[root@web01 pecl-memcache]# ls /opt/php7.3.5/lib/php/extensions/no-debug-non-zts-20180731/
memcached.so  memcache.so
```



#### 验证phpinfo

验证下是否正确结合了php-memcached，以及memcache，这两个

```shell
修改/opt/php7.3.5/lib/php.ini

# 修改如下3行信息
 751 extension_dir = "/opt/php7.3.5/lib/php/extensions/no-debug-non-zts-20180731/"
 752 extension = memcached.so
 753 extension = memcache.so

# 重新加载php-fpm服务，检验配置文件语法，正确才行
[root@web01 pecl-memcache]# /opt/php7.3.5/sbin/php-fpm  -t
[06-Nov-2020 15:56:16] NOTICE: configuration file /opt/php7.3.5/etc/php-fpm.conf test is successful


# 重启
pkill php-fpm
[root@web01 pecl-memcache]# ps -ef|grep php-fpm
root      33126   1597  0 15:57 pts/0    00:00:00 grep --color=auto php-fpm

[root@web01 pecl-memcache]# /opt/php7.3.5/sbin/php-fpm 
[root@web01 pecl-memcache]# ps -ef|grep php-fpm
root      33128      1  0 15:57 ?        00:00:00 php-fpm: master process (/opt/php7.3.5/etc/php-fpm.conf)
nginx     33129  33128  0 15:57 ?        00:00:00 php-fpm: pool www
nginx     33130  33128  0 15:57 ?        00:00:00 php-fpm: pool www
root      33132   1597  0 15:57 pts/0    00:00:00 grep --color=auto php-fpm
```

测试访问192.168.37.105/index.ph 看到两个插件则表示安装成功

![image-20201106155933961](notes/image-20201106155933961.png)



### 6.12、php写入memcached数据

#### 验证php操作memcached

php代码文件

```shell
[root@web01 pecl-memcache]# cd /opt/Nginx1.16.0/html/
[root@web01 html]# pwd
/opt/Nginx1.16.0/html

[root@web01 html]# cat test_memcached.php

<?php						#PHP开始标识

$memcache = new Memcache;		 #创建一个Memcache对象
$memcache->connect('192.168.37.105','11211') or die ("Could not connect Mc server");  #连接Memcached服务器
$memcache->set('name01','chaoge nb');			#设置一个变量到内存中
$get=$memcache->get('name01');					 #从内存中取出key
echo $get;                             			 #输出key值到屏幕

?>											#PHP结束标识
                              
```

确保memcached运行着

```shell
[root@web01 html]# memcached -m 16m -p 11211 -d -u root -c 8192 -P /var/run/11211.pid
[root@web01 html]# ps -ef|grep memcache|grep -v grep
memcach+   1289      1  0 15:08 ?        00:00:00 /usr/bin/memcached -u memcached -p 11211 -m 64 -c 1024
```

命令行测试该脚本

```shell
[root@web01 html]# /opt/php7.3.5/bin/php test_memcached.php 
chaoge nb				#发现输出key的值
```

网页测试脚本

![image-20201106161112432](notes/image-20201106161112432.png)



### 6.13、Memcached工具配置

#### Memadmin工具

在工作里，我们经常会去寻找一些强大的第三方工具，去帮助我们更方便的管理系统。

运维人员除了通过命令行的形式，去管理memcached，还可以使用一些很好用的第三方工具。例如memadmin工具，该软件依赖于PHP环境，且功能强大。

#### 环境部署

该工具是基于PHP开发的，因此得部署好PHP环境，超哥前面已经带着大家部署了LNMP环境，因此直接进行操作即可。

```shell
1.获取软件包，官网http://www.junopen.com/memadmin/
下载地址：http://www.junopen.com/memadmin/memadmin-1.0.12.tar.gz
wget http://www.junopen.com/memadmin/memadmin-1.0.12.tar.gz

2.解压缩
[root@web01 opt]# tar -zxf memadmin-1.0.12.tar.gz 

3.移动该程序，到lnmp站点下即可，此时去了解下nginx的配置
[root@web01 opt]# mv memadmin /opt/Nginx1.16.0/html/


4.修改nginx配置文件
http{

...
    include extra/myphp.conf;
    include extra/memcache-tool.conf;		#主要加这个
 }
 #创建extra/memcache-tool.conf
 
[root@web01 html]# cat /opt/Nginx1.16.0/conf/extra/memcache-tool.conf 

server {
listen 81;
server_name _;
location / {
    root html/memadmin/;
    index index.php index.html;
}

location ~ .*\.(php|php5)?$ {
    root html/memadmin/;
    fastcgi_pass 127.0.0.1:9000;
    fastcgi_index index.php;
    include fastcgi.conf;
}

}

5.检查nginx语法并重载配置文件
[root@web01 html]# /opt/Nginx1.16.0/sbin/nginx -t
nginx: the configuration file /opt/Nginx1.16.0//conf/nginx.conf syntax is ok
nginx: configuration file /opt/Nginx1.16.0//conf/nginx.conf test is successful
[root@web01 html]# /opt/Nginx1.16.0/sbin/nginx -s reload


4.此时已经可以直接访问该程序
http://192.168.37.105:81
默认的账号密码是 admin
#看到以下登录界面说明memcache-tool搭建成功
```

### 6.14、Memcached数据监控

#### 新建连接

这样的数据库管理界面，最好是通过nginx进行访问限制，只能内网访问。

![image-20201106164516060](notes/image-20201106164516060.png)

![image-20201106164554349](notes/image-20201106164554349.png)

### 



#### 开始管理

只要memcached正确启动，连接的ip+port正确，即可进入管理

基本界面

![image-20201106164716761](notes/image-20201106164716761.png)

数据库统计信息

常见数据库的key信息如下，主要统计memcached的读写次数，总key等。

![image-20201106164817990](notes/image-20201106164817990.png)

通过这样的页面，可以更为直观清晰的掌握数据库信息，这也是运维开发人员的重要技能。

#### 统计get次数

![image-20201106164946115](notes/image-20201106164946115.png)

设置监控刷新频率

![image-20201106165051173](notes/image-20201106165051173.png)

![image-20201106165334733](notes/image-20201106165334733.png)



#### 统计get次数

![image-20201106165716941](notes/image-20201106165716941.png)

#### 统计多个指标

![image-20201106165528930](notes/image-20201106165528930.png)

#### 统计命中数

命中率就表示用户get查找数据，在memcached缓存中成功找到了。

![image-20201106170032860](notes/image-20201106170032860.png)

多次未找到数据，命中率越来越低

成功找到数据，命中率变高，运维同学可以根据此命中率，来判断，用户获取数据是否有缓存失效等问题

![image-20201106170006909](notes/image-20201106170006909.png)



#### 读写数据

写入数据

![image-20201106170221434](notes/image-20201106170221434.png)

获取数据

![image-20201106170238925](notes/image-20201106170238925.png)



### 6.15、修改mariadb为mysql启动

#### 部署wordpress

```shell
1.准备好lnmp环境
可以去看超哥笔记
http://book.luffycity.com/linux-book/%E4%BC%81%E4%B8%9A%E7%BA%A7%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1%E5%AE%9E%E6%88%98/LNMP%E9%BB%84%E9%87%91%E6%9E%B6%E6%9E%84.html?h=wordpress
```

**2.注意严格按照超哥的笔记来操作，使用mysql，而不是mariadb数据库**

- wordpress只能用mysql，而不是mariadb。

  -且虚拟机的内存给大一点

#### 二进制新式安装数据库

```shell
1.先删除之前安装的mariadb
[root@web01 html]# yum remove mariadb mariadb-server -y
rpm -e --nodeps mariadb-libs

2.下载二进制mysl安装包
cd /opt && \
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-5.7.31-linux-glibc2.12-x86_64.tar.gz


3.解压缩
tar -zxf mysql-5.7.31-linux-glibc2.12-x86_64.tar.gz 

4.创建软链接
[root@web01 opt]# ln -s mysql-5.7.31-linux-glibc2.12-x86_64 mysql5.7.31

5.手动创建mysql配置文件 vim /etc/my.cnf
[root@web01 opt]# cat /etc/my.cnf
[mysqld]
basedir=/opt/mysql5.7.31/
datadir=/opt/mysql5.7.31/data
socket=/tmp/mysql.sock
server_id=1
port=3306
log_error=/opt/mysql5.7.31/data/mysql_err.log

[mysql]
socket=/tmp/mysql.sock

6.创建data目录
[root@web01 mysql5.7.31]# mkdir /opt/mysql5.7.31/data
```



#### 初始化数据库

```shell
1.确定mariadb卸载干净
[root@web01 mysql5.7.31]# rpm -qa mariadb-libs
[root@web01 mysql5.7.31]# rpm -qa mariadb

2.创建mysql用户
[root@web01 mysql5.7.31]# useradd -s /sbin/nologin -M mysql

3.给mysql目录授权
[root@web01 mysql5.7.31]# chown -R mysql.mysql /opt/mysql5.7.31/

4.初始化数据库
[root@web01 opt]# /opt/mysql5.7.31/bin/mysqld --initialize-insecure --user=mysql --basedir=/opt/mysql5.7.31/ --datadir=/opt/mysql5.7.31/data/

```



#### 配置mysql客户端

```shell
1.配置mysql启动脚本，定义mysqld.service，脚本如下
[root@web01 mysql]# cat /etc/systemd/system/mysqld.service
[Unit]
Description=MySQL server by chaoge
Documentation=man:mysqld(8)
Documentation=http://dev.mysql.com/doc/refman/en/using-systemd.html
After=network.target
After=syslog.target
[Install]
WantedBy=multi-user.target
[Service]
User=mysql
Group=mysql
ExecStart=/opt/mysql5.7.31/bin/mysqld --defaults-file=/etc/my.cnf
LimitNOFILE=5000
```

### 启动mysql数据库

```shell
[root@web01 opt]# systemctl start mysqld
[root@web01 opt]# 
[root@web01 opt]# 
[root@web01 opt]# systemctl status mysqld
● mysqld.service - MySQL server by chaoge
   Loaded: loaded (/etc/systemd/system/mysqld.service; disabled; vendor preset: disabled)
   Active: active (running) since 五 2020-11-06 17:45:23 CST; 5s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
 Main PID: 56571 (mysqld)
   CGroup: /system.slice/mysqld.service
           └─56571 /opt/mysql5.7.31/bin/mysqld --defaults-file=/etc/my.cnf

11月 06 17:45:23 web01 systemd[1]: Started MySQL server by chaoge.
11月 06 17:45:23 web01 systemd[1]: Starting MySQL server by chaoge...
[root@web01 opt]# systemctl enable mysqld
Created symlink from /etc/systemd/system/multi-user.target.wants/mysqld.service to /etc/systemd/system/mysqld.service.
[root@web01 opt]# 

```

#### 检查mysql启动状态

```shell
[root@web01 opt]# netstat -tunlp|grep mysql
tcp6       0      0 :::3306                 :::*                    LISTEN      56571/mysqld  

[root@web01 opt]# ps -ef|grep mysql |grep -v grep
mysql     56571      1  0 17:45 ?        00:00:00 /opt/mysql5.7.31/bin/mysqld --defaults-file=/etc/my.cnf
```

#### 配置mysql命令环境变量

```shell
[root@web01 mysql]# echo "export PATH=/opt/mysql5.7.31/bin:$PATH" >> /etc/profile
[root@web01 mysql]# source /etc/profile
[root@web01 mysql]# echo $PATH
```

#### 登录mysql

```shell
[syh@web01 bin]$ mysql -uroot -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 2
Server version: 5.7.31 MySQL Community Server (GPL)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 

```

#### 登录默认密码为空、设置密码加大安全性

```shell
[syh@web01 bin]$ mysqladmin -uroot password 'syh666'
```

```shell
1.在mysql中新建数据库且创建wordpress用户授权
#创建库
mysql> create database wordpress;
Query OK, 1 row affected (0.00 sec)

#创建用户
mysql> CREATE USER 'wordpress'@'host' IDENTIFIED BY '123456';
Query OK, 0 rows affected (0.00 sec)

#刷新权限
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)


#给用户授权
mysql>  GRANT ALL ON wordpress.* TO 'wordpress'@'%' IDENTIFIED BY '123456';
Query OK, 0 rows affected, 1 warning (0.00 sec)
```

#### 测试php访问mysql

```shell

1.在nginx站点目录下创建php脚本文件
[syh@web01 ~]$ sudo vim /opt/Nginx1.16.0/html/test_mysql.php

[root@web01 conf]# cat ../html/blog/test_mysql.php

[syh@web01 html]$ sudo cat test_mysql.php 

<?php
$link_id=mysqli_connect('localhost','wordpress','123456') or mysql_error();
if($link_id){

    echo "mysql successful by syh.\n";
}else {

    echo mysql_error();
}
?>

?>
```

此时访问网页

![image-20201106202920621](notes/image-20201106202920621.png)

### 6.16、再次安装wordopress

#### Wordpress博客程序准备

```shell
1.下载获取wordpress程序
sudo cd /opt && \
sudo wget https://wordpress.org/latest.zip

2.解压缩
[syh@web01 opt]$ sudo unzip latest.zip 

3.将代码目录移动至nginx站点目录下
[syh@web01 opt]$ sudo mv wordpress/ Nginx1.16.0/html/


4.给目录以及内容授权
[syh@web01 html]$ sudo chown -R nginx.nginx wordpress/


5.修改nginx配置文件


http{
...
    include extra/myphp.conf;
    include extra/memcache-tool.conf;
    include extra/my-wordpress.conf;
}

#创建站点资源配置
[syh@web01 conf]$ sudo cat extra/my-wordpress.conf 
server {
listen 82;
server_name _;
location / {
    root html/wordpress/;
    index index.php index.html;
}
location ~ .*\.(php|php5)?$ {
    root html/wordpress/;
    fastcgi_pass 127.0.0.1:9000;
    fastcgi_index index.php;
    include fastcgi.conf;
}
}


#检查nginx语法、并重载配置文件
[syh@web01 conf]$ sudo /opt/Nginx1.16.0/sbin/nginx  -t
nginx: the configuration file /opt/Nginx1.16.0//conf/nginx.conf syntax is ok
nginx: configuration file /opt/Nginx1.16.0//conf/nginx.conf test is successful

[syh@web01 conf]$ sudo /opt/Nginx1.16.0/sbin/nginx  -s reload

```

测试访问：192.168.37.105:82

![image-20201106210848584](notes/image-20201106210848584.png)



![image-20201106210942854](notes/image-20201106210942854.png)



![image-20201106211112940](notes/image-20201106211112940.png)

![image-20201106211125100](notes/image-20201106211125100.png)

![image-20201106211150819](notes/image-20201106211150819.png)



### 6.17、Wordpress结合memcached

#### memcached案例一

#### 部署memcached来缓存wordpress数据

```shell
wordpress缓存数据缓存到memcached中
https://cn.wordpress.org/plugins/memcached/
1.下载链接
cd /opt && \
sudo wget https://downloads.wordpress.org/plugin/memcached.3.2.2.zip

2.解压缩
[syh@web01 opt]$ sudo unzip memcached.3.2.2.zip 
Archive:  memcached.3.2.2.zip
   creating: memcached/
  inflating: memcached/object-cache.php  
  inflating: memcached/readme.txt  
  
3.将解压后的目录里php文件移动到wordpress站点目录的wp-content下
[syh@web01 memcached]$ sudo mv object-cache.php /opt/Nginx1.16.0/html/wordpress/wp-content/

4.修改该配置文件，修改wordpress的缓存放入memcached地址
[syh@web01 wp-content]$ grep 192.168.37.105 object-cache.php  -n
745:			$buckets = array( '192.168.37.105:11211' );

```

此时使用memadmin来统计get命中率

其实在登录的时候，wordpress的数据以及缓存到memached里了

![image-20201106213344540](notes/image-20201106213344540.png)



用wordpress来写一篇文章

![image-20201106213710038](notes/image-20201106213710038.png)

此时可以查看memcached的数据到底存了些什么

![image-20201106213815900](notes/image-20201106213815900.png)



也可以在命令行中，登录查看wordpress数据

```shellshell
1.wordpress数据是存在mysql数据库的，且缓存到memcached里，加快访问，可以在mysql里找数据
大致SQL如下
mysql> show databases;
mysql> use wordpress;
mysql> shwo tables;
mysql> desc wp_posts;

mysql> select * from wp_posts where ID=12\G;
*************************** 1. row ***************************
                   ID: 12
          post_author: 1
            post_date: 2020-11-06 21:36:54
        post_date_gmt: 2020-11-06 13:36:54
         post_content: <!-- wp:paragraph -->
<p>认真听课、做笔记、多做练习</p>
<!-- /wp:paragraph -->
           post_title: 如何学习lInux?
         post_excerpt: 
          post_status: inherit
       comment_status: closed
          ping_status: closed
        post_password: 
            post_name: 11-revision-v1
              to_ping: 
               pinged: 
        post_modified: 2020-11-06 21:36:54
    post_modified_gmt: 2020-11-06 13:36:54
post_content_filtered: 
          post_parent: 11
                 guid: http://192.168.37.105:82/?p=12
           menu_order: 0
            post_type: revision
       post_mime_type: 
        comment_count: 0
1 row in set (0.00 sec)

```





### 5.18、Memcached会话共享

#### memcached案例二(会话共享)

#### session的作用

session主要是web开发中设置，服务器会为每个用户创建一个会话(session)，存储用户的相关信息，便于再后续的请求中，都能够定位同一个上下文。

```
例如你在登录了淘宝网之后，又继续点击淘宝网的其他页面进行了跳转，淘宝网是怎么知道，你还是你？这就是因为session的作用，服务器通过你的session记录，知道了你，还是你。
如果用户没有session，服务器会创建一个session独享，直到会话过期或者主动释放（用户退出登录），服务器才会终止session。
```

![image-20200809173840451](notes/image-20200809173840451.png)

#### 分布式架构的session

##### 单体服务器架构

在单体服务器的年代，session直接保存在单台机器，完全没有任何问题，也就是我们学习的LNMP架构。

##### 分布式架构

这就涉及到我们所学的web集群，lnmp集群了

![image-20200809172440717](notes/image-20200809172440717.png)

随着分布式架构的流行，单个服务器已经不能满足系统的需要了，通常都会把系统部署在多台服务器上，通过负载均衡把请求分发到其中的一台服务器上；

那么很有可能第一次请求访问的 A 服务器，创建了 Session ，但是第二次访问到了 B 服务器，这时就会出现取不到 Session 的情况；

于是，分布式架构中，Session 共享就成了一个很大的问题。

##### 分布式架构session解决方案

- 禁用session

这种场景是存在的，当然不会用于普通的web站点，而是在一些【无状态服务】下进行的接口开发，每一次的接口访问，是不依赖于上一次的session的。这个我们了解即可。

- 存入cookie

cookie是存在用户浏览器本地的，如果我们把session也存入用户本地，这可以解决session分布式的问题，但是缺点也很大，就是信息不安全，如果黑客在你本地盗取了cookie，你的账户就有危险了。

- IP绑定策略

这种方案是利用负载均衡的IP绑定策略，例如超哥交给大家的Nginx负载均衡的算法，ip_hash，能够保证用户请求只发往一个后台机器，但是这种也有缺点，就是当后端对应的单台服务器如果挂掉，则会影响一大批用户，风险也很大。

- 服务器session同步

在服务器之间进行对session文件同步，可以保证每台服务器上都有有效的session信息，但是缺点是服务器规模如果较大，效率会很低，且有延迟。

- 【优选方案】使用缓存数据库

最优的方案是将用户session信息存入redis，memcached这样的数据库中，这的优点是

1.使用memcached进行session共享

2.可以水平扩展，增加memcached服务器

3.可以跨服务器进行session共享，甚至跨平台，如网页和app端

4.服务器重启数据也不丢失，这个得依赖于redis的持久化功能，memcached无法实现

#### 部署memcached会话共享

完成session共享有如下2个方案：

```
1.通过程序实现，web01只需要向memcached里写入session，web01向memcached读取session，当做普通数据来读写

2.通过php的配置修改，php默认是将用户session存储在文件里，改为Memcached存储
```

#### 修改php配置文件

```shell
有关session设置的，php配置文件，有如下
[syh@web01 lib]$ grep '^session' php.ini
session.save_handler = files
session.use_strict_mode = 0
session.use_cookies = 1
session.use_only_cookies = 1
session.name = PHPSESSID
session.auto_start = 0
session.cookie_lifetime = 0
session.cookie_path = /
session.cookie_domain =
session.cookie_httponly =
session.cookie_samesite =
session.serialize_handler = php
session.gc_probability = 1
session.gc_divisor = 1000
session.gc_maxlifetime = 1440
session.referer_check =
session.cache_limiter = nocache
session.cache_expire = 180
session.use_trans_sid = 0
session.sid_length = 26
session.trans_sid_tags = "a=href,area=href,frame=src,form="
session.sid_bits_per_character = 5
```

修改操作如下

修改两行有关会话数据的配置

```shell
[root@memcached01 php7.3.5]# grep 'session.save' lib/php.ini  -n
1337:; http://php.net/session.save-handler
1338:session.save_handler = files
1346:;     session.save_path = "N;/path"
1362:;     session.save_path = "N;MODE;/path"
1366:; http://php.net/session.save-path
1367:;session.save_path = "/tmp"
1458:;       (see session.save_path above), then garbage collection does *not*
```

修改命令

```shell
# 替换一行配置，并且在最底行加上一行配置
[root@memcached01 php7.3.5]# sed -i 's#session.save_handler = files#session.save_handler = memcache#;$a session.save_path = "tcp://10.0.1.40:11211"'  /opt/php7.3.5/lib/php.ini


# 检查替换结果
[root@memcached01 php7.3.5]# grep '^session.save' lib/php.ini  -n
1338:session.save_handler = memcache
1948:session.save_path = "tcp://192.168.37.105：11211"
```

再来检查php的配置，因为需要php能够支持session的处理，通过phpinfo检查，或者命令

```shell
[root@memcached01 php7.3.5]# /opt/php7.3.5/bin/php -i |grep session

# 或者添加phpinfo文件，在lnmp站点下

[syh@web01 html]$ cat index.php 
<?php phpinfo(); ?>

```

![image-20201107105544745](notes/image-20201107105544745.png)

这里是告诉大家，如果你公司用的是php进行开发，那么php保存会话，写入到memcached的配置就是这样，改如上两行配置即可。当你公司用php开发的网站，用户登录后，php就会将用户的session信息，存入到memcached里，这就是我们运维人员所需要学会，以及执行的任务。