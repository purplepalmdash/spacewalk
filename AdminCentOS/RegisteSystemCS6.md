## 注册CentOS6
CentOS6系列的机器注册到Spacewalk节点相类似，只不过需要制定不同版本的安装包:    

```
# rpm -Uvh \
http://yum.spacewalkproject.org/2.3-client/RHEL/6/x86_64/
spacewalk-client-repo-2.3-2.el6.noarch.rpm
#  rpm -Uvh http://dl.fedoraproject.org/pub/epel/
epel-release-latest-7.noarch.rpm
```

安装所需要的包:    

```
# yum install -y rhn-client-tools rhn-check \ 
rhn-setup rhnsd m2crypto yum-rhn-plugin
```

安装SSL自签名文件:    

```
# rpm -Uvh http://spacewalk/pub/rhn-org-trusted-ssl-cert-1.0-1.noarch.rpm
```

注册到已有的key:    

```
# rhnreg_ks --serverUrl=https://spacewalk/XMLRPC
--sslCACert=/usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT  --activationkey=1-centos6-x86_64
```

注册完毕后在Spacewalk的Web界面中可以看到系统信息。     

激活osad:    

```
# yum install -y osad
# vim /etc/sysconfig/rhn/osad.conf
osa_ssl_cert = /usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT
# yum install -y python-hashlib
# service osad start
# chkconfig osad on
```

在网页端激活provisioning权限后，在客户端安装包并激活远程命令:    

```
# yum install -y rhncfg-actions
# rhn-actions-control --enable-run
```

安装oscap:    

```
# yum install -y spacewalk-oscap
# yum install openscap-utils scap-security-guide -y
```

oscap的配置如下:    

![/images/2015_09_21_15_24_49_620x465.jpg](/images/2015_09_21_15_24_49_620x465.jpg)    

点击"调度"按钮，开始scap扫描。    
