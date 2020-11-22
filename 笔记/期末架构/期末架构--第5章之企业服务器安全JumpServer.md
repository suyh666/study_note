# 四、期末架构



## 第五章 企业服务之Jumpserver

### 5.1、网站架构中为甚么使用跳板机

#### 为什么需要堡垒机

![image-20200803171247396](C:\Users\admin\Desktop\notes\image-20200803171247396.png)

近年来数据安全事故频发，包括斯诺登事件、希拉里邮件丑闻以及携程宕机事件等，数据安全与防止泄露成为政府和企业都非常关心的议题，因此堡垒机也应运而生。

**案例一**

让我们共同回顾最具代表性的数据泄露引发的安全事故，美国著名的斯诺登事件。2013年6月，美国《华盛顿邮报》报道，美国国家安全局和联邦调查局于2007年启动了一个代号为“棱镜”的秘密监控项目，直接进入美国网际网路公司的中心服务器里挖掘数据、收集情报。洩露这些绝密文件的并非国家安全局的内部员工，而是国家安全局的外聘人员爱德华·斯诺登。

斯诺登事件若放在今天，将不可能发生，因为我们有了堡垒机！其中的管理员角色可以设置敏感操作的事前拦截、事中断开、事后审计，并且可以做到全程无代理实时监控。类似斯诺登这样的外聘人员将无法接触到这些敏感信息，更不用说泄露出来了。并且某些堡垒机支持录屏功能也可以帮助用户进行审计和追责。

**案例二**

2015年5月28日上午11点至晚上8点，在某旅游出行平台官网及APP上登录、下单或交易时，跳转均出现问题，导致操作无法顺利完成。造成直接经济损失巨大，按照其上一季度的财报公布的数据，宕机的损失为平均每小时106.48万美元。

最终，平台回应此事称系由于员工误操作删除了服务器上的执行代码导致。不论是因为黑客攻击还是员工误操作，真金白银800万美元的经验教训告诫我们对于数据的安全和备份必须要引起重视！堡垒机能解决这2个问题，一是攻击面小，二是可定制双机备份。



### 5.2、堡垒机的核心价值





### 5.3、跳板机与堡垒机的区别

#### 跳板机

跳板机就是一台服务器，运维人员在维护过程中首先要统一登录到这台服务器，然后再登录到目标设备进行维护和操作；

在腾讯，跳板机是开发者登录到服务器的唯一途径，开发者必须先登录跳板机，再通过跳板机登录到应用服务器。

跳板机属于内控堡垒机范畴，是一种用于单点登陆的主机应用系统。跳板机就是一台服务器，维护人员在维护过程中，首先要统一登录到这台服务器上，然后从这台服务器再登录到目标设备进行维护。但跳板机并没有实现对运维人员操作行为的控制和审计，此外，跳板机存在严重的安全风险，一旦跳板机系统被攻入，则将后端资源风险完全暴露无遗。

#### 跳板机的优缺点

优势：集中式进行管理

缺点：

没有实现对运维人员操作行为的控制和审计，使用跳板机的过程中还是会出现误操作，违规操作等导致的事故，一旦出现操作事故很难定位到原因和责任人。

#### 运维思想

- 审计是事后行为，可以发现问题及责任人，但是无法防止问题发生
- 只有事先严格控制，才能从源头解决根本问题。
- 系统账号的作用只是区分工作角色，但是无法确认该执行人的身份

### 5.4、堡垒机的核心作用

#### 堡垒机

**堡垒机的理念起于跳板机；**

人们逐渐认识到跳板机的不足，需要更新、更好的安全技术理念来实现运维操作管理，需要一种能满足角色管理与授权审批、信息资源访问控制、操作记录和审计、系统变更和维护控制要求，并生成一些统计报表配合管理规范来不断提升IT内控的合规性的产品。

2005年前后，运维堡垒机开始以一个独立的产品形态被广泛部署，有效地降低了运维操作风险，使得运维操作管理变得更简单、更安全。

2005年齐治科技研发出世界第一台运维堡垒机；（齐治科技官网http://www.shterm.com/）

#### 堡垒机作用

1）核心系统运维和安全审计管控；

2）过滤和拦截非法访问、恶意攻击，阻断不合法命令，审计监控、报警、责任追踪；

3）报警、记录、分析、处理；



### 5.5、堡垒机的价值



#### 堡垒机核心功能

1）单点登录功能

支持对X11、Linux、Unix、数据库、网络设备、安全设备等一系列授权账号进行密码的自动化周期更改，简化密码管理，让使用者无需记忆众多系统密码，即可实现自动登录目标设备，便捷安全；

2）账号管理

设备支持统一账户管理策略，能够实现对所有服务器、网路设备、安全设备等账号进行集中管理，完成对账号整个生命周期的监控，并且可以对设备进行特殊角

设置，如：审计巡检员、运维操作员、设备管理员等自定义，以满足审计需求；

3）身份认证

设备提供统一的认证接口，对用户进行认证，支持身份认证模式包括动态口令、静态密码、硬件key、生物特征等多种认证方式，设备具有灵活的定制接口，可以与其他第三方认证服务器直接结合；

安全的认证模式，有效提高了认证的安全性和可靠性；

4）资源授权

设备提供基于用户、目标设备、时间、协议类型IP、行为等要素实现细粒度的操作授权，最大限度保护用户资源的安全；

5）访问控制

设备支持对不同用户进行不同策略的制定，细粒度的访问控制能够最大限度的

保护用户资源的安全，严防非法、越权访问事件的发生；

6）操作审计

设备能够对字符串、图形、文件传输、数据库等安全操作进行行为审计；通过设备录像方式监控运维人员对操作系统、安全设备、网络设备、数据库等进行的各种操作，对违规行为进行事中控制；对终端指令信息能够进行精确搜索，进行录像精确定位

#### 堡垒机应用场景

1）多个用户使用同一账号

多出现在同一工作组中，由于工作需要，同时系统管理员账号唯一，因此只能多用户共享同一账号；如果发生安全事故，不仅难以定位账号的实际使用者和责任人，而且无法对账号的使用范围进行有效控制，存在较大的安全风险和隐患；

2）一个用户使用多个账号。

目前一个维护人员使用多个账号时较为普遍的情况，用户需要记忆多套口令同时在多套主机系统、网络设备之间切换，降低工作效率，增加工作复杂度；

3）缺少统一的权限管理平台，难以实现更细粒度的命令权限控制。

维护人员的权限大多是粗放管理，无基于最小权限分配原则的用户权限管理，难以实现更细粒度的命令权限控制，系统安全性无法充分保证；

4）无法制定统一的访问审计策略，审计粒度粗。

各个网络设备、主机系统、数据库是分别单独审计记录访问行为，由于没有统一审计策略，而且各系统自身审计日志内容深浅不一，难以及时通过系统自身审计发现违规操作行为和追查取证；

5）传统的网路安全审计系统无法对维护人员经常使用的SSH、RDP等加密、图形操作协议进行内容审计。

#### 目标价值

1）目标

堡垒机的核心思路是逻辑上将人与目标设备分离，建立“人-〉主账号（堡垒机用户账号）-〉授权—>从账号（目标设备账号）的模式;在这种模式下，基于唯一身份标识，通过集中管控安全策略的账号管理、授权管理和审计，建立针对维护人员的“主账号-〉登录—〉访问操作-〉退出”的全过程完整审计管理，实现对各种运维加密/非加密、图形操作协议的命令级审计。

2）系统价值

堡垒机的作用主要体现在下述几个方面：

#### 企业角度

通过细粒度的安全管控策略，保证企业的服务器、网络设备、数据库、安全设备等安全可靠运行，降低人为安全风险，避免安全损失，保障企业效益。

#### 管理员角度

所有运维账号的管理在一个平台上进行管理，账号管理更加简单有序；

通过建立用户与账号的唯一对应关系，确保用户拥有的权限是完成任务所需的最小权限；

直观方便的监控各种访问行为，能够及时发现违规操作、权限滥用等。

鉴于多账号同时使用超管进行的操作，便于实名制的认证和自然人的关联。

#### 普通用户角度

运维人员只需记忆一个账号和口令，一次登录，便可实现对其所维护的多台设备的访问，无须记忆多个账号和口令，提高了工作效率，降低工作复杂度。

#### 应用

一种用于单点登陆的主机应用系统，目前电信、移动、联通三个运营商广泛采用堡垒机来完成单点登陆。

在银行、证券等金融业机构也广泛采用堡垒机来完成对财务、会计操作的审计。

在电力行业的双网改造项目后，采用堡垒机来完成双网隔离之后跨网访问的问题，能够很好的解决双网之间的访问的安全问题。

#### 相关厂商

目前，已经有相当多的厂商开始涉足这个领域，如：思福迪、帕拉迪、圣博润、尚思卓越、绿盟、[3]科友、齐治、金万维、极地、派拉等，这些都是目前行业里做的专业且受到企业用户好评的厂商，但每家厂商的产品所关注的侧重又有所差别。

以某运维安全审计产品为例，其产品更侧重于运维安全管理，它集单点登录、账号管理、身份认证、资源授权、访问控制和操作审计为一体的新一代运维安全审计产品，它能够对操作系统、网络设备、安全设备、数据库等操作过程进行有效的运维操作审计，使运维审计由事件审计提升为操作内容审计，通过系统平台的事前预防、事中控制和事后溯源来全面解决企业的运维安全问题，进而提高企业的IT运维管理水平。

#### 案例

#### 某连锁酒店企业

##### 客户现状及需求：

IT系统分散在总部以及全国各地的分支连锁酒店，每个酒店所在的地区都有相应的技术人员进行系统运维；总部也有运维人员，对全国IT系统的总的运维质量负最终责任。

酒店实体越来越 多,总部的IT运维工作日益复杂，运维问题日益突出。一个最基础的场景是：当某酒店的IT系统出现问题，当地的IT运维人员无法解决时，就会向总部发起求 助。而此时，总部的技术工程师根本无法获悉最原始的问题，因为原来的问题在经过分部的运维工程师的操作后，已经面目全非，还可能引入了新的问题，整个过程没有记录，没有管控，找不到解决问题的线索。所以总部工程师迫切希望知道，从一开始问题的表象，到分支机构的运维人员的运维操作，都是怎么一回事。除此之外，还有另外的一些运维问题列表如下：

1、运维人员管理手段落后，时无法定责，也无法对各方的运维工作本身的质量和数量进行有效考核和评估。

2、设备账户管理缺失，该连锁酒店的每一名运维人员都要负责多套信息系统的运维管理工作，同时，大多数情况下，某套信息系统往往要多个运维人员联合管理。在这种情况下，口令丢失、登录失败、密码被随便修改等情况就时有发生。并且对第三方代维人员来说，也没有更强的针对设备账号的监测机制和有效的生命周期管理机制；

##### 解决之道：

来进行统一认证，认证成功后对其具有权限的IT设备进行运维操作。整个运维过程全程录像，并有危险操作的告警及阻断功能。

通 过这种“跳板机”的解决方案，运维人员只需要记住一个口令就可以运维到被授权的设备，运维过程全程录像，且可以对应到运维人员。使用运维审计集中管理客户端软件，分散在全国各地的酒店IT系统的运维录像可以被总部的运维人员随时查询，还可以通过播放器进行远程的录像流畅播放。

##### 客户收益:

运维安全审计堡垒平台之后，所有的运维人员都以统一的用户身份登入系统；所有的运维操作都被记录；操作对应到实际的自然人而不是设备账户。在出现问题后，可以迅速的调出运维操作录像进行查看，根据录像进行问题的追本溯源，直接定位问题根源所在，为解决IT系统的故障提供了宝贵的第一手资料。

在部署安全审计堡垒平台之后，问题的解决时间平均缩短到一到两个小时，数量级的提高了运维工作质量。另外，由于有录像可以学习，交流和借鉴，从一定程度上，提高了所有运维人员的运维经验。



### 5.6、堡垒机总结

### 简单总结堡垒机

简单来说，堡垒机的核心价值就是用来解决“运维权限混乱，操作无审计”的问题。当我们的公司运维人员越来越多，所需要管理的主机资源也越来越多时，管理跟技术手段不到位所带来的运维安全风险也越来越高，这个时候我们应该怎么做呢？

• 首先，在事前，我们需要规定用户所能访问资源的权限，使权限实现最小化

打个比方，我们把主机资源看作是一个房子，那我们的运维人员就是一个客人，普通客人只能在我们里面客厅活动，如果说他要进入卧室，那需要主人的同意。

• 第二，在事中的时候，我们需要实时监控我们运维人员的操作，一旦发现有危险操作时可以进行阻断。就好比，在房间里面装了一个“摄像头”，我们可以看到客人在房间里面的一举一动。

• 事后，我们需要追溯溯源，还原事故现场；

客人在“房间”里的一举一动，都会被录制下来，方便回放查看。

政府、企业，以及其他各行各业都会面临着运维安全风险：

• 第一，就是来源身份定位难：当多个运维人员使用相同的账号对主机进行操作时，我们的管理者无法区分谁是谁，一旦发生运维事故，很难快速地定位“责任人”。

• 第二，就是操作过程不透明：顾名思义，就是指我们的运维人员做了什么管理者无从得知？尤其是在第三方运维场景，我们经常会听到数据窃取、违规操作导致服务器异常等此类的新闻，我们缺乏相应的手段进行规避。

• 第三，账号共享、权限泛滥：我们知道人是最难管理的，一旦掌握了主机的账号密码，就不可避免地会造成账号的泄露，最终导致权限泛滥、难以管理。

• 第四，是合规性要求。无论是网络安全法、等级保护，还是行业的法律法规，都有要求我们建设安全、完善的审计系统，从而确保我们的审计信息是完整、安全、可靠。

针对上述我们提到的运维安全风险，安恒堡垒机是怎么解决的呢！简单来讲：

• 堡垒机给每个运维人员分配了一个堡垒机账户，确保身份的唯一，其次，利用堡垒机的单点登录能力，使我们的运维人员无需知道主机的账号密码，从而可以有效防止账号泄漏和共享，并且运维人员需要访问什么主机，需要我们的管理者事先授权。

• 另外，我们通过实时监控、录像回放等方式，也有效地解决了操作过程不透明的问题。

• 总的来讲，安恒堡垒机通过一定的技术手段实现了事前的授权、事中的监控、事后的审计，满足合规性的要求





### 5.7、JumpServer核心架构

#### JumpServer

![image-20200731154715994](C:\Users\admin\Desktop\notes\image-20200731154715994.png)

官网文档：https://docs.jumpserver.org/zh/master/

#### 部署堡垒机

#### 组件介绍

![image-20200803171414426](C:\Users\admin\Desktop\notes\image-20200803171414426.png)

#### 核心架构

![image-20200803171459645](C:\Users\admin\Desktop\notes\image-20200803171459645.png)

#### 安装环境

![image-20200803171649786](C:\Users\admin\Desktop\notes\image-20200803171649786.png)

服务器硬件环境

![image-20200803171808556](C:\Users\admin\Desktop\notes\image-20200803171808556.png)



### 5.8、JumpServer服务器初始化配置



#### 基本环境准备

```shell
JumpServer 环境要求:

硬件配置: 2个CPU核心, 4G 内存, 50G 硬盘（最低）
操作系统: Linux 发行版 x86_64

Python = 3.6.x
Mysql Server ≥ 5.6
Mariadb Server ≥ 5.5.56
Redis
```

#### 非常重要

一定注意，若是基础环境，没有正确安装，后面编译安装软件，会报错的

```shell
1.环境准备
    centos7
    关闭防火墙 firewalld selinux

    iptables -F
    systemctl stop firewalld
    systemctl disable firewalld



# yum源配置
    wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
    wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

# 基础环境安装
    yum install -y bash-completion vim lrzsz wget expect net-tools nc nmap tree dos2unix htop iftop iotop unzip telnet sl psmisc nethogs glances bc ntpdate  openldap-devel

2.第一个里程：需要部署跳板机依赖软件，重要
yum -y install git python-pip  gcc automake autoconf python-devel vim sshpass lrzsz readline-devel  zlib zlib-devel openssl openssl-devel


  git              --- 用于下载jumpserver软件程序
  python-pip       --- 用于安装python软件
  gcc             --- 解析代码中C语言信息（解释器）
  automake        --- 实现软件自动编译过程  
  autoconf        --- 实现软件自动配置过程
  python-devel    --- 系统中需要有python依赖
  readline-devel  --- 在操作python命令信息时，实现补全功能


3.修改系统字符集为中文
localedef -c -f UTF-8 -i zh_CN zh_CN.UTF-8
export LC_ALL=zh_CN.UTF-8
# 写入配置文件，永久生效
echo 'LANG="zh_CN.UTF-8"' > /etc/locale.conf

# 检查系统字符集
(py3) [root@jumpserver koko 10:30:23]$locale
LANG=en_US.UTF-8
LC_CTYPE="zh_CN.UTF-8"
LC_NUMERIC="zh_CN.UTF-8"
LC_TIME="zh_CN.UTF-8"
LC_COLLATE="zh_CN.UTF-8"
LC_MONETARY="zh_CN.UTF-8"
LC_MESSAGES="zh_CN.UTF-8"
LC_PAPER="zh_CN.UTF-8"
LC_NAME="zh_CN.UTF-8"
LC_ADDRESS="zh_CN.UTF-8"
LC_TELEPHONE="zh_CN.UTF-8"
LC_MEASUREMENT="zh_CN.UTF-8"
LC_IDENTIFICATION="zh_CN.UTF-8"
LC_ALL=zh_CN.UTF-8
```



### 5.9、部署Mysql5.6数据库详解

#### 部署mysql5.6

```shell
1.获取mysql5.6软件包
wget https://cdn.mysql.com//Downloads/MySQL-5.6/MySQL-5.6.49-1.el7.x86_64.rpm-bundle.tar
wget https://downloads.mysql.com/archives/get/p/23/file/mysql-5.6.49-linux-glibc2.12-x86_64.tar.gz编译包

2.解压缩
[root@jumpserver opt 17:42:14]$mkdir Mysql-rpm
# 指定目录解压
[root@jumpserver package]# tar -xf MySQL-5.6.49-1.el7.x86_64.rpm-bundle.tar  -C ./Mysql-rpm/
#有如下这些包
[root@jumpserver Mysql-rpm]# ls -lh
总用量 243M
-rw-r--r-- 1 7155 31415  21M 6月   3 13:36 MySQL-client-5.6.49-1.el7.x86_64.rpm
-rw-r--r-- 1 7155 31415 3.4M 6月   3 13:36 MySQL-devel-5.6.49-1.el7.x86_64.rpm
-rw-r--r-- 1 7155 31415  90M 6月   3 13:36 MySQL-embedded-5.6.49-1.el7.x86_64.rpm
-rw-r--r-- 1 7155 31415  67M 6月   3 13:36 MySQL-server-5.6.49-1.el7.x86_64.rpm
-rw-r--r-- 1 7155 31415 2.3M 6月   3 13:36 MySQL-shared-5.6.49-1.el7.x86_64.rpm
-rw-r--r-- 1 7155 31415 2.2M 6月   3 13:36 MySQL-shared-compat-5.6.49-1.el7.x86_64.rpm
-rw-r--r-- 1 7155 31415  58M 6月   3 13:36 MySQL-test-5.6.49-1.el7.x86_64.rpm

# yum批量安装
[root@jumpserver mysql_rpm 17:48:44]$yum localinstall ./*

3.查看mysql默认配置文件，注意如下的修改
[root@jumpserver Mysql-rpm]# cat /etc/my.cnf
[mysqld]
datadir=/var/lib/mysql				#数据库数据存储目录
socket=/var/lib/mysql/mysql.sock	#数据库启动后的进程文件
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd

[mysqld_safe]
log-error=/var/log/mysql/mysql.log				#错误日志 这里将mariadb改成mysql		
pid-file=/var/run/mysql/mysql.pid				#运行日志 这里将mariadb改成mysql		

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d



4.mysql5.6版本默认会生成随机密码，密码文件在
/root/.mysql_secret
[root@jumpserver Mysql-rpm]# cat /root/.mysql_secret
# The random password set for the root user at Tue Nov  3 10:51:47 2020 (local time): YyQUc89U7SWWwplh

#注意这里修改之前要先启动mysql服务、否则修改密码会报错
[root@jumpserver Mysql-rpm]# systemctl start mysql

5.查看密码后，修改该密码，注意-p参数后没有空格，注意该方式是不安全的，密码会暴露
[root@jumpserver Mysql-rpm]# mysqladmin -uroot -pYyQUc89U7SWWwplh password syh666
Warning: Using a password on the command line interface can be insecure.


# 最好的方式是进入mysql后，再修改密码
[root@jumpserver mysql_rpm 18:06:52]$mysql -uroot -psyh666
mysql> update mysql.user set password=password('syh888') where user='root';
Query OK, 4 rows affected (0.00 sec)
Rows matched: 4  Changed: 4  Warnings: 0

mysql> flush privileges;   # 必须刷新后，数据库密码才会变化
Query OK, 0 rows affected (0.00 sec)

# 此时密码已经更新了
[root@jumpserver Mysql-rpm]# mysql -uroot -psyh888

6.创建jumpserver数据库，修改字符集
mysql> create database jumpserver default charset 'utf8' collate 'utf8_bin';
Query OK, 1 row affected (0.00 sec)

7.创建jumpserver普通用户
mysql> create user 'jumpserver'@'%' IDENTIFIED BY 'syh888';
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)
'%''：表示该用户可在所有机器上登录

8.给jumpserver用户授权
mysql> grant all privileges on jumpserver.* to 'jumpserver'@'%' identified by 'syh888';
mysql> flush privileges;
Query OK, 0 rows affected (0.00 sec)

grant all privileges：表示授予所有权限
on jumpserver.*:表示针对jumpserver库和它所有的表
'jumpserver'@'%'：表示对jumpserver用户、能够在所有机器登录
identified by 'syh888'：表示指定用户密码
```



### 5.10、Pyhton3编译安装与虚拟环境详解

#### 部署python3.6.10

```shell
1.下载python3.6.10源代码，可以选择用第三方工具下载后上传至linux，较为快速
#先创建pyhton3的安装包目录
[root@jumpserver /]# mkdir install

cd /install && \
wget https://www.python.org/ftp/python/3.6.10/Python-3.6.10.tgz
tar -zxf Python-3.6.10.tgz

#创建python3的安装目录
[root@jumpserver install]# mkdir /opt/python3.6.10

#进入python3解压目录进行编译三部曲
cd Python-3.6.10/

#第一曲：安装到指定目录
[root@jumpserver Python-3.6.10]# ./configure --prefix=/opt/python3.6.10

#第二曲、第三曲
make && make install


#配置环境变量
[root@jumpserver bin]# echo PATH="/opt/python3.6.10/bin:$PATH" >> /etc/profile
[root@jumpserver bin]# tail -1 /etc/profile
PATH=/opt/python3.6.10/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin
[root@jumpserver bin]# source /etc/profile

[root@jumpserver bin]# python
python             python2.7-config   python3.6          python3.6m-config  
python2            python2-config     python3.6-config   python3-config     
python2.7          python3            python3.6m         python-config  

[root@jumpserver bin]# python3
Python 3.6.10 (default, Nov  3 2020, 13:56:14) 
[GCC 4.8.5 20150623 (Red Hat 4.8.5-39)] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 


#此时就可以进入python3解释器


2.创建python虚拟环境
[root@jumpserver bin]# python3.6 -m venv /opt/py3
#激活环境变量、实际是修改了环境变量
[root@jumpserver bin]# source /opt/py3/bin/activate
(py3) [root@jumpserver bin]# which python
/opt/py3/bin/python					
(py3) [root@jumpserver bin]# which python3
/opt/py3/bin/python3 			#此时已经进入python3的虚拟环境

#退出虚拟环境
(py3) [root@jumpserver bin]# deactivate

3.更换pip下载源、用于在python加速下载模块
mkdir ~/.pip
vim ~/.pip/pip.conf
[root@jumpserver .pip]# cat ~/.pip/pip.conf
[global]
index-url =  https://mirrors.aliyun.com/pypi/simple/
```





### 5.11、创建Pyhton3虚拟环境详解

创建虚拟环境主要是用于多个项目的不同、要使用多个项目的python3环境；不能多个项目使用通过一个python环境；毕竟每个项目用所需的模块版本可能不同、同一环境运行会出现问题；

![image-20201103142209968](notes/image-20201103142209968.png)

### 5.12、部署redis

```shell
1.在线下载redis
[root@jumpserver /]# yum install redis -y

#启动redis
[root@jumpserver /]# systemctl start redis

#查看运行端口
[root@jumpserver /]# netstat -tunpl|grep redis
tcp        0      0 127.0.0.1:6379          0.0.0.0:*               LISTEN      20840/redis-server  

#进入redis
[root@jumpserver /]# redis-cli
127.0.0.1:6379> 

#确定是否连接成功
127.0.0.1:6379> ping
PONG							#发送ping返回pong则表示连接成功
```



### 5.13、JumpServer后台程序部署

#### 部署jumeserver

```shell
1.下载jumpserver程序
cd /opt && \
wget https://github.com/jumpserver/jumpserver/releases/download/v2.1.0/jumpserver-v2.1.0.tar.gz

[root@jumpserver opt]# tar xf jumpserver-v2.1.0.tar.gz
[root@jumpserver opt]# ln -s /opt/jumpserver-v2.1.0 /opt/jumpserver


2.安装jumpserver代码依赖模块 
# 可能需要再次尝试这一步
#先进入虚拟环境
[root@jumpserver opt]# source /opt/py3/bin/activate
(py3) [root@jumpserver opt]# yum install -y bash-completion vim lrzsz wget expect net-tools nc nmap tree dos2unix htop iftop iotop unzip telnet sl psmisc nethogs glances bc ntpdate  openldap-devel
(py3) [root@jumpserver opt]# cd /opt/jumpserver/requirements/
pip install wheel && \
pip install --upgrade pip setuptools && \
pip install -r requirements.txt				#安装这个文件里面的模块

```



### 5.14、JumpServer配置文件修改

jumpserver配置文件路径：/opt/jumpserver/config.yml    要复制默认配置文件生成

```shell
# 备份配置文件
cd /opt/jumpserver && \
cp config_example.yml config.yml


#查看原配置文件
(py3) [root@jumpserver jumpserver]# grep -Ev '^#|^$' /opt/jumpserver/config.yml 
SECRET_KEY:						#要生成
BOOTSTRAP_TOKEN:				#要生成
DB_ENGINE: mysql
DB_HOST: 127.0.0.1
DB_PORT: 3306
DB_USER: jumpserver
DB_PASSWORD: 						#mysql数据库密码
DB_NAME: jumpserver
HTTP_BIND_HOST: 0.0.0.0
HTTP_LISTEN_PORT: 8080
WS_LISTEN_PORT: 8070
REDIS_HOST: 127.0.0.1
REDIS_PORT: 6379

```

```shell
#生成随机加密密钥
if [ "$SECRET_KEY" = "" ]; then SECRET_KEY=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 50`; echo "SECRET_KEY=$SECRET_KEY" >> ~/.bashrc; echo $SECRET_KEY; else echo $SECRET_KEY; fi

yN5BNda0Ty7TLNbKMUtZflBhmYGGpEotwFTuWgzsICBnotuliH

if [ "$BOOTSTRAP_TOKEN" = "" ]; then BOOTSTRAP_TOKEN=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 16`; echo "BOOTSTRAP_TOKEN=$BOOTSTRAP_TOKEN" >> ~/.bashrc; echo $BOOTSTRAP_TOKEN; else echo $BOOTSTRAP_TOKEN; fi
gWagtYEDkwG2LSJe


#修改配置文件
(py3) [root@jumpserver jumpserver]# grep -Ev '^#|^$' /opt/jumpserver/config.yml 
SECRET_KEY: yN5BNda0Ty7TLNbKMUtZflBhmYGGpEotwFTuWgzsICBnotuliH		这里的值可以写成$SECRET_KEY
BOOTSTRAP_TOKEN: gWagtYEDkwG2LSJe								  这里的值可以写成$BOOTSTRAP_TOKEN
DB_ENGINE: mysql
DB_HOST: 127.0.0.1
DB_PORT: 3306
DB_USER: jumpserver
DB_PASSWORD: syh888
DB_NAME: jumpserver
HTTP_BIND_HOST: 0.0.0.0
HTTP_LISTEN_PORT: 8080
WS_LISTEN_PORT: 8070
REDIS_HOST: 127.0.0.1
REDIS_PORT: 6379

(py3) [root@jumpserver jumpserver]# echo $SECRET_KEY
yN5BNda0Ty7TLNbKMUtZflBhmYGGpEotwFTuWgzsICBnotuliH
(py3) [root@jumpserver jumpserver]# echo $BOOTSTRAP_TOKEN
gWagtYEDkwG2LSJe

```



### 5.15、JumpServer核心后台启动

#### 数据库迁移

```shell
python3 /opt/jumpserver/apps/manage.py makemigrations
Migrations for 'tickets':
  tickets/migrations/0002_auto_20201103_1559.py
    - Alter field type on ticket

python3 /opt/jumpserver/apps/manage.py migrate


#此时发现数据表已经被迁移到数据库
mysql> show tables;
+----------------------------------------------+
| Tables_in_jumpserver                         |
+----------------------------------------------+
| applications_databaseapp                     |
| applications_remoteapp                       |
| assets_adminuser                             |
| assets_asset                                 |
| assets_asset_labels                          |
| assets_asset_nodes                           |
| assets_assetgroup                            |
| assets_authbook                              |
| assets_cluster                               |
| assets_commandfilter                         |
| assets_commandfilterrule                     |
| assets_domain                                |
| assets_favoriteasset                         |
| assets_gateway                               |
| assets_gathereduser                          |
| assets_label                                 |
| assets_node                                  |
| assets_platform                              |
| assets_systemuser                            |
| assets_systemuser_assets                     |
| assets_systemuser_cmd_filters                |
| assets_systemuser_groups                     |
| assets_systemuser_nodes                      |
| assets_systemuser_users                      |
| audits_ftplog                                |
| audits_operatelog                            |
| audits_passwordchangelog                     |
| audits_userloginlog                          |
| auth_group                                   |
| auth_group_permissions                       |
| auth_permission                              |
| authentication_accesskey                     |
| authentication_loginconfirmsetting           |
| authentication_loginconfirmsetting_reviewers |
| authentication_privatetoken                  |
| captcha_captchastore                         |
| django_admin_log                             |
| django_cas_ng_proxygrantingticket            |
| django_cas_ng_sessionticket                  |
| django_celery_beat_crontabschedule           |
| django_celery_beat_intervalschedule          |
| django_celery_beat_periodictask              |
| django_celery_beat_periodictasks             |
| django_celery_beat_solarschedule             |
| django_content_type                          |
| django_migrations                            |
| django_session                               |
| jms_oidc_rp_oidcuser                         |
| ops_adhoc                                    |
| ops_adhoc_execution                          |
| ops_adhoc_hosts                              |
| ops_celerytask                               |
| ops_commandexecution                         |
| ops_commandexecution_hosts                   |
| ops_task                                     |
| orgs_organization                            |
| orgs_organization_admins                     |
| orgs_organization_auditors                   |
| orgs_organization_users                      |
| perms_assetpermission                        |
| perms_assetpermission_assets                 |
| perms_assetpermission_nodes                  |
| perms_assetpermission_system_users           |
| perms_assetpermission_user_groups            |
| perms_assetpermission_users                  |
| perms_databaseapppermission                  |
| perms_databaseapppermission_database_apps    |
| perms_databaseapppermission_system_users     |
| perms_databaseapppermission_user_groups      |
| perms_databaseapppermission_users            |
| perms_remoteapppermission                    |
| perms_remoteapppermission_remote_apps        |
| perms_remoteapppermission_system_users       |
| perms_remoteapppermission_user_groups        |
| perms_remoteapppermission_users              |
| settings_setting                             |
| terminal                                     |
| terminal_command                             |
| terminal_commandstorage                      |
| terminal_replaystorage                       |
| terminal_session                             |
| terminal_status                              |
| terminal_task                                |
| tickets_comment                              |
| tickets_ticket                               |
| tickets_ticket_assignees                     |
| users_user                                   |
| users_user_groups                            |
| users_user_user_permissions                  |
| users_usergroup                              |
+----------------------------------------------+
90 rows in set (0.00 sec)

```

#### 启动jms

确保都是在python虚拟环境下的

```shell
(py3) [root@jumpserver jumpserver 18:33:30]$cd /opt/jumpserver
(py3) [root@jumpserver jumpserver 18:33:36]$./jms start -d
2020-11-03 16:02:08 Tue Nov  3 16:02:08 2020
2020-11-03 16:02:08 Jumpserver version v2.1.0, more see https://www.jumpserver.org

- Start Gunicorn WSGI HTTP Server
2020-11-03 16:02:08 Check database connection ...

...
gunicorn is running: 24380
celery_ansible is running: 24391
celery_default is running: 24404
beat is running: 24411
flower is running: 24415
daphne is running: 24428

```

访问:192.168.37.100:8080

![image-20201103160412147](notes/image-20201103160412147.png)

看到此页面说明jumpserver后台程序已启动、后续还要搭建koko服务、以及组件



### 5.16、koko部署



#### 部署koko

运行jumpserver还需要部署koko服务（基于GO语言开发的SSH客户端），KOKO是使用Go语言重新开发的Unix资产连接组件，

与之前使用的Coco（即基于Python语言开发的SSH客户端）相比，Koko支持多线程，性能更强，且负载更低。

```shell
# 下载源代码
cd /opt && \
wget https://github.com/jumpserver/koko/releases/download/v2.1.0/koko-v2.1.0-linux-amd64.tar.gz

# 解压缩配置
tar -xf koko-v2.1.0-linux-amd64.tar.gz && \
mv koko-v2.1.0-linux-amd64 koko && \
chown -R root:root koko && \
cd koko

# 修改配置文件
# BOOTSTRAP_TOKEN 需要从 jumpserver/config.yml 里面获取, 保证一致
cp config_example.yml config.yml && \
vi config.yml  

# 修改如下几处配置
(py3) [root@jumpserver koko]# grep -Ev '^#|^$' config.yml 
CORE_HOST: http://127.0.0.1:8080
BOOTSTRAP_TOKEN: gWagtYEDkwG2LSJe			#这里与上面jumpserver后台程序的配置文件写法一致
BIND_HOST: 0.0.0.0
SSHD_PORT: 2222
HTTPD_PORT: 5000
LOG_LEVEL: INFO
REDIS_HOST: 127.0.0.1
REDIS_PORT: 6379
REDIS_PASSWORD:
REDIS_CLUSTERS:
REDIS_DB_ROOM:

# 运行KoKo
./koko -d
```





#### koko常见问题

koko非常容易启动失败，如出现【名称重复，未认证等】，做如下操作

![image-20200803181609733](notes/image-20200803181609733.png)



### 5.17、部署Guacamole组件

#### 部署Guacamole

Guacamole 是一个基于 HTML 5 和 JavaScript 的 VNC 查看器，服务端基于 Java 的 VNC-to-XML 代理开发。要求浏览器支持 HTML 5。

Guacamole 是提供远程桌面的解决方案的开源项目，通过浏览器就能操作虚拟机，适用于Chrome，Firefox，IE10等浏览器（浏览器需要支持HTML5）。 由于使用 HTML5，Guancamole 只要在一个服务器安装成功，你访问你的桌面就是访问一个 web 浏览器。

Guacamole不是一个独立的Web应用程序，而是由许多部件组成的。Web应用程序实际上是整个项目里最小最轻量的，大部分的功能依靠Guacamole的底层组件来完成。

用户通过浏览器连接到Guacamole的服务端。Guacamole的客户端是用javascript编写的，Guacamole server通过web容器（比如tomcat）把服务提供给用户。一旦加载，客户端通过http承载着Guacamole自己的定义的协议与服务端通信。

<img src="notes/image-20200803101259286.png" alt="image-20200803101259286" style="zoom:50%;" />

```shell
1.获取源代码包，该资料，github也没有下载了，可以向超哥索要，qq:877348180，也可以选择用docker安装
#这里通过winodows本地拖入到linux、前提是linux下载好lrzsz
(py3) [root@jumpserver opt]# ls |grep gua
2020-07-22-16-48-00-docker-guacamole-v2.1.0.tar.gz

2.解压缩
(py3) [root@jumpserver opt]# tar -xf 2020-07-22-16-48-00-docker-guacamole-v2.1.0.tar.gz
(py3) [root@jumpserver opt]# mv docker-guacamole-2.1.0 guacamole

3.解压执行程序
cd /opt/guacamole && \
tar -xf guacamole-server-1.2.0.tar.gz && \
tar -xf ssh-forward.tar.gz -C /bin/ && \
chmod +x /bin/ssh-forward

4.编译安装程序
(py3) [root@jumpserver guacamole]# cd /opt/guacamole/guacamole-server-1.2.0/

5.安装编译所需的依赖环境，根据官方文档的要求来http://guacamole.apache.org/doc/gug/installing-guacamole.html
# 非常重要，必须安装
yum install cairo-devel libjpeg-turbo-devel     libjpeg-devel     libpng-devel libtool uuid-devel -y


# 可选的依赖环境
yum install  freerdp-devel pango-devel     libssh2-devel libtelnet-devel libvncserver-devel libwebsockets-devel  pulseaudio-libs-devel openssl-devel libvorbis-devel libwebp-devel -y


# 安装FFmpeg
# FFmpeg是用于处理多媒体文件的免费和开放源代码工具集合。它包含一组共享的音频和视频库，例如libavcodec，libavformat和libavutil。使用FFmpeg，您可以在各种视频和音频格式之间转换，设置采样率，捕获流音频/视频和调整视频大小。
sudo yum install epel-release -y
sudo rpm -v --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
sudo rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
yum install ffmpeg ffmpeg-devell -y 


#检查ffmpeg安装
ffmpeg -version


6.编译安装guacamole
cd /opt/guacamole/guacamole-server-1.2.0
./configure --with-init-dir=/etc/init.d && \
make && \
make install


7.配置好java环境
yum install -y java-1.8.0-openjdk

8.创建guacamole所需的文件夹
mkdir -p /config/guacamole /config/guacamole/extensions /config/guacamole/record /config/guacamole/drive && \
chown daemon:daemon /config/guacamole/record /config/guacamole/drive && \
cd /config

9.下载tomcat
cd /opt && wget https://mirrors.bfsu.edu.cn/apache/tomcat/tomcat-9/v9.0.39/bin/apache-tomcat-9.0.39.tar.gz



10.部署tomncat与guacamole结合

cd /opt && \
tar -xf apache-tomcat-9.0.39.tar.gz && \
mv apache-tomcat-9.0.39 tomcat9 && \
rm -rf /opt/tomcat9/webapps/* && \
sed -i 's/Connector port="8080"/Connector port="8081"/g' /opt/tomcat9/conf/server.xml && \
echo "java.util.logging.ConsoleHandler.encoding = UTF-8" >> /opt/tomcat9/conf/logging.properties && \
ln -sf /opt/guacamole/guacamole-1.0.0.war /opt/tomcat9/webapps/ROOT.war && \
ln -sf /opt/guacamole/guacamole-auth-jumpserver-1.0.0.jar /config/guacamole/extensions/guacamole-auth-jumpserver-1.0.0.jar && \
ln -sf /opt/guacamole/root/app/guacamole/guacamole.properties /config/guacamole/guacamole.properties


12.设置Guacamole运行环境
export JUMPSERVER_SERVER=http://127.0.0.1:8080
echo "export JUMPSERVER_SERVER=http://127.0.0.1:8080" >> ~/.bashrc
export BOOTSTRAP_TOKEN=zxffNymGjP79j6BN
echo "export BOOTSTRAP_TOKEN=zxffNymGjP79j6BN" >> ~/.bashrc
export JUMPSERVER_KEY_DIR=/config/guacamole/keys
echo "export JUMPSERVER_KEY_DIR=/config/guacamole/keys" >> ~/.bashrc
export GUACAMOLE_HOME=/config/guacamole
echo "export GUACAMOLE_HOME=/config/guacamole" >> ~/.bashrc
export GUACAMOLE_LOG_LEVEL=ERROR
echo "export GUACAMOLE_LOG_LEVEL=ERROR" >> ~/.bashrc
export JUMPSERVER_ENABLE_DRIVE=true
echo "export JUMPSERVER_ENABLE_DRIVE=true" >> ~/.bashrc


# 检查环境变量文件
"""参数解释
JUMPSERVER_SERVER 指 core 访问地址
BOOTSTRAP_TOKEN 为 Jumpserver/config.yml 里面的 BOOTSTRAP_TOKEN 值
JUMPSERVER_KEY_DIR 认证成功后 key 存放目录
GUACAMOLE_HOME 为 guacamole.properties 配置文件所在目录
GUACAMOLE_LOG_LEVEL 为生成日志的等级
JUMPSERVER_ENABLE_DRIVE 为 rdp 协议挂载共享盘
"""



[root@jumpserver guacamole]# tail -8 ~/.bashrc
SECRET_KEY=yN5BNda0Ty7TLNbKMUtZflBhmYGGpEotwFTuWgzsICBnotuliH
BOOTSTRAP_TOKEN=gWagtYEDkwG2LSJe
export JUMPSERVER_SERVER=http://127.0.0.1:8080
export BOOTSTRAP_TOKEN=zxffNymGjP79j6BN
export JUMPSERVER_KEY_DIR=/config/guacamole/keys
export GUACAMOLE_HOME=/config/guacamole
export GUACAMOLE_LOG_LEVEL=ERROR
export JUMPSERVER_ENABLE_DRIVE=true

13.启动服务
/etc/init.d/guacd start
sh /opt/tomcat9/bin/startup.sh
```





### 5.18、JumpServer部署lina、luna组件、nginx

#### 部署nginx

nginx用作对jumpserver的动静态文件处理，以及反向代理

```shell
[root@jumpserver opt 11:28:43]$部署nginx
nginx用作对jumpserver的动静态文件处理，以及反向代理

[root@jumpserver opt 11:28:43]$yum install nginx -y
修改nginx.conf

# 删除原有nginx.conf虚拟主机的配置
[root@jumpserver nginx 11:34:23]$sed -i  '38,58d' /etc/nginx/nginx.conf
[root@jumpserver nginx 11:31:27]$vim /etc/nginx/conf.d/jumpserver.conf # 加入如下配置

server {
    listen 80;

    client_max_body_size 100m;  # 录像及文件上传大小限制

    location /ui/ {
        try_files $uri / /index.html;
        alias /opt/lina/;
    }

    location /luna/ {
        try_files $uri / /index.html;
        alias /opt/luna/;  # luna 路径, 如果修改安装目录, 此处需要修改
    }

    location /media/ {
        add_header Content-Encoding gzip;
        root /opt/jumpserver/data/;  # 录像位置, 如果修改安装目录, 此处需要修改
    }

    location /static/ {
        root /opt/jumpserver/data/;  # 静态资源, 如果修改安装目录, 此处需要修改
    }

    location /koko/ {
        proxy_pass       http://localhost:5000;
        proxy_buffering off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        access_log off;
    }

    location /guacamole/ {
        proxy_pass       http://localhost:8081/;
        proxy_buffering off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $http_connection;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        access_log off;
    }

    location /ws/ {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://localhost:8070;
        proxy_http_version 1.1;
        proxy_buffering off;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

    location /api/ {
        proxy_pass http://localhost:8080;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location /core/ {
        proxy_pass http://localhost:8080;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location / {
        rewrite ^/(.*)$ /ui/$1 last;
    }
}
重启服务

nginx -t 
nginx -s reload
```

修改nginx.conf

```
# 删除原有nginx.conf虚拟主机的配置
[root@jumpserver nginx 11:34:23]$sed -i  '38,58d' /etc/nginx/nginx.conf
```

**[root@jumpserver nginx 11:31:27]$vim /etc/nginx/conf.d/jumpserver.conf # 加入如下配置**

```shell
server {
    listen 80;

    client_max_body_size 100m;  # 录像及文件上传大小限制

    location /ui/ {
        try_files $uri / /index.html;
        alias /opt/lina/;
    }

    location /luna/ {
        try_files $uri / /index.html;
        alias /opt/luna/;  # luna 路径, 如果修改安装目录, 此处需要修改
    }

    location /media/ {
        add_header Content-Encoding gzip;
        root /opt/jumpserver/data/;  # 录像位置, 如果修改安装目录, 此处需要修改
    }

    location /static/ {
        root /opt/jumpserver/data/;  # 静态资源, 如果修改安装目录, 此处需要修改
    }

    location /koko/ {
        proxy_pass       http://localhost:5000;
        proxy_buffering off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        access_log off;
    }

    location /guacamole/ {
        proxy_pass       http://localhost:8081/;
        proxy_buffering off;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $http_connection;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        access_log off;
    }

    location /ws/ {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://localhost:8070;
        proxy_http_version 1.1;
        proxy_buffering off;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

    location /api/ {
        proxy_pass http://localhost:8080;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location /core/ {
        proxy_pass http://localhost:8080;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    location / {
        rewrite ^/(.*)$ /ui/$1 last;
    }
}
```

重启服务

```shell
nginx -t 
nginx -s reload
```



#### 部署Lina组件

Lina 是 JumpServer 的前端 UI 项目, 主要使用 [Vue](https://cn.vuejs.org/), [Element UI](https://element.eleme.cn/) 完成, 名字来源于 Dota 英雄 [Lina](https://baike.baidu.com/item/莉娜/16693979)

```shell
cd /opt
wget https://github.com/jumpserver/lina/releases/download/v2.1.0/lina-v2.1.0.tar.gz

tar -xf lina-v2.1.0.tar.gz
mv lina-v2.1.0 lina
chown -R nginx:nginx lina  # 需要提前装好nginx
```

#### 部署luna

Luna是Web Terminal前端，jumpserver提供api数据

下载地址https://github.com/jumpserver/luna/releases

```shell
cd /opt && \ 
wget https://github.com/jumpserver/luna/releases/download/v2.1.1/luna-v2.1.1.tar.gz && \
tar -zxf luna-v2.1.1.tar.gz && \
mv /opt/luna-v2.1.1 /opt/luna
chown -R root.root /opt/luna-v2.1.1/
```



#### 至此jumpserver正确启动

```shell
服务全部启动后, 访问 JumpServer 服务器 nginx 代理的 80 端口, 不要通过8080端口访问 默认账号: admin 密码: admin
```

后续的使用请参考 [安全建议](https://docs.jumpserver.org/zh/master/install/install_security/) [快速入门](https://docs.jumpserver.org/zh/master/admin-guide/quick_start/)

访问入口

```shell
http://192.168.37.100
```

![image-20201104113619200](notes/image-20201104113619200.png)



### 5.19、koko无法启动的解决办法



注意：删除重新配置两个密钥之前要先删除网站的个人信息

![image-20201104172505455](notes/image-20201104172505455.png)

```shell
1.删除koko/key/目录下的.access_key文件
(py3) [root@jumpserver keys]# rm -rf .access_key 
(py3) [root@jumpserver keys]# pwd
/opt/koko/data/keys


2.重新生成两个重要的密钥、然后去修改jumpserver后台所有组件的配置、重启服务
SECRET_KEY
BOOTSTRAP_TOKEN

#重新生成这两个key
第一步：修改环境变量文件、删除两个变量、
SECRET_KEY
BOOTSTRAP_TOKEN
还剩如下参数：
export JUMPSERVER_SERVER=http://127.0.0.1:8080
export JUMPSERVER_KEY_DIR=/config/guacamole/keys
export GUACAMOLE_HOME=/config/guacamole
export GUACAMOLE_LOG_LEVEL=ERROR
export JUMPSERVER_ENABLE_DRIVE=true

第二部：重新登录linux会话、检查这两个变量的值是否为空

[root@jumpserver ~]# echo $SECRET_KEY

[root@jumpserver ~]# echo $BOOTSTRAP_TOKEN

第三步：重新生成两个密钥
[root@jumpserver ~]# if [ "$SECRET_KEY" = "" ]; then SECRET_KEY=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 50`; echo "SECRET_KEY=$SECRET_KEY" >> ~/.bashrc; echo $SECRET_KEY; else echo $SECRET_KEY; fi
AgZ144JYUh2IFnmpMWNZga1eGAUOzLdgvxDanR0yAERApR3zmJ

[root@jumpserver ~]# if [ "$BOOTSTRAP_TOKEN" = "" ]; then BOOTSTRAP_TOKEN=`cat /dev/urandom | tr -dc A-Za-z0-9 | head -c 16`; echo "BOOTSTRAP_TOKEN=$BOOTSTRAP_TOKEN" >> ~/.bashrc; echo $BOOTSTRAP_TOKEN; else echo $BOOTSTRAP_TOKEN; fi
02s0EHbKdjCQgAeL


第四步：修改jumpserver程序的配置文件 config.yml
#先进入虚拟环境
[root@jumpserver ~]# source /opt/py3/bin/activate

修改后的配置如下：
(py3) [root@jumpserver ~]# grep -Ev '^#|^$' /opt/jumpserver/config.yml 
SECRET_KEY: AgZ144JYUh2IFnmpMWNZga1eGAUOzLdgvxDanR0yAERApR3zmJ		#主要修改这两个密钥
BOOTSTRAP_TOKEN: 02s0EHbKdjCQgAeL
DB_ENGINE: mysql
DB_HOST: 127.0.0.1
DB_PORT: 3306
DB_USER: jumpserver
DB_PASSWORD: syh888
DB_NAME: jumpserver
HTTP_BIND_HOST: 0.0.0.0
HTTP_LISTEN_PORT: 8080
WS_LISTEN_PORT: 8070
REDIS_HOST: 127.0.0.1
REDIS_PORT: 6379

第五步：重新启动jumpserver核心后台程序
#先停止
(py3) [root@jumpserver ~]# /opt/jumpserver/jms stop
Stop service
Stop service: gunicorn Ok
Stop service: celery_ansible Ok
Stop service: celery_default Ok
Stop service: beat Ok
Stop service: flower Ok
Stop service: daphne Ok
#再启动
(py3) [root@jumpserver ~]# /opt/jumpserver/jms start -d

第六步：修改koko的配置文件
(py3) [root@jumpserver keys]# grep -Ev '^#|^$' /opt/koko/config.yml 
CORE_HOST: http://127.0.0.1:8080
BOOTSTRAP_TOKEN: 02s0EHbKdjCQgAeL		#主要修改这个
BIND_HOST: 0.0.0.0
SSHD_PORT: 2222
HTTPD_PORT: 5000
LOG_LEVEL: INFO
REDIS_HOST: 127.0.0.1
REDIS_PORT: 6379
REDIS_PASSWORD:
REDIS_CLUSTERS:
REDIS_DB_ROOM:


第七步：后台启动koko
(py3) [root@jumpserver koko]# ./koko -d
(py3) [root@jumpserver koko]# netstat -tunpl|grep koko
tcp6       0      0 :::5000                 :::*                    LISTEN      2594/./koko         
tcp6       0      0 :::2222                 :::*                    LISTEN      2594/./koko  

第八步：由于修改了密钥、还会影响guacamole服务、还要修改它的配置运行环境变量
export JUMPSERVER_SERVER=http://127.0.0.1:8080
echo "export JUMPSERVER_SERVER=http://127.0.0.1:8080" >> ~/.bashrc
export BOOTSTRAP_TOKEN=02s0EHbKdjCQgAeL
echo "export BOOTSTRAP_TOKEN=02s0EHbKdjCQgAeL" >> ~/.bashrc
export JUMPSERVER_KEY_DIR=/config/guacamole/keys
echo "export JUMPSERVER_KEY_DIR=/config/guacamole/keys" >> ~/.bashrc
export GUACAMOLE_HOME=/config/guacamole
echo "export GUACAMOLE_HOME=/config/guacamole" >> ~/.bashrc
export GUACAMOLE_LOG_LEVEL=ERROR
echo "export GUACAMOLE_LOG_LEVEL=ERROR" >> ~/.bashrc
export JUMPSERVER_ENABLE_DRIVE=true
echo "export JUMPSERVER_ENABLE_DRIVE=true" >> ~/.bashrc

第九步：重启guacd、tomcat服务
(py3) [root@jumpserver etc]# /etc/init.d/guacd restart
(py3) [root@jumpserver etc]# /opt/tomcat9/bin/shutdown.sh 
(py3) [root@jumpserver etc]# /opt/tomcat9/bin/startup.sh

```

此时访问页面发现状态是在线的、

![image-20201104172309732](notes/image-20201104172309732.png)

也可以进入到文件管理

![image-20201104172349723](notes/image-20201104172349723.png)

### 5.20、Jumpserver防火墙与改密



#### Jumpserver实践

#### 提前启动好jumpserver服务端

```shell
#进入虚拟环境
source /opt/py3/bin/activate
#启动jms
/opt/jumpserver/jms start -d 
#启动koko
/opt/koko/koko -d
#启动gua
/etc/init.d/guacd start 
#启动tomcat
sh /opt/tomcat9/bin/startup.sh 
#启动nginx
nginx
#启动数据库mysql
systemctl start mysql
#启动redis
systemctl start redis
# 快捷登录配置，以及免密登录客户端
[root@jumpserver ~ 16:20:50]$tail -2 ~/.bash_profile
alias sshweb01="ssh root@192.168.37.105"
```

#### 只允许跳板机登录client

所有主机想要被管理必须经过跳板机管理

```shell
注意，最好提前设置免密登录，来的方便


1.设置防火墙规则
client机器可以iptables规则，只允许跳板机连接

iptables -A INPUT -s 192.168.37.100 -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -p tcp --dport 22 -j REJECT

2.检查客户端机器防火墙规则
[root@web-01 ~]# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     tcp  --  192.168.178.134      anywhere             tcp dpt:ssh
REJECT     tcp  --  anywhere             anywhere             tcp dpt:ssh reject-with icmp-port-unreachable

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
```



### 5.21、Jumpserver创建邮箱与用户

#### 修改admin的密码

![image-20201104195502346](notes/image-20201104195502346.png)



#### 设置邮箱发送

第一步：填写基本信息、授权码

![image-20201104195758347](notes/image-20201104195758347.png)



第二步：邮件基本设置，必须，否则无法发邮件

```shell
这里的url站点，是做了nginx代理的，因此是80端口
```

![image-20201104195955160](notes/image-20201104195955160.png)



#### 用户创建

添加用户

![image-20201104200111698](notes/image-20201104200111698.png)

填写用户信息

![image-20201104200150775](notes/image-20201104200150775.png)

查看用户信息

![image-20201104200218333](notes/image-20201104200218333.png)

此时正确的应该是收到了用户创建的邮件。

![image-20201104200510983](notes/image-20201104200510983.png)

点击重置密码后，进入该页面，修改用户自己的密码

![image-20200803153702975](notes/image-20200803153702975.png)

### 5.22、Jumpserver资产创建管理

#### 资产管理配置

#### 添加管理用户**

管理用户是资产（被管控的服务器）上的root用户，或者是拥有sudo权限的用户，jumpserver使用该用户进行`推送系统用户`、`获取资产硬件信息`等

管理用户是，jumpserver操作`被管理机器`时的用户。

加一个root用户，进行使用

![image-20201104201127606](notes/image-20201104201127606.png)

![image-20201104201211987](notes/image-20201104201211987.png)



#### 添加资产主机

选择资产添加

![image-20201104201501881](notes/image-20201104201501881.png)



填写资产信息

![image-20201104201516471](notes/image-20201104201516471.png)

可添加多台主机进行管理

#### 查看资产主机

![image-20201104201618197](notes/image-20201104201618197.png)



#### 导出资产文件

可以导出资产文件，格式为csv数据文件

![image-20201104205930311](notes/image-20201104205930311.png)



![image-20201104210024194](notes/image-20201104210024194.png)



### 5.23、创建系统用户与资产权限

#### 系统用户**

系统用户是 JumpServer 跳转登录资产时使用的用户，可以理解为登录资产用户，如 web，sa，dba（`ssh web@some-host`），而不是使用某个用户的用户名跳转登录服务器（`ssh xiaoming@some-host`）；

简单来说是用户使用自己的用户名登录 JumpServer，JumpServer 使用系统用户登录资产。 系统用户创建时，如果选择了自动推送，JumpServer 会使用 Ansible 自动推送系统用户到资产中，如果资产（交换机）不支持 Ansible，请手动填写账号密码。

![image-20200803172025594](notes/image-20200803172025594.png)

添加过程

![image-20200803172107032](notes/image-20200803172107032.png)



jumpserver创建系统用户，会自动基于ansible在目标机器上创建该用户

![image-20201104211241979](notes/image-20201104211241979.png)



#### 资产授权

![image-20201104211643331](notes/image-20201104211643331.png)

#### 使用资产

- 系统内建webterminal
- 客户端工具

#### admin的用户界面/管理界面

#### web终端

```shell
得提前配置好luna，方可使用该功能
```

![image-20201105103445447](notes/image-20201105103445447.png)





![image-20201105103806268](notes/image-20201105103806268.png)

![image-20201105103826843](notes/image-20201105103826843.png)





### ssh终端

ssh得配置好koko组件方可使用

```shell
# 连接的命令就已经不同了，使用ssh命令，以及jumpserver平台账号，以及koko端口
[root@jumpserver koko]$ssh admin@192.168.37.100 -p2222
```

![image-20201105104538132](notes/image-20201105104538132.png)

![image-20201105104556284](notes/image-20201105104556284.png)

```shell
根据提示使用ssh终端管理即可

1. 输入p显示主机信息，然后输入主机id，即可自动连接，注意我们资产机，是不允许ssh登录的，只能通过jumpserver;
#通过防火墙规则就可以实现、只限制37.100可以进行连接连接

可以正常输入普通命令
使用sudo命令，权限被/etc/sudoers文件配置所控制
```





### 5.24、Jumpserver监控会话

#### 会话管理

可在jumpserver界面，掌握当前登录机器的数量

![image-20201105105738588](notes/image-20201105105738588.png)



#### 监控会话

点击会话ID，可以查看该会话，执行了哪些命令、以及强制中断该会话

![image-20201105110031218](notes/image-20201105110031218.png)

#### 终端会话

![image-20201105110107277](notes/image-20201105110107277.png)



#### 命令监控

可进入实时监控画面、画面加载需要时间

监控画面

![image-20201105110244241](notes/image-20201105110244241.png)



#### 历史会话

可查看视频回放以及视频下载

![image-20201105110325243](notes/image-20201105110325243.png)