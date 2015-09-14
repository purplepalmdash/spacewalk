## 创建安装树

### 查看Kickstart

点击系统-> Kickstart ， 可以看到，当前Spacewalk服务器上还没有Kickstart树.     

![/images/2015_09_14_12_18_15_813x435.jpg](/images/2015_09_14_12_18_15_813x435.jpg)   

我们现在来创建一个CentOS7的Kickstart.     

### 创建Kickstart
点击"创建Kickstart侧写"， 侧写也不知道是谁翻译的，完全没见过这个词，Profile不应该被翻译
成"配置档"之类的吗？     

![/images/2015_09_14_12_23_02_398x196.jpg](/images/2015_09_14_12_23_02_398x196.jpg)    

填入Kickstart配置档名字：    

![/images/2015_09_14_12_24_42_629x431.jpg](/images/2015_09_14_12_24_42_629x431.jpg)   

继续点下一步，不更改任何配置，直接点下一步:    

![/images/2015_09_14_12_26_11_470x318.jpg](/images/2015_09_14_12_26_11_470x318.jpg)     

配置Root密码，神翻译，叫根密码。。。       

![/images/2015_09_14_12_27_10_499x286.jpg](/images/2015_09_14_12_27_10_499x286.jpg)    

点击"完成"按钮后进入到Kickstart的详细配置页面，在这里我们可以对生成的Kickstart文件作详
细配置。    
配置Kickstart的语法不在我们的讨论范畴，直接接受生成的ks文件，进入到下一章的测试环节。    

### 检查Kickstart
我们可以在spacecmd中检查我们现有的Kickstart配置:    

```
# spacecmd
Welcome to spacecmd, a command-line interface to Spacewalk.

Type: 'help' for a list of commands
      'help <cmd>' for command-specific help
      'quit' to quit

WARNING: Cached credentials are invalid
INFO: Spacewalk Username: admin
Spacewalk Password: 
INFO: Connected to https://localhost/rpc/api as admin
spacecmd {SSM:0}> kickstart_list
CentOS7Kickstart
spacecmd {SSM:0}> kickstart_details CentOS7Kickstart
```

### TFTP文件准备
TFTP需要有相应的文件才可以被启动，我们首先安装PXE所需的包:    

```
# yum install -y cobbler-loaders
```

tftp服务器需要被xinetd所管理，更改其配置:    

```
# vim /etc/xinetd.d/tftp 
service tftp
{
        disable                 = no
```
重新启动xinetd，所以tftp服务也同时被启动.     

```
# service xinetd restart
#  netstat -anp | grep 69| grep xinetd
udp        0      0 0.0.0.0:69              0.0.0.0:*
```

SELINUX需要被禁用：    

```
# vim /etc/selinux/config
SELINUX=disabled
SELINUXTYPE=targeted 
```
在防火墙上打开相关端口的访问权限:    

```
# iptables -I INPUT -p udp -m multiport --dports 69,80,443,25151 -j ACCEPT
```
或者我们可以做得更彻底一点，因为这里是实验网络的缘故，直接关掉CentOS7自带的防火墙
firewalld服务。     

```
# systemctl disable firewalld.service
rm '/etc/systemd/system/dbus-org.fedoraproject.FirewallD1.service'
rm '/etc/systemd/system/basic.target.wants/firewalld.service'
```
做完所有的配置后，重新启动spacewalk服务器，下一章我们将使用一台从网络启动的虚拟机用于
测试我们的Kickstart树。     
