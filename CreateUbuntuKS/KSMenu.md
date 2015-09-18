## Kickstart条目
在上一节创建的Kickstart频道实际上是不可用的。Spacewalk自带的Cobbler版本太老的缘故，所以
很多最新的系统都不支持，我们虽然把breed改成了Jauny, 但Jauny仅仅是Ubuntu9.04, 对于
Ubuntu14.04等新的代号, Spacewalk自带的Cobbler实际上是不支持的。所以这章我们将定制化
专用于Ubuntu的Kickstart条目。 

### 方法1
方法1将手动更改默认生成的PXE条目，用于手动制定kickstart文件，

编辑`/var/lib/tftpboot/pxelinux.cfg/default`文件, 找到我们添加的条目，更改如下:

```
LABEL ubuntu1404:1:SpacewalkDefaultOrganization
        kernel /images/ubuntu1404:1:SpacewalkDefaultOrganization/vmlinuz
        MENU LABEL ubuntu1404:1:SpacewalkDefaultOrganization
        append initrd=/images/ubuntu1404:1:SpacewalkDefaultOrganization/initrd.gz
ks=http://192.168.0.79/trustykickstart.cfg ksdevice=eth0 --

        ipappend 2
```

Kickstart文件被存放在外部的一个服务器上，当然我们也可以直接使用本机已有的Kickstart文件
。    

这种方法的坏处是，一旦你添加新的Spacewalk Kickstart条目，这些临时的修改将被覆盖。    

### 方法2
方法2将生成不被Spacewalk所管理的Kickstart条目，这种方法可以持久化保存，更加灵活。    
首先列举出所有支持的distro，然后根据distro创建出新的profile, TrustyPreseed.cfg文件是我
们事先生成的Ubuntu的Kickstart文件。        

```
# cobbler distro list
   centos7:1:SpacewalkDefaultOrganization
   ubuntu1404:1:SpacewalkDefaultOrganization
# cobbler profile add --name=KickstartUbuntu
--kickstart=/var/lib/cobbler/kickstarts/TrustyPreseed.cfg
--distro=ubuntu1404:1:SpacewalkDefaultOrganization
```

运行完此命令后， `/var/lib/tftpboot/pxelinux.cfg/default`文件中将生成对应的条目，更改如
下:    

```
LABEL KickstartUbuntu
        kernel /images/ubuntu1404:1:SpacewalkDefaultOrganization/vmlinuz
        MENU LABEL KickstartUbuntu
        append initrd=/images/ubuntu1404:1:SpacewalkDefaultOrganization/initrd.gz lang=
text  auto-install/enable=true auto
ks=http://10.17.20.2/cblr/svc/op/ks/profile/KickstartUbuntu locale=  text
hostname=KickstartUbuntu domain=local.lan suite=trusty

        ipappend 2
```

这样生成的Kickstart条目是不受Spacewalk管理的，更加灵活和方便。

### 验证
在PXE菜单，选择KickstartUbuntu即可，系统将自动进入到安装状态。    

![/images/2015_09_18_12_11_51_510x247.jpg](/images/2015_09_18_12_11_51_510x247.jpg)   
