## 添加软件包
我们使用CentOS7的DVD安装光盘，往软件频道里添加软件。    

首先我们创建好本地软件包的目录结构:    

```
# mkdir /var/distro-trees/centos7_64 -p
# chmod 755 /var/
# chmod 755 /var/distro-trees/
# chmod 755 /var/distro-trees/centos7_64/
```

加载DVD ISO到某个位置，这里我们选择加载到/mnt2:    

```
# mount -t iso9660 -o loop /mnt/iso/CentOS-7-x86_64-Everything-1503-01.iso /mnt2/
```

从该加载位置拷贝DVD里所有内容到我们预定义好的目录下:    

```
# cp -ar /mnt2/* /var/distro-trees/centos7_64/
```

使用以下命令同步该目录下的软件包到软件频道:    

```
# yum install -y wget
# wget https://127.0.0.1/pub/RHN-ORG-TRUSTED-SSL-CERT -O \
/usr/share/rhn/RHN-ORG-TRUSTED-SSL-CERT --no-check-certificate 
# /usr/bin/rhnpush -v --channel=centos7-x86_64 --server=https://localhost/APP \
--dir="/var/distro-trees/centos7_64/Packages" -u admin -p spacewalk123
```
上传进度估计在30分钟左右，上传完毕后，刷新频道信息页，我们可以看到，ISO里的8652个包已经
被添加进频道了。   

![/images/2015_09_09_18_38_44_784x234.jpg](/images/2015_09_09_18_38_44_784x234.jpg)   
