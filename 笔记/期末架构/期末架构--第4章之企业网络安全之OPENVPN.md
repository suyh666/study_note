# 四、期末架构



## 第四章 企业网络安全之OPENVPN

### 4.1、趣谈VPN技术

## vpn作用

![image-20200805135613755](./notes/image-20200805135613755.png)

#### 能够访问墙外的内容

![image-20200805135645835](C:\Users\admin\Desktop\notes\image-20200805135645835.png)

#### 虚拟网络隧道

![image-20200805135829639](C:\Users\admin\Desktop\notes\image-20200805135829639.png)

#### 隧道数据传输

![image-20200805135901445](C:\Users\admin\Desktop\notes\image-20200805135901445.png)

#### 保护服务器安全

![image-20200805135742079](C:\Users\admin\Desktop\notes\image-20200805135742079.png)

### 4.2、VPN详细介绍

#### VPN介绍

VPN 直译就是虚拟专用通道，是提供给企业之间或者个人与公司之间**安全数据传输**的隧道。

虚拟私有网络（VPN）隧道是通过 Internet 隧道技术将两个不同地理位置的网络安全的连接起来的技术。

当两个网络是使用私有 IP 地址的私有局域网络时，它们之间是不能相互访问的，这时使用隧道技术就可以使得两个子网内的主机进行通讯。VPN 隧道技术经常被用于大型机构中不同办公区域子网的连接。

有时，使用 VPN 隧道仅仅是因为它很安全。服务提供商与公司会使用这样一种方式架设网络，他们将重要的服务器（如，数据库，VoIP，银行服务器）放置到一个子网内，仅仅让有权限的用户通过 VPN 隧道进行访问。

VPN (虚拟专用网)发展至今已经不在是一个单纯的经过加密的访问隧道了，它已经融合了**访问控制**、**传输管理**、**加密**、**路由选择**、**可用性管理**等多种功能，并在全球的信息安全体系中发挥着重要的作用。也在网络上，有关各种 VPN 协议优缺点的比较是仁者见仁，智者见智，很多技术人员由于出于使用目的考虑，包括访问控制、 安全和用户简单易用，灵活扩展等各方面，权衡利弊，难以取舍；尤其在 VOIP 语音环境中，网络安全显得尤为重要，因此现在越来越多的网络电话和语音网关支持 VPN 协议。

![image-20200805143632754](C:\Users\admin\Desktop\notes\image-20200805143632754.png)

- 虚拟网络通道，保证数据专有安全，而不同于公开在互联网中的数据传输
  - 如送快递，普通快递，是成堆的放在一起，各家快递，各家的数据需要传递
  - 专线快递，如寄送一个价值连城的夜明珠，私人飞机专运，保证了安全性。
- 专线通道，太过于昂贵

#### VPN示意图

![image-20200805143919144](C:\Users\admin\Desktop\notes\image-20200805143919144.png)

### 4.3、VPN使用场景分类

#### VPN分类

企业环境中一般根据VPN的应用领域不同，把VPN划分为4类应用。

##### 主机远程访问VPN服务

该情况一般用于超哥出差，休假等特殊情况远程办公，需要连接访问公司内部网络的场景，可以通过VPN拨号到公司内部，此时远程的超哥和办公室的其他同事就相当于在一个局域网了。

在家就可以访问公司内网，如文件服务器，OA办公四通，ERP，HTTP服务等等局域网应用。

![image-20200805145723159](C:\Users\admin\Desktop\notes\image-20200805145723159.png)

对于运维人员需要个人电脑，拨号到企业网站的IDC机房，远程维护服务器。

这是运维人员经常经常采用的方法，来维护企业里无外网IP地址的服务器等设备。

##### 企业网络之间的VPN服务

在公司的分支机构的局域网和公司总部的LAN之间的VPn连接，通过公网Internet建立VPN将各地的公司分部LAN连接到公司总部的LAN。

例如全国各地的大超市业务结算。

由于地域原因，通过VPN把不同地域的机器连接互相访问，就像是在一个局域网内。

##### 企业多IDC机房之间的VPN

不同机房之间的业务管理和业务访问的数据传输。

##### 企业外部VPN服务

在全球供应商，合作伙伴的LAN和本部公司的LAN建立VPN

### 4.4、VPN隧道之PPTP

#### VPN隧道协议

##### PPTP

**点对点隧道协议 (PPTP)** 是由包括微软和 3Com 等公司组成的 PPTP 论坛开发的一种点对点隧道协，基于拨号使用的 PPP 协议使用 PAP 或 CHAP 之类的加密算法，或者使用 Microsoft 的点对点加密算法 MPPE。其通过跨越基于 TCP/IP 的数据网络创建 VPN 实现了从远程客户端到专用企业服务器之间数据的安全传输。PPTP 支持通过公共网络(例如 Internet)建立按需的、多协议的、虚拟专用网络。PPTP 允许加密 IP 通讯，然后在要跨越公司 IP 网络或公共 IP 网络(如 Internet)发送的 IP 头中对其进行封装。

典型的Linux平台开源软件就是PPTP。

PPTP属于点对点方式应用，适合用户远程拨号到企业内部进行办公。

![image-20200805150752816](C:\Users\admin\Desktop\notes\image-20200805150752816.png)

**点对点隧道协议(PPTP，Point-to-Point Tunneling Protocol)将点对点协议(PPP，Point-to-Point Protocol)的数据帧封装进 IP 数据包中，通过 TCP／IP 网络进行传输**。PPTP 可以对 IP、IPX 或 NetBEUI 数据进行加密传递。PPTP 通过 PPTP 控制连接来创建、维护和终止一条隧道，并使用通用路由封装(GRE，Generic Routing Encapsulation)对 PPP 数据帧进行封装。封装前，PPP 数据帧的有效载荷(有效传输数据)首先必须经过加密、压缩或是两者的混合处理。

### 4.5、VPN隧道之L2TP

##### L2TP

**第 2 层隧道协议 (L2TP)** 是 IETF 基于 L2F (Cisco的第二层转发协议)开发的 PPTP 的后续版本。是一种工业标准 Internet 隧道协议，其可以为跨越面向数据包的媒体发送点到点协议 (PPP) 框架提供封装。PPTP 和 L2TP 都使用 PPP 协议对数据进行封装，然后添加附加包头用于数据在互联网络上的传输。**PPTP 只能在两端点间建立单一隧道。 L2TP 支持在两端点间使用多隧道，用户可以针对不同的服务质量创建不同的隧道**。**L2TP 可以提供隧道验证，而 PPTP 则不支持隧道验证**。**但是当 L2TP 或 PPTP 与 IPSEC 共同使用时，可以由 IPSEC 提供隧道验证，不需要在第2层协议上验证隧道使用 L2TP**。 **PPTP 要求互联网络为 IP 网络**。L2TP 只要求隧道媒介提供面向数据包的点对点的连接，L2TP 可以在 IP(使用UDP)，帧中继永久虚拟电路 (PVCs), X.25 虚拟电路(VCs)或 ATM VCs 网络上使用。

![image-20200805151126399](C:\Users\admin\Desktop\notes\image-20200805151126399.png)

L2TP和PPTP都是一个隧道协议，区别在于**L2TP 可以提供隧道验证，而 PPTP 则不支持隧道验证**。

##### IPSec

**IPSec** 的隧道是由**封装**、**路由**与**解封装**组成整个过程。隧道将原始数据包隐藏(或封装)在新的数据包内部。该新的数据包可能会有新的寻址与路由信息，从而使其能够通过网络传输。

隧道与数据保密性结合使用时，在网络上窃听通讯的人将无法获取原始数据包数据(以及原始的源和目标)。封装的数据包到达目的地后，会删除封装，原始数据包头用于将数据包路由到最终目的地。

![image-20200805151439660](C:\Users\admin\Desktop\notes\image-20200805151439660.png)

##### SSLVPN

SSL 协议提供了数据**私密性**、**端点验证**、**信息完整性**等特性。SSL 协议由许多子协议组成，其中两个主要的子协议是**握手协议**和**记录协议**。握手协议允许服务器和客户端在应用协议传输第一个数据字节以前，彼此确认，协商一种加密算法和密码钥匙。

在数据传输期间，记录协议利用握手协议生成的密钥加密和解密后来交换的数据。

SSL 独立于应用，因此任何一个应用程序都可以享受它的安全性而不必理会执行细节。SSL 置身于网络结构体系的**传输层**和**应用层**之间。此外，SSL 本身就被几乎所有的 Web 浏览器支持。这意味着客户端不需要为了支持 SSL 连接安装额外的软件。这两个特征就是 SSL 能应用于 VPN 的关键点。

![image-20200805152244183](C:\Users\admin\Desktop\notes\image-20200805152244183.png)

##### IPSec和SSL VPN区别

![image-20200805152640028](C:\Users\admin\Desktop\notes\image-20200805152640028.png)



### 4.6、VPN服务端初始化

#### VPN服务实践

利用虚拟专用网络，实现让外网主机获得架构中内网地址信息，实现利用内网地址进行数据传递

##### 机器准备

```shell
windows主机    外网主机        192.168.0.104（模拟公网地址）
vpnserver主机                 192.168.37.100（模拟公网地址）          192.168.150.100（模拟私网地址）
web01                                                          192.168.150.101（模拟私网地址）
```

PS：确保每台主机时间做好正确同步

整个部署架构，肯定是分为`客户端`和`服务端`





##### VPN服务端部署

准备好一台linux虚拟机（先弄1个网卡）

```SHELL
1.先添加1个网卡适配器、配置好静态IP；确保能够连接外网
#查看ip
[root@vpn-server ~]# ip a|grep ens32|awk 'NR==2{print $2}'
192.168.37.100/24

#确保网络能够连接外网
[root@vpn-server ~]# ping 1.2.4.8
PING 1.2.4.8 (1.2.4.8) 56(84) bytes of data.
64 bytes from 1.2.4.8: icmp_seq=1 ttl=128 time=39.8 ms
64 bytes from 1.2.4.8: icmp_seq=2 ttl=128 time=40.4 ms



2.配置好基础环境
yum install wget -y

wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo


yum install -y bash-completion vim lrzsz wget expect net-tools nc nmap tree dos2unix htop iftop iotop unzip telnet sl psmisc nethogs glances bc ntpdate  openldap-devel 


3.确保时间正确
ntpdate -u ntp.aliyun.com
修改时区
[root@vpn_server ~]# ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
[root@vpn_server ~]#
[root@vpn_server ~]#
[root@vpn_server ~]# date
Wed Aug  5 15:53:28 CST 2020

4.明确关闭防火墙，大坑
```



### 4.7、VPN服务端之模拟公、私网IP

服务器准备2块网卡，一个模拟公网，一个模拟局域网，在vmware上添加即可

这里mac和windows的网卡NAT添加是有区别的

windows的vmware workstation添加方式较为简单

mac的方式如下：

也可以看超哥的博客：https://www.cnblogs.com/pyyu/p/9689138.html

#### mac自定义vmware网络-外网

```shell
内网  172.20.1.63 仅主机模式
外网     10.0.1.63   NAT模式
```

1.vmware fusion偏好设置，添加nat自定义网络

偏好设置

![image-20200805175137984](notes/image-20200805175137984.png)

2.网络适配器1，选择vmnet2网络链接

![image-20200805175047469](notes/image-20200805175047469.png)

虚拟机连接

![image-20200805175022070](notes/image-20200805175022070.png)

#### mac设置仅主机-内网

在加一个网络设备-添加网卡

![image-20200805183027574](notes/image-20200805183027574.png)

mac的vmware fusion仅主机模式设备是：

vmnet1--仅主机，改成超哥这样的配置

vmnet2--nat

```shell
yumac:VMware Fusion root# cat /Library/Preferences/VMware\ Fusion/networking
VERSION=1,0
answer VNET_1_DHCP no
answer VNET_1_HOSTONLY_NETMASK 255.255.255.0
answer VNET_1_HOSTONLY_SUBNET 172.20.1.0
answer VNET_1_VIRTUAL_ADAPTER yes


answer VNET_2_DHCP no
answer VNET_2_HOSTONLY_NETMASK 255.255.255.0
answer VNET_2_HOSTONLY_SUBNET 10.0.1.0
answer VNET_2_NAT yes
answer VNET_2_NAT_PARAM_UDP_TIMEOUT 30
answer VNET_2_VIRTUAL_ADAPTER yes
```



windows的方式：

nat模式模拟外网、主机模式模拟内网

1.添加1个vpn虚拟主机、具体配置如下

![image-20201101163347786](C:\Users\admin\Desktop\notes\image-20201101163347786.png)

![image-20201101163448698](C:\Users\admin\Desktop\notes\image-20201101163448698.png)

2.设置并启用主机模式、NAT模式

![image-20201101163753509](C:\Users\admin\Desktop\notes\image-20201101163753509.png)



![image-20201101163832222](C:\Users\admin\Desktop\notes\image-20201101163832222.png)



3.设置完成后就可以启动虚拟机

4.将ifcfg-ens32的内容拷贝一份ifcfg-ens34,并改名为ifcfg-ens34

```shell
[root@vpn-server network-scripts]# cp ifcfg-ens32 ifcfg-ens34


#以下是ifcfg-ens32的内容
[root@vpn-server network-scripts]# cat ifcfg-ens32
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static										#<======
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens32												   # <====
UUID=1359dfe8-61d3-4044-bae9-3318741d55bb				       #<====
HWADDR=00:0c:29:92:39:bf								     #<====
DEVICE=ens32												#<====
ONBOOT=yes													#<====
IPADDR=192.168.37.100										 #<====
GATEWAY=192.168.37.2										 #<====								
NETMASK=255.255.255.0										 #<====
DNS=223.5.5.5												#<====


#以下是ifcfg-ens34的内容

[root@vpn-server network-scripts]# cat ifcfg-ens34
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static									#<====																	
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=ens34											#<====
UUID=b6fe7026-068a-4911-8917-907b60556b62			   #<====	与ens32不同、用uuidgen ens34s生成
HWADDR=00:0c:29:92:39:c9							#<====
DEVICE=ens34										#<====
ONBOOT=yes										    #<====
IPADDR=192.168.150.100								#<====
NETMASK=255.255.255.0								#<====	#单机不必设置网关

#两个配置修改完成后、重启网卡服务
systemctl restart network
```

```shell
#生成ens34的uuid
uuidgen ens34s
b6fe7026-068a-4911-8917-907b60556b62	
```

正确设置后，确保client-server能够通信

```shell
[C:\~]$ ping 192.168.37.100
正在 Ping 192.168.37.100 具有 32 字节的数据:
来自 192.168.37.100 的回复: 字节=32 时间<1ms TTL=64
来自 192.168.37.100 的回复: 字节=32 时间<1ms TTL=64
来自 192.168.37.100 的回复: 字节=32 时间<1ms TTL=64


[C:\~]$ ping 192.168.150.100
正在 Ping 192.168.150.100 具有 32 字节的数据:
#单机模式不通是正常的
```



### 4.8、vpn服务端openvom软件部署

```shell
1.下载软件
# openvpn  vpn软件
# easy-rsa 生成证书和密钥，类似于ssh-keygen
yum install openvpn easy-rsa -y


2.创建服务端所需的证书和密钥信息
# 拷贝easy-rsa整个文件夹配置，放入openvpn目录下
[root@vpn_server ~]# cp -r /usr/share/easy-rsa/ /etc/openvpn/
#再将vars.example文件内容拷贝到vars内、并重命名为vars
[root@vpn_server openvpn]#cp /usr/share/doc/easy-rsa-3.0.8/vars.example /etc/openvpn/easy-rsa/3.0.8/vars

#修改在openvpn目录下的vars配置文件，修改如下参数
set_var EASYRSA_REQ_COUNTRY     "CN"  # 国家 
set_var EASYRSA_REQ_PROVINCE    "BeiJing"  # 地区
set_var EASYRSA_REQ_CITY        "BeiJing" # 城市
set_var EASYRSA_REQ_ORG "LuffyCity"   #组织
set_var EASYRSA_REQ_EMAIL       "yc_uuu@163.com"  #邮箱
set_var EASYRSA_REQ_OU          "Linux IT"   #拥有者

#试验以下面这个为准
set_var EASYRSA_REQ_COUNTRY     "CN"# 国家 
set_var EASYRSA_REQ_PROVINCE    "ShaoGuan"# 地区
set_var EASYRSA_REQ_CITY        "SG"# 城市
set_var EASYRSA_REQ_ORG         "syh_person" #组织
set_var EASYRSA_REQ_EMAIL       "17688702764@163.com" #邮箱
set_var EASYRSA_REQ_OU          "linux syh" #拥有者

3.进入openvpn目录，生成easyrsa有关的证书和PKI信息目录
[root@vpn_server 3.0.8]# pwd
/etc/openvpn/easy-rsa/3.0.8
[root@vpn-server 3.0.8]# ll
总用量 96
-rwxr-xr-x 1 root root 76946 11月  1 21:49 easyrsa
-rw-r--r-- 1 root root  4616 11月  1 21:49 openssl-easyrsa.cnf
-rw-r--r-- 1 root root  8886 11月  1 22:05 vars
drwxr-xr-x 2 root root   122 11月  1 21:49 x509-types

#初始化pki文件
[root@vpn-server 3.0.8]# ./easyrsa init-pki

Note: using Easy-RSA configuration from: /etc/openvpn/easy-rsa/3.0.8/vars

init-pki complete; you may now create a CA or requests.
Your newly created PKI dir is: /etc/openvpn/easy-rsa/3.0.8/pki

# 初始化，会在当前目录创建PKI目录，用于存储一些中间变量及最终生成的证书
[root@vpn-server 3.0.8]# ll
总用量 96
-rwxr-xr-x 1 root root 76946 11月  1 21:49 easyrsa
-rw-r--r-- 1 root root  4616 11月  1 21:49 openssl-easyrsa.cnf
drwx------ 4 root root    87 11月  1 22:10 pki			#用来存放中间变量和最终的证书目录
-rw-r--r-- 1 root root  8886 11月  1 22:05 vars
drwxr-xr-x 2 root root   122 11月  1 21:49 x509-types


4.创建服务端ca证书，先不用密码，注意超哥执行命令的路径
# 创建根证书，首先会提示设置密码，用于ca对之后生成的server和client证书签名时使用
[root@vpn-server 3.0.8]# ./easyrsa build-ca nopass

Note: using Easy-RSA configuration from: /etc/openvpn/easy-rsa/3.0.8/vars
Using SSL: openssl OpenSSL 1.0.2k-fips  26 Jan 2017
Generating RSA private key, 2048 bit long modulus
.................................................................+++
...................................................+++
e is 65537 (0x10001)
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Common Name (eg: your user, host, or server name) [Easy-RSA CA]:openvpn   #输入openvpn

CA creation complete and you may now import and sign cert requests.
Your new CA certificate file for publishing is at:
/etc/openvpn/easy-rsa/3.0.8/pki/ca.crt


5.再创建私钥文件和证书请求文件
[root@vpn-server 3.0.8]# ./easyrsa gen-req openvpn nopass

Note: using Easy-RSA configuration from: /etc/openvpn/easy-rsa/3.0.8/vars
Using SSL: openssl OpenSSL 1.0.2k-fips  26 Jan 2017
Generating a 2048 bit RSA private key
..............................+++
............................+++
writing new private key to '/etc/openvpn/easy-rsa/3.0.8/pki/easy-rsa-1673.gYK3lW/tmp.0dtTXf'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Common Name (eg: your user, host, or server name) [openvpn]:		#默认不输入

Keypair and certificate request completed. Your files are:
req: /etc/openvpn/easy-rsa/3.0.8/pki/reqs/openvpn.req
key: /etc/openvpn/easy-rsa/3.0.8/pki/private/openvpn.key

私钥文件如上。


6.对整数签名，对信息进行确认，生成最终证书文件
[root@vpn-server 3.0.8]# ./easyrsa sign server openvpn

Note: using Easy-RSA configuration from: /etc/openvpn/easy-rsa/3.0.8/vars
Using SSL: openssl OpenSSL 1.0.2k-fips  26 Jan 2017


You are about to sign the following certificate.
Please check over the details shown below for accuracy. Note that this request
has not been cryptographically verified. Please be sure it came from a trusted
source or that you have verified the request checksum with the sender.

Request subject, to be signed as a server certificate for 825 days:

subject=
    commonName                = openvpn


Type the word 'yes' to continue, or any other input to abort.
  Confirm request details: yes				#输入yes
Using configuration from /etc/openvpn/easy-rsa/3.0.8/pki/easy-rsa-1701.KXhiot/tmp.KQrlRk
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'openvpn'
Certificate is to be certified until Feb  5 02:14:59 2023 GMT (825 days)

Write out database with 1 new entries
Data Base Updated

Certificate created at: /etc/openvpn/easy-rsa/3.0.8/pki/issued/openvpn.crt


7.生成pem文件，包含了证书、公私钥，根证书等。
#迪菲-赫尔曼密钥交换（英语：Diffie–Hellman key exchange，缩写为D-H） 是一种安全协议。它可以让双方在完全没有对方任何预先信息的条件下通过不安全信道创建起一个密钥。这个密钥可以在后续的通讯中作为对称密钥来加密通讯内容。

[root@vpn-server 3.0.8]#  ./easyrsa gen-dh
ote: using Easy-RSA configuration from: /etc/openvpn/easy-rsa/3.0.8/vars
Using SSL: openssl OpenSSL 1.0.2k-fips  26 Jan 2017
Generating DH parameters, 2048 bit long safe prime, generator 2
This is going to take a long time
.....++*++*

#最终结果
DH parameters of size 2048 created at /etc/openvpn/easy-rsa/3.0.8/pki/dh.pem


9.对证书与公私钥文件进行统一管理
[root@vpn-server pki]# cp /etc/openvpn/easy-rsa/3.0.8/pki/ca.crt  /etc/openvpn/server/
[root@vpn-server pki]# cp /etc/openvpn/easy-rsa/3.0.8/pki/private/openvpn.key  /etc/openvpn/server/
[root@vpn-server pki]# cp /etc/openvpn/easy-rsa/3.0.8/pki/issued/openvpn.crt  /etc/openvpn/server/
[root@vpn-server pki]# cp /etc/openvpn/easy-rsa/3.0.8/pki/dh.pem  /etc/openvpn/server/

[root@vpn-server pki]# tree /etc/openvpn/server/
/etc/openvpn/server/
├── ca.crt
├── dh.pem
├── openvpn.crt
└── openvpn.key

10.拷贝编写服务端配置文件
有关配置文件具体讲解，可以看
https://zzjlogin.github.io/Server/linux-server/openvpn/0040openvpn_config.html
这个地址

[root@vpn-server pki]# cp /usr/share/doc/openvpn-2.4.9/sample/sample-config-files/server.conf  /etc/openvpn/


[root@vpn-server openvpn]# ls
client  easy-rsa  server  server.conf


11.修改openvpn 服务端配置文件，修改如下参数

local 192.168.37.100 #本机的外网ip
port 1194  
proto tcp
dev tun   
ca /etc/openvpn/server/ca.crt
cert /etc/openvpn/server/openvpn.crt
key /etc/openvpn/server/openvpn.key  # This file should be kept secret
dh /etc/openvpn/server/dh.pem
topology subnet  #开启子网的掩码为/24 默认是/30
server 10.8.0.0 255.255.255.0 #配置服务器VPN子网，用于VPN客户端提取ip，服务器子集用10.8.0.1，说白了就是连接上VPN使用的ip地址
push "route 192.168.150.0 255.255.255.0" #用于VPN局域网的内网环境通信，推送内网路由
keepalive 10 120  #多久未使用VPN超时时间
;tls-auth ta.key 0 # This file is secret  # 注释掉这个，不使用该tls加密方式
status /var/log/openvpn-status.log  # 修改日志存放路径
log-append  /var/log/openvpn.log  # 运行日志
mute 20  # 重复日志限额
;explicit-exit-notify 1  #关闭自动重新连接功能


12.整体的匹配如下
[root@vpn-server server]# grep -n ^[a-Z] /etc/openvpn/server.conf
25:local 192.168.37.100
32:port 1194
35:proto tcp
52:dev tap
53:dev tun
78:ca /etc/openvpn/server/ca.crt
79:cert /etc/openvpn/server/openvpn.crt
80:key /etc/openvpn/server/openvpn.key  # This file should be kept secret
85:dh /etc/openvpn/server/dh.pem
92:topology subnet
101:server 10.8.0.0 255.255.255.0
108:ifconfig-pool-persist ipp.txt
141:push "route 192.168.150.0 255.255.255.0"
231:keepalive 10 120
252:cipher AES-256-CBC
281:persist-key
282:persist-tun
287:status /var/log/openvpn-status.log
297:log-append  /var/log/openvpn.log
306:verb 3
311:mute 20

#具体的配置参数查看可以参考本地文件：C:\Users\admin\Documents\WeChat Files\suory163\FileStorage\File\2020-11
```

到这里、服务端的就配置完了



### 4.9、openvpn服务启动

#### 启动服务端

#### 修改内核转发

```shell
1.添加内核转发参数
vim /etc/sysctl.conf

net.ipv4.ip_forward = 1

2.生效
sysctl -p
```



```shell
# 编写openvpn启动单元文件
[root@vpn_server openvpn]# cat /usr/lib/systemd/system/openvpn.service
# 依赖于network-online.target才可以运行
[Unit]
Description=openvpn service 
After=network-online.target
Wants=network-online.target

# 服务运行参数
[Service]
Type=forking
User=root
Group=root
ExecStart=/usr/sbin/openvpn --daemon --config /etc/openvpn/server.conf
ExecStop=/bin/kill -9 $MAINPID
Restart=on=failure
PrivateTmp=true

[Install]
WantedBy=multi-user.target


# 重新加载单元服务
systemctl daemon-reload


# 启动openvpn
[root@vpn-server openvpn]# systemctl start openvpn

#开机自启
[root@vpn-server openvpn]# systemctl enable openvpn
Created symlink from /etc/systemd/system/multi-user.target.wants/openvpn.service to /usr/lib/systemd/system/openvpn.service.

#查看服务端口
[root@vpn-server openvpn]# netstat -tunpl|grep openvpn
tcp        0      0 192.168.37.100:1194     0.0.0.0:*               LISTEN      1987/openvpn      
```



### 4.10、openvpn客户但部署与连接结束

 VPN客户端部署

下载客户端，各个操作系统都有

https://openvpn.net/download-open-vpn/

### 修改客户端配置文件

#### 指定openvpn服务端信息即可

```shell
windows，下载openvpn客户端软件后，进行客户端配置
```

#### 需要在服务端执行



```shell
1.在linux openvpn上生成提供给client使用的配置文件
[root@vpn-server 3.0.8]# ./easyrsa gen-req client01 nopass

Note: using Easy-RSA configuration from: /etc/openvpn/easy-rsa/3.0.8/vars
Using SSL: openssl OpenSSL 1.0.2k-fips  26 Jan 2017
Generating a 2048 bit RSA private key
....+++
.+++
writing new private key to '/etc/openvpn/easy-rsa/3.0.8/pki/easy-rsa-2051.sfRRJ4/tmp.EatCZC'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Common Name (eg: your user, host, or server name) [client01]:

Keypair and certificate request completed. Your files are:
req: /etc/openvpn/easy-rsa/3.0.8/pki/reqs/client01.req
key: /etc/openvpn/easy-rsa/3.0.8/pki/private/client01.key


2.签发客户端证书
[root@vpn-server 3.0.8]# ./easyrsa sign client client01

Note: using Easy-RSA configuration from: /etc/openvpn/easy-rsa/3.0.8/vars
Using SSL: openssl OpenSSL 1.0.2k-fips  26 Jan 2017


You are about to sign the following certificate.
Please check over the details shown below for accuracy. Note that this request
has not been cryptographically verified. Please be sure it came from a trusted
source or that you have verified the request checksum with the sender.

Request subject, to be signed as a client certificate for 825 days:

subject=
    commonName                = client01


Type the word 'yes' to continue, or any other input to abort.
  Confirm request details: yes
Using configuration from /etc/openvpn/easy-rsa/3.0.8/pki/easy-rsa-2078.vnbrmn/tmp.bbDamV
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'client01'
Certificate is to be certified until Feb  5 03:24:04 2023 GMT (825 days)

Write out database with 1 new entries
Data Base Updated

Certificate created at: /etc/openvpn/easy-rsa/3.0.8/pki/issued/client01.crt


3.客户端配置文件汇总
# 放入/etc/openvpn/client/
[root@vpn-server 3.0.8]# mkdir -p /etc/openvpn/client/client01

[root@vpn-server pki]# cp /etc/openvpn/easy-rsa/3.0.8/pki/private/client01.key /etc/openvpn/client/client01/
[root@vpn-server pki]# cp /etc/openvpn/easy-rsa/3.0.8/pki/issued/client01.crt /etc/openvpn/client/client01/
[root@vpn-server pki]# cp /etc/openvpn/easy-rsa/3.0.8/pki/ca.crt /etc/openvpn/client/client01/

4.检查
[root@vpn-server openvpn]# tree client/client01/
client/client01/
├── ca.crt
├── client01.crt
└── client01.key


5.导出配置文件，发送给windows客户端
利用python程序，快速运行http服务
[root@vpn_server 3.0.8]# cd /etc/openvpn/client/client01/
[root@vpn_server client01]# python -m SimpleHTTPServer 80
Serving HTTP on 0.0.0.0 port 80 ..

```

此时可以访问

![image-20201102113440714](notes/image-20201102113440714.png)

并且要再找到一个ovpn文件，也可以直接手动生成

```shell
[root@vpn_server client01]# cat client01.ovpn
client
dev tun
proto tcp
remote 192.168.37.100 1194
resolv-retry infinite
nobind
persist-key
persist-tun
ca ca.crt
cert client01.crt
key client01.key
cipher AES-256-CBC
verb 3
```

client01.ovpn文件

```shell
client
dev tun
proto tcp
remote 192.168.37.100 1194
resolv-retry infinite
nobind
persist-key
persist-tun
ca ca.crt
cert client01.crt
key client01.key
cipher AES-256-CBC
verb 3
```



![](notes/image-20201102113712529.png)

此时把这四个文件下载到客户端本地

![image-20201102145313367](notes/image-20201102145313367.png)

![image-20201102145345766](notes/image-20201102145345766.png)

![image-20201102145514194](notes/image-20201102145514194.png)

### 4.11、vpn客户端连接验证

#### 启动openvpn客户端

**openvpn需要手动设置ip地址，因为关闭了DHCP功能**

通过openvpn连接后，进行连接，连接成功后，已然进入VPN的内网环境了

```shell
可以检查网络情况
1.服务端的隧道网段
[root@vpn-server /]# ip a

7: tun0: <POINTOPOINT,MULTICAST,NOARP,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN group default qlen 100
    link/none 
    inet 10.8.0.1/24 brd 10.8.0.255 scope global tun0
       valid_lft forever preferred_lft forever
    inet6 fe80::ba30:3a75:e34f:fad5/64 scope link flags 800 
       valid_lft forever preferred_lft forever

```

客户端尝试和VPN网络隧道通信

```shell
1.和10.8.0.1地址通信
ping 10.8.0.1

2.windows客户端此时应该是可以连接内网服务器ip的
ssh 192.168.150.100
```



![image-20201102145927776](notes/image-20201102145927776.png)

此时可以与vpn子网连通

![image-20201102150023523](notes/image-20201102150023523.png)

此时客户端可以连接内网

#### 成功后示意图

```shell
1.服务端的日志展示

[root@vpn-server /]# tail -f /var/log/openvpn.log 
Mon Nov  2 15:09:03 2020 192.168.37.1:64444 [client01] Peer Connection Initiated with [AF_INET]192.168.37.1:64444
Mon Nov  2 15:09:03 2020 client01/192.168.37.1:64444 MULTI_sva: pool returned IPv4=10.8.0.2, IPv6=(Not enabled)
Mon Nov  2 15:09:03 2020 client01/192.168.37.1:64444 MULTI: Learn: 10.8.0.2 -> client01/192.168.37.1:64444
Mon Nov  2 15:09:03 2020 client01/192.168.37.1:64444 MULTI: primary virtual IP for client01/192.168.37.1:64444: 10.8.0.2
Mon Nov  2 15:09:03 2020 client01/192.168.37.1:64444 PUSH: Received control message: 'PUSH_REQUEST'
Mon Nov  2 15:09:03 2020 client01/192.168.37.1:64444 SENT CONTROL [client01]: 'PUSH_REPLY,route 192.168.150.0 255.255.255.0,route-gateway 10.8.0.1,topology subnet,ping 10,ping-restart 120,ifconfig 10.8.0.2 255.255.255.0,peer-id 0,cipher AES-256-GCM' (status=1)
Mon Nov  2 15:09:03 2020 client01/192.168.37.1:64444 Data Channel: using negotiated cipher 'AES-256-GCM'
Mon Nov  2 15:09:03 2020 client01/192.168.37.1:64444 Outgoing Data Channel: Cipher 'AES-256-GCM' initialized with 256 bit key
Mon Nov  2 15:09:03 2020 client01/192.168.37.1:64444 Incoming Data Channel: Cipher 'AES-256-GCM' initialized with 256 bit key
Mon Nov  2 15:09:03 2020 client01/192.168.37.1:64444 IP packet with unknown IP version=0 seen





==> /var/log/openvpn-status.log <==
[root@vpn-server /]# tail -f /var/log/openvpn-status.log
OpenVPN CLIENT LIST
Updated,Mon Nov  2 15:15:35 2020
Common Name,Real Address,Bytes Received,Bytes Sent,Connected Since
client01,192.168.37.1:64444,39576,17419,Mon Nov  2 15:09:03 2020
ROUTING TABLE
Virtual Address,Common Name,Real Address,Last Ref
10.8.0.2,client01,192.168.37.1:64444,Mon Nov  2 15:15:06 2020
GLOBAL STATS
Max bcast/mcast queue length,0
END

```

![image-20201102152249493](notes/image-20201102152249493.png)

