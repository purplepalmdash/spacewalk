## 创建Ubuntu Kickstart频道

### 准备
首先将Ubuntu 14.04的安装文件拷贝到我们的dist-tree目录下:    

```
# mount -t  iso9660 -o loop /mnt/iso/ubuntu-14.04.3-server-amd64.iso /mnt1/
# mkdir /var/distro-trees/ubuntu-14.04.3-amd64
# chmod 755 /var/distro-trees/ubuntu-14.04.3-amd64/
# cp -ar /mnt1/* /var/distro-trees/ubuntu-14.04.3-amd64/
```

我们需要从CentOS 7的安装文件里，拷贝一些PXE启动的文件，骗过Spacewalk，用于
Kickstarting:    

```
# mkdir -p /var/distro-trees/ubuntu-14.04.3-amd64/images/pxeboot
# cp /var/distro-trees/centos7_64/images/pxeboot/{initrd.img,vmlinuz} \ 
/var/distro-trees/ubuntu-14.04.3-amd64/images/pxeboot/
```

### 创建频道
在Spacewalk的后台页面，点击系统 -> Kickstart -> 发布 ，

![/images/2015_09_17_15_21_34_800x310.jpg](/images/2015_09_17_15_21_34_800x310.jpg)   

点击"创建发布", 填入图中内容:    

![/images/2015_09_17_15_24_14_619x369.jpg](/images/2015_09_17_15_24_14_619x369.jpg)    

创建完的发布列表如下:    

![/images/2015_09_17_15_25_23_671x293.jpg](/images/2015_09_17_15_25_23_671x293.jpg)   

现在我们创建Kickstart条目, 点击系统-> Kickstart, 在弹出的页面中选择"上传Kickstart文件":    

![/images/2015_09_17_15_15_33_461x227.jpg](/images/2015_09_17_15_15_33_461x227.jpg)   

上传页面如下:    

![/images/2015_09_17_15_28_09_382x342.jpg](/images/2015_09_17_15_28_09_382x342.jpg)    

写入或者上传对应的Kickstart文件:    

![/images/2015_09_17_15_31_11_445x490.jpg](/images/2015_09_17_15_31_11_445x490.jpg)   

### 修改breed
按照上述方法建立的Kickstart条目，其默认breed为Redhat系列，我们需要手动将其更改为Debian
系, 否则安装时会报错, 同时我们也需要修改initrd和kernel参数:    

用`cobbler distro list` 列出所有的distro:   

```
# cobbler distro list
   centos7:1:SpacewalkDefaultOrganization
   ubuntu1404:1:SpacewalkDefaultOrganization
```

用下列命令可以查看该distro的Breed:    

```
# cobbler distro report --name=ubuntu1404:1:SpacewalkDefaultOrganization
```

![/images/2015_09_17_15_37_29_986x239.jpg](/images/2015_09_17_15_37_29_986x239.jpg)    

更改Breed为ubuntu:    

```
# cobbler distro edit --name ubuntu1404:1:SpacewalkDefaultOrganization --breed=ubuntu \
 --os-version=jaunty \
 --initrd=/var/distro-trees/ubuntu-14.04.3-amd64/\
 install/netboot/ubuntu-installer/amd64/initrd.gz \
 --kernel=/var/distro-trees/ubuntu-14.04.3-amd64/install/vmlinuz
```

现在查看更改后的Breed, initrd, kernel参数:    

![/images/2015_09_17_15_43_20_1002x224.jpg](/images/2015_09_17_15_43_20_1002x224.jpg)   


加入测试机后，PXE启动时就可以看到我们添加的条目了:    

![/images/2015_09_17_15_51_07_558x313.jpg](/images/2015_09_17_15_51_07_558x313.jpg)    
