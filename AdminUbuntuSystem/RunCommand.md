## 运行命令

通过安装rhncfg包，可以远程执行命令:    

```
# wget https://launchpad.net/~mj-casalogic/+archive/ubuntu/
spacewalk-ubuntu/+files/rhncfg_5.10.14-1ubuntu1%7Esaucy2_all.deb
# dpkg -i rhncfg_5.10.14-1ubuntu1~saucy2_all.deb 
# rhn-actions-control --enable-run
# mkdir /var/spool/rhn
```
同时我们需要添加"Provisioning"的权限。   

设置完毕后，我们就可以在系统中的远程命令选项下，执行远端系统上的Shell命令了。

![/images/2015_09_17_14_27_51_497x573.jpg](/images/2015_09_17_14_27_51_497x573.jpg)   
