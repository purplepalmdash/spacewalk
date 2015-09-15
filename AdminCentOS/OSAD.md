## OSAD

### 核心组件
osad:    

用Python写的客户端的服务，用于响应服务器的ping请求，收到osa-dispatcher发来的消息时，自
动运行rhn_check以响应服务器端的事件。    

osa-dispatcher:    

用Python写的服务器端的服务，用于决定哪个运行osad实例的客户端需要被ping到或运行rhn_check
， 由此把消息传送到客户端。    

jabberd:    

jabberd守护进程实现了RFC 3920和3921定义的XMPP协议。 osad和osa-dispatcher都连接到这个守
护进程以进行消息交互。这个守护进程同时也处理了鉴权。     

### 服务器端
默认情况下，Spacewalk服务器端已经安装并激活了osa-dispatcher和jabberd, 我们可以通过
以下命令检查服务器端是否有运行osa-dispatcher和jabberd:     

```
[root@spacewalkrootnode ~]# ps -ef | grep -i jabber
jabber     738     1  0 03:16 ?        00:00:03 /usr/bin/router -c
/etc/jabberd/router.xml
jabber     739     1  0 03:16 ?        00:00:02 /usr/bin/sm -c /etc/jabberd/sm.xml
jabber     740     1  0 03:16 ?        00:00:02 /usr/bin/c2s -c /etc/jabberd/c2s.xml
jabber     743     1  0 03:16 ?        00:00:00 /usr/bin/s2s -c /etc/jabberd/s2s.xml
root      6861  6839  0 22:49 pts/0    00:00:00 grep --color=auto -i jabber
[root@spacewalkrootnode ~]# ps -ef | grep osa
root      1180     1  0 03:16 ?        00:02:37 /usr/bin/python -s
/usr/sbin/osa-dispatcher --pid-file /var/run/osa-dispatcher.pid
root      6863  6839  0 22:49 pts/0    00:00:00 grep --color=auto osa
```

### 客户器端
在CentOS 7客户端，按下列命令安装和配置osad服务:    

```
# yum install -y osad
# vim /etc/sysconfig/rhn/osad.conf
osa_ssl_cert = /usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT
# yum install -y python-hashlib
# service osad start
# systemctl enable osad.service
```

### 验证
安装完毕后，在Spacewalk服务器端的系统消息信息里，可以看到出现了osad的配置：   

![/images/2015_09_15_11_03_59_404x234.jpg](/images/2015_09_15_11_03_59_404x234.jpg)

现在对客户端安装软件，所有更改将被"即时"推送到客户端。如下图，选择安装gedit包后，事件将
马上进入到"正在安装"状态:    

![/images/2015_09_15_11_09_09_418x351.jpg](/images/2015_09_15_11_09_09_418x351.jpg)       
