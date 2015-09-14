## 配置DNS和DHCPD
Spacewalk服务器上需要配置好DNS服务器和DHCPD服务器，以便管理好Spacewalk服务器所在子网内
的IP地址及DNS服务，以下是配置步骤。      

### DHCP服务器
首先安装DHCP服务器:     

```
# yum install -y dhcp
```

按照以下的例子配置DHCP服务器, 相关选项含义可以通过参阅man dhcpd.conf来获得:      

```
/etc/dhcp/dhcpd.conf
# 
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp*/dhcpd.conf.example
#   see dhcpd.conf(5) man page
#
# specify name server's hostname or IP address
option domain-name-servers 10.17.20.2;
# default lease time
default-lease-time 600;
# max lease time
max-lease-time 7200;
# this DHCP server to be declared valid 
authoritative;
# specify network address and subnet mask
subnet 10.17.20.0 netmask 255.255.255.0 {
    # specify the range of lease IP address
    range dynamic-bootp 10.17.20.3 10.17.20.254;
    # specify broadcast address
    option broadcast-address 10.17.20.255;
    # specify default gateway
    option routers 10.17.20.1;
    # Specify default dns server
    option domain-name-servers 10.17.20.2;
    filename                   "/pxelinux.0";       
    # default-lease-time         21600;           
    # max-lease-time             43200;      
    next-server                10.17.20.2; 
}
```
写好配置文件后，启动dhcp服务, 并使得它开机启动:    

```
# service dhcpd start
Redirecting to /bin/systemctl start  dhcpd.service
# systemctl enable dhcpd.service
ln -s '/usr/lib/systemd/system/dhcpd.service' \ 
'/etc/systemd/system/multi-user.target.wants/dhcpd.service'
```

### DNS服务器
我们配置的DNS服务器仅仅在Spacewalk服务器所在的网段使用，目的主要是为了提供对类似于
`http://spacewalkrootnode`等地址的解析:     

安装bind9 DNS服务器:    
````
# yum install -y bind bind-utils
```

我们需要把`spacewalkrootnode` 映射为 `10.17.20.2`, 按以下步骤配置:    
首先更改named的配置文件：  `/etc/named.conf`.    
更改53端口所在的端口地址:    

```
options {
        listen-on port 53 { 127.0.0.1; 10.17.20.2; };
#        listen-on-v6 port 53 { ::1; };
...
```
添加允许查询的条目，以使得本地网络里的节点可以查询到它自身的dns:     

```
 allow-query     { localhost; 10.17.20.0/24;};
```
添加一个名为`spacewalkrootnode`的zone:    

```
zone "spacewalkrootnode" {
        type master;
        file "/etc/named/zones/db.spacewalkrootnode";
};
```

增加一个zone定义文件：    

```
# mkdir -p /etc/named/zones/
# vim /etc/named/zones/db.spacewalkrootnode
$TTL 604800
@       IN      SOA     spacewalkrootnode. root.spacewalkrootnode. (
                                3               ;Serial
                                604800          ;Refresh
                                86400           ;Retry 
                                2419200         ;Expire
                                604800 )        ;Negative Cache TTL
;
; name servers - NS records
      IN      NS      spacewalkrootnode.

; name servers  - A records
spacewalkrootnode.         IN      A       10.17.20.2
```
使用以下命令检查配置是否正确:    

```
# named-checkconf
# named-checkzone spacewalkrootnode
/etc/named/zones/db.spacewalkrootnode
zone spacewalkrootnode/IN: loaded serial 3
OK
```

启动named服务，并使得其开机自启动:    

```
# systemctl start named.service
# systemctl enable named.service
```

配置好DNS和DHCPD服务器的基础上，我们可以继续配置可Kickstartable的树。
