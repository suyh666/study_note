## 五、互联网之 LNMP架构

#### 5.1web服务器介绍

常见的服务器介绍

- Web服务器常指的是（world wide web ，www）服务器、也是HTTP服务器，主要用于提供网上信息浏览。
- 我们大部分人接触互联网，都基本上是通过浏览器访问互联网中各种资源。
- Web 网络服务是一种被动访问的服务程序，即只有接收到互联网中其他主机发出的 请求后才会响应，最终用于提供服务程序的 Web 服务器会通过 HTTP(超文本传输协议)或 HTTPS(安全超文本传输协议)把请求的内容传送给用户。

Unix和Linux平台下的常用Web服务器常见有：

- Apache
- Nginx
- Lighttpd
- Tomcat
- IBM WebSphere

其中最为广泛的是Nginx，在Windows平台上最常用的是微软的IIS(Internet Information Server，互联网信息服务)是 Windows 系统中默认的 Web 服务程序。



**Apache**

<img src="notes/image-20200210104433780.png" style="zoom:33%;" />

- Apache是世界主流的Web服务器，世界上大多著名网站都是Apache搭建，优势在于开放源代码，开发维护团队强大、支持跨平台应用（Unix、Linux、Windows），强大的移植性等优点。
- Apache属于重量级产品，功能以模块化定制，消耗内存较高，性能稍弱于其他轻量级Web服务器

**Lighttpd**

<img src="notes/image-20200210104619194.png" style="zoom:33%;" />

Lighttpd是一款高安全性、快速、且灵活的Web服务器产品，专为高性能环境而设计，相比其他Web服务器，内存占用量小，能够有效管理CPU负载，支持（FastCGI、SCGI，Auth，输出压缩，url重写，别名）等重要功能，是Nginx的重要对手之一。

**Tomcat服务器**

Tomcat是一个开源、运行基于Java的Web应用软件的容器，Tomcat Server根据servlet和JSP规范执行，但是Tomcat对于平台文件、高并发处理较弱。要使用Tomcat需要对Java的应用部署有足够的了解。

<img src="notes/image-20200210105330980.png" style="zoom:33%;" />

**IBM WebSphere Application Server**

<img src="notes/image-20200210105804208.png" style="zoom:33%;" />

WebSphere Applicaiton Server是一种强大的Web应用服务器，基于Java的应用环境、建立、部署和管理网站应用。

**Microsoft IIS**



<img src="notes/image-20200210110031186.png" style="zoom:33%;" />

- 微软的IIS是一种灵活，安全易管理的Web服务器，从流媒体到Web应用程序，IIS提供了图形化的管理界面，用于配置和管理网络服务。
- IIS是一整套Web组件，包含了Web服务器，FTP服务器，SMTP服务器等常用的网页浏览、文件传输，邮件新闻等功能。
- 缺点是只能运行在Windows平台，还得购买商业化的操作系统。



**Nginx**

<img src="notes/image-20200210145444396.png" style="zoom:33%;" />



- Nginx是俄罗斯人Igor Sysoev（伊戈尔·塞索耶夫）开发的一款高性能的HTTP和反向代理服务器。
- Nginx以高效的epoll、kqueue、eventport作为网络IO模型，在高并发场景下，Nginx能够轻松支持5w并发连接数的响应，并且消耗的服务器内存、CPU等系统资源消耗却很低，运行非常稳定。
- 国内著名站点，新浪博客、网易、淘宝、豆瓣、迅雷等大型网站都在使用Nginx作为`Web服务器`或是`反向代理服务器`。

#### 5.2Nginx的优势以及发展历史

**为何选择Nginx**

<img src="notes/image-20200210151307575.png" style="zoom:33%;" />

在互联网的快速普及，全球化、物联网的迅速发展，世界排名前3的分别是Apache、IIS、Nginx，而Nginx一直在呈现增长趋势。

**Nginx资源消耗低/性能强**

- 官方提供的测试数据，Nginx能支持5W的并发连接，在实际生产环境下可支撑2~4W的并发连接数。
- Apache使用的网络I/O模型是传统的select模型，以Prefork多进程模式运转，需要经常派生子进程，消耗系统资源要比Nginx高得多。且一个进程在同一时间只会处理一个请求。
- 如Apache Web Server支撑这日均千万PV的网站，服务器平均负载在50~60，CPU消耗在70%~90%，而整体迁移到Nginx后，系统平均负载降低至1~4，CPU使用率在20%~40%，效果可见。
- 常见LNMP（linux、Nginx、Mysql、PHP）服务器在3万并发连接下，开启10个Nginx进程消耗不过`（15MB*10=150MB)`内存。就算开启64个php-cgi进程，也就消耗`20MB*64=1280MB`，总共后台进程使用不到2GB内存，且服务器如果内存较小，开启php-cgi进程数量可以再少点。
- 在压力测试，3W的并发连接下，Nginx+PHP的程序任然能够飞速运转，从Nginx的日志统计下(每分钟的第15秒有多少条日志)，单机处理请求能力在700次/秒，那么日承受访问量在`700*60*60*24=6048,0000`，且服务器的负载不会太高。





**Nginx成本低**

- Nginx强大功能其一在于反向代理、负载均衡，属于是软件负载均衡，企业购买硬件的`F5、NetScaler`等硬件负载均衡设备价格昂贵，而Nginx属于开源软件，遵循BSD协议，可以免费试用，甚至二次开发用于商业用途。
- BSD协议指的是给与用户更自由的协议，可以自由试用、修改源代码



**Nginx优势**

- 配置文件简单易读
- 支持Rewrite重写，根据域名、URL的不同，转发HTTP请求到不同的后端服务器组
- 高可用性，稳定性，宕机几率很低
- 节省资源，支持GZIP压缩静态资源
- 支持热部署，可以7*24小时不间断运行，数月时间可不重启，在kill进程的情况下对软件修改。



#### 5.3Nginx的网络模型

**网络IO概念说明**

**内核空间/用户空间**

- 内核：操作系统的核心是内核，独立于普通的应用程序，可以操作底层硬件，处理受保护的内存空间。
- 操作系统为了保护内核，使得用户进程无法直接操作内核（kernel），操作系统单独开辟了两个虚拟内存空间，一是内核空间，二是用户空间。

**进程切换**

为了控制进程执行，操作系统内核得有能力挂起CPU上运行的进程，或是恢复之前已挂起的进程，这种行为称之为进程切换。

**进程阻塞**

正在执行的进程，由于某些事件的等待，如请求资源加载中，资源加载失败等，系统自动执行阻塞语句，block，让该进程处于阻塞状态，因此进程的阻塞是一种主动行为，因此也之后又处于运行状态的进程（获取到CPU）才能转变为阻塞状态，阻塞状态下不吃CPU资源。

**文件描述符**

- 这是计算机科学里的一个术语，表述指向文件引用的一个抽象概念。它是一个索引值，指向内核为每一个进程打开文件的记录表。
- 程序打开一个文件，系统内核就向该进程发送一个文件描述符。

**Linux IO模型**

数据的IO操作，例如读取文件，数据会被拷贝到操作系统内核的缓冲区，然后从缓冲区拷贝到应用程序的内存空间。

![](notes/微信图片_20201003140927.png)

```
一个读取操作，经历两个阶段
1.等待数据准备
2.数据从内核拷贝到进程
```



对于网络IO

![](notes/微信图片_20201003141124.png)

```
1.等待网络上的数据分段到达，然后复制到内核的缓冲区
2.数据从内核缓存区拷贝到进程

网络应用主要处理两个问题，网络IO，数据计算
网络IO的延迟等待，是最主要的性能瓶颈。
```

为何选择Nginx，重点在于网络模型的区别

常见的IO模型

- 阻塞
- 非阻塞
- IO多路复用
- 异步IO

网络IO指的是输入、输出，本质上是socket读取，socket在linux中被抽象为流，IO就是对流的操作。

**【IO模型之BlockIO】**

例子：超哥和媳妇在麦当劳点餐，点过之后不知道什么时候能做好，只能坐在餐厅等待，直到用餐结束才离开。
但是媳妇其实还想去买个包包的，但是不知道饭菜什么时候好，只能慢慢等，浪费了时间，也没去逛街买包。

说明：在linux下默认所有的socket都是blocking阻塞的，阻塞指的是进程被等待了，CPU处理其他任务了。

<img src="notes/image-20200210180924183.png" style="zoom: 50%;" />

阻塞过程：

- 用户进程调用recv()进行系统调用，内核开始IO第一个阶段，准备数据（网络IO情况下，内核要等待所有数据接收完毕）。
  这个过程指的是，数据拷贝到操作系统内核是需要一定时间的。

- 用户进程这里整个进程被阻塞，当内核数据准备好之后，将数据从内核中再拷贝到用户内存，内核返回结果后，用户进程解除block阻塞状态，重新运行。

- 同步阻塞IO特点是，IO执行的2个阶段都是阻塞的，用户空间发起调用，内核准备数据阻塞，内核拷贝数据到用户空间阻塞。



同步阻塞的优缺点：

1.能够及时返回数据，无延迟
2.对于开发人员负担较低
3.对用户不友好，性能较弱



**【IO模型之非阻塞IO】**

例子：超哥和媳妇还是在麦当劳，点好了餐，但是不愿意白等着，又想去逛会商场，又害怕餐好了来迟了。
因此我们去逛一会，回来问一下客服，餐做好没，没好的话，待会再来问一遍

说明：在网络IO时候，非阻塞IO同样进行recvform系统调用，且检查数据是否准备好了，这里和阻塞IO不一样，非阻塞模式将`整个阻塞时间片`拆分成N小的阻塞，CPU仍有机会不断的操作该进程。



<img src="notes/image-20200210205814736.png" style="zoom:50%;" />



过程解释：

- 当用户进程发起read操作，内核若是没有数据，不会立即阻塞用户进程，而是立即返回了error报错
  对于用户进程而言，这就不再是等待了，而是立即有了一个结果，已知是error的时候，知道了数据还未准备好，于是再次发起系统调用。
  一旦内核准备好了数据，且在此收到用户的系统调用，内核立即将数据拷贝到用户内存，然后返回给用户进程。
- 区别就在于发起recvfrom时候的阻塞拆分了，用户进程需要不断的主动询问内核数据是否准备好

**【同步阻塞与非阻塞的区别】**

1. 非阻塞优点是，能够在等待某一任务的时间里继续处理其他任务，也包括继续提交新任务

2. 非阻塞缺点缺点是：任务完成的延迟变大，因为要多次轮询系统调用操作，而有可能任务就在轮询过程中完成了，造成整体数据吞吐量降低。



**【IO模型之IO多路复用】**

例子：超哥还是和媳妇在餐厅吃饭点餐，这回餐厅装了电子屏，可以显示食物制作进度，这样超哥和他媳妇逛街一回，回来不用再去问服务员了，可能服务员压根不想理你呢，直接看电子屏即可快速知道结果

- 由于刚才所说的`同步非阻塞`需要不断的主动轮询，轮询会消耗大量CPU时间。
- 计算机系统后台可能存在N个任务，如果能循环的查询多个任务的进度，并且这个轮询还不是用户进程发起的（不是超哥反复回来询问），而是有人帮忙查询，那可太方便了。

- 因此Linux下的select、poll、epoll就是做这个事的，且epoll效率最高

**【select事件】**

`select轮询方式`和`非阻塞轮询`区别在于

- select轮询可以同时监听多个socket，当一个socket的数据准备好了，即可进行IO操作，进程发起recvform系统调用，将数据由内核拷贝到用户进程，这个过程是阻塞的。
- 但是这里的select或是poll阻塞进程，和同步IO的阻塞不一样，同步IO的阻塞是数据全部准备好，一次性处理，而select是有一部分数据就调用用户进程来处理。
- IO多路复用，就是轮询多个socket,好比餐厅的电子屏，监控着多个食品订单的完成情况



<img src="notes/image-20200210213338845.png" style="zoom:50%;" />

- IO多路复用指的就是linux下的select，poll，epoll，也被称作事件驱动IO
  它们的好处在于：单进程就可以同时处理多个socket连接。原理就是select，poll，epoll这个函数会不断的轮询负责的所有socket，当某个socket有数据到达了，就通知用户进程。
- 当用户进程调用了select这个函数，整个进程就会被阻塞了，kernel内核会监视所有select负责的socket，有一个socket准备好了数据，select就返回给用户进程，用户进程发起系统调用，数据拷贝到用户进程。
- 其实IO多路复用和阻塞IO并没有太大的区别，有可能还更差一点，因为还额外的调用了select，有了额外的消耗。好处就是select可以同时处理多个连接。
- 如果连接数不是很多的情况下，用IO多路复用还不如使用（多线程+阻塞IO）的性能更好。
  IO多路复用优势在于同时处理更多的连接，而不是处理速度



**【异步非阻塞IO模型】**

例子：超哥媳妇不想逛街了，又不想在餐厅等着，因此他俩直接选择回家休息。拿起手机点外卖，饭好了外卖员直接送到家，这也太舒服了，又不用干等着，还能做些别的事，饭菜就能送来了。

<img src="notes/image-20200210215153896.png" style="zoom:50%;" />

过程解释：

用户进程发起aio_read系统调用，无论内核数据是否准备好，都会直接返回给用户进程，用户进程可以继续做其他事情。等待socket数据准备好了，内核直接复制给用户进程，接着向进程发出通知，整个过程都是非阻塞的。



**【网络IO模型总结】**

1.阻塞和非阻塞的区别
阻塞IO会一直阻塞对应的进程，直到数据操作完毕，
非阻塞IO在内核准备数据阶段就立刻返回

2.同步IO和异步IO的区别
区别在于，同步IO在IO操作的时候会被进程阻塞，blocking io，non-blocking io，IO multiplexing都属于同步IO
异步IO，在进程发起IO操作之后，内核就直接返回了，直到内核发送一个信号，告诉进程IO完成了，整个进程完全没有被阻塞。

<img src="notes/image-20200211092355819.png" style="zoom:50%;" />

#### 5.4Nginx的架构组成

<img src="notes/image-20200210162748550.png" style="zoom:50%;" />

#### 5.5Nginx的安装

【Nginx的版本】

商业版、开源版、淘宝nginx、支持Linux与Windows平台下载使用

可通过以下网站访问下载源码包

```shell
nginx.com  商业版
nginx.org  开源版
https://tengine.taobao.org/
```





【环境准备】

```shell
1.阿里云yun仓库准备

1.1将旧的yum仓库进行备份
[root@localhost yum.repos.d]# mv ./*.repo repobak/

1.2访问阿里云镜像网站下载
浏览器访问：https://developer.aliyun.com/mirror/
选择centos、epel
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo   指定目录下载centos的repo文件
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo			指定目录下载epel的repo文件

1.3清除旧缓存
[root@localhost yum.repos.d]# yum clean all

1.4生成缓存
[root@localhost yum.repos.d]# yum makecache

2.gcc编译环境
yum install -y gcc gcc-c++ autoconf automake make 

3.第三方模块库包
yum install zlib zlib-devel openssl openssl-devel pcre pcre-devel wget httpd-tools vim pcre-devel

4.确保防火墙关闭、selinux关闭、网络连接状态正常
[root@localhost yum.repos.d]# systemctl stop firewalld						#关闭防火墙
[root@localhost yum.repos.d]# systemctl disable firewalld					#开机自动关闭防火墙
#检测selinux状态、Disable则为关闭；若没关闭则修改配置文件/etc/selinux/config  修改其SELINUX=disabled、重启机器即可
[root@localhost ~]# getenforce 											
Disabled

[root@localhost ~]# ping www.baidu.com										#说明网络正常
PING www.a.shifen.com (163.177.151.109) 56(84) bytes of data.
64 bytes from www.baidu.com (163.177.151.109): icmp_seq=1 ttl=55 time=16.1 ms
```

【编译安装nginx】

```shell
1.到官方网站（以上三个网址都行）下载源码包

[root@localhost opt]# wget http://nginx.org/download/nginx-1.18.0.tar.gz


2.解压缩源码包
[root@localhost nginx-1.18.0]# tar -xzvf nginx-1.18.0.tar.gz 

3.查看解压后的内容、并解释各个文件、目录的作用
[root@localhost nginx-1.18.0]# ll
总用量 760
drwxr-xr-x 6 1001 1001    326 10月  4 16:48 auto				检测系统模块
-rw-r--r-- 1 1001 1001 302863 4月  21 22:09 CHANGES			更改记录文件
-rw-r--r-- 1 1001 1001 462213 4月  21 22:09 CHANGES.ru
drwxr-xr-x 2 1001 1001    168 10月  4 16:48 conf				存放nginx配置文件
-rwxr-xr-x 1 1001 1001   2502 4月  21 22:09 configure		释放编译文件的定制脚本
drwxr-xr-x 4 1001 1001     72 10月  4 16:48 contrib			提供了perl与vim插件
drwxr-xr-x 2 1001 1001     40 10月  4 16:48 html				存放标准html页面语法
-rw-r--r-- 1 1001 1001   1397 4月  21 22:09 LICENSE
drwxr-xr-x 2 1001 1001     21 10月  4 16:48 man
-rw-r--r-- 1 1001 1001     49 4月  21 22:09 README
drwxr-xr-x 9 1001 1001     91 10月  4 16:48 src				存放nginx源码


4。复制Nginx默认提供的vim语法插件
[root@localhost ~]# mkdir ~/.vim							          在家目录创建隐藏目录
[root@localhost .vim]# cp -r /opt/nginx-1.18.0/contrib/vim/* ~/.vim      复制解压后的contrib目录下vim下的所有内容到隐藏目录


5.开始准备编译三部曲
第一曲：进入软件源代码、执行编译脚本文件、如定制安装路径、以及开启额外功能等、可通过--help查看编译脚本的帮助信息
[root@localhost nginx-1.18.0]# ./configure  --help

指定路径安装、以及打开某些功能、释放makefile等信息
[root@localhost nginx-1.18.0]# ./configure --prefix=/opt/nginx1.18/ --with-http_ssl_module  --with-http_flv_module --with-http_gzip_static_module --with-http_stub_status_module  --with-threads  --with-file-aio

第二曲：直接输入make命令下一步安装
[root@localhost nginx-1.18.0]# make

第三曲：make install开始安装
[root@localhost opt]# make install

6.查看安装目录内的内容
[root@localhost nginx1.18]# ls
conf  html  logs  sbin

依次是配置文件，静态文件，日志，二进制命令目录

7.配置nginx的环境变量

[root@localhost sbin]# vim /etc/profile.d/nginx.sh					创建并编辑nginx脚本
输入以下内容、拼接全局环境变量
export PATH="$PATH:/opt/nginx1.18/sbin/"		
[root@localhost sbin]# cat /etc/profile.d/nginx.sh 
export PATH="$PATH:/opt/nginx1.18/sbin/"

8.退出当前会话、重新登录、检查环境变量是否加载
exit
ssh root@192.168.37.134
[root@localhost ~]# $PATH
-bash: /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/nginx1.18/sbin/:/root/bin:


9.此时可直接输入nginx命令启动服务

[root@localhost ~]# nginx						首次输入nginx表示启动服务、当第一次启动后再次启动则会报错，因为端口被占用
[root@localhost ~]# ps -ef|grep nginx
root       5690      1  0 19:38 ?        00:00:00 nginx: master process nginx
nobody     5691   5690  0 19:38 ?        00:00:00 nginx: worker process
[root@localhost ~]# netstat -tunpl|grep nginx
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      5690/nginx: master 

[root@localhost ~]# nginx -s stop								关闭nginx服务、重新加载nginx配置
[root@localhost ~]# nginx -s reload								不重启加载nginx配置、较为友好


10.查看nginx的编译模块信息

[root@localhost ~]# nginx -V
nginx version: nginx/1.18.0
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC) 
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled
configure arguments: --prefix=/opt/nginx1.18/ --with-http_ssl_module --with-http_flv_module --with-http_gzip_static_module --with-http_stub_status_module --with-threads --with-file-aio

```

#### 5.6nginx的配置文件

nginx的配置文件路径：/opt/nginx1.18/conf/nginx.conf(安装路径下的conf目录下的nginx.conf)

- nginx.conf由指令与指令块构成
- 每行语句由分号结束，指令和参数之间由空格分隔
- 指令块可以大括号{}组织多条指令块
- 配置文件中#号添加注释信息
- 支持`$变量`使用变量
- 支持include语句，组合多个配置文件
- 部分指令支持正则表达式



<img src="notes/image-20200211094210904.png" style="zoom:50%;" />

【include语句案例使用】

```shell
1.在/opt/nginx1.18/conf目录下创建一个目录
[root@localhost conf]# mkdir syh

2.在syh目录下创建多个.conf结尾的文件
touch www.conf hello.conf

3.vim编辑nginx的主配置文件
vim /opt/nginx1.18/conf/nginx.conf 
光标移动到sever的第一个大括号、输入快捷键shift+5、移动到另一个对应的大括号、在上一行插入语句
include syh/*.conf
表示读取syh目录下的所有配置文件
```



<img src="../AppData/Roaming/Typora/draftsRecover/notes/微信图片_20201005104720.png" style="zoom: 67%;" />

【nginx.conf的内容注解】

```shell
######Nginx配置文件nginx.conf中文详解#####

#定义Nginx运行的用户和用户组
user www www;

#nginx进程数，建议设置为等于CPU总核心数。
worker_processes 8;

#全局错误日志定义类型，[ debug | info | notice | warn | error | crit ]
error_log /usr/local/nginx/logs/error.log info;

#进程pid文件
pid /usr/local/nginx/logs/nginx.pid;

#指定进程可以打开的最大描述符：数目
#工作模式与连接数上限
#这个指令是指当一个nginx进程打开的最多文件描述符数目，理论值应该是最多打开文件数（ulimit -n）与nginx进程数相除，但是nginx分配请求并不是那么均匀，所以最好与ulimit -n 的值保持一致。
#现在在linux 2.6内核下开启文件打开数为65535，worker_rlimit_nofile就相应应该填写65535。
#这是因为nginx调度时分配请求到进程并不是那么的均衡，所以假如填写10240，总并发量达到3-4万时就有进程可能超过10240了，这时会返回502错误。
worker_rlimit_nofile 65535;


events
{
    #参考事件模型，use [ kqueue | rtsig | epoll | /dev/poll | select | poll ]; epoll模型
    #是Linux 2.6以上版本内核中的高性能网络I/O模型，linux建议epoll，如果跑在FreeBSD上面，就用kqueue模型。
    #补充说明：
    #与apache相类，nginx针对不同的操作系统，有不同的事件模型
    #A）标准事件模型
    #Select、poll属于标准事件模型，如果当前系统不存在更有效的方法，nginx会选择select或poll
    #B）高效事件模型
    #Kqueue：使用于FreeBSD 4.1+, OpenBSD 2.9+, NetBSD 2.0 和 MacOS X.使用双处理器的MacOS X系统使用kqueue可能会造成内核崩溃。
    #Epoll：使用于Linux内核2.6版本及以后的系统。
    #/dev/poll：使用于Solaris 7 11/99+，HP/UX 11.22+ (eventport)，IRIX 6.5.15+ 和 Tru64 UNIX 5.1A+。
    #Eventport：使用于Solaris 10。 为了防止出现内核崩溃的问题， 有必要安装安全补丁。
    use epoll;

    #单个进程最大连接数（最大连接数=连接数*进程数）
    #根据硬件调整，和前面工作进程配合起来用，尽量大，但是别把cpu跑到100%就行。每个进程允许的最多连接数，理论上每台nginx服务器的最大连接数为。
    worker_connections 65535;

    #keepalive超时时间。
    keepalive_timeout 60;

    #客户端请求头部的缓冲区大小。这个可以根据你的系统分页大小来设置，一般一个请求头的大小不会超过1k，不过由于一般系统分页都要大于1k，所以这里设置为分页大小。
    #分页大小可以用命令getconf PAGESIZE 取得。
    #[root@web001 ~]# getconf PAGESIZE
    #4096
    #但也有client_header_buffer_size超过4k的情况，但是client_header_buffer_size该值必须设置为“系统分页大小”的整倍数。
    client_header_buffer_size 4k;

    #这个将为打开文件指定缓存，默认是没有启用的，max指定缓存数量，建议和打开文件数一致，inactive是指经过多长时间文件没被请求后删除缓存。
    open_file_cache max=65535 inactive=60s;

    #这个是指多长时间检查一次缓存的有效信息。
    #语法:open_file_cache_valid time 默认值:open_file_cache_valid 60 使用字段:http, server, location 这个指令指定了何时需要检查open_file_cache中缓存项目的有效信息.
    open_file_cache_valid 80s;

    #open_file_cache指令中的inactive参数时间内文件的最少使用次数，如果超过这个数字，文件描述符一直是在缓存中打开的，如上例，如果有一个文件在inactive时间内一次没被使用，它将被移除。
    #语法:open_file_cache_min_uses number 默认值:open_file_cache_min_uses 1 使用字段:http, server, location  这个指令指定了在open_file_cache指令无效的参数中一定的时间范围内可以使用的最小文件数,如果使用更大的值,文件描述符在cache中总是打开状态.
    open_file_cache_min_uses 1;

    #语法:open_file_cache_errors on | off 默认值:open_file_cache_errors off 使用字段:http, server, location 这个指令指定是否在搜索一个文件是记录cache错误.
    open_file_cache_errors on;
}



#设定http服务器，利用它的反向代理功能提供负载均衡支持
http
{
    #文件扩展名与文件类型映射表
    include mime.types;

    #默认文件类型
    default_type application/octet-stream;

    #默认编码
    #charset utf-8;

    #服务器名字的hash表大小
    #保存服务器名字的hash表是由指令server_names_hash_max_size 和server_names_hash_bucket_size所控制的。参数hash bucket size总是等于hash表的大小，并且是一路处理器缓存大小的倍数。在减少了在内存中的存取次数后，使在处理器中加速查找hash表键值成为可能。如果hash bucket size等于一路处理器缓存的大小，那么在查找键的时候，最坏的情况下在内存中查找的次数为2。第一次是确定存储单元的地址，第二次是在存储单元中查找键 值。因此，如果Nginx给出需要增大hash max size 或 hash bucket size的提示，那么首要的是增大前一个参数的大小.
    server_names_hash_bucket_size 128;

    #客户端请求头部的缓冲区大小。这个可以根据你的系统分页大小来设置，一般一个请求的头部大小不会超过1k，不过由于一般系统分页都要大于1k，所以这里设置为分页大小。分页大小可以用命令getconf PAGESIZE取得。
    client_header_buffer_size 32k;

    #客户请求头缓冲大小。nginx默认会用client_header_buffer_size这个buffer来读取header值，如果header过大，它会使用large_client_header_buffers来读取。
    large_client_header_buffers 4 64k;

    #设定通过nginx上传文件的大小
    client_max_body_size 8m;

    #开启高效文件传输模式，sendfile指令指定nginx是否调用sendfile函数来输出文件，对于普通应用设为 on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络I/O处理速度，降低系统的负载。注意：如果图片显示不正常把这个改成off。
    #sendfile指令指定 nginx 是否调用sendfile 函数（zero copy 方式）来输出文件，对于普通应用，必须设为on。如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘与网络IO处理速度，降低系统uptime。
    sendfile on;

    #开启目录列表访问，合适下载服务器，默认关闭。
    autoindex on;

    #此选项允许或禁止使用socke的TCP_CORK的选项，此选项仅在使用sendfile的时候使用
    tcp_nopush on;

    tcp_nodelay on;

    #长连接超时时间，单位是秒
    keepalive_timeout 120;

    #FastCGI相关参数是为了改善网站的性能：减少资源占用，提高访问速度。下面参数看字面意思都能理解。
    fastcgi_connect_timeout 300;
    fastcgi_send_timeout 300;
    fastcgi_read_timeout 300;
    fastcgi_buffer_size 64k;
    fastcgi_buffers 4 64k;
    fastcgi_busy_buffers_size 128k;
    fastcgi_temp_file_write_size 128k;

    #gzip模块设置
    gzip on; #开启gzip压缩输出
    gzip_min_length 1k;    #最小压缩文件大小
    gzip_buffers 4 16k;    #压缩缓冲区
    gzip_http_version 1.0;    #压缩版本（默认1.1，前端如果是squid2.5请使用1.0）
    gzip_comp_level 2;    #压缩等级
    gzip_types text/plain application/x-javascript text/css application/xml;    #压缩类型，默认就已经包含textml，所以下面就不用再写了，写上去也不会有问题，但是会有一个warn。
    gzip_vary on;

    #开启限制IP连接数的时候需要使用
    #limit_zone crawler $binary_remote_addr 10m;



    #负载均衡配置
    upstream pythonav.cn {

        #upstream的负载均衡，weight是权重，可以根据机器配置定义权重。weigth参数表示权值，权值越高被分配到的几率越大。
        server 192.168.80.121:80 weight=3;
        server 192.168.80.122:80 weight=2;
        server 192.168.80.123:80 weight=3;

        #nginx的upstream目前支持4种方式的分配
        #1、轮询（默认）
        #每个请求按时间顺序逐一分配到不同的后端服务器，如果后端服务器down掉，能自动剔除。
        #2、weight
        #指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况。
        #例如：
        #upstream bakend {
        #    server 192.168.0.14 weight=10;
        #    server 192.168.0.15 weight=10;
        #}
        #2、ip_hash
        #每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题。
        #例如：
        #upstream bakend {
        #    ip_hash;
        #    server 192.168.0.14:88;
        #    server 192.168.0.15:80;
        #}
        #3、fair（第三方）
        #按后端服务器的响应时间来分配请求，响应时间短的优先分配。
        #upstream backend {
        #    server server1;
        #    server server2;
        #    fair;
        #}
        #4、url_hash（第三方）
        #按访问url的hash结果来分配请求，使每个url定向到同一个后端服务器，后端服务器为缓存时比较有效。
        #例：在upstream中加入hash语句，server语句中不能写入weight等其他的参数，hash_method是使用的hash算法
        #upstream backend {
        #    server squid1:3128;
        #    server squid2:3128;
        #    hash $request_uri;
        #    hash_method crc32;
        #}

        #tips:
        #upstream bakend{#定义负载均衡设备的Ip及设备状态}{
        #    ip_hash;
        #    server 127.0.0.1:9090 down;
        #    server 127.0.0.1:8080 weight=2;
        #    server 127.0.0.1:6060;
        #    server 127.0.0.1:7070 backup;
        #}
        #在需要使用负载均衡的server中增加 proxy_pass http://bakend/;

        #每个设备的状态设置为:
        #1.down表示单前的server暂时不参与负载
        #2.weight为weight越大，负载的权重就越大。
        #3.max_fails：允许请求失败的次数默认为1.当超过最大次数时，返回proxy_next_upstream模块定义的错误
        #4.fail_timeout:max_fails次失败后，暂停的时间。
        #5.backup： 其它所有的非backup机器down或者忙的时候，请求backup机器。所以这台机器压力会最轻。

        #nginx支持同时设置多组的负载均衡，用来给不用的server来使用。
        #client_body_in_file_only设置为On 可以讲client post过来的数据记录到文件中用来做debug
        #client_body_temp_path设置记录文件的目录 可以设置最多3层目录
        #location对URL进行匹配.可以进行重定向或者进行新的代理 负载均衡
    }



    #虚拟主机的配置
    server
    {
        #监听端口
        listen 80;

        #域名可以有多个，用空格隔开
        server_name www.w3cschool.cn w3cschool.cn;
        index index.html index.htm index.php;
        root /data/www/w3cschool;

        #对******进行负载均衡
        location ~ .*.(php|php5)?$
        {
            fastcgi_pass 127.0.0.1:9000;
            fastcgi_index index.php;
            include fastcgi.conf;
        }

        #图片缓存时间设置
        location ~ .*.(gif|jpg|jpeg|png|bmp|swf)$
        {
            expires 10d;
        }

        #JS和CSS缓存时间设置
        location ~ .*.(js|css)?$
        {
            expires 1h;
        }

        #日志格式设定
        #$remote_addr与$http_x_forwarded_for用以记录客户端的ip地址；
        #$remote_user：用来记录客户端用户名称；
        #$time_local： 用来记录访问时间与时区；
        #$request： 用来记录请求的url与http协议；
        #$status： 用来记录请求状态；成功是200，
        #$body_bytes_sent ：记录发送给客户端文件主体内容大小；
        #$http_referer：用来记录从那个页面链接访问过来的；
        #$http_user_agent：记录客户浏览器的相关信息；
        #通常web服务器放在反向代理的后面，这样就不能获取到客户的IP地址了，通过$remote_add拿到的IP地址是反向代理服务器的iP地址。反向代理服务器在转发请求的http头信息中，可以增加x_forwarded_for信息，用以记录原有客户端的IP地址和原来客户端的请求的服务器地址。
        log_format access '$remote_addr - $remote_user [$time_local] "$request" '
        '$status $body_bytes_sent "$http_referer" '
        '"$http_user_agent" $http_x_forwarded_for';

        #定义本虚拟主机的访问日志
        access_log  /usr/local/nginx/logs/host.access.log  main;
        access_log  /usr/local/nginx/logs/host.access.404.log  log404;

        #对 "/" 启用反向代理
        location / {
            proxy_pass http://127.0.0.1:88;
            proxy_redirect off;
            proxy_set_header X-Real-IP $remote_addr;

            #后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;

            #以下是一些反向代理的配置，可选。
            proxy_set_header Host $host;

            #允许客户端请求的最大单文件字节数
            client_max_body_size 10m;

            #缓冲区代理缓冲用户端请求的最大字节数，
            #如果把它设置为比较大的数值，例如256k，那么，无论使用firefox还是IE浏览器，来提交任意小于256k的图片，都很正常。如果注释该指令，使用默认的client_body_buffer_size设置，也就是操作系统页面大小的两倍，8k或者16k，问题就出现了。
            #无论使用firefox4.0还是IE8.0，提交一个比较大，200k左右的图片，都返回500 Internal Server Error错误
            client_body_buffer_size 128k;

            #表示使nginx阻止HTTP应答代码为400或者更高的应答。
            proxy_intercept_errors on;

            #后端服务器连接的超时时间_发起握手等候响应超时时间
            #nginx跟后端服务器连接超时时间(代理连接超时)
            proxy_connect_timeout 90;

            #后端服务器数据回传时间(代理发送超时)
            #后端服务器数据回传时间_就是在规定时间之内后端服务器必须传完所有的数据
            proxy_send_timeout 90;

            #连接成功后，后端服务器响应时间(代理接收超时)
            #连接成功后_等候后端服务器响应时间_其实已经进入后端的排队之中等候处理（也可以说是后端服务器处理请求的时间）
            proxy_read_timeout 90;

            #设置代理服务器（nginx）保存用户头信息的缓冲区大小
            #设置从被代理服务器读取的第一部分应答的缓冲区大小，通常情况下这部分应答中包含一个小的应答头，默认情况下这个值的大小为指令proxy_buffers中指定的一个缓冲区的大小，不过可以将其设置为更小
            proxy_buffer_size 4k;

            #proxy_buffers缓冲区，网页平均在32k以下的设置
            #设置用于读取应答（来自被代理服务器）的缓冲区数目和大小，默认情况也为分页大小，根据操作系统的不同可能是4k或者8k
            proxy_buffers 4 32k;

            #高负荷下缓冲大小（proxy_buffers*2）
            proxy_busy_buffers_size 64k;

            #设置在写入proxy_temp_path时数据的大小，预防一个工作进程在传递文件时阻塞太长
            #设定缓存文件夹大小，大于这个值，将从upstream服务器传
            proxy_temp_file_write_size 64k;
        }


        #设定查看Nginx状态的地址
        location /NginxStatus {
            stub_status on;
            access_log on;
            auth_basic "NginxStatus";
            auth_basic_user_file confpasswd;
            #htpasswd文件的内容可以用apache提供的htpasswd工具来产生。
        }

        #本地动静分离反向代理配置
        #所有jsp的页面均交由tomcat或resin处理
        location ~ .(jsp|jspx|do)?$ {
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_pass http://127.0.0.1:8080;
        }

        #所有静态文件由nginx直接读取不经过tomcat或resin
        location ~ .*.(htm|html|gif|jpg|jpeg|png|bmp|swf|ioc|rar|zip|txt|flv|mid|doc|ppt|
        pdf|xls|mp3|wma)$
        {
            expires 15d; 
        }

        location ~ .*.(js|css)?$
        {
            expires 1h;
        }
    }
}
######Nginx配置文件nginx.conf中文详解#####

nginx.conf详解
```

**nginx.conf重要的指令块**

核心功能都在于http{}指令块里，http{}块还包含了以下指令

- server{} 指令块 ，对应一个站点配置，反向代理，静态资源站点
- location{} ，对应一个url
- upstream{} ，定义上游服务，负载均衡池

#### **5.9nginx的命令行指令**

- 启停指令
  - nginx -s stop
  - nginx -s reload
  - nginx 首次输入表示启动
- nginx帮助指令

```shell
[root@chaogelinux nginx114]# nginx  -h
nginx version: nginx/1.14.0
Usage: nginx [-?hvVtTq] [-s signal] [-c filename] [-p prefix] [-g directives]

Options:
  -?,-h         : this help   #帮助信息
  -v            : show version and exit        #显示版本
  -V            : show version and configure options then exit #显示编译信息与版本
  -t            : test configuration and exit    #测试配置文件语法
  -T            : test configuration, dump it and exit    #测试语法且输出内容
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload #发送信号，stop立即停止，quit优雅停止，reload重读配置文件，reopen重新记录日志
  -p prefix     : set prefix path (default: /home/Learn_Nginx/nginx114//)
  -c filename   : set configuration file (default: conf/nginx.conf) #使用指定配置文件
  -g directives : set global directives out of configuration file #覆盖默认参数
```



【命令行功能案例】

配置文件重读

在nginx正在运行时，如实修改了nginx.conf或是新增了一些功能配置，让其生效，可能需要重启整个nginx进程、

但是你不能保证某个时间段没有用户在访问nginx，重启会断开用户的连接，造成莫名的故障

因此nginx提供了reload重载功能，不停止服务，更新配置文件

```shell
1.修改nginx.conf
worker_processes  3; #定义nginx工作进程数的

2.重载配置文件
nginx -s reload

3.检查linux进程
[root@chaogelinux nginx114]# vim conf/nginx.conf
[root@chaogelinux nginx114]#
[root@chaogelinux nginx114]# ps -ef|grep nginx
root      6191     1  0 10:33 ?        00:00:00 nginx: master process nginx
nobody    6213  6191  0 10:33 ?        00:00:00 nginx: worker process
nobody    6214  6191  0 10:33 ?        00:00:00 nginx: worker process
nobody    6215  6191  0 10:33 ?        00:00:00 nginx: worker process
root      6345  5283  0 10:38 pts/0    00:00:00 grep --color=auto nginx
```



#### 5.1nginx的信号传递

nginx进程信号传递如图

<img src="notes/image-20200211145447382.png" style="zoom:33%;" />

```shell
1.master不处理请求，而是分配worker进程，负责重启，热部署，重载等功能。
2.master根据worker_processes 定义开始的workers数量
3.worker运行后，master处于挂起状态，等待信号
4.可以发送kill，或者nginx -s 参数发出信号
```

【信号集】



| nginx -s 对应参数 | 信号  | 含义                                      | English                                                      |
| :---------------- | :---- | :---------------------------------------- | :----------------------------------------------------------- |
| stop              | TERM  | 强制关闭整个服务                          | Shut down quickly.                                           |
| null              | INT   | 强制关闭整个服务                          | Shut down quickly.                                           |
| quit              | QUIT  | 优雅地关闭整个服务                        | Shut down gracefully.                                        |
| reopen            | USR1  | 重新打开日志记录                          | Reopen log files.                                            |
| reload            | HUP   | 重新读取配置文件,并且优雅地退出老的worker | Reload configuration, start the new worker process with a new configuration, and gracefully shut down old worker processes. |
| null              | USR2  | 平滑升级到新版本                          | Upgrade the nginx executable on the fly.                     |
| null              | WINCH | 优雅地关闭worker(在热更新的时候必用)      | Shut down worker processes gracefully.                       |

#### **5.11nginx的热部署功能**

- nginx作为一个优秀的反向代理服务器，同时具备高可用的特性，Nginx也支持热部署。
- 热部署指的是`在不重启或关闭进程情况下，新应用直接替换掉旧的应用

【思路流程】

```shell
热部署大致流程
1.备份旧的二进制文件
2.编译安装新的二进制文件，覆盖旧的二进制文件
3.发送USR2信号给旧master进程
4.发送WINCH信号给旧master进程
5.发送QUIT信号给旧master进程
```

【环境准备】

```shell
环境准备：
1.旧nginx版本 nginx version: nginx/1.18.0
2.新nginx版本 nginx version: nginx/1.19.3
```

【nginx热部署】

nginx工作模式是master-worker（包工头-工人）

刚才所说的nginx支持`reload重载`仅仅是nginx的master进程，检查配置文件语法是否正确，错则返回错误、正确也`不会改变`已经建立连接的worker，只得等待worker处理完毕请求之后，`杀死旧配置文件的worker，启动新配置文件的worker`。

但是Nginx这里提提供了热部署功能，就是在`不影响用户体验下，进行软件版本升级`，也就是不主动杀死worker，替换软件的二进制文件。

```shell
1.目前运行的是旧的nginx版本，如下检查
[root@localhost ~]# ps -ef|grep nginx
root       1254      1  0 11:57 ?        00:00:00 nginx: master process nginx
syh        1273   1254  0 12:01 ?        00:00:00 nginx: worker process
root       6970   6950  0 14:51 pts/1    00:00:00 grep --color=auto nginx
2.通过curl命令故意访问一个错的url、用于返回nginx的版本西悉尼
[root@localhost ~]# curl 192.168.37.134/220
<html>
<head><title>404 Not Found</title></head>
<body>
<center><h1>404 Not Found</h1></center>
<hr><center>nginx/1.18.0</center>								#得到版本号为1.18.0
</body>
</html>
```

```shell
1.备份旧版本的nginx二进制文件
cd/opt/nginx1.18/sbin
[root@localhost sbin]# mv ./nginx  old_nginx118
[root@localhost sbin]# ls
old_nginx118

2.检查旧版本nginx的编译参数

[root@localhost sbin]# old_nginx118 -V								#发现旧的nginx安装了以下功能模块、保持新的nginx也一样安装这些模块
nginx version: nginx/1.18.0
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC) 
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled
configure arguments: --prefix=/opt/nginx1.18/ --with-http_ssl_module --with-http_flv_module --with-http_gzip_static_module --with-http_stub_status_module --with-threads --with-file-aio


3.编译安装新版本

#下载新版本的nginx源码包
cd /opt
[root@localhost opt]# wget http://nginx.org/download/nginx-1.19.3.tar.gz

#解压新的源码包
[root@localhost opt]# tar -zxvf nginx-1.19.3.tar.gz
[root@localhost opt]# ls -l
总用量 2044
drwxr-xr-x 11 root root     151 10月  4 19:24 nginx1.18
drwxr-xr-x  9 1001 1001     186 10月  4 19:07 nginx-1.18.0
-rw-r--r--  1 root root 1039530 4月  21 22:33 nginx-1.18.0.tar.gz
drwxr-xr-x  8 1001 1001     158 9月  29 22:32 nginx-1.19.3						#解压出来的目录
-rw-r--r--  1 root root 1052581 9月  29 22:39 nginx-1.19.3.tar.gz

#开始编译
[root@localhost opt]# cd nginx-1.19.3
[root@localhost nginx-1.19.3]# ./configure --prefix=/opt/nginx1.18/ --with-http_ssl_module --with-http_flv_module --with-http_gzip_static_module --with-http_stub_status_module --with-threads --with-file-aio

#编译安装
[root@localhost nginx-1.19.3]# make && make install

4.查看旧的安装目录、发现有两个版本的nginx

[root@localhost sbin]# ls
nginx  old_nginx118

5.检查nginx的进程状态
注意这里的PID和PPID（pid是当前进程的id号，ppid是启动该进程的pid，也就是父ID，可知该pid由谁启动）
[root@localhost sbin]# ps -ef|grep nginx
		   pid   ppid
root       9760      1  0 15:32 ?        00:00:00 nginx: master process /opt/nginx1.18/sbin/old_nginx118
syh        9761   9760  0 15:32 ?        00:00:00 nginx: worker process
root       9763   6950  0 15:32 pts/1    00:00:00 grep --color=auto nginx


6.发送USR2信号给旧版本主进程，使得nginx旧版本停止接收请求，切换为新nginx版本
[root@localhost sbin]# kill -USR2 `cat ../logs/nginx.pid`
注：这里如果没有生成新的主进程、需要kill掉所有的nginx进程、再以绝对路径的形式重启旧的进程就会生成新的mast进程
[root@localhost sbin]# pkill -9 nginx
[root@localhost sbin]# /opt/nginx1.18/sbin/old_nginx118 

7.再次检查nginx的进程状态
[root@localhost sbin]# ps -ef|grep nginx
nginx-master首先会重命名pid文件，在文件后面添加.oldbin后缀
然后会再启动一个新的master进程以及worker，且使用的是新版Nginx 
nginx能够自动将新来的请求，过度到新版master进程下，实现平滑过度

#可以发现新的master进程由旧master启动，由PPID可看出
root       9760      1  0 15:32 ?        00:00:00 nginx: master process /opt/nginx1.18/sbin/old_nginx118
syh        9761   9760  0 15:32 ?        00:00:00 nginx: worker process
root       9765   9760  0 15:33 ?        00:00:00 nginx: master process /opt/nginx1.18/sbin/old_nginx118
syh        9766   9765  0 15:33 ?        00:00:00 nginx: worker process
root       9768   6950  0 15:33 pts/1    00:00:00 grep --color=auto nginx

[root@localhost sbin]# ls ../logs/
access.log        error.log         nginx.pid         nginx.pid.oldbin  

8.发送WINCH信号给旧master进程，优雅的关闭旧worker进程
[root@localhost sbin]# kill -WINCH `cat ../logs/nginx.pid.oldbin`

9.再次检查nginx的进程、旧master的worker已经关闭了，旧master不会自己退出，用作版本回退
[root@localhost sbin]# ps -ef|grep nginx
root       9760      1  0 15:32 ?        00:00:00 nginx: master process /opt/nginx1.18/sbin/old_nginx118
root       9765   9760  0 15:33 ?        00:00:00 nginx: master process /opt/nginx1.18/sbin/old_nginx118
syh        9766   9765  0 15:33 ?        00:00:00 nginx: worker process
root       9774   6950  0 15:45 pts/1    00:00:00 grep --color=auto ngin

10.可手动杀死原来的master进程
[root@localhost sbin]# kill -9 9760
[root@localhost sbin]# ps -ef|grep nginx
root       9765      1  0 15:33 ?        00:00:00 nginx: master process /opt/nginx1.18/sbin/old_nginx118
syh        9766   9765  0 15:33 ?        00:00:00 nginx: worker process
root       9777   6950  0 15:47 pts/1    00:00:00 grep --color=auto nginx

11.可查看新的进程版本
[root@localhost sbin]# nginx -v
nginx version: nginx/1.19.3
```





#### 5.12**日志的切割**

日志切割是线上很常见的操作，控制单个文件大小，便于管理日志

```shell
1.查看nginx的日志
[root@localhost /]# ls /opt/nginx1.18/logs/
access.log        error.log         nginx.pid         nginx.pid.oldbin 

#大致查看日志内容
tail -f  access.log
192.168.37.134 - - [05/Oct/2020:14:52:50 +0800] "GET /220 HTTP/1.1" 404 153 "-" "curl/7.29.0"
192.168.37.1 - - [05/Oct/2020:15:48:18 +0800] "GET / HTTP/1.1" 200 612 "-" "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36"


2.给文件重命名
[root@localhost logs]# mv access.log  access_$(date +"%Y%m%d-%H%M%S").log
[root@localhost logs]# ls
access_20201005-193548.log  error.log  nginx.pid  nginx.pid.oldbin

3.发送USR1信号给nginx-master,重新打开日志记录，生成新的access日志文件
[root@localhost logs]# nginx -s reopen									#等同于 Kill -USR1 nginx.pid 
[root@localhost logs]# ls
access_20201005-193548.log  access.log  error.log  nginx.pid  nginx.pid.oldbin

4.注意，在以上的nginx重命名日志切割，不要着急立即对文件修改，且要sleep 等待1秒
由于nginx的工作模式，master下发指令给worker只是做了标记，当业务量大的时候，这个修改操作可能会慢一点，不会理解生效

5.在生产环境下，主要以crontab形式，执行cut_nginx_log.sh脚本的

#创建脚本文件、并写入内容
[root@localhost logs]# mkdir -p /nginx_scripts
[root@localhost logs]# vim /nginx_scripts/cut_nginx_log.sh
#!/bin/bash
logs_path="/opt/nginx1.18/logs/"
mkdir -p ${logs_path}$(date +"%Y")/$(date +"%m")/$(date +"%d")		#创建多级目录
mv ${logs_path}access.log ${logs_path}$(date +"%Y")/$(date +"%m")/$(date +"%d")/access_$(date +"%Y-%m-%d-%H").log	#改名
nginx -s reopen		#重新打开生成新的access日志文件



6.执行脚本、看是否有误
[root@localhost logs]# bash /nginx_scripts/cut_nginx_log.sh 
[root@localhost logs]# bash /nginx_scripts/cut_nginx_log.sh
[root@localhost logs]# tree
.
├── 2020
│   └── 10
│       └── 05
│           └── access_2020-10-05-20.log
├── access.log
├── error.log
├── nginx.pid
└── nginx.pid.oldbin


7.写入定时任务

[root@localhost nginx_scripts]# crontab  -e
[root@localhost nginx_scripts]# crontab  -l
0 0 * * * /bin/bash /nginx_scripts/cut_nginx_log.sh

```



#### **5.13nginx静态资源站点搭建**

【**nginx虚拟主机**】

- 虚拟主机指的就是一个独立的站点，具有独立的域名，有完整的www服务，例如网站、FTP、邮件等。
- Nginx支持多虚拟主机，在一台机器上可以运行完全独立的多个站点。

<img src="notes/image-20200211162644884.png" style="zoom: 50%;" />

- 一些草根流量站长，常会搭建个人站点进行资源分享交流，并且可能有多个不同业务的站点，如果每台服务器只运行一个网站，那么将造成资源浪费，成本浪费。
- 利用虚拟主机的功能，就不用为了运行一个网站而单独配置一个Nginx服务器，或是单独再运行一组Nginx进程。
- 虚拟主机可以在一台服务器，同一个Nginx进程上运行多个网站。

nginx.conf主配置文件中，最简单的一段虚拟主机配置如下

```shell
http{

    server {
            #监听端口
            listen 80;
            #填写域名匹配
            server_name  localhost;
            #访问日志
            access_log  logs/host.access.log  main;
            #url匹配
            location / {
                        root   html;
            index  index.html index.htm;
            }
    }
}
```

【搭建一个静态网站】

```shell
准备好资源文件目录，内容自定义
[root@localhost html]# ls /www/html/
index.html  1.jpg


1.修改nginx.conf
server {
          listen       80;
          server_name  localhost;
          #默认编码
          charset utf-8;
          #日志功能开启
          access_log  logs/host.access.log  main;
        location / {
              #定义虚拟主机的资源目录,
            root   /www/html/;
            #定义首页文件的名字
            index  index.html index.htm;
        }

      }

2.重载nginx配置文件
nginx -s reload
```

访问首页

```
http://192.168.37.134/index.html
简写
http://192.168.37.134/
```

![](notes/微信图片_20201006111257.png)

访问静态图片

```
http://192.168.37.134/1.jpg
```

<img src="1.jpg" style="zoom:33%;" />

访问动态图片

```
http://192.168.37.134/52f862debc6341429d8e96f57f36eb68.gif
```

<img src="notes/微信图片_20201006112534.png" style="zoom:33%;" />



#### **5.14nginx的静态资源压缩**

nginx支持gzip对资源压缩传输，经过gzip压缩后的页面大小可以为原本的30%甚至更小，用户浏览体验会快很多。

```
准备好资源文件
-rw-r--r-- 1 root root 140K 10月 11 2019 1.jpg
-rw-r--r-- 1 root root  64K 10月  6 11:22 2.gif
-rw-r--r-- 1 root root 1.1M 8月  10 2019 52f862debc6341429d8e96f57f36eb68.gif
-rw-r--r-- 1 root root   13 10月  6 11:07 index.html
-rw-r--r-- 1 root root 4.6M 10月  6 12:11 test.txt
```

未开启gzip功能进行访问

![](notes/微信图片_20201006135556.png)



开启gzip压缩功能

```shell
nginx.conf开启gzip压缩功能，添加如下语句，针对静态资源压缩
server {



        gzip on;
        gzip_http_version 1.1;
        gzip_comp_level 4;
        gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php image1/jpeg image1/gif image1/png;
}
#重载nginx
nginx -s reload
```

再次访问

<img src="notes/微信图片_20201006135022.png"  />

结果：开启了gzip压缩后，整体的传输资源大小，以及相应速度，都大幅度提高了

#### **5.15nginx基于IP的多虚拟主机**

Linux操作系统都能够支持给网卡绑定多个IP地址，可以使得一块网卡上运行多个基于IP的虚拟主机。

【环境准备】

```shell
1.添加2个ip别名
[root@localhost html]# ifconfig ens33:1 192.168.37.140 netmask 255.255.255.0 broadcast 192.168.37.255 up
[root@localhost html]# ifconfig ens33:2 192.168.37.141 netmask 255.255.255.0 broadcast 192.168.37.255 up

[root@localhost html]# ifconfig |grep "inet 192"
        inet 192.168.37.134  netmask 255.255.255.0  broadcast 192.168.37.255
        inet 192.168.37.140  netmask 255.255.255.0  broadcast 192.168.37.255
        inet 192.168.37.141  netmask 255.255.255.0  broadcast 192.168.37.255
        
#确保2个ip可通信即可
[root@localhost html]# ping 192.168.37.140
PING 192.168.37.140 (192.168.37.140) 56(84) bytes of data.
64 bytes from 192.168.37.140: icmp_seq=1 ttl=64 time=0.012 ms

[root@localhost html]# ping 192.168.37.141
PING 192.168.37.141 (192.168.37.141) 56(84) bytes of data.
64 bytes from 192.168.37.141: icmp_seq=1 ttl=64 time=0.013 ms
```

以上绑定IP 地址为临时性修改、请参考连接做永久性修改https://www.cnblogs.com/configure/p/6531313.html

【配置三个主机的配置文件】

```shell
思路流程：
1.将192.168.37.134配置文件放在主配置文件nginx.conf中；
2.将192.168.37.140和192.168.37.141的配置文件放在/opt/nginx1.18/conf/syh中；
3.分别编写好三个主机的配置文件内容；
4.准备三个主机的访问内容数据；
5.重读配置文件
6.访问测试三个网站
```

```shell
1.编写第一个主机的配置文件、内容如下：
[root@localhost syh]# vim /opt/nginx1.18/conf/nginx.conf
注意在http{}标签中的最后一行添加、表示能够读取conf/syh目录下的配置文件
http{


include syh/*.conf;
}

    server {
      #监听的端口和ip
        listen       192.168.37.134:80;
       #主机域名,_为无域名
        server_name  _;
        #编码格式1定义
        charset utf-8;
        #access_log  logs/host.access.log  main;
        location / {
        #HTML文件存放的目录
            root   /www/134;
        #默认首页文件，从左往右寻找，index.html或是index.htm文件
            index  index.html index.htm;
        }
     }
2.编写第二个主机的配置文件、内容如下：

server {
        listen       192.168.37.140:80;
        server_name  _;
        location / {
            root   /www/140;
            index  index.html index.htm;
        }
}

3.编写第三个主机的配置文件、内容如下：
server {
        listen       192.168.37.141:80;
        server_name  _;
        charset utf-8;
        location / {
            root   /www/141;
            index  index.html index.htm;
	}
}

4.创建三个站点文件夹
[root@localhost syh]# mkdir -p /www/{134,140,141}

5.创建三个站点的网页文件、并写如数据

[root@localhost www]# echo " I am 134,hello">>134/index.html
[root@localhost www]# echo " I am 140,hello">>140/index.html
[root@localhost www]# echo " I am 141,hello">>141/index.html

6.最后确保修改好所有配置文件、检车无误后再重读配置文件
[root@localhost www]# nginx -t
nginx: the configuration file /opt/nginx1.18//conf/nginx.conf syntax is ok
nginx: configuration file /opt/nginx1.18//conf/nginx.conf test is successful

[root@localhost www]# nginx -s reload


7.测试访问三个网站站点
[root@localhost www]# nginx -s reload
[root@localhost www]# curl 192.168.37.134
 I am 134,hello
[root@localhost www]# curl 192.168.37.140
 I am 140,hello
[root@localhost www]# curl 192.168.37.141
 I am 141,hello

```

浏览器访问测试

![](notes/微信图片_20201006151608.png)

![](notes/微信图片_20201006151613.png)

![](notes/微信图片_20201006151615.png)



#### **5.16nginx基于域名的多虚拟主机**

- 基于多IP的虚拟主机可能会造成IP地址不足的问题，如果没有特殊需求，更常用的是基于多域名的形式。
- 只需要你单独配置DNS服务器，将主机名对应到正确的IP地址，修改Nginx配置，可以识别到不同的主机即可，这样就可以使得多个虚拟主机用同一个IP，解决了IP不足的隐患。

【准备工作】

```shell
1.在本地客户端hosts文件中，添加对应的解析记录，由于测试使用；客户端可以是windows、linux、mac

#在linux客户端的hosts文件添加ip 绑定域名
[root@localhost ~]# vim /etc/hosts
192.168.132.134 nginx1.com
192.168.132.134 nginx2.com
192.168.132.134 nginx3.com  

2.准备三个站点的工作目录、及页面文件
[root@localhost www]# mkdir nginx{1..3}
[root@localhost www]# echo "i am nginx1"> nginx1/index.html
[root@localhost www]# echo "i am nginx2"> nginx2/index.html
[root@localhost www]# echo "i am nginx3"> nginx3/index.html

```



【修改主配置文件ngixn.conf】

```shell
1.第一个server{}指令块的配置
[root@localhost www]# vim /opt/nginx1.18/conf/nginx.conf

    server {
    #监听的端口和ip
        listen      80;
          #主机域名
        server_name  www.syhnginx1.com;
        charset utf-8;
        #access_log  logs/host.access.log  main;
         #url匹配
        location / {
         #HTML文件存放的目录
            root   /www/nginx1;
            #默认首页文件，从左往右寻找，index.html或是index.htm文件
            index  index.html index.htm;
        }
    }
2.第二个server{}的配置
[root@localhost syh]# vim nginx2.conf 
server {
        listen       80;
        server_name  www.syhnginx2.com;
        location / {
            root   /www/nginx2;
            index  index.html index.htm;
        }
}


3.第三个server{}的配置
[root@localhost syh]# vim nginx3.conf
server {
        listen       80;
        server_name  www.syhnginx3.com;
        charset utf-8;
        location / {
            root   /www/nginx3;
            index  index.html index.htm;
        }
}

4.检查nginx配置文件语法是否有误
[root@localhost syh]# nginx -t
nginx: the configuration file /opt/nginx1.18//conf/nginx.conf syntax is ok
nginx: configuration file /opt/nginx1.18//conf/nginx.conf test is successful

5.重启nginx
[root@localhost syh]# nginx -s stop
[root@localhost syh]# nginx

```

测试访问多域名网站

```shell
[root@localhost ~]# curl www.syhnginx3.com
i am nginx3
[root@localhost ~]# curl www.syhnginx2.com
i am nginx2
[root@localhost ~]# curl www.syhnginx1.com
i am nginx1
```



#### **5.17nginx基于端口的多虚拟主机**

基于端口的配置在生产环境比较少见，用于特殊场景，例如公司内部测试平台网站，使用特殊端口的后台，OA系统、网站后台，CRM后台等。

案例：运行基于80、81、82端口的虚拟主机运行

【环境准备】

```shell
1.准备三个站点的工作目录、及页面文件
[root@localhost www]# mkdir nginx{1..3}
[root@localhost www]# echo "my port is 80"> nginx1/index.html
[root@localhost www]# echo "my port is 81"> nginx2/index.html
[root@localhost www]# echo "my port is 82> nginx3/index.html
```

三个网站的配置文件配置如下

```shell
1.第一个注解配置如下

    server {
        listen      80;
        server_name  www.syhnginx1.com;

        charset utf-8;

        #access_log  logs/host.access.log  main;

        location / {
            root   /www/nginx1;
            index  index.html index.htm;
        }
}

2.第二个主机配置如下
server {
        listen       81;
        server_name www.syhnginx2.com;
        location / {
            root   /www/nginx2;
            index  index.html index.htm;
        }
}

3.第三个主机配置如下
server {
        listen       82;
        server_name  www.syhnginx3.com;
        charset utf-8;
        location / {
            root   /www/nginx3;
            index  index.html index.htm;
        }
}

4.检测nginx配置语法
[root@localhost www]# nginx -t
nginx: the configuration file /opt/nginx1.18//conf/nginx.conf syntax is ok
nginx: configuration file /opt/nginx1.18//conf/nginx.conf test is successful

5.重读nginx配置文件
[root@localhost www]# nginx -s reload
```

三个不同的端口网站测试

```shell
[root@localhost ~]# curl 192.168.37.134:82
my port is 82
[root@localhost ~]# curl 192.168.37.134:81
my port is 81
[root@localhost ~]# curl 192.168.37.134:80
my port is 80
```





#### **5.18nginx的访客日志功能**

- 日志对于程序员很重要，可用于问题排错，记录程序运行状态，一个好的日志能够给与精确的问题定位。
- Nginx日志功能需要在nginx.conf中打开相关指令`log_format`，设置日志格式，以及设置日志的存储位置`access_log`，指定日志的格式，路径，缓存大小。

```shell
nginx.conf中有关访客日志定义如下
 
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';

 access_log  logs/access.log  main;

参数解释 
$remote_addr ：记录访问网站的客户端地址
$remote_user ：记录远程客户端用户名称
$time_local ：记录访问时间与时区
$request ：记录用户的 http 请求起始行信息
$status ：记录 http 状态码，即请求返回的状态，例如 200 、404 、502 等
$body_bytes_sent ：记录服务器发送给客户端的响应 body 字节数
$http_referer ：记录此次请求是从哪个链接访问过来的，可以根据 referer 进行防盗链设置
$http_user_agent ：记录客户端访问信息，如浏览器、手机客户端等
$http_x_forwarded_for ：当前端有代理服务器时，设置 Web 节点记录客户端地址的配置，此参数生效的前提是代理服务器上也进行了相关的 x_forwarded_for 设置
```

查看日志格式

```
tail -2 logs/access.log

192.168.178.1 - - [11/Feb/2020:19:24:37 +0800] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.130 Safari/537.36"
127.0.0.1 - - [12/Feb/2020:10:26:26 +0800] "GET / HTTP/1.1" 200 65 "-" "curl/7.29.0"
```

【**多虚拟主机定义日志**】

由于Nginx支持多虚拟主机，日志功能也是可以区分开的，用`access_log`定义存储位置。

日志指令语法

```shell
1.要将日志的格式化功能打开（日志格式化功能可卸载全局也可卸载局部）
http{
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                     '"$http_user_agent" "$http_x_forwarded_for"';
}


2.第一个虚拟主机的日志功能
    server {
        listen      80;
        server_name  www.syhnginx1.com;
        charset utf-8;
        access_log  logs/www.syhnginx1.log  main;
        location / {
            root   /www/nginx1;
            index  index.html index.htm;
        }
}
3.第二个主机的日志功能

server {
        listen       81;
        server_name www.syhnginx2.com;
        access_log  logs/www.syhnginx2.log  main;
        location / {
            root   /www/nginx2;
            index  index.html index.htm;
        }
}

4.第二个主机的日志功能

server {
        listen       82;
        server_name  www.syhnginx3.com;
        access_log  logs/www.syhnginx3.log  main;
        charset utf-8;
        location / {
            root   /www/nginx3;
            index  index.html index.htm;
        }
}

5.检车nainx的配置语法是否有误
[root@localhost logs]# nginx -t
nginx: the configuration file /opt/nginx1.18//conf/nginx.conf syntax is ok
nginx: configuration file /opt/nginx1.18//conf/nginx.conf test is successful

6.重载nginx
[root@localhost logs]# nginx -s reload

7.分别在三个服务器发送请求、检测日志动态
[root@localhost logs]# curl 192.168.37.134:80
my port is 80
[root@localhost logs]# curl 192.168.37.134:81
my port is 81
[root@localhost logs]# curl 192.168.37.134:82
my port is 82

[root@localhost logs]# tail -f /opt/nginx1.18/logs/www.syhnginx1.log 
192.168.37.134 - - [06/Oct/2020:20:40:40 +0800] "GET / HTTP/1.1" 200 14 "-" "curl/7.29.0" "-"

[root@localhost logs]# tail -f /opt/nginx1.18/logs/www.syhnginx2.log 
192.168.37.134 - - [06/Oct/2020:20:40:43 +0800] "GET / HTTP/1.1" 200 14 "-" "curl/7.29.0" "-"

[root@localhost logs]# tail -f /opt/nginx1.18/logs/www.syhnginx3.log 
192.168.37.134 - - [06/Oct/2020:20:40:46 +0800] "GET / HTTP/1.1" 200 14 "-" "curl/7.29.0" "-"

```

#### **5.19nginx的目录浏览功能**

例如将你搭建的网站资源共享到网页展示

【环境准备】

```shell
#在站点目录下创建几个文件
[root@localhost nginx1]# ls
haha.txt  index.htm  meimei.sh  ququ.py

```



【修改主机的文件配置】

```shell
    server {
        #监听的端口和ip
        listen      80;
        #主机域名
        server_name  www.syhnginx1.com;
          #目录有中文的时候，这里必须改
        charset utf-8;
          #服务器日志功能路径指定
        access_log  logs/www.syhnginx1.log  main;
           #url匹配
        location / {
        #需要列出目录索引的位置
            root   /www/nginx1;
            #index  index.html index.htm;
           # 目录浏览功能开启
            autoindex on;
        }
        
        
  #注意：/www/nginx1下不能有以html结尾的文件、否则默认访问html页面

```

访问页面

![](notes/微信图片_20201007114031.png)

#### **5.20nginx的状态页功能**

Nginx状态信息（status）配置及信息详解 nginx与php-fpm一样内建了一个状态页，对于想了解nginx的状态以及监控nginx非常有帮助。为了后续的zabbix监控，我们需要先了解一下nginx的状态页。

Nginx状态信息（status）介绍 Nginx软件在编译时又一个with-http_stub_status_module模块，这个模块功能是记录Nginx的基本访问状态信息，让使用者了解Nginx的工作状态。 要想使用状态模块，在编译时必须增加--with-http_stub_status_module参数。

```SHELL
检查Nginx是否开启此功能
[root@localhost ~]# nginx -v
nginx version: nginx/1.19.3
[root@localhost ~]# nginx -V
nginx version: nginx/1.19.3
built by gcc 4.8.5 20150623 (Red Hat 4.8.5-39) (GCC) 
built with OpenSSL 1.0.2k-fips  26 Jan 2017
TLS SNI support enabled
configure arguments: --prefix=/opt/nginx1.18/ --with-http_ssl_module --with-http_flv_module --with-http_gzip_static_module --with-http_stub_status_module --with-threads --with-file-aio

```

【修改配置文件、新增状态页功能】

```shell
1.确保主配配置文件存在include语法
http{
...
include syh/*.conf;
}

2.在syh目录下创建配置文件status.conf
  server {
  listen 85;
  location / {
         stub_status on;
         access_log off;
}
}


3.重载配置文件
[root@localhost syh]# nginx -s reload
```

使用ab命令，进行nginx压力测试

```shell
yum -y install httpd-tools

ab -kc 1000 -n 100000 http://127.0.0.1/  #开启会话保持，1000个并发，发送十万个请求

-n requests #执行的请求数，即一共发起多少请求。
-c concurrency #请求并发数。
-k #启用HTTP KeepAlive功能，即在一个HTTP会话中执行多个请求。
```



浏览器访问：192.168.37.134：85、返回如下信息

```shell
Active connections: 2 
server accepts handled requests
 100199 100199 100021 
Reading: 0 Writing: 1 Waiting: 1 


#参数解释
Active connections：正在处理的活动连接数
server:	nginx启动后一共处理的请求数
accepts : nginx启动后创建的握手次数
handled requests:表示总共处理了多少次请求
Reading: nginx读取客户端的header信息数
Writing: nginx返回给客户端的header信息数
Waiting: nginx处理完请求等待下一次请求的驻留连接数、在开启keepalive情况下；此值等于=active-（reading+writing）
```

#### **5.21nginx的错误日志功能**

Nginx能够将自身运行故障的信息也写入到指定的日志文件中。对于错误信息的调试，是维护Nginx的重要手段，指令是`error_log`，可以放在http{}全局中，也可以单独为虚拟主机记录。

```shell
语法：
error_log file  level;

日志级别在乎debug|info|notice|warn|error|crit|alert|emerg
级别越高，日志记录越少，生产常用模式是warn|error|crit级别
日志的记录，会给服务器增加额外大量的IO消耗，按需修改
```

nginx.conf修改如下，针对虚拟主机添加错误日志

```shell
1.编辑一个虚拟主机的配置
server {
        listen       81;
        server_name www.syhnginx2.com;
        access_log  logs/www.syhnginx2.log  main;
        #指定错误日志的路径
        error_log logs/www.syhnginx2_error.log;
        location / {
            root   /www/nginx2;
            index  index.html index.htm;
        }
}

2.重载配置文件
[root@localhost syh]# nginx -s reload

3.查看错误日志
[root@localhost logs]# ll
总用量 18900
drwxr-xr-x 3 root root       16 10月  5 20:04 2020
-rw-r--r-- 1 syh  root    13575 10月  6 20:01 access.log
-rw-r--r-- 1 syh  root    15249 10月  7 14:55 error.log
-rw-r--r-- 1 root root        5 10月  7 14:15 nginx.pid
-rw-r--r-- 1 root root        5 10月  5 15:32 nginx.pid.oldbin
-rw-r--r-- 1 root root 19301075 10月  7 14:16 www.syhnginx1.log
-rw-r--r-- 1 root root        0 10月  7 14:55 www.syhnginx2_error.log			#生成一个刚才定义好的错误日志
-rw-r--r-- 1 root root      850 10月  6 20:40 www.syhnginx2.log
-rw-r--r-- 1 root root      661 10月  6 20:40 www.syhnginx3.log

```

访问一个不存在的资源网站、查看错误日志记录

```shell
[root@localhost logs]# tail -f www.syhnginx2_error.log 
2020/10/07 14:57:33 [error] 1393#1393: *100200 open() "/www/nginx2/favicon.ico" failed (2: No such file or directory), client: 192.168.37.1, server: www.syhnginx2.com, request: "GET /favicon.ico HTTP/1.1", host: "192.168.37.134:81", referrer: "http://192.168.37.134:81/"
2020/10/07 14:58:13 [error] 1393#1393: *100200 open() "/www/nginx2/dasfafwqrqrq" failed (2: No such file or directory), client: 192.168.37.1, server: www.syhnginx2.com, request: "GET /dasfafwqrqrq HTTP/1.1", host: "192.168.37.134:81"

```



#### **5.22nginx的location用法**

Nginx的locaiton作用是根据用户请求的URI不同，来执行不同的应用。

针对用户请求的网站URL进行匹配，匹配成功后进行对应的操作。

```shell
nginx.conf中server{}指令块的location指令如下

        location / {
            root   html;
            index  index.html index.htm;
        }


        location = /50x.html {
            root   html;
        }
```

语法

```shell
location [ = | ~| ~* | ^~ ]  url {
    #指定对应的动作
}

#正则表达式解释
匹配符 匹配规则 优先级
=    精确匹配    1
^~    以某个字符串开头，不做正则    2
~*    正则匹配    3
/blog/ 匹配常规字符串，有正则就优先正则 4
/    通用匹配，不符合其他location的默认匹配    5
```

实战演练

| 请求url         | 完整url                              | 匹配后动作 |
| --------------- | ------------------------------------ | ---------- |
| /               | http://192.168.37.134/               | 配置A      |
| /index.html     | http://192.168.37.134/index.html     | 配置B      |
| /blog/blog.html | http://192.168.37.134/blog/blog.html | 配置C      |
| /img/1.jpg      | http://192.168.37.134/1.jpg          | 配置D      |
| /blog/1.jpg     | http://192.168.37.134/blog/1.jpg     | 配置E      |

修改nginx.conf如下

```shell
[root@bogon extra]# cat www.conf
server {
    listen 83;
    server_name _;

    #最低级匹配，不符合其他locaiton就来这
    location / {
        return 401;
}
    #优先级最高
    location = / {
        return 402;
}
    #以/blog/开头的url，来这里，如符合其他locaiton，则以其他优先
    location /blog/ {
        return 403;
}
    #匹配任何以/img/开头的请求，不匹配正则
    location ^~ /img/ {
        return 404;
}
    #匹配任何以.gif结尾的请求，支持正则
    location ~* \.(gif|jpg|jpeg)$ {
        return 500;
}


}
```

分别验证

```shell
[root@localhost syh]# curl -s -o /dev/null -I -w "%{http_code}\n" 192.168.37.134:84/
402
[root@localhost syh]# curl -s -o /dev/null -I -w "%{http_code}\n" 192.168.37.134:84/czdvsdf
401
[root@localhost syh]# 
[root@localhost syh]# 
[root@localhost syh]# curl -s -o /dev/null -I -w "%{http_code}\n" 192.168.37.134:84/blog/asfda
403
[root@localhost syh]# curl -s -o /dev/null -I -w "%{http_code}\n" 192.168.37.134:84/img/asfaf
404
[root@localhost syh]# curl -s -o /dev/null -I -w "%{http_code}\n" 192.168.37.134:84/imgsad/sdaf/1.jpg
405


#通过curl命令，检测nginx的locaiton匹配
curl命令
-s 不输出错误和进度信息，静默输出
-o  输出写入到指定文件中 /dev/null 就是丢弃输出，扔进黑洞
-I 只显示响应头
-w 完成后输出哪些内容
```



#### **5.23nginx的地址重写**

Nginx rewrire技术主要是实现URL地址重写，且支持正则表达式的规则。

<img src="notes/image-20200212175225568.png" style="zoom:33%;" />

rewrite能够实现URL的跳转，需要nginx在编译安装的时候，装好了PCRE这个软件。

通过rewrite可以规范URL、根据变量进行URL跳转等，常用的功能如

- 对于爬虫的封禁，让其跳转无用页面
- 动态的URL伪装成HTMl页面，便于搜索引擎的抓取
- 旧域名、旧目录的更新，需要跳转到新的URL地址

语法

```shell
rewrite ^/(.*) http://192.168.178.134/$1 permanent;
#解释
rewrite是指令，开启一个跳转规则
正则是 ^/(.*) 表示匹配所有，匹配成功后跳转到后面的url地址
$1 表示取出前面正则括号里的内容
permanent表示 301 重定向的标记
```

【rewrite的结尾参数 flag标记】

| 标记      | 解释a                                          |
| --------- | ---------------------------------------------- |
| last      | 规则匹配完成后，继续向下匹配新的Locaiton       |
| break     | 本条规则完成匹配后，立即停止                   |
| redirect  | 返回302临时重定向，浏览器地址栏显示跳转后的URL |
| permanent | 返回301永久重定向，浏览器地址显示跳转后的URL   |

*last和break用于实现URL重写，浏览器地址栏不发生变化*

*redirect和permanent用于实现URL跳转，浏览器地址栏跳转新的URL*

【实现301url跳转】

```shell
1.创建编辑新的配置文件、被include包含
vim  conf/syh/rewrite.conf】
server{

listen 90;
server_name -;
#请求直接跳转到百度
rewrite ^/(.*)  http://www.baidu.com/$1 permanent; 
}


```

【访问192.168.37.134:90】

![](notes/微信图片_20201007173720.png)



#### **5.24nginx的站点访问认证**

有时候，我们一些站点内容想要进行授权查看，只能输入账号密码之后才能访问，例如一些重要的内网平台，CRM，CMDB，企业内部WIKI等等。

```shell
htpasswd是Apache密码生成工具，Nginx支持auth_basic认证，因此我门可以将生成的密码用于Nginx中，输入一行命令即可安装：yum -y install httpd-tools ，参数如下：

-c 创建passwdfile.如果passwdfile 已经存在,那么它会重新写入并删去原有内容.
-n 不更新passwordfile，直接显示密码
-m 使用MD5加密（默认）
-d 使用CRYPT加密（默认）
-p 使用普通文本格式的密码
-s 使用SHA加密
-b 命令行中一并输入用户名和密码而不是根据提示输入密码，可以看见明文，不需要交互
-D 删除指定的用户

#接认证文件，htpasswd -bc .access username password  #在当前目录生成.access文件，用户名username，密码：password，默认采用MD5加密方式。
```







```shell
nginx的认证模块指令，语法：
location / {

    auth_basic "string"; 可以填写off或是string
    auth_basic_user_file conf/htpasswd;  
}
```

【实战案例】

```shell
1.安装apache的密码生成工具包
[root@localhost ~]# yum -y install httpd-tools

2.在include包含的目录下创建新的配置文件、编辑内容如下
server{
        listen 100;
        server_name -;
        location / {
        root /www/auth/html;
        index index.html index.htm
        #指定认证的名字
        auth_basic "string";
        #指定用于生成的加密文件的名字
        auth_basic_user_file /opt/nginx1.18/conf/syh/htpasswd；
        }
}


3.创建访问的页面站点文件
[root@localhost auth]# vim auth.html 内容如下：
<meta charset=utf-8>
我是认证功能、你好

4.重载配置文件
[root@localhost syh]# nginx -s reload

5.生成密码加密文件

[root@localhost conf]# htpasswd -bc .htpasswd syh 123456
Adding password for user syh

-b：
-c：
文件名.htpasswd、点表示隐藏文件
用户名：syh
密码：123456

```

【访问测试】

![](notes/微信图片_20201007205806.png)

![](notes/微信图片_20201007205741.png)





#### **5.25.LNMP黄金架构**

<img src="notes/image-20200212203206493.png" style="zoom:33%;" />

为什么要搭建lnmp架构？

- 之前学习Apache了解过LAMP（Linux、Apache、Mysql、Perl/PHP/Python）
- 由于上述架构，已然是一个商业版软件的代名词，后来自由软件趋势兴起，如何用开源软件替代商业软件，这是人们追求的，所有的Linux发行版几乎都会内置这样的软件。
- 但是由于Nginx的兴趣，卓越的性能，更简单的配置，更强大的功能，更低的资源消耗，LAMP已经渐渐被LNMP取代。

LNMP是网站架构初期最合适的单体架构。因为初创型技术团队对于技术的选型，需要考虑如下因素

1. 在创业初期，研发资源有限，研发人力有限，技术储备有限，需要选择一个易维护、简单的技术架构；
2. 产品需要快速研发上线，并能够满足快速迭代要求，现实情况决定了一开始没有时间和精力来选择一个过于复杂的分布式架构系统，研发速度必须要快；
3. 创业初期，业务复杂度比较低，业务量也比较小，如果选择过于复杂的架构，反而会增加研发难度以及运维难度；
4. 遵从选择合适的技术而不是最好的技术原则，并权衡研发效率和产品目标，同时创业初期只有一个PHP研发人员，过于复杂的技术架构必然会带来比较高昂的学习成本。

基于如上的因素，LNMP架构就是最合适的。

<img src="notes/image-20200212205020615.png" style="zoom:33%;" />

如此架构，一般三台服务器足以，Nginx与后台系统部署在一台机器，Mysql数据库单独服务器，Mencached缓存一台服务器。这样的架构优势在于

- 单体架构，架构简单，清晰的分层结构；
- 可以快速研发，满足产品快速迭代要求；
- 没有复杂的技术，技术学习成本低，同时运维成本低，无需专业的运维，节省开支。



#### **5.26LNMP工作流程**

LNMP工作流是用户通过浏览器输入域名访问Nginx web服务，Nginx判断请求是静态请求则由Nginx返回给用户。如果是动态请求(如.php结尾)，那么Nginx会将该请求通过FastCGI接口发送给PHP引擎（php-fpm进程）进行解析，如果该动态请求需要读取mysql数据库，php会继续向后读取数据库，最终Nginx将获取的数据返回给用户。



<img src="notes/image-20200214182854973.png" style="zoom:33%;" />



#### **5.27LNMP之Nginx搭建**

【nginx的安装步骤】

```shell
1.由于使用旧机器、先删除旧的编译安装过的nginx的安装目录、移除环境变量
[root@localhost opt]# ls
nginx1.18

[root@localhost opt]# rm -rf ./*
环境变量文件：/etc/profile

[root@localhost ~]# which nginx
/usr/bin/which: no nginx in (/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/opt/nginx1.18/sbin/:/root/bin)

2.安装nginx所需的pcre库，让nginx支持url重写的rewrite功能
yum install pcre pcre-devel -y


3.安装openssl-devel模块，nginx需要支持https
yum install openssl openssl-devel -y


4.安装gcc编译器
yum install gcc -y

5.下载nginx源码包
[root@localhost opt]# wget https://tengine.taobao.org/download/tengine-2.3.2.tar.gz
[root@localhost opt]# ls
tengine-2.3.2.tar.gz

6、创建普通用户
[root@localhost opt]# useradd nginx -u 1111 -s /sbin/nologin -M

-u: 指定uid为111
-s: 设置开机禁止登录
-M: 表示不创建用户家目录

7.开始压缩编译nginx
[root@localhost opt]# tar -zxvf tengine-2.3.2.tar.gz 
[root@localhost opt]# ls
tengine-2.3.2  tengine-2.3.2.tar.gz
[root@localhost tengine-2.3.2]# ./configure --user=nginx --group=nginx --prefix=/install/nginx232/ --with-http_stub_status_module --with-http_ssl_module

#检测上一条执行命令是否有误
[root@localhost tengine-2.3.2]# echo $?
0
[root@localhost tengine-2.3.2]# make && make install
[root@localhost tengine-2.3.2]# echo $?
0

8.配置软连接，生产环境常用操作，便于运维、开发、测试使用，以及nginx以后的升级
[root@localhost opt]# ln -s /install/nginx232
[root@localhost opt]# ll
总用量 2776
lrwxrwxrwx  1 root root      17 10月  8 17:06 nginx232 -> /install/nginx232
drwxrwxr-x 14 root root    4096 10月  8 16:48 tengine-2.3.2
-rw-r--r--  1 root root 2835884 9月   5 2019 tengine-2.3.2.tar.gz

9.配置nginx环境变量
[root@localhost sbin]# pwd
/install/nginx232/sbin
vim /etc/profile，在结尾添加以下内容：
PATH="$PATH:/install/nginx232/sbin/"	#注意赋值不能存在空格
exit退出或source /etc/profile
ssh root@192.168.37.134

[root@localhost etc]# which nginx
/install/nginx232/sbin/nginx

```

#### **5.28LNMP之Mysql搭建**

Mysql是互联网领域非常重要的，深受广大用户欢迎的一款开源数据库。MysSQL是一款关系型数据库，且把数据保存在不同的二维表，且把数据表再放入数据库中管理，而不是所有的数据统一放在一个大仓库，这样的设计提高MySQL的读写速度。

我们会在后面章节专门学习MYSQL，进阶DBA

安装方式我们可以选择多种

- Yum/rpm包安装，简单、快速、无法定制化、新手推荐使用
- 二进制安装，解压缩后直接简单配置即可使用，速度较快，专业DBA常用
- 源码编译安装，特点是可以定制化安装需求，缺点过程较为复杂



```shell
1.创建mysql用户
[root@localhost opt]# useradd -s /sbin/nologin mysql
[root@localhost opt]# id mysql
uid=1000(mysql) gid=1000(mysql) 组=1000(mysql)

2.下载mysql二进制软件包，提前配置好yum源，下载wget命令
[root@web01 ~]# yum install wget -y
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

[root@localhost opt]# mkdir tools
[root@web01 ~]# cd /home/mysql/tools/
# 该mysql文件600M左右，下载时间看网速
wget http://mirrors.163.com/mysql/Downloads/MySQL-5.7/mysql-5.7.29-linux-glibc2.12-x86_64.tar.gz


```





```shell
mysql二进制安装包体积较大，名字和源代码包有些区别，但是安装过程较快

mysql二进制包  mysql-5.7.26-linux-glibc2.12-x86_64.tar.gz        615M
mysql源码包   mysql-5.7.26.tar.gz                52M
```

【二进制方式安装Mysql】

```shell
1。解压并且移动mysql二进制软件包路径
[root@localhost tools]# mv ../mysql-5.7.29-linux-glibc2.12-x86_64.tar.gz  ./
[root@localhost tools]# tar -zxvf mysql-5.7.29-linux-glibc2.12-x86_64.tar.gz 
[root@localhost tools]# ls
mysql-5.7.29-linux-glibc2.12-x86_64  mysql-5.7.29-linux-glibc2.12-x86_64.tar.gz

2.生成软连接
[root@localhost tools]# ln -s ./mysql-5.7.29-linux-glibc2.12-x86_64 ./mysql5.7.29
[root@localhost tools]# ll
总用量 649172
lrwxrwxrwx. 1 root root        37 10月  9 20:34 mysql5.7.29 -> ./mysql-5.7.29-linux-glibc2.12-x86_64
drwxr-xr-x. 9 root root       129 10月  9 20:29 mysql-5.7.29-linux-glibc2.12-x86_64
-rw-r--r--. 1 root root 664749587 12月 18 2019 mysql-5.7.29-linux-glibc2.12-x86_64.tar.gz

3.卸载centos7自带的mariadb库，防止冲突
[root@localhost tools]# rpm -e --nodeps mariadb-libs

4.手动创建mysql配置文件 vim /etc/my.cnf
[mysqld]
basedir=/opt/tools/mysql5.7.29						#mysql的安装路径、也就是生成软连接的路径
datadir=/opt/tools/mysql5.7.29/data					#mysql生成数据文件夹的路径
socket=/tmp/mysql.sock								#mysql进程启动的通信文件
server_id=1
port=3306											#mysql端口
log_error=/opt/tools/mysql5.7.29/data/mysql_err.log	    #mysql运行的错误日志

[mysql]
socket=/tmp/mysql.sock

```

【初始化mysql服务端】

```shell
1.卸载系统自带的centos7 mariadb-libs，且安装mysql的依赖环境
rpm -qa mariadb-libs #检查是否存在
yum install libaio-devel -y

2.创建mysql数据文件夹且授权
[root@localhost mysql5.7.29]# mkdir data
[root@localhost tools]# chown -R  mysql.mysql /opt/tools/mysql5.7.29/
[root@localhost tools]# ll
总用量 649172
lrwxrwxrwx.  1 root  root         37 10月  9 20:34 mysql5.7.29 -> ./mysql-5.7.29-linux-glibc2.12-x86_64
drwxr-xr-x. 10 mysql mysql       141 10月  9 20:54 mysql-5.7.29-linux-glibc2.12-x86_64
-rw-r--r--.  1 root  root  664749587 12月 18 2019 mysql-5.7.29-linux-glibc2.12-x86_64.tar.gz

3.初始化数据库
[root@localhost mysql5.7.29]# /opt/tools/mysql5.7.29/bin/mysqld --initialize-insecure --user=mysql --basedir=/opt/tools/mysql5.7.29/ --datadir=/opt/tools/mysql5.7.29/data/

# 参数解释
--user=mysql 指定用户
--basedir 指定mysql安装目录
--datadir=/opt/mysql/data 指定数据文件夹
--initialize-insecure 关闭mysql安全策略
--initialize 开启mysql安全模式
```

【配置mysql客户端】

```shell
1.配置mysql启动脚本，定义mysqld.service，脚本如下
[root@localhost mysql5.7.29]# vim /etc/systemd/system/mysqld.service

[Unit]
Description=MySQL server by syh
Documentation=man:mysqld(8)
Documentation=http://dev.mysql.com/doc/refman/en/using-systemd.html
After=network.target
After=syslog.target

[Install]
WantedBy=multi-user.target

[Service]
User=mysql
Group=mysql
ExecStart=/opt/tools/mysql5.7.29/bin/mysqld --defaults-file=/etc/my.cnf			#指定安装的路径
LimitNOFILE=5000

```

【配置Mysql环境变量】

```shell
方法一：
1.在/etc/profile文件中添加以下内容：
PATH="$PATH:/opt/tools/mysql5.7.29/bin"
source /etc/profile		#使配置文件生效

[root@localhost bin]# which mysql
/opt/tools/mysql5.7.29/bin/mysql

[root@localhost bin]# echo "$PATH"
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/opt/tools/mysql5.7.29/bin


方法二：
[root@web01 mysql]# echo "export PATH=/opt/mysql/bin:$PATH" >> /etc/profile
[root@web01 mysql]# source /etc/profile
[root@web01 mysql]# echo $PATH
/opt/mysql/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin

```



【启动mysql数据库】

```shell
[root@localhost bin]# systemctl start mysqld
● mysqld.service - MySQL server by syh
   Loaded: loaded (/etc/systemd/system/mysqld.service; disabled; vendor preset: disabled)
   Active: active (running) since 五 2020-10-09 22:11:01 CST; 2s ago
     Docs: man:mysqld(8)
           http://dev.mysql.com/doc/refman/en/using-systemd.html
 Main PID: 17173 (mysqld)
   CGroup: /system.slice/mysqld.service
           └─17173 /opt/tools/mysql5.7.29/bin/mysqld --defaults-file=/etc/my.cnf

10月 09 22:11:01 localhost.localdomain systemd[1]: Started MySQL server by syh.
10月 09 22:11:01 localhost.localdomain systemd[1]: Starting MySQL server by syh...

[root@localhost bin]# systemctl status mysqld
[root@localhost bin]# systemctl enable mysqld  #开机自启动
```

【检查mysql启动状态】

```shell
1.检查mysql的进程
[root@localhost bin]# ps -ef|grep mysqld
mysql     17173      1  0 22:11 ?        00:00:00 /opt/tools/mysql5.7.29/bin/mysqld --defaults-file=/etc/my.cnf

2.检查mysql的端口
[root@localhost bin]# netstat -tunlp|grep mysql
tcp6       0      0 :::3306                 :::*                    LISTEN      17173/mysqld  

```

【登录mysql】

默认mysql账号root是没有密码的，我们给其设置密码，加大安全性

```shell
1.修改mysql密码并登录
    mysqladmin -uroot password '123456'

[root@localhost man]# mysql -uroot -p123456
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 4
Server version: 5.7.29 MySQL Community Server (GPL)

Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> 
2.常用的mysql查询语句
#显示所有数据库
mysql> show databases;			
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)

# 查看当前登录的用户
mysql> select user();
+----------------+
| user()         |
+----------------+
| root@localhost |
+----------------+
1 row in set (0.00 sec)

 #查看mysql所有用户信息
mysql> select user,authentication_string,host from mysql.user;
+---------------+-------------------------------------------+-----------+
| user          | authentication_string                     | host      |
+---------------+-------------------------------------------+-----------+
| root          | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 | localhost |
| mysql.session | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | localhost |
| mysql.sys     | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | localhost |
+---------------+-------------------------------------------+-----------+
3 rows in set (0.00 sec)


```

#### **5.29LNMP之php搭建**

php(FastCGI形式)安装

```shell
1.检查nginx和mysql是否启动
[root@localhost man]# ps -ef|grep mysqld
[root@localhost ~]# ps -ef|grep nginx


2.下载PHP源码包
[root@localhost opt]# wget http://mirrors.sohu.com/php/php-7.3.5.tar.gz
[root@localhost opt]# ls
php-7.3.5.tar.gz

3.准备php需要的系统库

[root@localhost php-7.3.5]# yum install  gcc gcc-c++ make zlib-devel libxml2-devel libjpeg-devel libjpeg-turbo-devel libiconv-devel freetype-devel libpng-devel gd-devel libcurl-devel libxslt-devel libxslt-devel -y

4.发现缺少库包

没有可用软件包 libiconv-devel。


5.编译安装libiconv-devel
	5.1下载源码包
	[root@localhost opt]# wget http://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.15.tar.gz
	5.2解压缩
	[root@localhost opt]# tar -zxvf libiconv-1.15.tar.gz 
	5.3指定路径安装
	[root@localhost libiconv-1.15]# mkdir -p /mytools/libiconv1.15
	[root@localhost libiconv-1.15]# ./configure  --prefix=/mytools/libiconv1.15/

	5.4编译安装
	[root@localhost libiconv-1.15]# make && make install


6.解压缩php源码包，编译安装

[root@localhost opt]# tar -zxvf php-7.3.5.tar.gz 
cd php-7.3.5
./configure --prefix=/mytools/php7.3.5 \
--enable-mysqlnd \
--with-mysqli=mysqlnd \
--with-pdo-mysql=mysqlnd \
--with-iconv-dir=/mytools/libiconv1.15 \
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

	6.1发现少了一个openssl-devel包、通过yum下载
	[root@localhost php-7.3.5]# yum install openssl-devel -y
	
7.编译且编译安装
[root@web01 php-7.3.5]# make && make install

8.配置php环境变量
[root@localhost bin]# vim /etc/profile
输入以下内容：
PATH="$PATH:/mytools/php7.3.5/sbin/"
[root@localhost bin]# source /etc/profile
[root@localhost bin]# which php
[root@localhost sbin]# which php
/mytools/php7.3.5/bin/php
```

待看到如下显示，表示正确的编译了php

![](notes/微信图片_20201009220956.png)

对于如上的参数，根据自己实际工作环境优化增删即可

部分参数解释

```shell
--prefix=  指定php安装路径
--enable-mysqlnd 使用php自带的mysql相关软件包
--with-fpm-user=nginx  指定PHP-FPM程序的用户是nginx，和nginx服务保持统一
--enable-fpm 激活php-fpm方式，以FastCGI形式运行php程序
```



【PHP配置文件】

配置文件路径

```shell
1.配置文件路径：/opt/php-7.3.5---源码包解压出来的路径
[root@localhost php-7.3.5]# ls php.ini*
php.ini-development  php.ini-production

```

俩配置文件，分别默认用于开发环境，生成环境，配置参数有所不同

```shell
[root@localhost php-7.3.5]# vimdiff php.ini-development  php.ini-production
开发环境下开起了更多的日志、调试信息，生产环境该参数都关闭了
```

拷贝php配置文件到php默认目录，且改名

```shell
[root@localhost php-7.3.5]# cp ./php.ini-development  /mytools/php7.3.5/lib/php.ini

```





【FASTCGI配置文件】

```shell
1.FASTCGI配置文件路径：/mytools/php7.3.5/etc
[root@localhost etc]# ls
pear.conf  php-fpm.conf.default  php-fpm.d

2.生成2个php-frpm的配置文件，先用默认配置，后续可以方便恢复
[root@localhost etc]# cp php-fpm.conf.default  php-fpm.conf
[root@localhost etc]# ls
pear.conf  php-fpm.conf  php-fpm.conf.default  php-fpm.d

3.拷贝php-fpm.d/www.conf.default
[root@localhost php-fpm.d]# cp www.conf.default  www.conf
[root@localhost php-fpm.d]# ls
www.conf  www.conf.default

```

【启动PHP服务（FastCGI模式）】

```shell
1.启动php-fpm服务
[root@localhost ~]# php-fpm
[10-Oct-2020 22:54:11] ERROR: [pool www] cannot get uid for user 'nginx'
[10-Oct-2020 22:54:11] ERROR: FPM initialization failed
#这里发现不能启动
此时创建nginx用户和nginx用户组
[root@localhost ~]# useradd nginx && groupadd nginx


2.再次启动nginx
[root@localhost ~]# php-fpm

3.检查php端口、进程
[root@localhost ~]# netstat -tunpl|grep php-fpm
tcp        0      0 127.0.0.1:9000          0.0.0.0:*               LISTEN      1552/php-fpm: maste

[root@localhost ~]# ps -ef|grep php
root       1552      1  0 22:55 ?        00:00:00 php-fpm: master process (/mytools/php7.3.5/etc/php-fpm.conf)
nginx      1553   1552  0 22:55 ?        00:00:00 php-fpm: pool www
nginx      1554   1552  0 22:55 ?        00:00:00 php-fpm: pool www
root       1559   1516  0 22:58 pts/0    00:00:00 grep --color=auto php



```

【修改nginx配置文件支持php】

```shell
1.修改nginx的配置文件内容如下：				学习使用可以进行这样修改
[root@localhost conf]# vim /install/nginx232/conf/nginx.conf
events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;
    keepalive_timeout  65;
    gzip  on;
include extra/my_php.conf;
}

```

【配置php的解析配置文件】

```shell
1.创建文件夹
[root@localhost conf]# mkdir extra

2.在extra创建文件
[root@localhost extra]# vim my_php.conf
server {
listen 80;
server_name -;
location / {
    root html/blog;
    index index.html;
}

#添加有关php程序的解析
#请求走向以php或php5即为的文件
server {
listen 80;
server_name -;
location / {
    root html/;
    index index.html;
}

#添加有关php程序的解析
location ~ .*\.(php|php5)?$ {

    root /install/nginx232/html/myphp;
    fastcgi_pass 192.168.37.136:9000;
    fastcgi_index index.php;
    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    include fastcgi_params;
}

}
/scripts：php服务器上的/scripts
$fastcgi_script_name：表示文件名




3.检查nginx语法、并重载配置文件
[root@localhost conf]# nginx -t && nginx -s reload
nginx: the configuration file /install/nginx232//conf/nginx.conf syntax is ok
nginx: configuration file /install/nginx232//conf/nginx.conf test is successful



4.在php服务器上的/scripts创建php页面文件
mkdir /scripts
cd /scripts
vim index.php 
<p>你好<>
<?php phpinfo(); ?>   


5.编辑php的www.conf配置文件
[root@localhost etc]# vim php-fpm.d/www.conf、修改内容如下：
listen = 0.0.0.0:9000			#表示绑定这台机器，所有的网卡，也就是可以对外提供访问


6.重启php服务
pkill php-fpm
php-fpm

7.访问nginx服务器
http://192.168.37.134/index.php
看到如下界面、说明nginx何以和php进行联通

```

![](notes/微信图片_20201010175328.png)



【测试php链接mysql】

```shell
1.在php服务器的/scripts下生成脚本
[root@localhost scripts]# vim test_mysql.php
<?php
$link_id=mysqli_connect('192.168.37.135','root','123456') or mysql_error();
if($link_id){

    echo "mysql successful by chaoge.\n";
}else {

    echo mysql_error();
}
?>


2.检查三台机器、确保mysql、php、nginx服务都正常启动
netstat -tunpl|grep mysql
netstat -tunpl|grep php
netstat -tunpl|grep nginx

3.浏览器访问192.168.37.134/test_mysql.php
#发现不能连接数据库、考虑到远程授权问题

Warning: mysqli_connect(): (HY000/2002): Connection refused in /scripts/test_mysql.php on line 3
Fatal error: Uncaught Error: Call to undefined function mysql_error() in /scripts/test_mysql.php:3 Stack trace: #0 {main} thrown in /scripts/test_mysql.php on line 3

4.进入mysql交互界面，创建syh用户、并指定可在192.168.37.136远程访问
mysql> CREATE USER 'syh'@'%' IDENTIFIED BY '123456';
%:表示所有数据库和所有表，
BY+用户密码


5.再次浏览器访问192.168.37.134/test_mysql.php
#看到以下界面则表示连接成功
```

![](notes/微信图片_20201010215416.png)





#### **5.30LNMP之搭建wordpress站点**

【环境准备】

```shell
1.启动mysql数据库、用于创建wordpress数据库
systemctl start mysqld
mysql -uroot p123456

2.创建workpress数据库
mysql> create database wordpress;
Query OK, 1 row affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| wordpress          |
+--------------------+
5 rows in set (0.00 sec)


3.创建wordpress用户d
mysql> create user wordpress;
Query OK, 0 rows affected (0.00 sec)

4.给用户wordpresss授予wordpress数据库的所有权限、允许wordpress用户在所有ip登录、并设置密码为123456
mysql> grant all on wordpress.* to wordpress@'%' identified by '123456';
Query OK, 0 rows affected, 1 warning (0.00 sec)

5.刷新授权表
mysql> flush privileges;
Query OK, 0 rows affected (0.02 sec)

6.查询用户信息
mysql> use mysql;	#进入mysql库
mysql> show tables;	#显示mysql库里的表
mysql> desc user;	#查询user表的结构
mysql> select user,authentication_string,host from mysql.user;		#查询这几个字段信息
+---------------+-------------------------------------------+----------------+
| user          | authentication_string                     | host           |
+---------------+-------------------------------------------+----------------+
| root          | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 | localhost      |
| mysql.session | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | localhost      |
| mysql.sys     | *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE | localhost      |
| myuser        | *FABE5482D5AADF36D028AC443D117BE1180B9725 | 192.168.1.3    |
| syh           | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 | 192.168.37.136 |
| syh           | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 | %              |
| wordpress     | *6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 | %              |


7.确保nginx的配置文件支持php
[root@localhost extra]# vim my_php.conf 

listen 80;
server_name -;
location / {
    root html/myphp;
    index index.php index.html;
}

#添加有关php程序的解析
location ~ .*\.(php|php5)?$ {

    root /install/nginx232/html/myphp;
    fastcgi_pass 192.168.37.136:9000;
    fastcgi_index index.php;
    #fastcgi_param  SCRIPT_FILENAME  $document_root$fastcgi_script_name;
    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    include fastcgi_params;


```

【workpress程序准备】

```shell
1.在安装php服务器上下载workpress程序包
wget https://wordpress.org/latest.tar.gz

2.解压缩
[root@localhost opt]# tar -zxvf latest.tar.gz 

3.将解压出的文件夹移动到存放nginx安装且能够解析php的目录
[root@localhost opt]# mv wordpress/ /scripts


4.将wordpress下的内容移动到 
[root@localhost scripts]# mv wordpress/* ./
[root@localhost scripts]# ll
总用量 204
-rw-r--r--  1 nobody 65534   405 2月   6 2020 index.php
-rw-r--r--  1 nobody 65534 19915 2月  12 2020 license.txt
-rw-r--r--  1 nobody 65534  7278 6月  26 21:58 readme.html
drwxr-xr-x  2 nobody 65534     6 10月 11 20:22 wordpress
-rw-r--r--  1 nobody 65534  7101 7月  29 01:20 wp-activate.php
drwxr-xr-x  9 nobody 65534  4096 9月   2 02:54 wp-admin
-rw-r--r--  1 nobody 65534   351 2月   6 2020 wp-blog-header.php
-rw-r--r--  1 nobody 65534  2332 7月  23 08:52 wp-comments-post.php
-rw-r--r--  1 nobody 65534  2913 2月   6 2020 wp-config-sample.php
drwxr-xr-x  4 nobody 65534    52 9月   2 02:54 wp-content
-rw-r--r--  1 nobody 65534  3940 2月   6 2020 wp-cron.php
drwxr-xr-x 24 nobody 65534  8192 9月   2 02:54 wp-includes
-rw-r--r--  1 nobody 65534  2496 2月   6 2020 wp-links-opml.php
-rw-r--r--  1 nobody 65534  3300 2月   6 2020 wp-load.php
-rw-r--r--  1 nobody 65534 48761 7月   7 11:59 wp-login.php
-rw-r--r--  1 nobody 65534  8509 4月  14 19:32 wp-mail.php
-rw-r--r--  1 nobody 65534 20181 7月   6 18:50 wp-settings.php
-rw-r--r--  1 nobody 65534 31159 7月  24 05:11 wp-signup.php
-rw-r--r--  1 nobody 65534  4755 2月   6 2020 wp-trackback.php
-rw-r--r--  1 nobody 65534  3236 6月   9 03:55 xmlrpc.php

7.修改以上文件的属主和属组
[root@localhost myphp]# chown -R nginx.nginx ./*



6.重启nginx
nginx -s reload


9.使用浏览器访问192.168.37.134、看到以下界面说明成功
```



![](notes/微信图片_20201011122906.png)



![](notes/微信图片_20201011123027.png)



![](notes/微信图片_20201011123816.png)

这里遇到一个问题、就是无法写入wp-config.php;可根据页面提示将文中内容复制后、写入wp-config.php即可

<img src="notes/微信图片_20201011142123.png"  />



![](notes/微信图片_20201011142031.png)

