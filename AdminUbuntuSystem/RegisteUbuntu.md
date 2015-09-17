## 注册Ubuntu系统
本节我们将把一台已有的Ubuntu机器注册到Spacewalk服务器。    

### 创建新密钥
在Spacewalk服务器上， 点击系统 -> 激活码 -> 密钥, 创建一个新的密钥，结果如下所示:    

![/images/2015_09_16_12_30_04_488x405.jpg](/images/2015_09_16_12_30_04_488x405.jpg)   

在"子频道"一栏里，选择上三个子频道:    

![/images/2015_09_16_12_30_46_379x541.jpg](/images/2015_09_16_12_30_46_379x541.jpg)    

### 节点机准备
新增一台已经安装好的Ubuntu机器到SpacewalkNetwork网络。    
注意更改其DNS地址为Spacewalk服务器，IP地址可以由DHCP获得或者静态设置。   

可以参考下列配置，我们增加了一台主机名为UbuntuNode, IP地址为10.17.20.202的机器:   

```
adminubuntu@UbuntuNode:~$ cat /etc/hosts
127.0.0.1       localhost
127.0.1.1       UbuntuNode
10.17.20.201    UbuntuNode

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
adminubuntu@UbuntuNode:~$ cat /etc/hostname
UbuntuNode
adminubuntu@UbuntuNode:~$ cat /etc/network/interfaces
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto eth0
iface eth0 inet static
address 10.17.20.202
netmask 255.255.255.0
gateway 10.17.20.1
dns-nameservers 10.17.20.2
```

### 安装包
Ubuntu系统上需要安装相应的客户端包才可以使用Spacewalk服务器提供的服务。    

```
# apt-get install -y apt-transport-spacewalk rhnsd
```

修正一个python XMLRPCLib的Bug:     

```
# vim /usr/lib/python2.7/xmlrpclib.py
def dump_nil (self, value, write):
- if not self.allow_none:
- raise TypeError, "cannot marshal None unless allow_none is enabled"
+# if not self.allow_none:
+# raise TypeError, "cannot marshal None unless allow_none is enabled"
write("<value><nil/></value>")
```

现在开始，把节点机注册到Spacewalk服务器上:     

```
# apt-get install -y python-libxml2
# mkdir /var/lock/subsys
# rhnreg_ks --activationkey=1-ubuntu1404 --serverUrl=http://spacewalkrootnode/XMLRPC 
```

登记仓库到本机：    

```
# cat /etc/apt/sources.list.d/spacewalk.list
deb spacewalk://spacewalkrootnode channels: main trusty-backports trusty-updates trusty-security
# mv /etc/apt/sources.list /etc/apt/sources.list.bak
# apt-get update
```
登记完毕后，本机就可以使用Spacewalk服务器提供的仓库来安装、同步软件了。可以通过以下命令
检查本机注册到的频道:    

```
# rhn-channel --list
trusty-backports
trusty-security
trusty-updates
ubuntu1404
```

### 已知问题
用Spacewalk频道安装包时，会出现以下错误:    

```
# apt-get install vim
Reading package lists... Done
Building dependency tree       
Reading state information... Done
vim is already the newest version.
You might want to run 'apt-get -f install' to correct these:
The following packages have unmet dependencies:
 dh-python : Depends: python3:any (>= 3.3.2-2~) but it is not installable
 lsb-release : Depends: python3:any (>= 3.3.2-2~) but it is not installable
 python-apt : Depends: python:any (>= 2.7.1-0ubuntu2) but it is not installable
 python-dbus : Depends: python:any (>= 2.7.1-0ubuntu2) but it is not installable
 python-gi : Depends: python:any (>= 2.7.1-0ubuntu2) but it is not installable
 python-gobject-2 : Depends: python:any (>= 2.7.1-0ubuntu2) but it is not installable
 python-libxml2 : Depends: python:any (>= 2.7.1-0ubuntu2) but it is not installable
 python-newt : Depends: python:any (>= 2.7.1-0ubuntu2) but it is not installable
 python-openssl : Depends: python:any (>= 2.7.1-0ubuntu2) but it is not installable
E: Unmet dependencies. Try 'apt-get -f install' with no packages (or specify a
solution).
```
这是因为Debian 引入的Multi-Arch:    
[https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=723586](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=723586)    

而Spacewalk在创建软件库用的metadata时候，忽略了Multi-Arch这个选项。   

我们可以手动的更改Packages和Packages.gz文件，把这个选项添加上去:    

```
# vim /var/cache/rhn/repodata/ubuntu1404/Packages
Package: python
Version: 2.7.5-5ubuntu3
+ Multi-Arch: allowed
Architecture: amd64

--------------

Package: python3
Version: 3.4.0-0ubuntu2
+ Multi-Arch: allowed
Architecture: amd64
```

然后手动创建Packages.gz:    

```
# mv Packages.gz Packages.gz.back
# gzip -c Packages>Packages.gz
```
这时再执行apt-get安装包就不会出现上面的冲突了。    
