## 软件管理

### 软件安装/管理
软件的安装和删除和CentOS系列的节点机一样，在Spacewalk服务器上选择好需要安装的软件后，在
客户机器上运行`rhn_check`即可.    

### OSAD
刚注册进Spacewalk的Ubuntu机器详细状态如下:    

![/images/2015_09_17_12_19_05_360x173.jpg](/images/2015_09_17_12_19_05_360x173.jpg)   

我们需要手动安装两个包，以激活Ubuntu上的osad配置:    

```
# wget https://launchpad.net/~mj-casalogic/+archive/ubuntu/
spacewalk-ubuntu/+files/osad_5.11.27-1ubuntu1%7Esaucy5_all.deb
# wget https://launchpad.net/~mj-casalogic/+archive/ubuntu/
spacewalk-ubuntu/+files/pyjabber_0.5.0-1.4ubuntu3%7Esaucy1_all.deb
# dpkg -i pyjabber_0.5.0-1.4ubuntu3~saucy1_all.deb 
# dpkg -i osad_5.11.27-1ubuntu1~saucy5_all.deb
```

配置osad:    

```
# cd /usr/share/rhn
# wget http://spacewalkrootnode/pub/RHN-ORG-TRUSTED-SSL-CERT
# vim /etc/sysconfig/rhn/up2date
sslCACert=/usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT
# vim /etc/sysconfig/rhn/osad.conf
osa_ssl_cert = /usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT
# service osad start
# update-rc.d osad defaults
```

现在osad就被激活并配置成功了，所有的更改将即时被"推送"到客户端。    

osad激活后的系统事件窗口参数如下:    

![/images/2015_09_17_14_09_43_387x206.jpg](/images/2015_09_17_14_09_43_387x206.jpg)   
