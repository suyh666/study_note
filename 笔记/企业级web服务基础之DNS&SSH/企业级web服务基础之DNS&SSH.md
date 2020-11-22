

## 三、企业级web服务基础之DNS&SSH

## 1、互联网服务基础之DNS&SSH

### 	1.1、ip地址划分

公网ip：全世界唯一的一个ip地址、精确到某一个地址

ip地址划分

| 地址类型 | IP范围                    | 适用场景                                      |
| -------- | ------------------------- | --------------------------------------------- |
| A类地址  | 0.0.0.0~126.255.255.255   | 超大型的网络公司、如苹果公司、阿里等          |
| B类地址  | 128.0.0.0~191.255.255.255 | 大型局域网公司。（127段是回环地址、已被占用） |
| C类地址  | 192.0.0.0~223.255.255.255 | 学校、中小企业                                |
| D类I地址 |                           | 广播地址                                      |
| E类地址  |                           | 系统保留地址                                  |



### 1.2、公网地址与局域网地址

公网ip：全世界都能访问的ip，只有精确唯一的ip

局域网ip：公司内部（好比小区内部）的ip地址、

不同局域网内的ip数据交互：

公司A的192.168.10.11要想发数据给公司B的195.168.10.11、必须先将数据发送给公司A的公网ip、再将数据发送给公司B的公网ip、最后再转发到公司B的某一个局域网内的ip

<img src="notes/image-20191223182600180-1603867854483.png" style="zoom:50%;" />



### 1.3、DNS解析、DNS配文件、nstookup

**DNS域名解析：**

访问www.baidu.com这个域名、实际就是访问一个ip地址里的html文件、实际是将这个ip解析为www.baidu.com这个域名、方便记忆。



**dns服务器：**

linux的一台服务器、里面安装了dns如那件、一个存放大量域名的key 、value型的数据库软件。



常用的dns服务器：

腾讯：

阿里：223.5.5.5

114：114.114.114.114

谷歌：8.8.8.8

通过ping命令或nslookup查看域名的解析关系（就是域名对应的ip）

ping命令

```shell
[root@localhost ~]# ping www.pythonav.cn
PING www.pythonav.cn (123.206.16.61) 56(84) bytes of data.
64 bytes from www.pythonav.cn (123.206.16.61): icmp_seq=1 ttl=52 time=42.5 ms
```

nslookup命令

安装dns套件软件包：yum install bind-utils -y

使用案例

```shell
[root@localhost ~]# nslookup www.pythonav.cn		查看该域名对应的解析情况
Server:		192.168.0.1								#从这个解析器查找
Address:	192.168.0.1#53

Non-authoritative answer:
Name:	www.pythonav.cn							#找到的域名
Address: 123.206.16.61							#域名对应的ip
```





**dns客户端的配置文件：**

1、/etc/hosts	针对本地进行子自定义域名和ip强制解析

```shell
[root@localhost ~]# cat /etc/hosts
127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
::1         localhost localhost.localdomain localhost6 localhost6.localdomain

127.0.0.2 www.baidu.com 			#将本地的百度域名和这个ip绑定


[root@localhost ~]# ping www.baidu.com 
PING www.baidu.com (127.0.0.2) 56(84) bytes of data.						#所以显示的是127.0.0.2
64 bytes from www.baidu.com (127.0.0.2): icmp_seq=1 ttl=64 time=0.041 ms
64 bytes from www.baidu.com (127.0.0.2): icmp_seq=2 ttl=64 time=0.091 ms
```

2、/etc/resolv.conf		配置公网的域名解析的服务器地址

```shell
[root@localhost ~]# cat /etc/resolv.conf			
nameserver 192.168.0.1						
nameserver 114.114.114.114					#114的dns域名解析服务器ip
nameserver 8.8.8.8							#谷歌的dns域名解析服务器ip
nameserver 223.5.5.5						#腾讯的dns域名解析服务器ip
search localdomain

[root@localhost ~]# nslookup www.baidu.com
Server:		192.168.0.1							#这里用的是这个dns服务器ip去解析的
Address:	192.168.0.1#53

Non-authoritative answer:
www.baidu.com	canonical name = www.a.shifen.com.
Name:	www.a.shifen.com
Address: 163.177.151.110
Name:	www.a.shifen.com
Address: 163.177.151.109
```

### 1.4、DNS劫持

DNS劫持：用户访问百度首页】使用的是dns服务器是8.8.8.8；正常的是返回百度的首页、以及域名的ip给用户；但是经常返回的不是正常的百度首页和ip、说明被串改了dns服务器、从串改的dns服务器的本地/etc/hosts文件章自定义了域名和ip、导致返回给用户不是正常百度网页；



### 1.5、DNS域名解析的流程

1. 首先检查本地host文件有没强制性的绑定域名和ip；有的话直接按配置的查找；
2. 若本地host文件无做强制性绑定、则从本地的dns缓存查找；有的话直接找出；
3. 若本地dns缓存也没有保留、则通过首选的dns服务器查找；有的话直接找出；
4. 若首选dns服务器也没有、则通过上一级的根dns服务器（比首选·dns数据更全面）区查找；

### 1.6、dnsmasq搭建dns局域网

小型的域名解析需求使用：dnsmasq

大型企业使用的域名解析服务使用：bind

**dnsmasq安装**

yum install dnsmasq -y

**dnsmasq的配置文件**

1、主配置文件、安装后自动生成

```shell
/etc/dnsmasq.conf
```



2、内部需要解析的ip和域名

```shell
[root@localhost ~]# vim /etc/dnsmasq.hosts			#自己创建
```

3、dnsmasq的上游服务器

```shell
可以配置为resolv.conf，添加nameserver
/etc/resolv.dnsmasq.conf			#需自己创建
```



dnsmasq主配置文件配置内容

```shell
#过滤文件中排除空行^$和  ^#或者^;的行 
[root@local-pyyu ~]# grep -Ev '^$|^[#;]' /etc/dnsmasq.conf
#定义dnsmasq从哪里获取上游DNS服务器的地址，默认是从/etc/resolv.conf获取
resolv-file=/etc/resolv.dnsmasq.conf
#访问baidu.com时的所有域名都会被解析成123.206.16.61
address=/baidu.com/127.0.0.10
address=/taobao.com/123.206.16.61
#定义dnsmasq监听的地址，默认是监控本机的所有网卡上。局域网内主机若要使用dnsmasq服务时，指定本机的IP地址
listen-address=192.168.0.105
#本地域名配置文件（不支持泛域名），添加内部需要解析的地址和域名（重新加载即可生效）
addn-hosts=/etc/dnsmasq.hosts
#记录dns查询日志服务器
log-queries
##设置日志记录
log-facility=/var/log/dnsmasq.log
#包含其他文件夹下所有配置文件
conf-dir=/etc/dnsmasq.d,.rpmnew,.rpmsave,.rpmorig
```

内地解析地址配置

```shell
#添加需要内部解析的域名
[root@local-pyyu ~]# cat /etc/dnsmasq.hosts
123.206.16.61 testchaoge.com
```

添加上游dns服务器地址

```shell
#当某个域名无法解析的时候，发给上游服务器查询
[root@local-pyyu ~]# cat /etc/resolv.dnsmasq.conf
nameserver 223.5.5.5
nameserver 114.114.114.114
```

配置日志切割

```shell
[root@local-pyyu ~]# cat /etc/logrotate.d/dnsmasq
/var/log/dnsmasq.log {
    daily
    copytruncate
    missingok
    rotate 30
    compress
    notifempty
    dateext
    size 200M
}
```

启动dnsmasq服务

```shell
systemctl start dnsmasq
```

配置dns客户端地址

```shell
[root@localhost ~]# cat /etc/resolv.conf
#nameserver 192.168.0.1
#search localdomain
nameserver 192.168.0.105			#设置本地电脑为dns服务器
```

测试dns域名解析

```shell
[root@localhost ~]# nslookup www.baidu.com				#用本地dns解析
Server:		192.168.0.105
Address:	192.168.0.105#53

Name:	www.baidu.com
Address: 127.0.0.10
[root@localhost ~]# nslookup www.baidu.com 114.114.114.114			#用公网dns解析
Server:		114.114.114.114
Address:	114.114.114.114#53

Non-authoritative answer:
www.baidu.com	canonical name = www.a.shifen.com.
Name:	www.a.shifen.com
Address: 163.177.151.110
Name:	www.a.shifen.com
Address: 163.177.151.109

```

### 1.7、ssh远程加密连接.

ssh是一套网络协议、目的在于保证安全的网络以及加密远程登录信息

**ssh存在的原因？**

之前的运维人员通过ftp及telent工具进行服务器远程登录、两种协议都基于明文传输（账号、密码暴露在互联网中、别人可以通过抓包技术抓取数据）、很容易被黑客攻击

**ssh加密类型**



| ssh加密类型 | 解释                                          |
| ----------- | --------------------------------------------- |
| 对称加密    | 对数据进行加密、解密都需要用到这把钥匙        |
| 非对称加密  | 2把钥匙、1把公钥（锁）、1把私钥（开锁的钥匙） |

### 1.8、对称加密

加密、解密使用同一把密钥

![](notes/image-20200102153232967-1603867866976.png)

![](notes/image-20200102153616934-1603867875664.png)

对称加密优缺点：

数据加密强度高、但是当机器数量多时、无法保障密钥的安全性、一旦密钥被client盗窃丢失、则其他服务器的安全性无法得到保障

### 1.9、非对称加密

非对称加密分为：公钥（Public Key）与私钥（Private Key）
使用公钥加密后的密文，只能使用对应的私钥才能解开，破解的可能性很低。

![](notes/image-20200102174031585-1603867878147.png)

非对称加密的优缺点：

非对称加密分为：公钥（Public Key）与私钥（Private Key）
使用公钥加密后的密文，只能使用对应的私钥才能解开，破解的可能性很低。但是存在黑客中途拦截的情况

### 1.9、中间人攻击

中间人攻击：

1. 拦截客户但的请求；
2. 向客户端发送自己的、假的公钥；客户端不知情时、使用此公钥进行二次加密后再发送给黑客；
3. 黑客获取客户端使用公钥加密的数据后、并使用自己的私钥进行解密、从而获取了账号、密码对服务器造成威胁

![](notes/image-20200103092049627-1603867881178.png)

### 1.10、登录linux服务器的两大形式

如何避免登录服务器时的安全隐患

确保服务器的目标是身份是正确的

【基于口令的验证】

ssh公私钥都是基于命令在本地生成、无法工人、所以必须在clinet端自行对公钥确认

```shell
yumac: ~ yuchao$sshpyyu
The authenticity of host 'pyyuc (123.206.16.61)' can't be established.				#无法确认这台机器的真实性
ECDSA key fingerprint is SHA256:CVwhwfUiD53HpLPrretR4pGltYRL6QB+5lyI.				#目标机器的指纹信息、
Are you sure you want to continue connecting (yes/no)? yes							#询问是否登录
```

【获取远程主机的指纹信息--生成公钥生成】

```shell
如何验证这个公钥指纹，通过命令远程获取公钥指纹信息
[root@localhost ~]# ssh-keyscan 192.168.0.105 |grep ecdsa
# 192.168.0.105:22 SSH-2.0-OpenSSH_7.4
# 192.168.0.105:22 SSH-2.0-OpenSSH_7.4
# 192.168.0.105:22 SSH-2.0-OpenSSH_7.4
192.168.0.105 ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDXWuIbZ0wU7Hr19aEL5VkLPapLUPKaxHB69hUzTDSVTz6sXz8VxrEdZnZC8+TG15lWhx31u9PjA2lqYffaV3T4=
```

【可登录后查看指纹信息】

```shell
[root@localhost ~]# cat /etc/ssh/ssh_host_ecdsa_key.pub 
ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDXWuIbZ0wU7Hr19aEL5VkLPapLUPKaxHB69hUzTDSVTz6sXz8VxrEdZnZC8+TG15lWhx31u9PjA2lqYffaV3T4= 
#发现和远程获取的密钥是一致的
```

【假设用户确定连接远程主机的公钥】

```shell
Are you sure you want to continue connecting (yes/no)?   yes
```

【系统回应信息】

```shell
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '123.206.16.61' (ECDSA) to the list of known hosts.
root@123.206.16.61's password:
#输入正确密码即可登录
```

【ssh配置文件】

```shell
[root@chaogelinux .ssh]# pwd
/root/.ssh
[root@chaogelinux .ssh]# ls
authorized_keys  id_rsa  id_rsa.pub  known_hosts
```

- known_hosts：客户端接收到服务端的公钥后、服务端的公钥信息会存放在客户端的$HOME/.ssh/know_hosts文件中、下次再连接时、系统能够识别除服务端的公钥已存在本地、无需提示警告、则直接提示输入密码登录即可。
- authorized_keys：服务端远程主机将用户的公钥、保存在已登录用户的$HOME/.ssh/authorized_keys中
- id_rsa ：私钥文件
- id_rsa.pub：公钥文件

基于口令登录的缺点：针对数量多的的机器、每次都要记住密码进行登录、后期难以维护、更希望通过公钥实现免密登录

【基于公钥的密码认证】

登录流程原理：

1. 客户端发送自己的公钥给服务端、写入到服务端的authorized_key文件中；
2. 服务端收到客户端的连接请求后、在自身的authorized_key中、匹配客户端的公钥信息、并生成一个随机数R、使用公钥对随机数R进行加密、生成一个加密后的随机数R、再将这个随机数发给客户端
3. 客户端通过私钥解开加密的随机数、再对随机数R和当前会话的sessionkey采用MD5算法生成的Digest1、再发给服务端
4. 服务端对随机数R和当前会话session_key采用同样的算法生成Disgest2
5. 结果比较Disgest1和Digest2是否一致、从而完成认证

![](notes/微信图片_20201021120222-1603867886045.png)

配置SSH公钥认证

1.客户端本地生成公钥

```shell
[root@chaogelinux .ssh]# ssh-keygen -t rsa  #指定rsa密钥类型，默认一路回车

#会生成如下的公私钥
Your identification has been saved in /root/.ssh/id_rsa.
Your public key has been saved in /root/.ssh/id_rsa.pub.
```

2.客户端发送自己的公私钥到服务端中

```shell
#发送自己的公钥，写入到远端server的authorized_keys中
ssh-copy-id root@123.206.16.61
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/Users/yuchao/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@123.206.16.61's password:

Number of key(s) added:        1

Now try logging into the machine, with:   "ssh 'root@123.206.16.61'"
and check to make sure that only the key(s) you wanted were added.
```

3.此时即可免密登录了

```shell
yumac: ~ yuchao$ssh root@123.206.16.61   #直接输入登录命令即可
Last failed login: Fri Jan  3 16:54:46 CST 2020 from 189.39.13.1 on ssh:notty
There were 3 failed login attempts since the last successful login.
Last login: Fri Jan  3 16:14:59 2020 from 222.35.146.118
[root@chaogelinux ~]#
[root@chaogelinux ~]# cat ~/.ssh/authorized_keys
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDbkHjqnRe31HCteHc0BSfovvs9GqutyBfWvAJQy51Std1Ir3qBnHs29aKjwPL/jm/xHlZWgQ8mD/Xr091j2EVwGKUSMfCfH9nwbfu+0mwfwZKseJx5uliERShCpkRzA3Bhe6KOAqL1cgpfFzwKzO2Raga1PIGiCYcM/DIDlQh75/rEk9H5FGutamGiGrrtJfL4drRg6zEknrxSDWAMB3/MH6WUmkSWmGnECxOsPSy1PJN6Kqp1B yuchao@yumac
```



### 1.11、服务安全与ssh的配置文件

ssh的配置文件路径：/etc/ssh/sshd_config 

```shell
[root@chaogelinux ~]# grep -Ev '^$|^[# ]' /etc/ssh/sshd_config
Port 22    #默认端口
AddressFamily any    #配置地址家族，any支持ipv4，ipv6
ListenAddress 0.0.0.0 #设置sshd服务监听的ip地址，注意多网卡的绑定
HostKey /etc/ssh/ssh_host_rsa_key  #ssh各密钥存放的位置 
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key
PermitRootLogin yes  #是否允许root管理员直接登录，保证系统安全
StrictModes yes    #当用户的私钥改变，直接拒绝连接
MaxAuthTries 6    #最大密码尝试次数
MaxSessions 10    #最大终端数
AuthorizedKeysFile .ssh/authorized_keys    #信任主机的公钥文件存放地
PasswordAuthentication yes #是否设置密码验证机制
PermitEmptyPasswords no  #是否允许空密码登录，禁止
```

以上是参数默认值、一般正式生产环境、服务器一般禁止root登录、可大大降低服务器被黑客攻击的机率、需要修改sshd的主配置文件、修改以下参数‘

```shell
PermitRootLogin no   #禁止root直接登录,使用普通用户登录安全性较高
```

修改后生效的方法

```shell
systemctl restart sshd
systemctl enable sshd
```

以后root则不能登录、可使用普通用户登录、但是当我们进行权限范围较大时、要使用sudo命令结合使用



【**ssh与服务器安全实战案例**】

1、首先修改sshd的主配置文件、以下时默认取出注释行于空行内容

```shell
[root@localhost ~]# grep -Ev '^[# ]|^$' /etc/ssh/sshd_config 			取出注释行与空行内容
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key
SyslogFacility AUTHPRIV
AuthorizedKeysFile	.ssh/authorized_keys
PasswordAuthentication yes
ChallengeResponseAuthentication no
GSSAPIAuthentication yes
GSSAPICleanupCredentials no
UsePAM yes
X11Forwarding yes
AcceptEnv LANG LC_CTYPE LC_NUMERIC LC_TIME LC_COLLATE LC_MONETARY LC_MESSAGES
AcceptEnv LC_PAPER LC_NAME LC_ADDRESS LC_TELEPHONE LC_MEASUREMENT
AcceptEnv LC_IDENTIFICATION LC_ALL LANGUAGE
AcceptEnv XMODIFIERS
Subsystem	sftp	/usr/libexec/openssh/sftp-server
```

2、一般修改的要求如下

修改ssh的端口号、默认22、---Port   3142 

禁止root登录、PermitRootLogin yes no

禁止使用密码登录、只能通过公公私钥进行登录、PasswordAuthentication  no

```shell
[root@localhost ~]# grep -Ev '^[# ]|^$' /etc/ssh/sshd_config 
Port 3142																						#端口号
AddressFamily any
ListenAddress 0.0.0.0
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key
SyslogFacility AUTHPRIV
PermitRootLogin no																				#禁止root登录
AuthorizedKeysFile	.ssh/authorized_keys
PasswordAuthentication no																		#禁止使用密码登录、使用公私钥登陆
ChallengeResponseAuthentication no
GSSAPIAuthentication yes
GSSAPICleanupCredentials no
UsePAM yes
X11Forwarding yes
UseDNS yes
AcceptEnv LANG LC_CTYPE LC_NUMERIC LC_TIME LC_COLLATE LC_MONETARY LC_MESSAGES
AcceptEnv LC_PAPER LC_NAME LC_ADDRESS LC_TELEPHONE LC_MEASUREMENT
AcceptEnv LC_IDENTIFICATION LC_ALL LANGUAGE
AcceptEnv XMODIFIERS
Subsystem	sftp	/usr/libexec/openssh/sftp-server
```

tip：别立刻重启

```shell
#1、在服务器配置一个普通用户、并设置密码
[root@localhost ~]# useradd yuchao
[root@localhost ~]# passwd yuchao
更改用户 yuchao 的密码 。
新的 密码：
无效的密码： 密码少于 8 个字符
重新输入新的 密码：
passwd：所有的身份验证令牌已经成功更新。

#2在自己本地机器，生成一个普通用户的公私钥对
ssh-keygen -t rsa

#3.发送公钥给服务器，配置公钥登录
ssh-copy-id yuchao@192.168.0.106

#4.在正确配置了公私钥登录之后，yuchao这个用户就可以免密登录linux服务器了
ssh yuchao@192.168.0.106
```

让普通用户能够使用sudo命令的权限

```shell
1.使用root登录服务器，配置yuchao用户支持sudo命令
vim  /etc/sudoers文件
添加如下行
## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
yuchao  ALL=(ALL)       ALL

2.此时尝试用yuchao用户登录，是否能够使用sudo命令
```

```shell
1.使用root用户重启sshd服务
ssh root@192.168.0.106

2.重启sshd服务
systemctl restart sshd

3.此时机器已经禁止root登录，禁止密码登录，且修改了ssh端口为3142

4.此时只能使用配置好的yuchao用户进行免密登录了
ssh yuchao@192.168.0.106 -p 3142

5.利用sudo命令创建文件
[yuchao@localhost /]$ sudo touch 11.txt
[sudo] yuchao 的密码：
[yuchao@localhost /]$ ls
11.txt  123.txt  bin  boot  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
```



最后一步、重启sshd服务后、root用户则无法登录、只能使用yuchao用户进行登录

注意：ssh如果重启失败、要先关闭防火墙、selinux

```shell
[yuchao@localhost /]$ systemctl  status firewalld 			#检查防火墙状态
● firewalld.service - firewalld - dynamic firewall daemon
   Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
   Active: inactive (dead) since 二 2020-09-22 15:31:36 CST; 27min ago
     Docs: man:firewalld(1)
  Process: 657 ExecStart=/usr/sbin/firewalld --nofork --nopid $FIREWALLD_ARGS (code=exited, status=0/SUCCESS)
 Main PID: 657 (code=exited, status=0/SUCCESS)


[yuchao@localhost /]$ getenforce 				#检查selinux状态   disabled--表示关闭
Disabled										
```

## 2、互联网服务之存储篇

### 2.1、ftp文件传输协议介绍

FTP：文件传输协议。ftp是一种在互联网间能够进行文件传输的协议、基于c/s模式、默认服务端口为20（用于传输数据）、21（用于接收客户端发送的指令、参数）

FTP服务器：按照FTP协议在互联网上提供文件存储、文件访问的服务

FTP客户端：向服务器索要资源

**FTP工作模式：**

1. 主动模式：FTP服务器主动向客户端发起连接请求
2. 被动模式：FTP服务器等待客户端发起连接请求

**VSFTP服务的安装**

```shell
[yuchao@localhost ~]$ sudo yum install vsftpd -y	

#注意关闭防火墙规则
iptables -F
```



vsftp的配置文件

```shell
#过滤出非注释行，非空行
[root@chaogelinux ~]# grep -vE '^#|^$' /etc/vsftpd/vsftpd.conf
anonymous_enable=YES  #是否开启匿名用户允许访问
local_enable=YES    #是否允许本地用户登录FTP
write_enable=YES    #write_enable=YES #全局设置，是否容许写入，开启允许上传的权限
local_umask=022     #本地用户上传文件的umask
dirmessage_enable=YES    #允许为目录配置显示信息,显示每个目录下面的message_file文件的内容
xferlog_enable=YES    #开启日志功能，以及存放路径
xferlog_file=/var/log/vsftpd.log  #日志路径
connect_from_port_20=YES    #使用20端口进行连接
xferlog_std_format=YES    #标准日志格式
listen=YES        #绑定到监听端口
listen_ipv6=YES    #开启ipv6
pam_service_name=vsftpd    #设置PAM的名称
userlist_enable=YES    #设置用户已列表，允许或是禁止
tcp_wrappers=YES    #控制主机访问，检查/etc/hosts.allow  hosts.deny的配置达到防火墙作用
```

vsftp服务程序

vsftp允许用户三种认证模式登录到ftp服务器

1. 本地用户模式：基于linux本地账号密码进行认证、配置简单、一旦破解、则服务器就会存在安全隐患
2. 匿名用户模式：任何人无需密码直接登录
3. 虚拟用户模式：单独为FTP创建用户数据库、基于口令验证账户信息、只适用于FTP、不影响其他用户信息、最为安全

安装ftp客户端

ftp客户端又多种形式、有图形化、命令行

```shell
ftp> ascii  # 设定以ASCII方式传送文件(缺省值) 
ftp> bell   # 每完成一次文件传送,报警提示. 
ftp> binary # 设定以二进制方式传送文件. 
ftp> bye    # 终止主机FTP进程,并退出FTP管理方式. 
ftp> case   # 当为ON时,用MGET命令拷贝的文件名到本地机器中,全部转换为小写字母. 
ftp> cd     # 同UNIX的CD命令. 
ftp> cdup   # 返回上一级目录. 
ftp> chmod  # 改变远端主机的文件权限. 
ftp> close  # 终止远端的FTP进程,返回到FTP命令状态, 所有的宏定义都被删除. 
ftp> delete # 删除远端主机中的文件. 
ftp> dir [remote-directory] [local-file] # 列出当前远端主机目录中的文件.如果有本地文件,就将结果写至本地文件. 
ftp> get [remote-file] [local-file] # 从远端主机中传送至本地主机中. 
ftp> help [command] # 输出命令的解释. 
ftp> lcd # 改变当前本地主机的工作目录,如果缺省,就转到当前用户的HOME目录. 
ftp> ls [remote-directory] [local-file] # 同DIR. 
ftp> macdef                 # 定义宏命令. 
ftp> mdelete [remote-files] # 删除一批文件. 
ftp> mget [remote-files]    # 从远端主机接收一批文件至本地主机. 
ftp> mkdir directory-name   # 在远端主机中建立目录. 
ftp> mput local-files # 将本地主机中一批文件传送至远端主机. 
ftp> open host [port] # 重新建立一个新的连接. 
ftp> prompt           # 交互提示模式. 
ftp> put local-file [remote-file] # 将本地一个文件传送至远端主机中. 
ftp> pwd  # 列出当前远端主机目录. 
ftp> quit # 同BYE. 
ftp> recv remote-file [local-file] # 同GET. 
ftp> rename [from] [to]     # 改变远端主机中的文件名. 
ftp> rmdir directory-name   # 删除远端主机中的目录. 
ftp> send local-file [remote-file] # 同PUT. 
ftp> status   # 显示当前FTP的状态. 
ftp> system   # 显示远端主机系统类型. 
ftp> user user-name [password] [account] # 重新以别的用户名登录远端主机. 
ftp> ? [command] # 同HELP. [command]指定需要帮助的命令名称。如果没有指定 command，ftp 将显示全部命令的列表。
ftp> ! # 从 ftp 子系统退出到外壳。
```

FileZilla图形化安装

```shell
安装ftp命令行
yum install ftp -y
```



### 2.2、匿名用户认证连接vsftpd

【匿名用户模式登录】

匿名用户：最不安全的登录模式、无需使用密码登录，一般访问不重要的资源、且放在企业内网环境中、置于防火墙规则下、保证基本的安全性

匿名用户需要配置的基本文件

```shell
[root@chaogelinux ~]# grep '^anon' /etc/vsftpd/vsftpd.conf
anonymous_enable=YES    #允许匿名访问
anon_upload_enable=YES    #允许匿名用户上传
anon_mkdir_write_enable=YES    #允许匿名用户创建目录
anon_other_write_enable=YES    #允许匿名用户修改目录
#listen_ipv6					#注释这个参数
```

重启vsftpd服务、且开机自启

```shell
[root@chaogelinux ~]# systemctl restart vsftpd  #重启服务
[root@chaogelinux ~]# systemctl enable vsftpd        #开启自启
```

通过客户端连接ftp服务器

```shell
[root@chaogelinux ~]# ftp 192.168.0.106       #连接ftp服务器的ip地址
[root@localhost ~]# ftp 192.168.0.106
Connected to 192.168.0.106 (192.168.0.106).
220 (vsFTPd 3.0.2)
Name (192.168.0.106:root): anonymous		#输入默认账号
331 Please specify the password.
Password:								#密码为空
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp>

ftp> ls    
227 Entering Passive Mode (10,141,32,137,98,195).
150 Here comes the directory listing.
drwxr-xr-x    2 0        0            4096 Oct 30  2018 pub				#这里查看的时ftp中的/var/ftp/pub目录
226 Directory send OK.
ftp> cd pub        #切换工作目录
250 Directory successfully changed.
ftp> mkdir chaoge                #发现在这里创建文件夹报错了
550 Create directory operation failed.
```

由于使用的时匿名用户登录ftp、然后检查目录权限

```shell
[root@localhost ftp]# ll
总用量 0
drwxr-xr-x 2 root root 6 4月   1 12:55 pub			#其他用户无写入权限、属主为root
```

更改目录为ftp属主

```shell
#检查系统用户ftp
[root@chaogelinux ftp]# id ftp
uid=14(ftp) gid=50(ftp) 组=50(ftp)

#更改pub的属主
[root@chaogelinux ftp]# chown -Rf ftp /var/ftp/pub/        #递归处理所有的文件及子目录  去除错误信息
[root@chaogelinux ftp]# ll
总用量 4
drwxr-xr-x 2 ftp root 4096 10月 31 2018 pub
```

再次登录ftp、写入数据

```shell
[root@localhost ~]# ftp 192.168.0.106
Connected to 192.168.0.106 (192.168.0.106).
220 (vsFTPd 3.0.2)
Name (192.168.0.106:root): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
227 Entering Passive Mode (192,168,0,106,52,48).
150 Here comes the directory listing.
drwxr-xr-x    4 14       0              28 Sep 23 05:36 pub
226 Directory send OK.
ftp> cd pub
250 Directory successfully changed.
ftp> ls
227 Entering Passive Mode (192,168,0,106,140,50).
150 Here comes the directory listing.
drwx------    2 14       50              6 Sep 23 05:21 su
drwx------    2 14       50              6 Sep 23 05:24 test
226 Directory send OK.
ftp> mkdir suge											#创建文件夹
257 "/pub/suge" created
ftp> ls
227 Entering Passive Mode (192,168,0,106,176,118).
150 Here comes the directory listing.
drwx------    2 14       50              6 Sep 23 05:21 su
drwx------    2 14       50              6 Sep 23 06:02 suge
drwx------    2 14       50              6 Sep 23 05:24 test
226 Directory send OK.
ftp> rename suge su2								#重命名文件夹
350 Ready for RNTO.
250 Rename successful.
ftp> ls
227 Entering Passive Mode (192,168,0,106,22,218).
150 Here comes the directory listing.
drwx------    2 14       50              6 Sep 23 05:21 su
drwx------    2 14       50              6 Sep 23 06:02 su2
drwx------    2 14       50              6 Sep 23 05:24 test
226 Directory send OK.
ftp> rmdir su										#删除文件夹
250 Remove directory operation successful.
ftp> ls
227 Entering Passive Mode (192,168,0,106,217,164).
150 Here comes the directory listing.
drwx------    2 14       50              6 Sep 23 06:02 su2
drwx------    2 14       50              6 Sep 23 05:24 test
226 Directory send OK.
```

检查ftp服务器上的资源

```shell
[root@localhost /]# ls /var/ftp/pub/
su2  test
```



### 2.3、本地用户认证连接vsftpd

【本地用户验证模式】

本地用户模式相对匿名用户更安全、修改配置文件、关闭匿名参数、开启本地用户模式

修改配置参数如下

```shell
vim /etc/vsftpd/vsftpd.conf
anonymous_enable=NO    #关闭匿名用户模式
local_enable=YES    #开启本地用户模式
write_enable=YES    #设置可写权限
local_umask=022    #文件umask值
userlist_enable=YES    #启用【禁止登录的用户名单】文件名是，ftpusers，user_list
userlist_deny=YES    #开启禁止登录名单
```

重启vsftp服务、并开机自动加载

```shell
systemctl restart vsftpd
systemctl enable vsftpd
```

使用客户端的用户进行连接ftp

```shell
1.首先确保ftp服务器有一个普通用户
[root@localhost ~]# grep 'syh' /etc/passwd
syh:x:1000:1000::/home/syh:/bin/bash

2.通过客户端连接ftp
[root@localhost ~]# ftp 192.168.0.106				
Connected to 192.168.0.106 (192.168.0.106).
220 (vsFTPd 3.0.2)
Name (192.168.0.106:root): syh
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 

3.查看ftp下的内容
ftp> ls													#等同于查看用户的家目录
227 Entering Passive Mode (192,168,0,106,242,206).
150 Here comes the directory listing.
drwxrwxr-x    2 1000     1000            6 Sep 23 06:28 suyonghong
226 Directory send OK.


4.创建目录
ftp> mkdir haha
257 "/home/syh/haha" created
ftp> ls
227 Entering Passive Mode (192,168,0,106,44,22).
150 Here comes the directory listing.
drwxr-xr-x    2 1000     1000            6 Sep 23 06:35 haha
drwxrwxr-x    2 1000     1000            6 Sep 23 06:28 suyonghong

5.查看ftp的资源
[syh@localhost ~]$ ls
haha  suyonghong
```

禁止用户登录的配置文件

```shell
[root@localhost ~]# cat /etc/vsftpd/ftpusers 
# Users that are not allowed to login via ftp
root
bin
daemon
adm
lp
sync
shutdown
halt
mail
news
uucp
operator
games
nobody

#tip:配置文件存在的用户则不能通过ftp登录

1.添加syh加入禁止用户的名单
vim /etc/vsftpd/ftpusers 

# Users that are not allowed to login via ftp
root
bin
daemon
adm
lp
sync
shutdown
halt
mail
news
uucp
operator
games
nobody
syh

2.在客户端使用syh登录
[root@localhost ~]# ftp 192.168.0.106
Connected to 192.168.0.106 (192.168.0.106).
220 (vsFTPd 3.0.2)
Name (192.168.0.106:root): syh
331 Please specify the password.
Password:
530 Login incorrect.
Login failed.										#提示登录失败
ftp> 
```

### 2.4、虚拟用户认证连接vsftpd

【虚拟用户的认证】

虚拟用户：通过虚拟技术创建出来的用户、相对服务器来说是最安全的

1、DB工具的安装--能将普通文件转换为vsftpd识别的数据库加密文件

```shell
yum install db4 db4-utils -y
```



2、创建用于验证vsftpd数据的文件、并编辑此文件

```shell
[root@localhost vsftpd]# touch ftp_user.txt
[root@localhost vsftpd]# ls
ftpusers  ftp_user.txt  user_list  vsftpd.conf  vsftpd_conf_migrate.sh
[root@localhost vsftpd]# vim ftp_user.txt
yuchao
123456
syh
123456
```

3、考虑到文件的安全性、以及vsftpd不能识别txt文本、还得将此文件通过ad_load命令加密处理、以及修改权限、使其他用户无法访问

```shell
#1.进行文件加密
[root@localhost vsftpd]# db_load -T -t hash -f /etc/vsftpd/ftp_user.txt /etc/vsftpd/ftp_user.db

[root@localhost vsftpd]# cat ftp_user.db				
そ˽౓эh^123456yuchao[root@localhost vsftpd]# 

[root@localhost vsftpd]# file ftp_user.db					#查看文件类型
ftp_user.db: Berkeley DB (Hash, version 9, native byte-order)

#2.降低文件权限
[root@localhost vsftpd]# chmod 600 ftp_user.db 

#3.删除旧的数据文本
[root@localhost vsftpd]# rm ftp_user.txt 
rm：是否删除普通文件 "ftp_user.txt"？y
```

4、创建当虚拟用户登陆后默认访问的文件夹路径、切和linux中的一个本地用户做一个映射关系、防止匿名用户登陆后、创建了文件夹、但系统没有此用户的报错权限问题

```shell
#1.新建一个用户，指定用户家目录，且禁止登录
useradd -d /var/ftpdir -s /sbin/nologin virtual_chao

#2.检查此ftpdir的属性
[root@chaogelinux vsftpd]# ls -ld /var/ftpdir/
drwx------ 2 virtual_chao virtual_chao 4096 1月   8 09:46 /var/ftpdir/

#3.更改文件夹权限
[root@chaogelinux vsftpd]# chmod -Rf 755 /var/ftpdir/

#4.添加virtual_chao用户至ftpusers禁止用户登录文件，增大系统安全，但是不会影响虚拟用户登录
[root@chaogelinux vsftpd]# echo 'virtual_chao' >> ftpusers
```

5、创建支持虚拟用户的PAM文件、PAM是一组安全机制模块、编辑认证文件/etc/pam.d/vsftpd

```shell
#注释掉文中所有语句，添加以下2句，注意db参数指定的是db_load生成的文件路径，无需添加后缀

[root@chaogelinux pam.d]# cat /etc/pam.d/vsftpd
auth required pam_userdb.so db=/etc/vsftpd/ftp_user
account required pam_userdb.so db=/etc/vsftpd/ftp_user
```

6、修改vsftpd的配置文件

```shell
[root@chaogelinux pam.d]# grep -Ev '^$|^#' /etc/vsftpd/vsftpd.conf
anonymous_enable=NO     #禁止匿名模式
local_enable=YES        #允许本地用户
write_enable=YES    
local_umask=022
anon_upload_enable=YES
anon_mkdir_write_enable=YES
anon_other_write_enable=YES
dirmessage_enable=YES
xferlog_enable=YES
connect_from_port_20=YES
xferlog_std_format=YES
listen=NO
listen_ipv6=YES
pam_service_name=vsftpd        #指定PAM认证文件
userlist_enable=YES
userlist_deny=YES
tcp_wrappers=YES
guest_enable=YES        #开启虚拟用户
guest_username=virtual_chao        #指定虚拟用户账号
allow_writeable_chroot=YES      #如果用户被限制只能在其家目录，允许用户可以对家目录写入数据
```

7.针对/etc/vsftpd/ftp_user.txt里的用户用户进行权限设置

- yuchao:可上传、新建、修改、查看、删除的权限
- syh:只可读

可通过vsftp配置实现、定义user_cinfig_dir参数实现不同的虚拟用户有不同的权限配置

```shell
#1.新建管理虚拟用户的文件夹
[root@localhost var]# mkdir /etc/vsftpd/virtual_user_dir


#2.进入文件夹、创建yuchao用户权限的配置文件
[root@localhost var]# cd /etc/vsftpd/virtual_user_dir
[root@localhost virtual_user_dir]# touch yuchao 
[root@localhost virtual_user_dir]# ls
yuchao
vim yuchao
anon_upload_enable=YES
anon_mkdir_write_enable=YES
anon_other_write_enable=YES


#3.进入文件夹、创建syh用户权限的配置文件
[root@localhost var]# cd /etc/vsftpd/virtual_user_dir
[root@localhost virtual_user_dir]# touch syh
[root@localhost virtual_user_dir]# ls
yuchao syh
vim syh
anon_upload_enable=NO
anon_mkdir_write_enable=NO
anon_other_write_enable=NO

#4.编辑vsftpd配置文件、添加如下一行
cat /etc/vsftpd/vsftpd.conf
user_config_dir=/etc/vsftpd/virtual_user_dir
```

8.重启vsftp服务

```shell
[root@localhost virtual_user_dir]# systemctl  restart vsftpd
```

9.使用yuchao用户在客户端进行读写操作

```shell
[root@localhost ~]# ftp 192.168.0.106
Connected to 192.168.0.106 (192.168.0.106).
220 (vsFTPd 3.0.2)
Name (192.168.0.106:root): yuchao
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
227 Entering Passive Mode (192,168,0,106,242,215).
150 Here comes the directory listing.
drwxr-xr-x    2 0        0               6 Sep 23 12:30 我是映射家目录					#查看虚拟家目录内容、说明有可读权限
226 Directory send OK.
ftp> mkdir haha																	   #创建haha目录、说明有写的权限
257 "/haha" created
ftp> ls
227 Entering Passive Mode (192,168,0,106,72,231).
150 Here comes the directory listing.
drwx------    2 1002     1002            6 Sep 23 12:34 haha								
drwxr-xr-x    2 0        0               6 Sep 23 12:30 我是映射家目录
ftp> rename haha xixi
350 Ready for RNTO.
250 Rename successful.																#重命名haha目录为xixi、说明有修改的权限
ftp> ls
227 Entering Passive Mode (192,168,0,106,104,61).
150 Here comes the directory listing.
drwx------    2 1002     1002            6 Sep 23 12:34 xixi								
drwxr-xr-x    2 0        0               6 Sep 23 12:30 我是映射家目录
226 Directory send OK.
ftp> rmdir xixi																		#删除目录成功、说明有删除的权限
250 Remove directory operation successful.
ftp> ls
227 Entering Passive Mode (192,168,0,106,77,93).
150 Here comes the directory listing.
drwxr-xr-x    2 0        0               6 Sep 23 12:30 我是映射家目录
226 Directory send OK.
```

10.使用syh用户在客户端看能否进行读写操作

```shell
[root@localhost ~]# ftp 192.168.0.106
Connected to 192.168.0.106 (192.168.0.106).
220 (vsFTPd 3.0.2)
Name (192.168.0.106:root): syh
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
227 Entering Passive Mode (192,168,0,106,185,240).
150 Here comes the directory listing.
drwxr-xr-x    2 0        0               6 Sep 23 12:30 我是映射家目录						#查看映射家目录/var/ftpdir下的内容
ftp> mkdir hah
550 Permission denied.																#权限不足、说明没有写的权限
ftp> rename 我是家目录 hello
550 Permission denied.																#修改目录名字、也权限不足

#说明：syh只有读的权限
```

### 2.5、Linux与Windows之间进行文件共享(Samba服务)

特点：用于linux与windows之间的文件共享、用于局域网内

【samba服务的安装】

```shell
#1.安装samba工具
yum install samba -y
```

### **2.6、Sam的核心配置文件**

​	samba的配置文件分为全局配置、共享配置

- ​		[global]：全局配置
- ​		共享配置
- ​				[homes]	

```shell
#1.学习samba的配置文件
[root@localhost ~]# vim /etc/samba/smb.conf					#samba的配置文件路径

[global]														
        workgroup = SAMBA
        security = user

        passdb backend = tdbsam

        printing = cups
        printcap name = cups
        load printers = yes
        cups options = raw

[homes]														
        comment = Home Directories
        valid users = %S, %D%w%S
        browseable = No
        read only = No
        inherit acls = Yes
```

【全局配置】

```shell
workgroup = MYGROUP
Samba服务器加入的工作组名，一个局域网内，必须有相同的工作组名。

server string = Samba Server Version %v
Samba服务器注释，可以不选，%v代表显示Samba版本号

netbios name = samba
主机NetBIOS名

netbios name = samba
主机NetBIOS名

interfaces = lo eth0
设置Samba服务器端监听网卡，可以写网卡名称或者IP地址

hosts allow/deny = 10.10.10.1 
允许连接到Samba server客户端IP，多个参数用空格分开。可以用一个IP表示，也可以用一个网段表示。

max connections = 0
用来指定连接Samba server服务器最大连接数如果操作则连接请求被拒绝。0表示不限制。

deadtime = 0
来设置断掉一个没有任何文件的链接时间。单位十分钟，0代表Samba server不自动断开任何连接

time server = yes/no
用来设置让nmdb成为Windows客户端的时间服务器

log file = /var/log/samba/%m.log
设置Samba server日志文件存储位置和日志名称。文件后面加一个%m（主机名），每个主机都会有一个主机名.log日志文件

max log size = 50
限制每个日志文件的最大容量为50KB，0代表不限制

Security = user
设置客户端访问Samba服务器的验证方式，Samba4版本已经不使用share和server方式，这里不介绍
1) user:Samba用户名和密码登录
2) domain：添加Samba服务器到N域，由NT与控制起来进行身份验证。域安全级别，使用主域控制器（PDC）来完成认证

passdb backend = tdbsam
后台管理用户密码方式
1）smbpasswd：该方式是使用smb自己的工具smbpasswd来给系统用户
2）tdbsam：该方式则是使用一个数据库文件来建立用户数据库。
3）ldapsam：该方式则是基于LDAP的账户管理方式来验证用户。

smb passwd file = /etc/samba/smbpasswd
用来定义samba用户的密码文件。smbpasswd文件如果没有那就要手工新建。

username map = /etc/samba/smbusers
用来定义用户名映射，比如可以将root换administrator、admin等。

guest account = nobody
用来设置guest用户名。

socket options = TCP_NODELAY SO_RCVBUF=8192 SO_SNDBUF=8192
用来设置服务器和客户端之间会话的Socket选项，可以优化传输速度

load printers = yes/no
设置是否在启动Samba时就共享打印机。
```



【共享参数】

```shell
comment = 任意字符串
comment是对该共享的描述，可以是任意字符串。

browseable = yes/no
browseable用来指定该共享是否可以浏览。

path = 共享目录路径
path用来指定共享目录的路径。

writable = yes/no
用来指定该共享路径是否可写

invalid users = 禁止访问该共享的用户

invalid users用来指定不允许访问该共享资源的用户。
例如：invalid users = root，@bob（多个用户或者组中间用逗号隔开。）

public = yes/no
用来指定该共享是否允许guest账户访问。

guest ok = yes/no
用来指定该共享是否允许guest账户访问。
```



【配置共享资源】

- 全局配置：针对整体的资源共享设置、生效于每一个独立的共享资源
- 共享参数：可设置单独的共享资源、仅仅对该资源有效

```shell
#修改smb.conf如下，添加以下参数
[chaoge]
comment = This is test configure
path = /home/chaoge
public = no
writable = yes
guest ok = yes
```

### 2.7、pdbedit命令**

pdbedit是s'amba的管理命令

```shell
pdbedit -a username：新建Samba账户。

pdbedit -r username：修改Samba账户。

pdbedit -x username：删除Samba账户。

pdbedit -u, --user=USER      use username

pdbedit -L：列出Samba用户列表，读取passdb.tdb数据库文件。

pdbedit -Lv：列出Samba用户列表详细信息。

pdbedit -c “[D]” -u username：暂停该Samba用户账号。

pdbedit -c “[]” -u username：恢复该Samba用户账号。
```

### **2.8、创建用于访问samba共享资源的账户信息**

tips:此用户必须在系统中存在

```shell
[root@localhost ~]# id syh												#检查syh用户是否存在
uid=1000(syh) gid=1000(syh) 组=1000(syh)

[root@localhost ~]# pdbedit -a -u syh									#创建并指定syh用户
new password:
retype new password:
Unix username:        syh
NT username:          
Account Flags:        [U          ]
User SID:             S-1-5-21-1891962639-648204531-1389874737-1000
Primary Group SID:    S-1-5-21-1891962639-648204531-1389874737-513
Full Name:            
Home Directory:       \\localhost\syh
HomeDir Drive:        
Logon Script:         
Profile Path:         \\localhost\syh\profile
Domain:               LOCALHOST
Account desc:         
Workstations:         
Munged dial:          
Logon time:           0
Logoff time:          三, 06 2月 2036 23:06:39 CST
Kickoff time:         三, 06 2月 2036 23:06:39 CST
Password last set:    四, 24 9月 2020 10:18:54 CST
Password can change:  四, 24 9月 2020 10:18:54 CST
Password must change: never
Last bad password   : 0
Bad password count  : 0
Logon hours         : FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF

[root@localhost home]# ll													#检查共享资源目录的权限
总用量 0
drwx------. 4 syh    syh    130 9月  24 10:28 syh
drwx------. 3 yuchao yuchao 111 9月  23 12:21 yuchao

[root@localhost home]# systemctl stop firewalld									#关闭防火墙
[root@localhost home]# systemctl status firewalld								#检查防火墙是否运行

[root@localhost home]# systemctl restart smb									#重启smb服务
[root@localhost home]# netstat -tunpl |grep smb 								#查看smb服务的端口是否开启
tcp        0      0 0.0.0.0:445             0.0.0.0:*               LISTEN      1489/smbd           
tcp        0      0 0.0.0.0:139             0.0.0.0:*               LISTEN      1489/smbd           
tcp6       0      0 :::445                  :::*                    LISTEN      1489/smbd           
tcp6       0      0 :::139                  :::*                    LISTEN      1489/smbd  
```

### 2.9进行客户端远程连接

windows连接步骤：

1. win+r进行运行界面、输入\\\192.168.0.106
2. 输入使用samba创建的账号和密码进行登录即可
3. 查看指定目录下的内容是否于服务器一致
4. 此时就可以共享文件夹了

mac连接步骤：

1、![](notes/image-20200108152322657-1603867911460.png)

2、<img src="notes/image-20200108152431519-1603867913400.png" style="zoom:50%;" />

3、<img src="notes/image-20200108152455303-1603867916186.png" style="zoom:50%;" />

4、<img src="notes/image-20200108152523747-1603867919065.png" style="zoom:50%;" />

5、<img src="notes/image-20200108152705070-1603867922532.png" style="zoom:50%;" />



<img src="notes/image-20200108152721358-1603867928763.png" style="zoom:50%;" />



### 2.10、NFS网络文件系统

### 2.11、NFS服务作用

用于linux与linux之间进行文件共享、网络文件共享；NSF服务可将远程linux系统上的文件共享资源挂载到本地目录

各种共享服务适用场景

| 服务名称            | 使用场景               |
| ------------------- | ---------------------- |
| samba服务           | linux的办公局域网共享  |
| windows网络共享服务 | windows的办公局域共享  |
| NFS服务             | 中小型网站集群架构后端 |
| GlusterFS、FastDFS  | 大型网络集群、         |

### 2.12、企业集群运用NFS的原因



![](notes/image-20200310095658885-1603868002976.png)



用户A：请求上传图片、经过互联网-->负载均衡-->web01机器

用户B：请求查看图片、但是分配到的web02机器上没有图片资源、从而造成客户心里不爽

![](notes/image-20200310100922965-1603868005534.png)

此图是搭建了NFS的情况

1. 无论哪个web设备上有无资源、只要用户获取资源、都会从NFS服务器上寻找资源、再一层一层返回给用户展示
2. 中小型企业不会购买硬件存储、由于成本高；而大公司由于业务发展快、需购买硬件存储分散网站的压力；随着网站并发压力的增长、硬件设备的扩展也会变得费劲、且金钱会翻倍增长



### **2.13、NFS的工作原理**

![](notes/image-20200108161228232-1603868008064.png)

1. 在NFS服务器设置一个共享目录/vedio、其他有权限访问的客户端就可把此目录/vedio挂载到本地客户端的挂载点（一个路径、目录）
2. 客户端正确挂载完毕后、可进入NSF客户端的挂载点、就可以访问NFS服务端的共享目录/vedio的内容

### 2.14、NFS与RPC

NFS与RPC的原理.

NFS是通过网络来进行数据传输（网络文件系统）、因此就存在port端口、通过port来进行数据传输；但是传输的端口是随机算账（每次重启可查看端口号）

而随机的端口号是通过RPC服务进行实现

RPC的作用:

记录每个NFS的功能对应端口、且当NFS客户端发送请求时、把该功能的端口信息和功能传递个i发出请求的NFS客户端、保证客户端能正确连接到NFS端口、达到传输目的。

<img src="notes/image-20200310104034925-1603868010790.png" style="zoom: 50%;" />

RPC是如何知道NFS的每个端口呢？

NFS启动时会随机采用若干个端口、并主动在RPC服务之注册以及功能信息

### 2.15、RPCBIND服务

注意：

1. 在启动NFS服务之前、必须先启动RPC服务；centos7服务器下为rpcbind服务、否则NFserver无法向RPC注册信息；

2. 如果重启了RPC服务、则原来注册的NFS服务端信息也就失效了、也必须重启服务、再次注册信息给RPC服务

   

### 2.16、NFS的工作原理

<img src="notes/image-20200310111226134-1603868013721.png" style="zoom: 50%;" />

当访问程序通过NFS客户端向NFS服务端存储文件时、其请求流程如下：

1. 用户访问网站程序、由程序在NFS客户端发出存储文件请求、此时DNS客户端（执行程序的机器）的RPC服务就会网络向NFS服务器的111端口发出NFS文件存取的请求
2. NFS服务器的RPC找到对应的注册端口、通知NFS客户端RPC服务
3. 此时NFS客户端获取正确的端口、并与NFS daemon连接存储数据
4. NFS客户端把数据存取成功后给会前端程序、告知用户存取结果完成能完成一次请求过程



### 2.17、NFS安装及配置使用

【NFS软件安装列表】

- nfs-utils：NFS的服务主程序、包括rpc.nfsd、rpc.mountd两个守护进程以及相关文档、命令
- rpcbind：时Centos7/6环境下的RPC程序

```shell
1.检查默认NFS软件安装情况
[root@localhost ~]# rpm -qa nfs-utils rpcbind
rpcbind-0.2.0-49.el7.x86_64
nfs-utils-1.3.0-0.66.el7_8.x86_64

2.安装软件包命令
yum install nfs-utils rpcbind -y
```

【环境配置】

- NFS属于cs模式、准备一台服务端、一台客户端
- 在server机器上创建用于NFS共享的文件夹、且设置好权限

```shell
[root@localhost ~]# mkdir /nfsshare
[root@localhost /]# chmod -Rf 777 /nfsshare/
```

### 2.18、修改NFS配置文件

默认路径为/etc/exports

```shell
exports配置文件语法

NFS共享目录  NFS客户端地址(参数1、参数2...) 客户点地址2（参数1、参数2...）

例如
/        master(rw)  master2(rw,no_root_squash)
/pub   *(rw)
/home/chao   123.206.16.61(ro)
```

【nfs语法参数解释】

1.NFS共享目录：为NFS服务器要共享的实际目录，必须绝对路径，注意目录的本地权限，如果要读写共享，要让本地目录可以被NFS客户端的(nfsnobody)读写
2.NFS客户端地址，也就是NFS服务器端授权可以访问共享目录的客户端地址，详见下表
3.权限参数，对授权的NFS客户端访问权限设置，见下表



【NFS客户端地址配置说明】

| 客户端地址         | 具体地址       | 说明                                  |
| ------------------ | -------------- | ------------------------------------- |
| 单一客户端         | 192.168.0.106  | 用的少                                |
| 整个网段           | 192.168.1.0/24 | 24表示子网掩码255.255.255.0、用到较多 |
| 授权域名客户端     | nsf.chaoge.com | 弃用                                  |
| 授权整个域名客户端 | *.chaoge.com   | 弃用                                  |

### 2.19、配置案例

案例1

```shell
#修改nfs配置文件为如下示例
[root@chaogelinux nfsShare]# cat /etc/exports
/nfsShare *(insecure,rw,sync,root_squash)

#表示共享该文件夹，且提供给所有网段的机器可访问，配置规则是，可读写，数据同步写入到磁盘，把root管理员映射为本地的匿名用户，insecure是客户端从大于1024的端口发送链接
```

案例2

```shell
/home/chaoge   192.168.178.0/24(ro)
只读共享，例如一些生成服务器的日志目录，又不想给开发服务器的权限，可以用此办法，共享目录给他人只读查看
```

【配置参数解释】

ro 只读
rw 读写
root_squash 当nfs客户端以root访问时，它的权限映射为NFS服务端的匿名用户，它的用户ID/GID会变成nfsnobody
no_root_squash 同上，但映射客户端的root为服务器的root，不安全，避免使用
all_squash 所有nfs客户端用户映射为匿名用户，生产常用参数
sync 数据同步写入到内存与硬盘，优点数据安全，缺点性能较差
async 数据写入到内存，再写入硬盘，效率高，但可能内存数据会丢

### 2.20、启动NFS服务端

- NFS服务基于RPC协议通信、默认端口为111、确保系统运行rpcbind服务、检查111端口
- 注意：rpcbind服务即使停止、111端口也不会掉、因为还有rpcbind.socket服务

```shell
[root@chaogelinux ~]# systemctl status rpcbind
● rpcbind.socket - RPCbind Server Activation Socket
   Loaded: loaded (/usr/lib/systemd/system/rpcbind.socket; enabled; vendor preset: enabled)
   Active: active (running) since 二 2020-03-10 10:59:12 CST; 4h 35min ago
   Listen: /var/run/rpcbind.sock (Stream)
           0.0.0.0:111 (Stream)
           0.0.0.0:111 (Datagram)

3月 10 10:59:12 chaogelinux systemd[1]: Listening on RPCbind Server Activation Socket.


# 启动rpcbind服务
systemctl restart rpcbind

#启动
systemctl restart nfs-server
```



### 2.21、配置NFS本地服务端过程

创建共享目录且设置权限

```shell
1.确保RPC服务启动了
systemctl start rpcbind

2.创建需要共享的目录，以及资料，并且授权
mkdir -p /nfsshare
touch /nfsshare/hello.txt  

3.修改文件夹的user，group，这里更换权限是防止NFS客户端无法写入数据，当然也可以修改服务端目录777权限，但是不安全，不推荐
chown -R nfsnobody.nfsnobody /nfsshare/

4.上一步修改的用户是nfs的匿名用户
[root@localhost /]# grep 'nfsnobody'  /etc/passwd
nfsnobody:x:65534:65534:Anonymous NFS User:/var/lib/nfs:/sbin/nologin
```

配置NFS配置文件、且查看挂载情况---挂载到本地服务器验证

```shell
1.编辑配置文件，写入如下挂载参数
[root@localhost /]# cat /etc/exports
/nfsshare *(insecure,rw,sync,root_squash)

2.重新加载nfs服务
[root@localhost /]# systemctl reload nfs

3.查看NFS服务端挂载情况
[root@localhost /]# showmount -e
Export list for localhost.localdomain:
/nfsshare *

4.查看NFS服务端挂载默认的参数，如下大多数参数都是默认的，不做过多了解
[root@localhost /]# cat /var/lib/nfs/etab
/nfs_data    *(rw,sync,wdelay,hide,nocrossmnt,secure,root_squash,no_all_squash,no_subtree_check,secure_locks,acl,no_pnfs,anonuid=65534,anongid=65534,sec=sys,rw,secure,root_squash,no_all_squash)

5.把本地机器当做客户端做一个简单的挂载测试
[root@localhost mnt]# mount -t nfs 192.168.0.106:/nfsshare /mnt 

# 发现已经可以查看到挂载目录的数据
[root@localhost mnt]# ls /mnt/
haha.txt

# 检查挂载情况
[root@chaogelinux ~]# df -h |tail -1
123.206.16.61:/nfs_data   50G   22G   26G   47% /mnt

[root@localhost mnt]# df -h |tail -1
192.168.0.106:/nfsshare  8.0G  1.4G  6.7G   17% /mnt

至此NFS服务端挂载成功，配置完毕
```

6.6.9配置NFS客户端过程

在另外一台linux机器上连接nfs服务

```shell
[root@localhost ~]# yum install nfs-utils rpcbind -y  #安装操作nfs的命令套件

# 确保rpcbind服务正常
systemctl status rpcbind

# 检查远程挂载情况，需要服务端防火墙关闭，或是配置了规则
[root@localhost ~]# showmount -e 192.168.0.106
Export list for 192.168.0.106:
/nfsshare *

[root@localhost ~]# mount -t nfs 192.168.0.106:/nfsshare /mnt
```

此时就可以在客户端查看挂载点的内容

```shell
[root@localhost mnt]# ls
haha.txt
```

配置每次开机挂机都能使用nfs

```shell
[root@web01 nfsShare]# tail -1 /etc/fstab
123.206.16.61:/nfsShare /nfsShare nfs defaults 0 0
```

卸载挂载点（在哪挂的就在哪卸）

```shell
[root@localhost ~]# umount 192.168.0.106:/nfsshare/				卸载挂载点、查看挂载目录无文件内容
[root@localhost ~]# ls /mnt/
```

### 2.22、autofs自动挂载

【autofs自动挂载服务】

作用：对于/etc/fstab配置文件中配置了过多的自动挂载配置、随着资源过多、而且有的资源并不会一指使用、这就会给服务器造成硬件资源的压力、及网络宽带塔里；autofs的作用就是能够在后台检测用户是否访问了该挂载点、访问则自动挂载、不访问则不挂载、或挂载后一段时间不使用也会自动卸载

### 2.23、autofs与mount的区别

autofs：他是一个守护进程、会自动检测该文件系统是否存在、存在且由用户访问时自动挂载、称为动态挂载

mount：手动挂载、写死

### 2.24、autofs的缺点

不适用于高并发访问服务器的场景、宁可使用mount挂载、也不要频繁挂载

### 2.25、安装及修改配置autofs

1、在需要挂载的机器安装.

```shell
[root@localhost ~]# yum install autofs -y
```

2、修改autofs主配置文件

```shell
#1、参考以下的格式（挂载点  配置文件）填写
[root@localhost ~]# grep -Ev '^#' /etc/auto.master
/misc	/etc/auto.misc
/net	-hosts
+dir:/etc/auto.master.d
+auto.master


#2、添加以下内容
[root@localhost ~]# grep -v '^#' /etc/auto.master
/misc    /etc/auto.misc
/- /etc/auto.home  #添加了这里，需要创建auto.home配置文件
/net    -hosts
+dir:/etc/auto.master.d
+auto.master

#3、定义子配置文件内容
[root@localhost ~]# cat /etc/auto.home
/var/autofs  -rw,soft,intr 192.168.0.106:/nfsshare
挂载点							NFS远程共享文件夹	

#4、查看挂载情况
[root@localhost ~]# df -h
文件系统                 容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root   17G  1.6G   16G   10% /
devtmpfs                 1.9G     0  1.9G    0% /dev
tmpfs                    1.9G     0  1.9G    0% /dev/shm
tmpfs                    1.9G   12M  1.9G    1% /run
tmpfs                    1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/sda1               1014M  142M  873M   14% /boot
tmpfs                    378M     0  378M    0% /run/user/0


#5、启动autofs
systemctl restart autofs
systemctl enable autofs

#7、进入/var/autofs挂载点则会自动进行挂载
[root@localhost ~]# cd /var/autofs/
[root@localhost autofs]# ls
123.txt  haha.txt
[root@localhost autofs]# df -h
文件系统                 容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root   17G  1.6G   16G   10% /
devtmpfs                 1.9G     0  1.9G    0% /dev
tmpfs                    1.9G     0  1.9G    0% /dev/shm
tmpfs                    1.9G   12M  1.9G    1% /run
tmpfs                    1.9G     0  1.9G    0% /sys/fs/cgroup
/dev/sda1               1014M  142M  873M   14% /boot
tmpfs                    378M     0  378M    0% /run/user/0	
192.168.0.106:/nfsshare  8.0G  1.4G  6.7G   17% /var/autofs				#存在挂载点说明挂载成功
```

可设置多少秒不使用可自动取消挂载

```shell
[root@web01 ~]# cat /etc/autofs.conf |grep -i "timeout ="
timeout = 10
```

i4.清空所有防火墙规则

```SHELL
[root@localhost ~]# iptables -F
[root@localhost ~]# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination  
```

5.云服务器上不要轻易设置、默认拒绝的规则、否则会导致ssh流量无法进入、直接断开远程连接

6.删除第一条规则

```shell
[root@localhost ~]# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         
DROP       icmp --  anywhere             anywhere             icmp echo-request

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination         
[root@localhost ~]# iptables -D INPUT 1
[root@localhost ~]# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination         

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination         

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination 
```

7.禁止访问本机的80端口

```shell
#禁止流量进入，指定tcp类型，拒绝的端口是80，动作是拒绝
iptables -A INPUT -p tcp --dport 80 -j DROP

#客户端访问
192.168.0.106（启动了nginx）

[root@localhost ~]# curl  192.168.0.106
curl: (7) Failed connect to 192.168.0.106:80; 拒绝连接

```



8.禁止ftp服务器的21端口

```SHELL
[root@localhost ~]# iptables -A INPUT -p tcp --dport 21 -j REJECT 

[root@localhost ~]# ftp 192.168.0.106
ftp: connect: 拒绝连接
ftp> 

```

9.只允许指定的ip远程连接此服务器、拒绝其他主机22端口流量

```shell
#iptables自上而下匹配
iptables -A INPUT -s 222.35.242.139/24 -p tcp --dport 22 -j ACCEPT
iptables -A INPUT -p tcp --dport 22 -j REJECT

[root@chaogelinux ~]# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     tcp  --  222.35.242.0/24      anywhere             tcp dpt:ssh
REJECT     tcp  --  anywhere             anywhere             tcp dpt:ssh reject-with icmp-port-unreachable

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination


#换一台ip的机器，直接被拒绝
[root@web01 ~]# ssh root@123.206.16.61
ssh: connect to host 123.206.16.61 port 22: Connection refused

#只要删除第二条拒绝的规则，即可
[root@chaogelinux ~]# iptables -D INPUT 2

#又可以连接了
[root@web01 ~]# ssh root@123.206.16.61


```

10.禁止指定的机器ip、访问本地的80端口策略、可f封禁某些恶意请求

```shell
#此时的防火墙规则
[root@chaogelinux ~]# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
ACCEPT     tcp  --  222.35.242.0/24      anywhere             tcp dpt:ssh
REJECT     tcp  --  anywhere             anywhere             tcp dpt:ssh reject-with icmp-port-unreachable

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination


#在规则链开头，追加一个新规则,禁止某个ip地址，访问本机的80端口
[root@chaogelinux ~]# iptables -I INPUT -p tcp -s 222.35.242.139/24 --dport 80 -j REJECT
[root@chaogelinux ~]# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
REJECT     tcp  --  222.35.242.0/24      anywhere             tcp dpt:http reject-with icmp-port-unreachable
ACCEPT     tcp  --  222.35.242.0/24      anywhere             tcp dpt:ssh
REJECT     tcp  --  anywhere             anywhere             tcp dpt:ssh reject-with icmp-port-unreachable

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
[root@chaogelinux ~]#


#此时已经无法访问
yumac: ~ yuchao$curl 123.206.16.61
curl: (7) Failed to connect to 123.206.16.61 port 80: Connection refused
```

11.禁止所有的主机网段、访问本机的8000-9000端口

```shell
[root@chaogelinux ~]# iptables -A INPUT -p tcp -s 0/0 --dport  8000:9000 -j REJECT

```



### 4.1web基础之http请求

#### 4.1.1web基础

HTTP：web使用的一种名为超文本传输协议、完成客户端到服务端的一系列请求过程

![](notes/image-20200113104128851-1603868027131.png)

我们在浏览器输入某个网址或域名、这里的网址或域名称为URL（统一资源定位符）、通过获取某台服务器上的文件资源、从而在界面渲染成图像呈现给用户

#### 4.1.2TCP与IP协议

TCP\IP协议：是互联网中运作的基础、HTTP是TCP\IP的子集；TCP/IP协议是互联网协议的总称

常用的协议：

- ICMP：lnternet控制报文协议、属于TCP/IP的协议簇的一个子协议、用于早IP主机、路由器之间传递控制信息
- IP：网际协议、互联网协议
- DNS：建议在TCP或UDP协议之上、默认使用53端口。客户端默认以UPD传输协议传输、但在广域网中不适合传送大的数据包、即规定报文长度超过512字节的则采用TCP协议进行数据传输
- UDP：支持无连接的传输协议、称为用户数据包协议、为应用程序提供一种无需建立连接就可以发送封装的IP数据包的方法
- FTP：文件传输协议、是TCP/IP的协议之一；FTP又客户端、服务端两部分组成；FTP服务端存储资源、FTP客户端则通过访问FTP服务端上获取资源
- SNMP：IP网络管理节点的标准协议；属于应用层协议。管理源通过该协议能够有效管理网络效能、发现并解决网络问题以及规划网络增长；还可通过SNMP接收随机消息（及事件报告）网络管理系统获知网络系统出现问题。
- HTTP：简单的请求-响应的协议、通常运行在tcp协议之上、它指定客户端可能发送服务器什么样的消息以及得到什么样的响应



TCP/IP协议分为应用层、运输层、网络层、数据链路层



<img src="notes/image-20200113105732826-1603868030368.png" style="zoom:50%;" />



- 数据链路层：处理网络连接的硬件层、如操作系统、网卡等硬件设备驱动
- 网络层：处理网络上传的数据包、规定了数据传输的路线、目的地
- 传输层：用于在网络中两台计算机之间的数据传输、TCP（传输控制协议）、UDP（用户数据报协议）
- 应用层：决定了向用户提供应用服务器时的活动，如FTP、DNS、HTTP等

#### 4.1.3TCP传输原理

<img src="notes/image-20200113110505055-1603868032902.png" style="zoom:50%;" />



HTTP协议传输时、通过分层的顺序与目标机器通信；发送端从应用层向下走、接收方目的地是应用层

1. 客户端发送页面请求；
2. TCP/IP协议为了传输方便、在TCP传输层、将请求报文分割、且在每个报文上打上标记、转发给网络层；
3. 网络层再增加一个标记、目标机器的MAC地址发给链路层、这时网卡就知道数据发给谁了
4. 接收者在链路层收到数据、向上走、最终到应用层



#### 4.1.4TCP协议之三次握手

TCP协议位于传输层，提供可靠的字节流服务（Byte Stream Service），指的是以字节流的形式传递给接收者，没有固定的报文边界限制，只能知道总共发送的数据，但是不知道一次能读取到多少数据，为了更容易传输大数据将数据切割了。

为了数据传输的准确性，服务端和客户端之间需要三次的交互（三次握手）

- 第一次，client发送`syn`包`（syn=j）`给server，进入SYN_SEND状态，等待服务器确认
- 第二次，server收到`syn`包，确认client发来的syn（ack=j+1），同时自己发送一个SYN包（syn=k），也就是SYN+ACK包，此时server进入SYN_RECV状态
- 第三次，客户端接收到服务器的`SYN+ACK`包，向服务器发送确认包`ACK(ack=k+1)`，此时包发送完毕了，客户端和服务器进入`ESTABLISHED`状态，完成三次握手。

<img src="notes/image-20200113153621079-1603868039050.png" style="zoom:33%;" />







#### 4.1.5数据包封装

<img src="notes/image-20200113140445806-1603868042145.png" style="zoom:33%;" />

数据包的封装：数据包在每一次传输时、都会加上上一层所属的首部信息；反之接收方在数据传输的时候、每一层都会进行相应的解包。

#### 4.1.6IP协议

IP协议处于OSI模型的网络层、该协议的作用就是将数据包发送到到指定的目的地

数据包里面重要的就是IP地址和MAC地址

#### 4.1.7IP与APR

MAC地址属于链路数据、IP的通信依赖于MAC地址、通常在就局域网内的通信比较少、大多数都是进行公网通讯、这就要经过多台计算机中转才能到达目标机器；在中转时、利用下一站的中转设备MAC地址来搜索下一个中转目标、APR协议就是格局通信方的IP地址来解析出MAC地址。

<img src="notes/image-20200113143833131-1603868044680.png" style="zoom:33%;" />

这就好比送快递的货车，将货物送到集散中心，明确送货后，快递公司能够查询送件的目的地，明确出一条路线，下一站送往哪一个集散中心，最终的集散中心再判断能否送到客户家里。

#### 4.1.8DNS协议

<img src="notes/image-20200113154222880-1603868047750.png" style="zoom:33%;" />



#### 4.1.9HTTP之URL与URI

- URL（Uniform Resource Locator,统一资源定位符，就是协议类型+域名或IP地址+端口+多级目录资源（具体定位一个人在哪个地方）
- URI（Uniform Resource Identifier ）中文叫“统一资源标识符”，是一个用于标识某一互联网资源名称的字符串，在世界范围内标识定位某一个唯一信息资源。（定位一个人的身份证号、唯一的）
- URI主要用在各种www客户端和服务器程序上，URI可以用一种统一的格式来描述各种信息资源，包括文件，服务器地址和目录等

【URL的组成】

1. 协议
2. 主机ip或域名
3. 端口
4. 文件资源具体地址

第一部分用"://"隔开，第二部分用"/"符号隔开；例如：https://www.luffycity.com/play/22554

#### 4.2.0HTML

超文本标记语言（英语：HyperText Markup Language，简称：HTML）是一种用于创建网页的标准标记语言。

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>chaoge.linux</title>
</head>
<body>
    <h1>我的第一个标题</h1>
    <p>我的第一个段落。</p>
</body>
</html>
```

获取html源码的方式

```shell
curl www.pythonav.cn

浏览器检查网页源码
```

#### 4.2.1CSS

- CSS 指层叠样式表 (**C**ascading **S**tyle **S**heets)
- CSS作用是定义如何显示HTML元素样式

例如：美化网页中的图片

#### 4.2.3JS

- JavaScript 是互联网上最流行的脚本语言，这门语言可用于 HTML 和 web，更可广泛用于服务器、PC、笔记本电脑、平板电脑和智能手机等设备。
- JavaScript 是一种轻量级的编程语言。
- JavaScript 是可插入 HTML 页面的编程代码。
- JavaScript 插入 HTML 页面后，可由所有的现代浏览器执行。

#### 4.2.4静态网页资源

- 在网页设计中，纯HTMl格式的网页（包含图片，视频，JS，CSS等样式）通常被称作“静态网页”。
- 静态网页是相对于动态网页而言的，是指没有后台数据库，不包含程序，不可交互的网页。
- 静态网页的特点
  开发人员写了什么，显示就是什么，一旦编写完成，就不会有任何改变。静态网页一般适用于更新较少的展示型网页，例如（酒水，家具，水果等宣传页），是很多中小网站的展示方式。

静态网页资源对应文件扩展名为

- 纯文本文件，如.htm .html .xml .js .css
- 图片或数据文档，如 .jpg .gif .bmp .txt .ppt
- 视频类文件 .mp4 .avi .flv 等

静态网页重要特性

- 每个页面有一个固定的url地址，url地址不含有问号"?"或"&"等符号
- 网页一经发布到服务器，网页内容是保存在服务器文件系统上的，每个网页都是独立的一个文件
- 网页内容固定不变，容易被搜索引擎收录（优点）
- 网页没有数据库支撑，在网站制作和维护上工作量很大（缺点）
- 网页的交互性很差，缺少程序的功能实现（缺点）
- 客户端解析网址时，由于不需要读取数据库，因此服务器端可以接受更高的并发访问。请求到来时，直接从磁盘上返回数据。（优点）

#### 4.2.5动态网页资源

动态网页地址

动态网页与静态网页的对比

1. 服务端需要通过执行程序做出处理，发送给客户端的是程序的运行结果
2. 动态网页是和静态网页相对而言的，动态网页的url后缀一般是.asp  .aspx  .php .js .cgi 
3. 并且动态网页都有标志性的符号"? &"，后端都有数据库的支持。

动态网页资源特点

1. 网页以数据库技术为支撑，大大降低网站维护的工作量
2. 动态网页技术的网站可以实现更多的功能，如用户注册，用户登录，投票，用户管理，博客管理等
3. 动态网页不是独立存在服务器上的网页文件，用户请求动态程序时，服务器解析程序并且可能读取数据库返回一个完整的网页内容
4. 搜索引擎（爬虫）一般不会抓取网址中的“？”后面的内容，因此企业都会做伪静态技术页面



#### 4.2.6并发模型

进程：linux的资源单位

线程：由进程发起、正真负责工作的是进程

【单进程模式】

单进程：

这就是一台服务器，单进程处理请求，一次只能处理一个请求，剩余的请求排队，当服务器连接数满了，服务器就会拒绝连接了。（比如银行内只有一个窗口办理业务，办理业务的人数多的话只能一个一个排队等候办理）

单进程和单线程

单进程和单线程没有区别，因为一个进程至少有一个工作线程。循环处理请求是最初级的做法。当大量请求进来的时候，单线程一个一个处理请求，请求容易积压，得不到响应。这是无并发的模式。

【多进程模式】

多进程：

服务器接收多个用户请求，服务器主进程监听端口80以及用户连接数、主进程派生出多个子进程来处理、处理完毕后再由主进程销毁

父进程启动多个子进程，每个子进程响应一个请求

主进程
    子进程1
    子进程2
    子进程3
    子进程3
这样的设计好处是隔离性，即使挂掉某一个子进程也不影响父进程，缺点是对系统资源消耗较大。

【复用的IO模型】

银行开10个柜台假设已是极限，如何还能加快事情的处理效率？

- 原本一个柜台只有一个客服，处理一个客户要5分钟
- 现在每个柜台安排多个工作人员，第一个人负责客户问题接待，第二个人负责票据打印，第三个人负责备案文档....
- 如此这般，当第二个工作人员打印票票据的时候，第一个工作人员，又可以接待下一个客户
- 对于用户而言，无感知后台的工作人员是几位，处理一个客户问题的时间大大缩短了

复用模型：这种模型是一个进程响应n个请求，但并不是单纯的一个进程，而是背后的多线程在工作。

4.2.7HTTP请求和响应

<img src="notes/image-20200114110437946-1603868053046.png" style="zoom:33%;" />

如图一次完整的http请求处理流程

1. 建立连接，客户端发起请求，与服务器完成三次握手之后再建立连接
2. 接收请求，服务器接收到请求后，内核根据socket把客户端请求的资源交给应用程序
3. 处理请求，对请求报文解析，明确客户端请求的资源与请求方法等信息
4. 访问资源，内核去磁盘中调取资源
5. 构建响应，应用程序创建响应报文
6. 发送响应，通过socket发给客户端
7. 记录事务，记录日志

#### 4.2.7HTTP事务

**事务**

事务是指是程序中一系列严密的逻辑操作，而且所有操作必须全部成功完成，否则在每个操作中所作的所有更改都会被撤消。可以通俗理解为：就是把多件事情当做一件事情来处理，好比大家同在一条船上，要活一起活，要完一起完 。

事务有四个特性（ACID）

- **原子性**（Atomicity）：执行操作指令，要么全部成功，要么全部失败，只要一个失败，其他指令都失效，进行数据回滚，回到执行命令之前的操作
- **一致性**（Consistency）**：**事务的执行使数据从一个状态转换为另一个状态，但是对于整个数据的完整性保持稳定。
- **隔离性**（Isolation）**：**隔离性是当多个用户并发访问数据库时，比如操作同一张表时，数据库为每一个用户开启的事务，不能被其他事务的操作所干扰，多个并发事务之间要相互隔离。
- **持久性**（Durability）**：**当事务正确完成后，它对于数据的改变是永久性的。

例子说明

```shell
A想要从自己的帐户中转1000块钱到B的帐户里。那个从A开始转帐，到转帐结束的这一个过程，称之为一个事务。在这个事务里，要做如下操作：

 1. 从A的帐户中减去1000块钱。如果A的帐户原来有3000块钱，现在就变成2000块钱了。
 2. 在B的帐户里加1000块钱。如果B的帐户如果原来有2000块钱，现在则变成3000块钱了。
如果在A的帐户已经减去了1000块钱的时候，忽然发生了意外，比如停电什么的，导致转帐事务意外终止了，而此时B的帐户里还没有增加1000块钱。那么，我们称这个操作失败了，要进行回滚。回滚就是回到事务开始之前的状态，也就是回到A的帐户还没减1000块的状态，B的帐户的原来的状态。此时A的帐户仍然有3000块，B的帐户仍然有2000块。

我们把这种要么一起成功（A帐户成功减少1000，同时B帐户成功增加1000），要么一起失败（A帐户回到原来状态，B帐户也回到原来状态）的操作叫原子性操作。

如果把一个事务可看作是一个程序,它要么完整的被执行,要么完全不执行。这种特性就叫原子性。
```



HTTP事务

![](notes/image-20200117113513732-1603868056401.png)

**1.DNS解析**

1.浏览器解析www.pythonav.cn这个域名对应的IP地址
2.浏览器搜索自身的DNS缓存，是否存在域名对应的记录
3.DNS缓存中也找不到，系统读取hosts文件，查看是否存在对应IP记录
4.hosts文件也没有，浏览器会发起DNS系统调用，向本地配置的DNS服务器地址发起域名解析请求，对应的域名服务器再寻找是否存在记录

**2.建立TCP连接**

解析到对应的IP地址之后，User-Agent正常是浏览器，会以随机端口（1024<端口<65535）向服务器的80端口发起tcp连接
此请求经过TCP/IP的四层封包之后，进入服务器,进行解包操作，进入网卡，然后内核的TCP\IP协议栈，且通过防火墙，最后达到web应用程序，建立TCP连接。

1. 第一次握手：客户端发送syn包(syn=j)到服务器，并进入SYN_SEND状态，等待服务器确认；

2. 第二次握手：服务器收到syn包，必须确认客户的SYN（ack=j+1），同时自己也发送一个SYN包（syn=k），即SYN+ACK包，此时服务器进入SYN_RECV状态；

3. 第三次握手：客户端收到服务器的SYN＋ACK包，向服务器发送确认包ACK(ack=k+1)，此包发送完毕，客户端和服务器进入ESTABLISHED状态，完成三次握手。


握手过程中传送的包里不包含数据，三次握手完毕后，客户端与服务器才正式开始传送数据。理想状态下，TCP连接一旦建立，在通信双方中的任何一方主动关闭连接之前，TCP 连接都将被一直保持下去。断开连接时服务器和客户端均可以主动发起断开TCP连接的请求，断开过程需要经过“第四次握手”，就是服务器和客户端交互，最终确定断开。

**3.发起HTTP请求**

HTTP协议即超文本传送协议(Hypertext Transfer Protocol )，是Web联网的基础，也是手机联网常用的协议之一，HTTP协议是建立在TCP协议之上的一种应用。HTTP连接最显著的特点是客户端发送的每次请求都需要服务器回送响应，在请求结束后，会主动释放连接。从建立连接到关闭连接的过程称为“一次连接”。

1）在HTTP 1.0中，客户端的每次请求都要求建立一次单独的连接，在处理完本次请求后，就自动释放连接。
2）在HTTP 1.1中则可以在一次连接中处理多个请求，并且多个请求可以重叠进行，不需要等待一个请求结束后再发送下一个请求。由于HTTP在每次请求结束后都会主动释放连接，因此HTTP连接是一种“短连接”，要保持客户端程序的在线状态，需要不断地向服务器发起连接求。通常 的做法是即时不需要获得任何数据，客户端也保持每隔一段固定的时间向服务器发送一次“保持连接”的请求，服务器在收到该请求后对客户端进行回复，表明知道客户端“在线”。若服务器长时间无法收到客户端的请求，则认为客户端“下线”，若客户端长时间无法收到服务器的回复，则认为网络已经断开。



经过TCP三次握手后，浏览器发起HTTP请求，使用HTTP的GET方法，请求URL是/，按照HTTP/1.0协议。

请求生成的日志

```shell
192.168.178.1 - - [17/Jan/2020:14:30:11 +0800] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.117 Safari/537.36"
```



【request Method】

GET、向服务器获取数据，资源
POST、向服务器提交数据，登录，注册
HEAD、获取URL的响应头信息（只要脑袋），不要响应主体信息(不要身体数据)
PUT、将请求主体部分发给服务器
DELETE、删除服务器指定的资源
TRACE、追踪请求到达服务器发生的变动
OPTIONS、让服务器返回对指定的URL支持的所有请求方法



【URL请求体】

<img src="notes/image-20200117142810462-1603868059681.png" style="zoom:50%;" />

- Accept 就是告诉服务器端，我接受那些MIME类型

- Accept-Encoding 这个看起来是接受那些压缩方式的文件

- Accept-Lanague 告诉服务器能够发送哪些语言

- Connection 告诉服务器支持keep-alive特性

- Cookie 每次请求时都会携带上Cookie以方便服务器端识别是否是同一个客户端

- Host 用来标识请求服务器上的那个虚拟主机，比如Nginx里面可以定义很多

- 虚拟主机．那这里就是用来标识要访问那个虚拟主机。

- User-Agent 用户代理，一般情况是浏览器，也有其他类型，如：wget curl 搜引擎的蜘蛛等

- If-Modified-Since 是浏览器向服务器端询问某个资源文件如果自从什么时间修

  过，那么重新发给我，这样就保证服务器端资源．文件更新时，浏览器再次去

  求，而不是使用缓存中的文件。

- If-None-Match：本地缓存中存储的文档的ETag标签是否与服务器文档的Etag不匹配；

【状态码】

HTTP状态码是用以表示网页服务器超文本传输协议响应状态的3位数字代码。

HTTP请求状态如何，用状态码表示结果

状态码类别

1xx 信息状态码，服务器收到请求，需要客户端继续操作
2xx 操作成功
3xx 重定向状态码，需要进一步的操作
4xx 客户端错误，请求语法错误等
5xx 服务端错误，服务器处理过程中出错了

常见状态码

```
一些常见HTTP状态码为：
200 – 服务器成功返回网页
404 – 请求的网页不存在
503 – 服务不可用

常见HTTP状态码大全

1xx（临时响应）
表示临时响应并需要请求者继续执行操作的状态代码。

代码 说明
http状态码 100 （继续） 请求者应当继续提出请求。 服务器返回此代码表示已收到请求的第一部分，正在等待其余部分。
http状态码 101 （切换协议） 请求者已要求服务器切换协议，服务器已确认并准备切换。

2xx （成功）
表示成功处理了请求的状态代码。
代码 说明
http状态码 200 （成功） 服务器已成功处理了请求。 通常，这表示服务器提供了请求的网页。
http状态码 201 （已创建） 请求成功并且服务器创建了新的资源。
http状态码 202 （已接受） 服务器已接受请求，但尚未处理。
http状态码 203 （非授权信息） 服务器已成功处理了请求，但返回的信息可能来自另一来源。
http状态码 204 （无内容） 服务器成功处理了请求，但没有返回任何内容。
http状态码 205 （重置内容） 服务器成功处理了请求，但没有返回任何内容。
http状态码 206 （部分内容） 服务器成功处理了部分 GET 请求。

3xx （重定向）
表示要完成请求，需要进一步操作。 通常，这些状态代码用来重定向。

代码 说明
http状态码 300 （多种选择） 针对请求，服务器可执行多种操作。 服务器可根据请求者 (user agent) 选择一项操作，或提供操作列表供请求者选择。
http状态码 301 （永久移动） 请求的网页已永久移动到新位置。 服务器返回此响应（对 GET 或 HEAD 请求的响应）时，会自动将请求者转到新位置。
http状态码 302 （临时移动） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。
http状态码 303 （查看其他位置） 请求者应当对不同的位置使用单独的 GET 请求来检索响应时，服务器返回此代码。
http状态码 304 （未修改） 自从上次请求后，请求的网页未修改过。 服务器返回此响应时，不会返回网页内容。
http状态码 305 （使用代理） 请求者只能使用代理访问请求的网页。 如果服务器返回此响应，还表示请求者应使用代理。
http状态码 307 （临时重定向） 服务器目前从不同位置的网页响应请求，但请求者应继续使用原有位置来进行以后的请求。

4xx（请求错误）
这些状态代码表示请求可能出错，妨碍了服务器的处理。

代码 说明
http状态码 400 （错误请求） 服务器不理解请求的语法。
http状态码 401 （未授权） 请求要求身份验证。 对于需要登录的网页，服务器可能返回此响应。
http状态码 403 （禁止） 服务器拒绝请求。
http状态码 404 （未找到） 服务器找不到请求的网页。
http状态码 405 （方法禁用） 禁用请求中指定的方法。
http状态码 406 （不接受） 无法使用请求的内容特性响应请求的网页。
http状态码 407 （需要代理授权） 此状态代码与 401（未授权）类似，但指定请求者应当授权使用代理。
http状态码 408 （请求超时） 服务器等候请求时发生超时。
http状态码 409 （冲突） 服务器在完成请求时发生冲突。 服务器必须在响应中包含有关冲突的信息。
http状态码 410 （已删除） 如果请求的资源已永久删除，服务器就会返回此响应。
http状态码 411 （需要有效长度） 服务器不接受不含有效内容长度标头字段的请求。
http状态码 412 （未满足前提条件） 服务器未满足请求者在请求中设置的其中一个前提条件。
http状态码 413 （请求实体过大） 服务器无法处理请求，因为请求实体过大，超出服务器的处理能力。
http状态码 414 （请求的 URI 过长） 请求的 URI（通常为网址）过长，服务器无法处理。
http状态码 415 （不支持的媒体类型） 请求的格式不受请求页面的支持。
http状态码 416 （请求范围不符合要求） 如果页面无法提供请求的范围，则服务器会返回此状态代码。
http状态码 417 （未满足期望值） 服务器未满足”期望”请求标头字段的要求。

5xx（服务器错误）
这些状态代码表示服务器在尝试处理请求时发生内部错误。 这些错误可能是服务器本身的错误，而不是请求出错。

代码 说明
http状态码 500 （服务器内部错误） 服务器遇到错误，无法完成请求。
http状态码 501 （尚未实施） 服务器不具备完成请求的功能。 例如，服务器无法识别请求方法时可能会返回此代码。
http状态码 502 （错误网关） 服务器作为网关或代理，从上游服务器收到无效响应。
http状态码 503 （服务不可用） 服务器目前无法使用（由于超载或停机维护）。 通常，这只是暂时状态。
http状态码 504 （网关超时） 服务器作为网关或代理，但是没有及时从上游服务器收到请求。
http状态码 505 （HTTP 版本不受支持） 服务器不支持请求中所用的 HTTP 协议版本。
```

响应头信息

![](notes/image-20200117145856083 (1)-1603868064140.png)

```shell
Connection            使用keep-alive特性
Content-Encoding      使用gzip方式对资源压缩
Content-Length: 主体的长度
Content-type          MIME类型为html类型，字符集是 UTF-8
Date                  响应的日期
Server                使用的WEB服务器
Last-Modified：最后一次修改的时间
Server：服务器程序软件名称和版本
```

4.浏览器解析HTML

浏览器拿到index.html文件之后，解析html网页文件，遇见静态资源（js、css、img）就去服务器再次发请求下载，这个时候就用上keep-alive特性了，建立一次HTTP连接，可以请求多个资源，下载资源的顺序就是按照代码里的顺序，但是由于每个资源大小不一样，而浏览器又多线程请求请求资源，顺序并不一定是代码里面的顺序。

<img src="notes/image-20200117151921428-1603868067155.png" style="zoom:50%;" />

5.浏览器对页面进行渲染

浏览器对请求到的静态资源进行渲染

1. dns解析
2. 发起tcp三次握手
3. 建立tcp连接后发起http请求
4. 服务器响应http请求，返回html资源
5. 浏览器解析html代码，请求html中的其他静态资源
6. 浏览器渲染页面，呈现画面












