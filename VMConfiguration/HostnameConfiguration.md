## 主机名配置
安装和配置Spacewalk前我们需要配置好虚拟机的主机名，之后各节点将使用主机名来获取服务。    

更改/etc/hosts文件, 我们设置主机名为spacewalkrootnode, 之后的配置都基于此主机名:    

```
# vi /etc/hosts
10.17.20.2      spacewalkrootnode
127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```
更改sysconfig中关于network的条目:    

```
# vi /etc/sysconfig/network
NETWORKING=yes
NETWORKING_IPV6=no
HOSTNAME=localhost.localdomain
```
更改hostname文件：    

```
# vi /etc/hostname 
spacewalkrootnode
```
现在可以重启虚拟机，并使用以下命令验证是否修改成功:    

```
# hostname
spacewalkrootnode
# hostname --fqdn
spacewalkrootnode
```
