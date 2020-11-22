## 四、互联网服务基础之LAMP架构

#### 4.1Aphache的介绍

​	**黄金架构LAMP**

L：linux 

A：apache

m：mysql

p：php/python

LAMP之间的工作联系

<img src="notes/image-20200117165409223.png" style="zoom:33%;" />

1. 当用户发送请求、通过浏览器浏览时、如果用户请求的是静态网页数据、则从apache （web_server）读取硬盘数据获取、然后返回给用户；
2. 若用户请求的是提交数据请求、获取的是动态界面请求、就通过apache服务器将请求转发到应用服务器（存放后端编程代码、用于解析请求处理）处理；应用服务器就通过后台数据库交互、实现数据返回给用户

**Apache**

Apache Web Server虽然称之为`web服务器`，但是不是意味着他是一个`物理服务器`，它只是电脑软件中的一个软件而已，**Web服务器的作用是将HTTP请求从前端转发到后端应用上。**



### PHP

- PHP是一门服务端脚本编程语言，主要用于web开发，常用PHP脚本嵌入HTML源码中执行。
- PHP是全球知名的编程语言之一，程序员可以免费试用，PHP支持多种操作系统，开发效率高，支持多种数据库操作。
- 国内众多网站，百度、雅虎、新浪都在大量使用PHP语言进行开发，知名的论坛软件Discuz也是由PHP开发且占据了80%的论坛软件市场。

**Mysql**

- Mysql是一款数据库管理系统，也就是一个存储数据的工具，用户可以自行对数据库进行增加、删除、修改、查询等操作。
- MySQL是数据库管理系统中的一款软件，被业界广泛使用，例如新浪、QQ、淘宝、都在大量使用MySQL数据库。
- 腾讯QQ使用Linux与MySQL数据库，存储注册用户2.8亿的信息，活跃人数9000万，凭借万台服务器搭建的数据库集群，腾讯QQ同时在线人数也达到了千万，这证明了MySQL数据库的大容量、快速响应特点。
- MySQL是一款关系型数据库，尤其适合Web应用，特别是电商领域，MySQL遍布各种行业、移动、爱立信、惠普、银行、思科、摩托萝拉、等等。

**LAMP架构介绍**

 LAMP（Linux-Apache-MySQL-PHP）网站架构是目前国际流行的Web框架，该框架包括：Linux操作系统，Apache网络服务器，MySQL数据库，Perl、PHP或者Python编程语言，所有组成产品均是开源软件，是国际上成熟的架构框架，很多流行的商业应用都是采取这个架构，LAMP具有通用、跨平台、高性能、低价格的优势，因此LAMP无论是性能、质量还是价格都是企业搭建网站的首选平台。

- LAMP是一个多C/S架构的平台，最初级为web客户端(浏览器)基于TCP/IP通过http协议发起传送，这个请求可能是动态的，也可能是静态的。
- 所以web服务器通过发起请求的后缀来判断，如果是静态的资源就由web服务器自行处理，然后将资源发给客户端。如果是动态这时web服务器会通过CGI（Common Gateway interface）协议发起给php。
- 这里但是如果php是以模块形式与Web服务器联系，（安装在同一台服务器）那么他们是通过内部共享内存的方式。
- 如果是php单独的放置与一台服务器，那么他们是通过sockets套接字监听的方式通信（安装在不同的服务器上，远程通信，这又是一个C/S架构）。
- 这时php会相应的执行一段程序，如果在执行程序时，需要用到数据。
- 那么php就会通过mysql协议发送给mysql服务器（也可以看作是一个C/S架构）。由mysql服务器处理，将数据供给php程序。

【流程解析】

<img src="notes/image-20200203165504534.png" style="zoom:50%;" />

1.用户通过浏览器发送http请求，到达`web服务器（apache或nginx）`

2.web服务器解析用户请求信息，明确他到底要什么，如果是`静态资源请求`，直接通过linux内核读取硬盘上的数据，然后构建响应报文，给与用户。如果`是动态资源请求`，则转发请求给`应用服务器（php,python）`，由php解析动态请求，解析完毕后，返回给apache，发给用户

3.如果涉及数据库操作，利用`php-mysql`驱动，获取数据库数据，再返回给php，最终给与用户

<img src="notes/image-20200203170245218.png" style="zoom:50%;" />

#### 4.2Apache的搭建



【apache服务搭建步骤】

​	1.关闭防火墙以及selinux

```shell
[root@chaogelinux ~]# iptables -F
[root@chaogelinux ~]# systemctl stop firewalld
[root@chaogelinux ~]# systemctl disable firewalld
[root@chaogelinux ~]# getenforce
Disabled
setenforce 0		#可不重启直接关闭selinux状态
```

​	2.配置yum源仓库

```shell
1.配置yum源		这里用阿里云yum源地址
浏览器访问：https://developer.aliyun.com/mirror,选择centos或epel

2.通过命令下载centos7 或epel

wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

3.清空缓存yum、针对阿里云的yum源生成新的缓存、用于加速下载软件
yum clean all
yum makecache


```

3.安装、启动apache

```shell
1..通过命令直接安装apache
yum install httpd -y

2.启动apache服务、并配置开机自启动
systemctl start httpd
systemctl status httpd
systemctl enable httpd					开机自启动
systemctl disable httpd					开机不自启动

```

4.检查apche的端口、进程情况

```shell
1..检查80端口是否启用
[root@localhost ~]# netstat -tunpl|grep '80'
tcp6       0      0 :::80                   :::*                    LISTEN      1198/httpd     

2.检查apache进程是否存在
[root@localhost ~]# ps -ef|grep httpd
root       1198      1  0 11:39 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache     1209   1198  0 11:39 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache     1210   1198  0 11:39 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache     1211   1198  0 11:39 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache     1212   1198  0 11:39 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
apache     1213   1198  0 11:39 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
root       1220   1171  0 11:40 pts/1    00:00:00 grep --color=auto httpd

3.使用curl命令火获取页面html

[root@localhost ~]# curl  192.168.0.106

获取成功则说明搭建成功
```

#### 4.3Apache配置文件

【apache主配置文件】

| 文件路径                           | 作用                          |
| ---------------------------------- | ----------------------------- |
| /etc/httpd/conf/httpd.conf         | apache主配置文件              |
| /etc/httpd                         | apache主配置目录              |
| /etc/httpd/conf.d/*.conf           | apache子配置文件              |
| /usr/sbin/httpd                    | 二进制脚本                    |
| /var/log/httpd                     | 日志路径access_log、error_log |
| /var/www/html                      | 站点资源路径                  |
| /usb/lib/stemd/system/http.service | http服务脚本文件              |
| /usr/lib64/httpd/moduies           | http模块文件路径              |

【httpd主配置文件】

```shell
#过滤掉注释空白行
[root@chaogelinux ~]# grep -Ev '^[# ]|^$' /etc/httpd/conf/httpd.conf
ServerRoot "/etc/httpd"
Listen 80
Include conf.modules.d/*.conf
User apache
Group apache
ServerAdmin root@localhost
<Directory />
</Directory>
DocumentRoot "/var/www/html"
<Directory "/var/www">
</Directory>
<Directory "/var/www/html">
</Directory>
<IfModule dir_module>
</IfModule>
<Files ".ht*">
</Files>
ErrorLog "logs/error_log"
LogLevel warn
<IfModule log_config_module>
</IfModule>
<IfModule alias_module>
</IfModule>
<Directory "/var/www/cgi-bin">
</Directory>
<IfModule mime_module>
</IfModule>
AddDefaultCharset UTF-8
<IfModule mime_magic_module>
</IfModule>
EnableSendfile on
IncludeOptional conf.d/*.conf
```

主配置文件中，主要分为3类

- 全局配置，全局性
- 主服务器配置
- 虚拟主机

<img src="notes/image-20200114161611655.png" style="zoom:50%;" />

**常见参数解析**

| 参数                                 | 解析                   |
| ------------------------------------ | ---------------------- |
| ServerRoot "/etc/httpd"              | 定义服务工作目录       |
| ServerAdmin root@localhost           | 管理员邮箱地址         |
| User apache                          | 运行服务的用户信息     |
| Group apache                         | 运行服务的用户组       |
| ServerName www.example.com:80        | 填写服务器域名         |
| DocumentRoot "/var/www/html"         | 定义网站根目录         |
|                                      | 定义网站数据目录的权限 |
| Listen                               | 监听的IP地址和端口号   |
| DirectoryIndex index.html            | 默认的首页页面文件     |
| ErrorLog "logs/error_log"            | 定义错误日志位置       |
| CustomLog "logs/access_log" combined | 定义访问日志路径       |

apache常见配置

【修改首页内容】

```shell
[root@localhost /]# vim /var/www/html/index.html 
你好吃饭了吗？

```

![](notes/微信图片_20200928161205.png)

【修改网站资源目录路径】



```shell
1.修改配置文件如下，两处修改
[root@chaogelinux ~]# cat /etc/httpd/conf/httpd.conf
DocumentRoot "/www"

<Directory "/www">
    AllowOverride None
    # Allow open access:
    Require all granted
</Directory>


2.创建资源目录，创建html文件
[root@chaogelinux conf]# cat /www/index.html
<meta charset=utf8>
我是新的首页，你好兄弟们


3.修改了配置文件，还得重启http服务才能生效
systemctl restart httpd

4.注意关闭防火墙和selinux，影响实验
[root@chaogelinux conf]# systemctl stop firewalld
[root@chaogelinux conf]# systemctl disable firewalld
[root@chaogelinux conf]# iptables -F
[root@chaogelinux conf]# setenforce 0  #临时关闭selinux
[root@chaogelinux conf]# grep -i '^selinux' /etc/selinux/config    
SELINUX=disabled    #永久关闭selinux，重启机器生效
SELINUXTYPE=targeted
```

此时可以访问新资源目录的内容了

![](notes/image-20200114180933338.png)



#### 4.4Apache的工作模式

Apache提供了三种稳定的（多进程处理模块）`MPM(Mutli-Processing Modules 多通道处理模块)`，使得Apache能够使用更多不同的工作环境，扩展了Apache的功能。

```shell
检查默认的apache工作模式
[root@chaogelinux ~]# httpd -V|grep -i "server mpm"
Server MPM:     prefork
```

可以在编译apache软件时候，添加编译参数，更改mpm模式

```shell
--with-mpm=prefork|worker|event
```

apache的三种工作模式

```shell
1.prefork
Apache在启动之初，就预先fork一些子进程，然后等待请求进来。之所以这样做，是为了减少频繁创建和销毁进程的开销。每个子进程只有一个线程，在一个时间点内，只能处理一个请求。

优点：成熟稳定，兼容所有新老模块。同时，不需要担心线程安全的问题。
缺点：一个进程相对占用更多的系统资源，消耗更多的内存。而且，它并不擅长处理高并发请求。

2.worker
使用了多进程和多线程的混合模式。它也预先fork了几个子进程(数量比较少)，然后每个子进程创建一些线程，同时包括一个监听线程。每个请求过来，会被分配到1个线程来服务。线程比起进程会更轻量，因为线程通常会共享父进程的内存空间，因此，内存的占用会减少一些。在高并发的场景下，因为比起prefork有更多的可用线程，表现会更优秀一些。

优点：占据更少的内存，高并发下表现更优秀。
缺点：必须考虑线程安全的问题。

3.event
它和worker模式很像，最大的区别在于，它解决了keep-alive场景下，长期被占用的线程的资源浪费问题。event MPM中，会有一个专门的线程来管理这些keep-alive类型的线程，当有真实请求过来的时候，将请求传递给服务线程，执行完毕后，又允许它释放。这样增强了高并发场景下的请求处理能力。

HTTP采用keepalive方式减少TCP连接数量，但是由于需要与服务器线程或进程进行绑定，导致一个繁忙的服务器会消耗完所有的线程。Event MPM是解决这个问题的一种新模型，它把服务进程从连接中分离出来。在服务器处理速度很快，同时具有非常高的点击率时，可用的线程数量就是关键的资源限制，此时Event MPM方式是最有效的，但不能在HTTPS访问下工作。
```



#### 4.5apache之userdir功能

userdir模块可以很方便的与他人共享目录资源

**修改/etc/httpd/conf.d/userdir.conf**

```shell
1.启用userdir，编辑配置文件内容
<IfModule mod_userdir.c>
    #
    # UserDir is disabled by default since it can confirm the presence
    # of a username on the system (depending on home directory
    # permissions).
    #
    #UserDir disabled  #添加注释，表示启用

    #
    # To enable requests to /~user/ to serve the user's public_html
    # directory, remove the "UserDir disabled" line above, and uncomment
    # the following line instead:
    #
    UserDir public_html
</IfModule>
添加一些认证参数、可使用文件中定义的用户、利用其账号密码登录认证
<Directory "/home/*/public_html">
    #AllowOverride FileInfo AuthConfig Limit Indexes					
    AllowOverride all								允许所有人修改该文件
    authuserfile "/etc/httpd/passwd"        		  用于存放账号密码的目录
    authname "input your account"					 登录提示
    authtype basic									认证类型（账号密码类型）
    require user syh								使用syh登录
    #Options MultiViews Indexes SymLinksIfOwnerMatch IncludesNoExec
    #Require method GET POST OPTIONS
    
    
2.创建网站数据文件夹、注意权限
[syh@localhost ~]$ su - syh
[syh@localhost ~]$ mkdir public_html


3.创建首页文件
[syh@localhost ~]$ echo '我是网站首页、欢饮访问'>public_html/index.html


4.设置家目录syh目录权限
[root@localhost ~]# chmod -Rf 755 /home/syh

5.创建需要验证的账户密码、生成数据文件
[root@localhost ~]# htpasswd -c /etc/httpd/passwd syh
New password: 
Re-type new password: 
Adding password for user syh

6.使用root用户重启apache
[root@localhost ~]# systemctl restart httpd

7.浏览器访问地址http://192.168.0.106/~syh/

8.出现以下界面及说明配置成功
```

![](notes/微信图片_20200929140914.png)

![](notes/微信图片_20200929141015.png)



#### 4.6虚拟主机

【虚拟主机】

虚拟主机，也叫“网站空间”，就是把一台运行在互联网上的物理服务器划分成多个“虚拟”服务器。虚拟主机技术极大的促进了网络技术的应用和普及。同时虚拟主机的租用服务也成了网络时代的一种新型经济形式。

![](notes/image-20200115173512746.png)



- 如果每台Linux服务器，只能跑一个网站，那一些只有简单业务的网站，或者个人博客站点，对于硬件资源来说是浪费的，且需要支付服务器的费用。
- 对于Apache是支持虚拟主机的，能够以用户请求的不同的IP、域名、端口来区分不同的站点。



【基于IP的虚拟主机】

在一台服务器上绑定多个IP地址，每个IP地址部署一个站点，访问不同的IP地址，apache服务器返回不同的网站资源。



1、添加三个IP地址

```shell
1.添加三个虚拟临时ip地址
[root@localhost httpd]# ip address add 192.168.0.200/24 dev ens33
[root@localhost httpd]# ip address add 192.168.0.201/24 dev ens33
[root@localhost httpd]# ip address add 192.168.0.202/24 dev ens33

2.检查添加的ip地址是否存在
[root@localhost httpd]# ip address|grep 'inet 192'
    inet 192.168.0.106/24 brd 192.168.0.255 scope global noprefixroute ens33		这是本机服务器的ip
    inet 192.168.0.200/24 scope global secondary ens33
    inet 192.168.0.201/24 scope global secondary ens33
    inet 192.168.0.202/24 scope global secondary ens33

```



2、准备三个站点的文件夹和配置站点html文件

```shell
1.创建三个站点的目录
[root@localhost httpd]# mkdir -p /www/{dnf,lol,cf}
[root@localhost httpd]# ls /www/
cf  dnf  lol


2.为三个站点添加html网页文件
[root@localhost httpd]# echo '欢迎访问192.168.0.200，我是dnf站点，你好'>/www/dnf/index.html
[root@localhost httpd]# echo '欢迎访问192.168.0.201，我是cf站点，你好'>/www/cf/index.html
[root@localhost httpd]# echo '欢迎访问192.168.0.201，我是lol站点，你好'>/www/lol/index.html

```



3、为三个站点定义域名

```shell
192.168.0.200			www.dnf.com
192.168.0.201			www.cf.com
192.168.0.202			www.lol.com
```





4、修改httpd.conf主配置文件、最底添加虚拟主机的配置



```shell
#第一块IP配置区域
<VirtualHost 192.168.0.200>
DocumentRoot /www/dnf
#ServerName www,dnf.com
<Directory /www/dnf >
AllowOverride None
Require all granted
</Directory>
</VirtualHost>


#第二块IP配置区域
<VirtualHost 192.168.0.201>
DocumentRoot /www/cf
#ServerName www.cf.com
<Directory /www/cf >
AllowOverride None
Require all granted
</Directory>


#第三块IP配置区域
<VirtualHost 192.168.0.202>
DocumentRoot /www/lol
#ServerName www.lol.com
<Directory /www/lol >
AllowOverride None
Require all granted
</Directory>
</VirtualHost>

```

5、重启apache服务

```shell
[root@localhost httpd]# systemctl restart httpd
```

7、分别访问三个站点

```shell
客户端浏览器访问
192.168.0.200
192.168.0.201
192.168.0.202
```



【多域名虚拟主机】

刚才的操作是多IP虚拟主机，那么当服务器仅允许有一个IP地址的时候，就无法实现多虚拟主机了，Apache还支持基于不同的域名、返回不同的站点资源。

![](notes/image-20200116201438930.png)

这种配置方式，需要配置多个域名，可以使用本地hosts文件，也可以配置DNS记录。

1、首先要在客户端配置好hots文件

```shell
mac的hosts文件路径  /etc/hosts
windows的hosts文件路径  C:\Windows\System32\drivers\etc\hosts

1.以windows10为例子、以管理员身份编辑C:\Windows\System32\drivers\etc\hosts文件、内容如下

# Copyright (c) 1993-2009 Microsoft Corp.
#
# This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
#
# This file contains the mappings of IP addresses to host names. Each
# entry should be kept on an individual line. The IP address should
# be placed in the first column followed by the corresponding host name.
# The IP address and the host name should be separated by at least one
# space.
#
# Additionally, comments (such as these) may be inserted on individual
# lines or following the machine name denoted by a '#' symbol.
#
# For example:
#
#      102.54.94.97     rhino.acme.com          # source server
#       38.25.63.10     x.acme.com              # x client host
       192.168.0.200    www.dnf.com									添加这三个iP和域名对应的关系
       192.168.0.201    www.cf.com
       192.168.0.202    www.lol.com

# localhost name resolution is handled within DNS itself.
#	127.0.0.1       localhost
#	::1             localhost


2.以linux客户端为例、编辑/etc/hosts文件、内容如下
vim /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain
192.168.0.200 www.dnf.com
192.168.0.201 www.cf.com
192.168.0.202 www.lol.com


```

2.由于前面创建了网页文件、这里直接修改httpd.conf配置文件

```shell
为每块ip设置对应的域名

#第一块IP配置区域
<VirtualHost 192.168.0.200>
DocumentRoot /www/dnf
ServerName www,dnf.com
<Directory /www/dnf >
AllowOverride None
Require all granted
</Directory>
</VirtualHost>


#第二块IP配置区域
<VirtualHost 192.168.0.201>
DocumentRoot /www/cf
ServerName www.cf.com
<Directory /www/cf >
AllowOverride None
Require all granted
</Directory>


#第三块IP配置区域
<VirtualHost 192.168.0.202>
DocumentRoot /www/lol
ServerName www.lol.com
<Directory /www/lol >
AllowOverride None
Require all granted
</Directory>
</VirtualHost>

```

4.通过域名访问即可

```shell
1.windows客户端可通过浏览器分别访问三个域名网址

2.linux客户但可通过命令获取网页
[root@localhost etc]# curl  www.dnf.com
欢迎访问192.168.0.200，我是dnf站点，你好
[root@localhost etc]# curl  www.cf.com
欢迎访问192.168.0.201，我是cf站点，你好
[root@localhost etc]# curl  www.lol.com
欢迎访问192.168.0.201，我是lol站点，你好

```

#### 4.7apache资源限制访问

案例

拒绝其他人访问站点资源

<img src="notes/image-20200116213944629.png" style="zoom: 33%;" />

```shell
1.创建一个资源文件，禁止用户访问
[root@chaogelinux ~]# mkdir -p /www/denyhtml/
[root@chaogelinux ~]# echo "小样，想偷看我？" > /www/denyhtml/index.html

2.修改httpd配置文件
<Directory "/www/99" >
Order allow,deny
#Allow from 192.168.178.0/24
</Directory>
</VirtualHost>

3.打开注释，允许某个网段访问

4.重启httpd
[root@chaogelinux ~]# systemctl restart httpd

5.即可访问资源了
```

**注意！httpd的版本问题**

```shell
apache目录站点 访问限制总结：

2.4之前版本的：

语法是
Order Deny,allow
Allow from 192.168.178.190		允许某个IP可以访问
Deny from 192.168.0.102			决绝某个IP不能访问

2.4版本开始：

不再使用上述的语法了

改为 
Require all granted  # 允许所有ip访问
Require ip 192.168.178.190  #只允许某个ip访问
Require ip 192.168.178.0/24 # 允许某个我那网段访问
```



【定义访客日志格式】

- 有时候我们需要定制apache默认显示的日志格式，增加或者减少日志记录的内容，更好的让运维人员掌握用户访问信息。

- 并且日志可能会给系统造成大量的IO操作，造成较多的负担，如果关闭日志功能，甚至可能提高40%的性能，那当然是不能关闭，而是调整日志级别。

  

- 查看httpd配置文件

```shell
ErrorLog "logs/error_log"

#
# LogLevel: Control the number of messages logged to the error_log.
# Possible values include: debug, info, notice, warn, error, crit,
# alert, emerg.
#
LogLevel warn												表示日志级别为警告

<IfModule log_config_module>
    #
    # The following directives define some format nicknames for use with
    # a CustomLog directive (see below).
    #
    LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
    LogFormat "%h %l %u %t \"%r\" %>s %b" common

    <IfModule logio_module>
      # You need to enable mod_logio.c to use %I and %O
      LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio
    </IfModule>

    #
    # The location and format of the access logfile (Common Logfile Format).
    # If you do not define any access logfiles within a <VirtualHost>
    # container, they will be logged here.  Contrariwise, if you *do*
    # define per-<VirtualHost> access logfiles, transactions will be
    # logged therein and *not* in this file.
    #
    #CustomLog "logs/access_log" common

    #
    # If you prefer a logfile with access, agent, and referer information

```

【日志级别】

```shell
# LogLevel: Control the number of messages logged to the error_log.
# Possible values include: debug, info, notice, warn, error, crit,
# alert, emerg.
#
LogLevel warn

#解释
emerg 紧急 - 系统无法使用。 "Child cannot open lock file. Exiting"  
alert 必须立即采取措施。 "getpwuid: couldn't determine user name from uid"  
crit 致命情况。 "socket: Failed to get a socket, exiting child"  
error 错误情况。 "Premature end of script headers"  
warn 警告情况。 "child process 1234 did not exit, sending another SIGHUP"  
notice 一般重要情况。 "httpd: caught SIGBUS, attempting to dump core in ..."  
info 普通信息。 "Server seems busy, (you may need to increase StartServers, or Min/MaxSpareServers)..."  
debug 出错级别信息 "Opening config file ..."
```



【日志格式】

```shell
 LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined #组合日志
    LogFormat "%h %l %u %t \"%r\" %>s %b" common    #通用日志格式

%…a: 远程IP地址
%…A: 本地IP地址
%…B: 已发送的字节数，不包含HTTP头
%…b: CLF格式的已发送字节数量，不包含HTTP头。例如当没有发送数据时，写入‘-’而不是0。
%…{FOOBAR}e: 环境变量FOOBAR的内容
%…f: 文件名字
%…h: 远程主机
%…H 请求的协议
%…{Foobar}i: Foobar的内容，发送给服务器的请求的标头行。
%…l: 远程登录名字（来自identd，如提供的话）
%…m 请求的方法
%…{Foobar}n: 来自另外一个模块的注解“Foobar”的内容
%…{Foobar}o: Foobar的内容，应答的标头行
%…p: 服务器响应请求时使用的端口
%…P: 响应请求的子进程ID。
%…q 查询字符串（如果存在查询字符串，则包含“?”后面的部分；否则，它是一个空字符串。）
%…r: 请求的第一行，如 "GET / HTTP/1.1"
%…>s: 状态。对于进行内部重定向的请求，这是指*原来*请求 的状态。如果用%…>s，则是指后来的请求。
%…t: 以公共日志时间格式表示的时间（或称为标准英文格式）
%…{format}t: 以指定格式format表示的时间
%…T: 为响应请求而耗费的时间，以秒计
%…D: Apache 2.0 开始，提供了一个新的参数 %D。可以记录服务器处理请求的微秒时间
%…u: 远程用户（来自auth；如果返回状态（%s）是401则可能是伪造的）
%…U: 用户所请求的URL路径
%…v: 响应请求的服务器的ServerName
%…V: 依照UseCanonicalName设置得到的服务器名字
%…{Referer}i：请求报文中首部“referer”的值；即从哪个页面中的超链接跳转至当前页面的；
%…{User-Agent}i：请求报文中首部“User-Agent”的值；即发出请求的应用程序；
```

【日志显示格式】

```shell
192.168.178.1 - - [17/Jan/2020:09:24:14 +0800] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X) AppleWebKit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1"


192.168.178.1 - - [17/Jan/2020:09:24:24 +0800] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Linux; Android 5.0; SM-G900P Build/LRX21T) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.117 Mobile Safari/537.36"

192.168.178.1 - - [17/Jan/2020:09:24:35 +0800] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Linux; Android 8.0.0; Pixel 2 XL Build/OPD1.170816.004) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.117 Mobile Safari/537.36"
```

![](notes/image-20200117094349563.png)

#### 4.8apache状态页与ad命令

【status】状态页

对于运维人员来说，需要实时监控httpd实时运行情况，负载情况，连接数等，能够更好的掌握服务器情况，需要在编译安装apache的时候，开启mod_status模块

```shell
/etc/httpd/conf/httpd.conf配置文件中添加状态参数

<Location /server-status>
SetHandler server-status
<RequireAll>
Require ip 192.168.0.0/24
</RequireAll>
</Location>

重启httpd 
访问状态页路径
http://192.168.0.106/server-status
```

【状态页参数解析】

<img src="notes/image-20200117103329820.png" style="zoom: 33%;" />

【针对apache进行压力测试】

使用Apache自带的压力测试工具

```shell
1.可能需要单独安装软件包
[root@localhost ~]# yum install httpd-tools -y
```

【ad命令常用参数】

```shell
此外，我们再根据上面的用法介绍界面来详细了解每个参数选项的作用。

-n    即requests，用于指定压力测试总共的执行次数。
-c    即concurrency，用于指定的并发数。
-t    即timelimit，等待响应的最大时间(单位：秒)。
-b    即windowsize，TCP发送/接收的缓冲大小(单位：字节)。
-p    即postfile，发送POST请求时需要上传的文件，此外还必须设置-T参数。
-u    即putfile，发送PUT请求时需要上传的文件，此外还必须设置-T参数。
-T    即content-type，用于设置Content-Type请求头信息，例如：application/x-www-form-urlencoded，默认值为text/plain。
-v    即verbosity，指定打印帮助信息的冗余级别。
-w    以HTML表格形式打印结果。
-i    使用HEAD请求代替GET请求。
-x    插入字符串作为table标签的属性。
-y    插入字符串作为tr标签的属性。
-z    插入字符串作为td标签的属性。
-C    添加cookie信息，例如："Apache=1234"(可以重复该参数选项以添加多个)。
-H    添加任意的请求头，例如："Accept-Encoding: gzip"，请求头将会添加在现有的多个请求头之后(可以重复该参数选项以添加多个)。
-A    添加一个基本的网络认证信息，用户名和密码之间用英文冒号隔开。
-P    添加一个基本的代理认证信息，用户名和密码之间用英文冒号隔开。
-X    指定使用的和端口号，例如:"126.10.10.3:88"。
-V    打印版本号并退出。
-k    使用HTTP的KeepAlive特性。
-d    不显示百分比。
-S    不显示预估和警告信息。
-g    输出结果信息到gnuplot格式的文件中。
-e    输出结果信息到CSV格式的文件中。
-r    指定接收到错误信息时不退出程序。
-h    显示用法信息，其实就是ab -help。
```

案例

```shell
1.发出100个并发数，总共发出10000个请求
[root@localhost ~]# ab -c 100 -n 100000 http://192.168.0.106/

2.检查状态页的信息
http://192.168.0.106ver-status
```

#### 4.9curl命令实践

curl是基于URL语法在命令行下工作的传输工具，支持诸多协议，FTP、HTTP、HTTPS、等。

```shell
-A/--user-agent <string>              设置用户代理发送给服务器
-b/--cookie <name=string/file>    cookie字符串或文件读取位置
-c/--cookie-jar <file>                    操作结束后把cookie写入到这个文件中
-C/--continue-at <offset>            断点续转
-D/--dump-header <file>              把header信息写入到该文件中
-e/--referer                                  来源网址
-f/--fail                                          连接失败时不显示http错误
-o/--output                                  把输出写到该文件中
-O/--remote-name                      把输出写到该文件中，保留远程文件的文件名
-r/--range <range>                      检索来自HTTP/1.1或FTP服务器字节范围
-s/--silent                                    静音模式。不输出任何东西
-T/--upload-file <file>                  上传文件
-u/--user <user[:password]>      设置服务器的用户和密码
-w/--write-out [format]                指定输出内容
-x/--proxy <host[:port]>              在给定的端口上使用HTTP代理
-#/--progress-bar                        进度条显示当前的传送状态
```

1、保存网页html元素

```shell
[root@localhost ~]# curl www.baidu.com>/tmp/index.html
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  2381  100  2381    0     0  40482      0 --:--:-- --:--:-- --:--:-- 41051
```



2、参数保存网页

同于第一条命令，-o 指定文件名字

```shell
[root@localhost ~]# curl -o /tmp/apache.html www.dnf.com
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  6294    0  6294    0     0  54896      0 --:--:-- --:--:-- --:--:-- 55210
```



3、测试网页返回值

- -o 输出内容到文件
- -s 不输出页面内容
- -w 指定输出内容

```shell
[root@localhost ~]# curl -o /dev/null -s -w %{http_code} www.pythonav.cn
000[root@localhost ~]# 
```



4、保存站点的cookie

```shell
[root@localhost ~]# curl -c cookies.txt www.baidu.com
[root@localhost ~]# cat cookies.txt 
# Netscape HTTP Cookie File
# http://curl.haxx.se/docs/http-cookies.html
# This file was generated by libcurl! Edit at your own risk.

.baidu.com	TRUE	/	FALSE	1601467092	BDORZ	27315

```



5、模拟客户端

伪装user-agent

```shell
[C:\~]$ curl -A "Mozilla/5.0 (iPhone; CPU iPhone OS 13_2_3 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/13.0.3 Mobile/15E148 Safari/604.1" 192.168.0.106
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100    22  100    22    0     0     22      0  0:00:01 --:--:--  0:00:01 22000

```



6、下载资源

-O 大写字母O参数，直接保存资源到本地，用源文件名

```shell
！  anaconda-ks.cfg  cookies.txt
[root@localhost ~]# curl -O http://hcdn1.luffycity.com/static/frontend/public_class/PY1@2x_1566529821.1110113.png
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   269  100   269    0     0    563      0 --:--:-- --:--:-- --:--:--   562
[root@localhost ~]# ls
！  anaconda-ks.cfg  cookies.txt  PY1@2x_1566529821.1110113.png
```



7、断点续传

```

```

#### 4.10黄金架构LAMP的介绍.

1、什么是lamp？

```shell
Linux 操作系统
Apache/Nginx    web服务器 
Mysql/Mariadb
Perl/Php/Python
```

2、linux系统与windos系统

linux系统应用的便利：

1. 操作系统主要是提供给程序员API（程序员调用的接口用于操作文件的读写），用于构建和运行应用的一个平台。
2. Linux的特点是几乎所有的开发任务相关工具，都有很完善的支持，从底层的编译器，make编译工具，到bash脚本，git代码管理，vim编辑器，依赖管理工具等等都很齐全。
3. Linux几乎都是现有命令行，再由图形化操作接口，更容易实现自动化。
4. Linux系统主要是以开发者为中心。

windows系统：

1. Windows主要以消费者为中心
2. WIndows几乎都是图形化接口





3、Apache

Apache Web Server虽然称之为`web服务器`，但是不是意味着他是一个`物理服务器`，它只是电脑软件中的一个软件而已，Web服务器的作用是将HTTP请求从前端转发到后端应用上。

<img src="notes/image-20200118161748267.png" style="zoom:50%;" />



4、PHP

- PHP是一门服务端脚本编程语言，主要用于web开发，常用PHP脚本嵌入HTML源码中执行。
- PHP是全球知名的编程语言之一，程序员可以免费试用，PHP支持多种操作系统，开发效率高，支持多种数据库操作。
- 国内众多网站，百度、雅虎、新浪都在大量使用PHP语言进行开发，知名的论坛软件Discuz也是由PHP开发且占据了80%的论坛软件市场。

<img src="notes/image-20200118161810191.png" style="zoom:50%;" />

5、Mysql

- Mysql是一款数据库管理系统，也就是一个存储数据的工具，用户可以自行对数据库进行增加、删除、修改、查询等操作。
- MySQL是数据库管理系统中的一款软件，被业界广泛使用，例如新浪、QQ、淘宝、都在大量使用MySQL数据库。
- 腾讯QQ使用Linux与MySQL数据库，存储注册用户2.8亿的信息，活跃人数9000万，凭借万台服务器搭建的数据库集群，腾讯QQ同时在线人数也达到了千万，这证明了MySQL数据库的大容量、快速响应特点。
- MySQL是一款关系型数据库，尤其适合Web应用，特别是电商领域，MySQL遍布各种行业、移动、爱立信、惠普、银行、思科、摩托萝拉、等等。



6、LAMP架构

-  LAMP（Linux-Apache-MySQL-PHP）网站架构是目前国际流行的Web框架，该框架包括：Linux操作系统，Apache网络服务器，MySQL数据库，Perl、PHP或者Python编程语言，所有组成产品均是开源软件，是国际上成熟的架构框架，很多流行的商业应用都是采取这个架构，LAMP具有通用、跨平台、高性能、低价格的优势，因此LAMP无论是性能、质量还是价格都是企业搭建网站的首选平台。
-  LAMP是一个多C/S架构的平台，最初级为web客户端(浏览器)基于TCP/IP通过http协议发起传送，这个请求可能是动态的，也可能是静态的。
-  所以web服务器通过发起请求的后缀来判断，如果是静态的资源就由web服务器自行处理，然后将资源发给客户端。如果是动态这时web服务器会通过CGI（Common Gateway interface）协议发起给php。
-  这里但是如果php是以模块形式与Web服务器联系，（安装在同一台服务器）那么他们是通过内部共享内存的方式。
-  如果是php单独的放置与一台服务器，那么他们是通过sockets套接字监听的方式通信（安装在不同的服务器上，远程通信，这又是一个C/S架构）。
-  这时php会相应的执行一段程序，如果在执行程序时，需要用到数据。
-  那么php就会通过mysql协议发送给mysql服务器（也可以看作是一个C/S架构）。由mysql服务器处理，将数据供给php程序。

【流程解析】

![]()<img src="notes/image-20200203165504534.png" alt="notes/image-20200203165504534" style="zoom:50%;" />

1. 用户通过浏览器发送http请求，到达`web服务器（apache或nginx）`
2. web服务器解析用户请求信息，明确他到底要什么，如果是`静态资源请求`，直接通过linux内核读取硬盘上的数据，然后构建响应报文，给与用户。如果`是动态资源请求`，则转发请求给`应用服务器（php,python）`，由php解析动态请求，解析完毕后，返回给apache，发给用户
3. 如果涉及数据库操作，利用`php-mysql`驱动，获取数据库数据，再返回给php，最终给与用户

<img src="notes/image-20200203170245218.png" style="zoom: 50%;" />



#### 4.11LAMP系统部署

【amp环境部署】

1、关闭防火墙及selinux

```shell
[root@chaogelinux ~]# iptables -F
[root@chaogelinux ~]# systemctl stop firewalld
[root@chaogelinux ~]# systemctl disable firewalld
[root@chaogelinux ~]# getenforce
Disabled
```

2、下载apache包

```shell
yum install httpd -y
```

3、启动apache服务

```shell
[root@chaogelinux ~]# systemctl start httpd 
[root@chaogelinux ~]# systemctl status httpd
```

4、浏览器访问服务器IP访问





【部署mysql（mariadb）】

1、下载安装、启动mariad服务端、客户端包

```shell
[root@local_server_myariadb  ~]# yum install mariadb-server mariadb 
[root@local_server_myariadb  ~]# systemctl start mariadb
```

2、检查mariadb的数据文件夹，存在mysql.sock文件，表示启动了，以及属主、属组权限

```shell
[root@local_server_myariadb ~]# ls -l /var/lib/mysql/
总用量 37852
-rw-rw----. 1 mysql mysql    16384 9月  30 14:19 aria_log.00000001
-rw-rw----. 1 mysql mysql       52 9月  30 14:19 aria_log_control
-rw-rw----. 1 mysql mysql 18874368 9月  30 14:19 ibdata1
-rw-rw----. 1 mysql mysql  5242880 9月  30 14:19 ib_logfile0
-rw-rw----. 1 mysql mysql  5242880 9月  30 14:19 ib_logfile1
drwx------. 2 mysql mysql     4096 9月  30 14:19 mysql
srwxrwxrwx. 1 mysql mysql        0 9月  30 14:19 mysql.sock
drwx------. 2 mysql mysql     4096 9月  30 14:19 performance_schema
drwx------. 2 mysql mysql        6 9月  30 14:19 test

```

3、登录mariadb数据库

```shell
[root@local_server_myariadb ~]# mysql -uroot -p
Enter password: 
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 2
Server version: 5.5.65-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> show 
```



4、设置mariadb用户root的登录密码

```shell
[root@chaogelinux ~]# mysqladmin -uroot password "chaoge666"
再次登录必须输入正确密码
[root@chaogelinux ~]# mysql -uroot -pchaoge666
```



【php部署】

1、解决php安装的依赖开发环境

```shell
[root@local_php ~]#  yum install -y zlib-devel libxml2-devel libjpeg-devel libjpeg-turbo-devel libiconv-devel freetype-devel libpng-devel gd-devel libcurl-devel libxslt-devel libtool-ltdl-devel pcre pcre-devel apr apr-devel zlib-devel gcc make -y
```

2、安装php，以及php连接mysql数据库的驱动

```shell
[root@local_php /]# yum install php php-mysql -y
```

3、php不需要额外修改，但是需要修改apache配置文件，支持php

```shell
[root@chaogelinux ~]# cat /etc/httpd/conf/httpd.conf  #添加如下相关配置

DocumentRoot "/var/www/html"											#定义apache的网页根目录、网站资料存放位置首页html文件
    TypesConfig /etc/mime.types											#让apache识别php程序以.php、.phps结尾的请求
    AddType application/x-httpd-php  .php
    AddType application/x-httpd-php-source  .phps
    DirectoryIndex  index.php index.html								#返回默认的页面文件 index.php（手动编写php代码）
```

4、修改首页文件内容

```shell
[root@local_server_apache html]# cat /var/www/html/index.php 
<meta charset=utf-8>
我是新的首页，你好兄弟们
<?php
phpinfo();
?>
#访问道以下界面说明可与php联通
```

<img src="notes/image-20200203204746123.png" style="zoom:33%;" />

【测试php连接mysql】

```shell
1.添加php脚本
[root@chaogelinux www]# cat /php_scripts/.php
<?php
     $conn = mysql_connect('localhost','root','chaoge666');
 if ($conn)
   echo "php已成功连接mysql，你真棒";
 else
   echo "你咋回事，这都搞不定，细心检查下吧";
 mysql_close();
?>
```

访问php脚本文件，测试是否能够连接mysql数据库





若是关闭了数据库，或者出现其他配置错误问题，则会显示如下页面







#### 4.12.基于LAMP搭建论坛

Crossday Discuz! Board是一套通用的社区论坛软件系统，用户可以在不需要任何编程的基础上，通过简单的设置和安装，搭建起具备完善功能、很强负载能力和可高度定制的论坛服务。

Discuz! 的基础架构采用世界上最流行的web编程组合PHP+MYSQL实现，是一个经过完善设计，适用于各种服务器环境的高效论坛系统解决方案。

<img src="notes/image-20200203212611021.png" style="zoom: 50%;" />

```shell
1.下载discuz源码包
wget http://download.comsenz.com/DiscuzX/3.2/Discuz_X3.2_SC_UTF8.zip

2.安装解压缩命令，解压缩源代码
yum install unzip -y 
[root@chaogelinux www]# unzip Discuz_X3.2_SC_UTF8.zip

3.把解压出的upload文件，拷贝到apache的根目录下
[root@chaogelinux www]# mv upload/* /var/www/html					#httpd.conf内的Doucmen指定目录
mv：是否覆盖"./index.php"？ y

4.给与最高权限，便于实验
[root@chaogelinux www]# chmod -R 777 /www/*
```

访问apache首页，查看是否进入论坛安装界面

```shell
http://192.168.0.108/install/
```

然后通过安装向导一步一步安装

<img src="notes/image-20200203211409364.png" style="zoom:33%;" />

<img src="notes/image-20200203211525362.png" style="zoom:33%;" />

<img src="notes/image-20200203211541640.png" style="zoom:33%;" />

<img src="notes/image-20200203211705352.png" style="zoom: 33%;" />

<img src="notes/image-20200203211724668.png" style="zoom:33%;" />

